---
description: 這裡紀錄文字類View元件的雜談，例如 TextView, EditText, SearchView 等。
---

# 元件雜談：文字類

## EditText

### 如何對齊其他元件

使用 `marginStart="-4dp"` 即可。

{% embed url="https://stackoverflow.com/q/31735291/9735961" %}

主因： `EditText` 預設內建採用一個 drawable 做為背景，該 drawable 有 padding.

如果想要避免掉的話，也可以指定 `background="null"` 或指派顏色:

{% embed url="https://stackoverflow.com/a/31735381/9735961" %}

```markup
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:background="#00ff00"
        android:text="@string/hello_world"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="HINT"
        android:text="TEXT"
        android:background="@android:color/transparent"
        android:singleLine="true"/>
    <TextView
        android:background="#00ff00"
        android:text="@string/hello_world"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

或者直接提供新的 nine-patch background:

{% embed url="https://stackoverflow.com/a/64387960/9735961" %}

```markup
<?xml version="1.0" encoding="utf-8"?>
<inset xmlns:android="http://schemas.android.com/apk/res/android"
    android:inset="@dimen/spacing_0dp">
    <selector>
        <item android:state_enabled="false">
            <nine-patch
                android:alpha="?android:attr/disabledAlpha"
                android:src="@drawable/abc_textfield_default_mtrl_alpha"
                android:tint="?attr/colorControlNormal" />
        </item>
        <item
            android:state_focused="false"
            android:state_pressed="false">
            <nine-patch
                android:src="@drawable/abc_textfield_default_mtrl_alpha"
                android:tint="?attr/colorControlNormal" />
        </item>
        <item>
            <nine-patch
                android:src="@drawable/abc_textfield_activated_mtrl_alpha"
                android:tint="?attr/colorControlActivated" />
        </item>
    </selector>
</inset>
```

### 輸入監聽

{% hint style="danger" %}
若要達成一次性 TextWatcher, 應避免在監聽內呼叫 editText.removeTextChangedListener(this), **可能會造成閃退**。
{% endhint %}

舉例：

```kotlin
editText.addTextChangedListener(object : TextWatcher {
    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
    }
    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
    }
    override fun afterTextChanged(s: Editable?) {
        println("Text end with $s")
        editText.removeTextChangedListener(this)
    }
})
```

以上 code 在大多時候不會造成問題，但是若該 EditText 有兩個以上的 `TextWatcher` （例如：該 EditText 有**設置 Two-way Binding** 時）就**可能觸發 `IndexOutOfBoundException` ** 。

原因：

> 因為 TextView 的實作裡 sendBeforeTextChanged() / sendOnTextChanged() / sendAfterTextChanged() 會去 iterate 所有的 listeners，呼叫 beforeTextChanged / afterTextChanged / afterTextChanged; 而你在 iteration 的同時取消註冊了 listener，於是 iteration 跑到後來就 IOOBE 了。\
> ——[鄭行健](https://www.facebook.com/groups/523386591081376/posts/4383388048414525/?comment\_id=4383406325079364)，2021

如果需要實作一次性 TextWatcher, 建議使用其他方法，例如 **Boolean Flag 寫法**。

### 限制中文輸入

沒有實際測試過。

{% embed url="https://blog.csdn.net/u013904672/article/details/54314721" %}

{% embed url="https://www.cxyzjd.com/article/SmallWalnutBG/109628456" %}

（限制長度的版本）

{% embed url="https://www.jianshu.com/p/be34e099b1d8" %}



## SearchView

### 如何移除底線？

{% embed url="https://stackoverflow.com/a/38663324/9735961" %}

如果使用內建 `SearchView`&#x20;

```
android:queryBackground="@android:color/transparent"
```

如果使用 `androidx.appcompat.widget.SearchView`&#x20;

```
app:queryBackground="@android:color/transparent"
```

如果想要透過 `theme` / `style` 設定

{% embed url="https://stackoverflow.com/a/46776249/9735961" %}

in `style.xml` :

```markup
<style name="Base.Widget.AppCompat.SearchView.ActionBar">
    <item name="queryBackground">@null</item>
    <item name="submitBackground">@null</item>
    <item name="searchHintIcon">@null</item>
    <item name="defaultQueryHint">@string/abc_search_hint</item>
</style>
```

and apply style:

```markup
<androidx.appcompat.widget.SearchView
    style="@style/Base.Widget.AppCompat.SearchView.ActionBar" />
```

需要注意的是， `style` 裡面的 `item` 可能也需要因應你使用的元件而加上 `android:` prefix.

