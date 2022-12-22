---
description: 底部對話視窗是 material 系列元件。
---

# BottomSheetDialog

{% embed url="https://material.io/components/sheets-bottom" %}

{% embed url="https://blog.csdn.net/yechaoa/article/details/110134991" %}

{% embed url="https://segmentfault.com/a/1190000012841147" %}

{% embed url="https://thumbb13555.pixnet.net/blog/post/323341972-bottom-sheet" %}

## 使用 ScrollView

如果直接使用 `ScrollView` , 會發現滑回上方時無法正確滑到最上方。

此時，將 `ScrollView` 改為 `androidx.core.widget.NestedScrollView` 即可。

{% embed url="https://stackoverflow.com/a/41680628/9735961" %}

## 設定圓角

若想要圓角，可參考設置 `shapeApperanceOverlay`:

```markup
<style name="ShapeAppearanceOverlay.UpRounded">
    <!-- BottomSheet Style -->
    <item name="cornerSizeTopLeft">24dp</item>
    <item name="cornerFamilyTopLeft">rounded</item>
    <item name="cornerSizeTopRight">24dp</item>
    <item name="cornerFamilyTopRight">rounded</item>
</style>

<style name="BottomSheet.UpRounded" parent="Widget.MaterialComponents.BottomSheet">
    <item name="shapeAppearanceOverlay">@style/ShapeAppearanceOverlay.UpRounded</item>
</style>

<style name="BottomSheetDialog.UpRounded" parent="@style/ThemeOverlay.MaterialComponents.BottomSheetDialog">
    <item name="bottomSheetStyle">@style/BottomSheet.UpRounded</item>
</style>

<!-- Apply it in app theme -->
<style name="Theme.MyAppTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="bottomSheetDialogTheme">@style/BottomSheetDialog.UpRounded</item>
</style>
```

{% embed url="https://stackoverflow.com/a/57627229/9735961" %}

