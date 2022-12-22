# 元件風格 Theme & Style

這大概是全 android 與網頁最接近的地方。

你可以針對個別元件指定 style, 也可以為整個頁面（或應用）套用 theme.

{% embed url="https://developer.android.com/guide/topics/ui/look-and-feel/themes" %}



{% embed url="https://proandroiddev.com/theming-basics-in-android-13c57bc20605" %}

## 套用與覆蓋順序

{% embed url="https://medium.com/androiddevelopers/whats-your-text-s-appearance-f3a1729192d" %}

![](<../.gitbook/assets/image (1).png>)

{% hint style="success" %}
View Specification > Style > Default Style > Theme > TextAppearance
{% endhint %}



## Style 的部分修改（繼承自預設風格）

命名時使用 `ThemeOverlay` 開頭作為關鍵字：

{% embed url="https://proandroiddev.com/theming-basics-in-android-13c57bc20605" %}

```markup
<!-- res/values/theme_overlays.xml -->
<resources>
    <style name="ThemeOverlay.StylesNThemes.Button.Red" parent="">
      <item name="colorPrimary">@color/red</item>
    </style>
</resources>
```

如此，即可享有 Theme 預設( `textViewStyle` )的風格（例如顏色、大小、排版、...），只覆寫部分。

{% hint style="warning" %}
使用 ThemeOverlay 時， **parent 不可指定對象**。&#x20;
{% endhint %}

套用：

```markup
<Button
    android:theme="@style/ThemeOverlay.StylesNThemes.Button.Red"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

