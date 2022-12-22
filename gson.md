# Gson

Gson 大概是最受歡迎的 Json 處理庫了。



## Kotlin 專屬 extension function

在 Kotlin 內使用 Gson, 可以考慮寫成 extension function.

不過撰寫為泛型通用時比較麻煩，需要使用到 Kotlin`reified` + `inline` 宣告特性來處理：

{% embed url="https://nphau.medium.com/android-kotlin-how-to-use-typetoken-generics-with-gson-in-kotlin-e2b17c0ac24c" %}

```kotlin
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import java.lang.reflect.Type

inline fun <reified T> typeToken(): Type = object : TypeToken<T>() {}.type

// Convert a String to an Object
inline fun <reified T> String.jsonToObject(): T {
    return Gson().fromJson(this, typeToken<T>())
}

inline fun <reified T> JsonObject.toObject(): T {
    return Gson().fromJson(this, typeToken<T>())
}

inline fun <reified T> JsonElement.toObject(): T {
    return Gson().fromJson(this, typeToken<T>())
}

// How to use it
// ======
data class DemoIntArray {
    val title: String,
    val intArray: IntArray
}

val jsonString = "{ \"title\": \"test\", \"intArray\": [1,2,3] }"
val demoArray = jsonString.jsonToObject<DemoIntArray>()
assertEquals("test", demoArray.title)
assertEquals(3, demoArray.intArray.size)
// ======

```

## 從物件實體獲得 Token

Gson 從字串解析時需要傳入一個 type token.

Type Token 可以透過多種方式獲得：

```kotlin
val typeToken = object : TypeToken<MyObject>() {}.type
val myObj = Gson().fromJson(jsonString, typeToken)
```

如果你拿不到類，但是可以獲得同類實體，也能用以下方式獲取 token:

```kotlin
val compareObj: MyObject
val typeToken = TypeToken.get(compareObj::class.java).type
val myObj = Gson().fromJson(jsonString, typeToken)
```



## JsonNull 與處理方式

有些 API 會回傳 null 值：

```json
{
    "result" {
        "data1": 123.456,
        "data2": null
    }
}
```

上述資料 `data2` 在 Gson 裡面就會被解析成為 `JsonNull` 類。

這在寫 code 判斷 null 時可能會有些不便：

```kotlin
resultObj.get("data2")?.asFloat // will throw UnsupportedOperationException
```

如果希望 JsonNull 也被回傳為 null 來判斷，可以使用以下程式碼：

{% embed url="https://stackoverflow.com/a/55927694/9735961" %}

```kotlin
import com.google.gson.JsonElement
import com.google.gson.JsonObject

fun JsonObject.getNullable(key: String): JsonElement? {
    val value: JsonElement = this.get(key) ?: return null
    if (value.isJsonNull) {
        return null
    }
    return value
}
```

使用上就比照使用即可：

```kotlin
resultObj.getNullableko("data2")?.asFloat
```



