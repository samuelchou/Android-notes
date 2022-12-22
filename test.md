---
description: 撰寫 test 的小撇步
---

# 測試小技巧

## 測試方式

一般使用 org.junit.Assert.assertEquals 來進行測試：

```kotlin
import org.junit.Assert.assertEquals
import org.junit.Test


class MathTest {

    @Test
    fun `calculate`() {
        val cal = Calculator()
        assertEquals(100, cal.plusTwo(33, 67))
    }
}

class Calculator {

    fun plusTwo(number1: Int, number2: Int): Int {
        return number1 + number2
    }
}
```

### 特殊比較： (Byte) Array

byteArray: 直接比較會變成「比較指標」，基本上是一定會 fail 的。\
如需比較內容，需要呼叫 Arrays.equals(), 或者 byteArray.contentEquals(byteArray)

```kotlin
@Test
fun `byte array comparison`() {
    val testString = "Hello, World!"
    
    // this will ALWAYS FAIL
    // assertEquals(testString.toByteArray(), testString.toByteArray())

    // comparison of byteArray should do this instead.
    assertEquals(true, Arrays.equals(testString.toByteArray(), testString.toByteArray()))

    // or by Kotlin extension
    assertEquals(true, testString.toByteArray().contentEquals(testString.toByteArray()))
}

```



## 讀取與使用資源

請在對應的專案資料夾位置建立 `resources` 資料夾：

```
app
> src
  > test
    > java
    > resources
      > folder
        > example.json
```

如此就可以在測試過程中使用相關資源函式：

```kotlin
val classLoader = javaClass.classLoader!!
val inputStream = classLoader.getResourceAsStream("folder/example.json")
```

* `getResourceAsStream` 方法**必須包含路徑全名**（絕對位置，連同資料夾）。
* 底層資料夾**一定要命名**為 `resources` （不可以是 `res` ！），不然在測試中就會讀不到...！！！

推薦設置便捷方法，可以快速閱讀並獲得檔案：

```kotlin
/**
 * Read from resources folder and get string back.
 * @param fileName the file name of mock response. (type is needed. ex. "response.json")
 * @param filePathSuffix the folder path of mock response file. (last slash is needed. ex. "response-raw/")
 */
fun Any.readStringFromResourcesFile(
    fileName: String, filePathSuffix: String
): String {
    val classLoader = javaClass.classLoader!!
    val inputStream = classLoader.getResourceAsStream("$filePathSuffix$fileName")
    val source = inputStream.source().buffer()
    return source.readString(Charsets.UTF_8)
}
```





## 測試環境統一部署

如果你有些操作是腳本內「每一個」測試都需要執行的（例如：虛擬伺服器或reader的啟動與關閉），可以獨立出去為function, 並透過 `@Before` `@After` `@BeforeClass` `@AfterClass` 等關鍵字標記前後執行：

```kotlin
class ConnectAPITest {
    private lateinit var mockWebServer: MockWebServer

    @Before
    fun setup() {
        mockWebServer = MockWebServer()
    }

    @Test
    fun `test 1`() {
        // ...
    }


    @Test
    fun `test 2`() {
        // ...
    }

    @After
    fun shutdown() {
        mockWebServer.shutdown()
    }
}
```

* 有關 `@Before` 與 `@BeforeClass` / `@After` 與 `@AfterClass` 的差別：動態與靜態（物件實例私用或class共享）
  * 舉例來說，你無法在 `@BeforeClass` 宣告／指定一個 var: 那是實例變數。
* Kotlin使用 `@BeforeClass` / `@AfterClass` 的限制：無法與 `lateinit var` 共同使用。
  * 標示的方法必須是靜態。然而， `lateinit var` 無法在 `companiong object` 中使用。

## 處理 Log not mocked 問題

你測試的對象可能會呼叫 `Log` 系列方法，它們在 Android 很常使用、但是在 UnitTest 中「並沒有被 mock 過」。這導致 UnitTest 在執行到 `Log` 系列方法時會拋出異常，並且不允許你進行完整測試。

要解決這個問題，有兩種方法：

1. 把所有 mock 錯誤都無視 / 返回預設值
2. 試圖 mock Log 系列

### &#x20;無視 mock 錯誤並返回預設值

{% embed url="https://stackoverflow.com/a/42653151/9735961" %}

{% embed url="https://stackoverflow.com/a/57958441/9735961" %}

在 build.gradle (app) 加入以下段落：

```groovy
android {
    // ...
    
    testOptions {
        unitTests.returnDefaultValues = true
    }
}
```

然而該方法很危險。 Stack Overflow 的留言提到：

> **Caution:** Setting the `returnDefaultValues` property to true should be done with care. The null/zero return values can introduce regressions in your tests, which are hard to debug and might allow failing tests to pass. **Only use it as a last resort.**

### 試圖 mock Log 系列

{% embed url="https://medium.com/@gal_41749/android-unitests-and-log-class-9546b6480006" %}

將以下 class 放置到資料夾： test/java/android/util :&#x20;

```java
package android.util;

public class Log {
    public static int d(String tag, String msg) {
        System.out.println("d: " + tag + ": " + msg);
        return 0;
    }

    public static int i(String tag, String msg) {
        System.out.println("i: " + tag + ": " + msg);
        return 0;
    }

    public static int w(String tag, String msg) {
        System.out.println("w: " + tag + ": " + msg);
        return 0;
    }

    public static int w(String tag, String msg, Throwable tr) {
        System.out.println("w: " + tag + ": " + msg);
        tr.printStackTrace(System.out);
        return 0;
    }

    public static int e(String tag, String msg) {
        System.out.println("e: " + tag + ": " + msg);
        return 0;
    }

    public static int e(String tag, String msg, Throwable tr) {
        System.out.println("e: " + tag + ": " + msg);
        tr.printStackTrace();
        return 0;
    }
}
```

