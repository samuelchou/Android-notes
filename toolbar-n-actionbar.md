---
description: 在各頁面上懸浮著的神奇東東。（工具列）
---

# Toolbar, ActionBar

## 基礎

{% embed url="https://medium.com/evan-android-note/android-%E7%9A%84%E5%90%84%E7%A8%AEbar-%E5%BE%9Eactionbar%E5%88%B0collapsingtoolbarlayout-c95d33640be4" %}

如果你非常乖巧的使用 Android 內建主題，他會幫你生成自然的「眉毛」——任何畫面最上方都有一小排 Banner, 然後上面 show 你的主題顏色 + 頁面名稱（有時候是應用名稱）。

不過大多時候會有對他做客製化的要求。所以通常都會自幹 Toolbar.

### ActionBar

是最早期的版本。後續已經被 Toolbar 取代。

不過支援庫中仍能看到 `supportActionBar` 等命名。所以在此特別提及聲明一下。

### 客製化 Toolbar 步驟

1. 更換佈景主題：將應用佈景主題（ `Theme` ）更換為 `NoActionBar` 結尾的佈景主題，例如 `Theme.MaterialComponents.DayNight.NoActionBar`&#x20;
2. 製作自定義的 Toolbar: 在介面 XML 中引入 Toolbar 介面元件，例如 `Toolbar` 或  `androidx.appcompat.widget.Toolbar` 等
3. 在頁面套用自定義的 Toolbar: 參考後續章節
4. 使用 Toolbar 相關功能



## 在 Activity 中使用 Toolbar

步驟較為繁瑣：

1. 指定使用的 actionBar
2. 覆寫 menu 生成
3. 處理 menu 按鈕監聽

```kotlin

// in Activity
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // ...
    setSupportActionBar(toolbar)
}

override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    menuInflater.inflate(R.menu.menu_main, menu)
    return true
}

override fun onOptionsItemSelected(item: MenuItem): Boolean {
    // listen to click event
    // return true to remain selected; false to make it single-click
    when (item.itemId) {
        R.id.optionDoSomething -> {
            // do something...
        }
        android.R.id.home -> {
            onBackPressed() // The left-top arrow = "back" operation
            return false
        }
    }
    return super.onOptionsItemSelected(item)
}

```





## 在 Fragment 中使用 Toolbar

{% embed url="https://stackoverflow.com/a/45653449/9735961" %}

雖然 callout `AppCompatActivity.setSupportActionBar(toolbar)` 一樣有效，但建議為了避免 Activity 與 Fragment 的 Toolbar 設定互相衝突、干擾，建議直接透過 toolbar 設置內容：

```java
toolbar.inflateMenu(R.menu.frag_menu_items);
Menu menu = toolbar.getMenu();
```

