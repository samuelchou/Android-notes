---
description: 如果你只是想要一個 Chip 造型安插在文字中，你可以考慮使用 ChipDrawable.
---

# ChipDrawable

{% embed url="https://stackoverflow.com/a/51610997/9735961" %}

{% embed url="https://www.jianshu.com/p/d64a75ec7c74" %}

{% embed url="https://material.io/components/chips/android#using-chips" %}
滾動到 Standalone ChipDrawable 章節
{% endembed %}



如果你只是想要一個 Chip 造型安插在文字中，你可以考慮使用 ChipDrawable.

然而... ChipDrawable 難用地要死。如果你想要客製化設計（例如調整高度寬度等），勸你最好不要這麼做...





## 步驟

1. 生成一個 XML 檔案做為基底。
2. 呼叫 `ChipDrawable.createFromResource` 生成一個 `ChipDrawable`.
3. 設定參數。
4. 完成！

### 注意事項

XML 檔案的格式嚴苛：

1. 必須置於 res/xml 資料夾下方。
2. **母標籤必須是 `chip` .**

{% hint style="danger" %}
`ChipDrawable.createFromResource` 不接受 res/xml 以外的資源來源。
{% endhint %}

{% hint style="danger" %}
使用 `com.google.android.material.chip.Chip` 或類似的 class 做為母標籤會導致生成 FATAL: `NotFoundException: Can't load badge resource`
{% endhint %}

以下是範例：

```xml
<chip xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:text="@string/hello_world"
    app:chipIcon="@drawable/ic_launcher_foreground" />
```

初始化：

```kotlin
val chipDrawable = ChipDrawable.createFromResource(this, R.xml.intext_chip)
chipDrawable.apply {
    setTextColor(Color.GREEN)
    text = "CHIP"
    setBounds(0, 0, intrinsicWidth, intrinsicHeight)
}
```

在 textView 中使用（透過 `SpannableString` ）：

```kotlin
val span = ImageSpan(chipDrawable)
val spannableString = SpannableString(textView.text)
spannableString.setSpan(
    span, 0, spannableString.length, Spannable.SPAN_EXCLUSIVE_INCLUSIVE
)
textView.text = spannableString
```



## 風格設置

設置 ChipDrawable 對應風格叫做 `chipStandaloneStyle`, 與一般 `chipStyle` 項目不同。

{% embed url="https://medium.com/over-engineering/hands-on-with-material-components-for-android-chips-21dc67c8b956" %}

## 文字風格變更

截至版本 `material:1.5.0`, 唯一變更文字（造型）的方法只有 chipDrawable.setTextAppearance.

{% hint style="danger" %}
遺憾地是，`chipDrawable.setText` 目前 (`1.5.0`) 似乎並不吃 `SpannableString` 的格式設置。
{% endhint %}

### 色彩

目前版本[並沒有直接能更改文字顏色的方法](https://github.com/material-components/material-components-android/issues/1165)（例如 `chipDrawable.setChipTextColor` 之類）。

想要文字變色，你需要遵照以下方法：

{% hint style="warning" %}
因為上述提及的 bug （不吃 `SpannableString` 格式），你必須設置 `TextAppearance`.
{% endhint %}

```xml
<!-- in style.xml -->
<style name="TextAppearanceWhite" parent="TextAppearance.AppCompat.Small">
    <item name="android:textColor">@color/white</item>
</style>
```

```kotlin
val chipDrawable = ChipDrawable.createFromResource(context, R.xml.chip_custom)

chipDrawable.text = "TEST"
chipDrawable.setTextAppearanceResource(R.style.TextAppearanceWhite)

```



