{% hint style="danger" %}
儘管如此，BSD **展開** `STATE_EXPANDED` 時會有預設白方形背景填滿，導致任何圓角設置消失。\
目前並沒有簡單安全的修改方式。\
此問題困擾了諸多開發者，但是[官方說這是刻意為之](https://github.com/material-components/material-components-android/pull/437?fbclid=IwAR31Uc8XVv757e8vAsI--lDPHq18zKJwpMbpXpJOpXOCxhhtyWAwr\_hbhyg#issuecomment-536668983)，不打算修改。
{% endhint %}

## 設置高度

```kotlin
val dialog = super.onCreateDialog(savedInstanceState) as BottomSheetDialog
val heightPercent = 0.3f
val peekHeight = (resources.displayMetrics.heightPixels * heightPercent).toInt()

dialog.findViewById<View>(R.id.design_bottom_sheet)?.let { root ->
    val behavior = BottomSheetBehavior.from(root)
    behavior.peekHeight = setPeekHeight
}
```

需要注意的是， `R.id.design_bottom_sheet` 必須在 show 之後再找尋，否則可能為 null:

```kotlin
val dialog = super.onCreateDialog(savedInstanceState) as BottomSheetDialog
val heightPercent = 0.3f
val peekHeight = (resources.displayMetrics.heightPixels * heightPercent).toInt()

dialog.setOnShowListener {
    dialog.findViewById<View>(R.id.design_bottom_sheet)?.let { root ->
        val behavior = BottomSheetBehavior.from(root)
        behavior.peekHeight = peekHeight
        Log.d(TAG, "onCreateDialog: set dialog percent to $peekHeight px ($heightPercent).")
    } ?: run {
        Log.e(TAG, "onCreateDialog: failed setting dialog percent to $peekHeight px ($heightPercent).")
    }
}
```

## 鍵盤遮擋問題

當聚焦 BottomSheetDialog 內的 EditText 開啟鍵盤時，偶爾會有遮擋問題。\
此時可以透過設置 `android:windowSoftInputMode` 來解決：

```markup
<!-- In themes.xml -->
<style name="CustomBottomSheetDialog" parent="Theme.Design.Light.BottomSheetDialog">
    <item name="android:windowIsFloating">false</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    <item name="android:windowSoftInputMode">adjustResize</item>
</style>

<!-- Apply it in app theme -->
<style name="Theme.MyAppTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="bottomSheetDialogTheme">@style/CustomBottomSheetDialog</item>
</style>
```

若不想套用至所有 BSD, 也可以單獨套用：

```kotlin
// In your BottomSheetDialogFragment
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setStyle(DialogFragment.STYLE_NORMAL, R.style.CustomBottomSheetDialog)
}
```

{% embed url="https://stackoverflow.com/a/55021173/9735961" %}

{% embed url="https://blog.csdn.net/wjploop/article/details/81941046" %}

## 鍵盤遮擋問題 Part. 2: 設定狀態為展開

有時候高度設置可能還不夠。這時候可以設置完全展開：

```kotlin
val layout = (dialog as? BottomSheetDialog)?.findViewById(R.id.design_bottom_sheet) as? FrameLayout
layout?.let {
    BottomSheetBehavior.from(it).state = BottomSheetBehavior.STATE_EXPANDED
}

// or...
val dialog: BottomSheetDialog
dialog.behavior.state = BottomSheetBehavior.STATE_EXPANDED
```

{% embed url="https://stackoverflow.com/a/62958074/9735961" %}

{% embed url="https://stackoverflow.com/a/56617198/9735961" %}

{% embed url="https://stackoverflow.com/a/55021173/9735961" %}

## 綜合題：圓角造型下的鍵盤遮擋問題

如果想要保持開啟鍵盤時 BSD 還能完全顯示，在某些情況下，前述的 `windowsInputMode` 與 Material 主題混用時會失敗。

此時可以透過設定 `STATE_EXPANDED` 來達成。但上述曾提到 [Material 官方刻意寫死「展開時會將背景變為方形」](https://issuetracker.google.com/issues/144859239#comment2)，導致設定 state 會使得圓角消失。

想要同時擁有圓角、又不被鍵盤遮擋，只能同時實作**「套用 Material 以外主題，套用自定義 background」並「設定狀態為展開」**才能達成。

第一步：自定義上圓角矩形

```markup
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="?attr/backgroundColor" />
    <corners
        android:topLeftRadius="24dp"
        android:topRightRadius="24dp" />
</shape>
```

第二步：BSD 主題設置更換

```markup
<style name="BottomSheet.UpRounded" parent="Widget.Design.BottomSheet.Modal">
    <item name="android:background">@drawable/shape_rect_up_rounded</item>
    <item name="android:backgroundTint">@color/white</item>
</style>

<style name="BottomSheetDialog.UpRounded" parent="@style/ThemeOverlay.MaterialComponents.BottomSheetDialog">
    <item name="bottomSheetStyle">@style/BottomSheet.UpRounded</item>
</style>
```

第三步：設置給 BSDF 使用&#x20;

```markup
<!-- In App Theme -->
<style name="Theme.MyAppTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="bottomSheetDialogTheme">@style/BottomSheetDialog.UpRounded</item>
</style>
```

或者，若不想覆寫 App Theme 預設值：

```kotlin
// In your BottomSheetDialogFragment
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setStyle(DialogFragment.STYLE_NORMAL, R.style.BottomSheetDialog_UpRounded)
}
```

第四步：指定狀態預設為展開

```kotlin
// In your BottomSheetDialogFragment
override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
    val dialog = super.onCreateDialog(savedInstanceState) as BottomSheetDialog
    dialog.behavior.state = BottomSheetBehavior.STATE_EXPANDED
    return dialog
}
```

完成。

{% hint style="danger" %}
**一定要替換**原先使用的 `Widget.MaterialComponents.BottomSheet` 主題。該主題會導致 `STATE_EXPANDED` 時會補上方形背景、不顯示圓角背景。

替換的主題無硬性規範。\
已知 `Widget.Design.BottomSheet.Modal` 十分適合、無其他副作用。
{% endhint %}



## Handle 把手

{% embed url="https://ithelp.ithome.com.tw/articles/10299677" %}

Material 3 有提供把手：

{% embed url="https://m3.material.io/components/bottom-sheets/overview" %}

需要

1. 元件本身： `BottomSheetDragHandleView`
2. 設定 CoordinatorLayout style: `@style/Widget.Material3.BottomSheet` (不確定是否必要)
3. **設定 themes - AppThemes: M3 系列** (例如 `Theme.Material3.DayNight.NoActionBar` )
   1. 這步驟很重要！！！沒有做的話你的 Handle 不論如何都不會出來。



