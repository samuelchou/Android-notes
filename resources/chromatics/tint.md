# tint

tint 一直以來都是一個很難以處理的東西.....



## selector drawable + tint

想像有一個以下 drawable selector:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/cic_heart" android:state_checked="true" />
    <item android:drawable="@drawable/ic_heart_outlined" />
</selector>
```

其中 `ic_heart_outlined` 是無色彩的，需要由 tint 去上色。但是 `cic_heart` 已經上色，無需上色。\
你會如何辦到呢？

第一直覺當然是寫個 color selector:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="@null" android:state_checked="true" />
    <item android:color="@color/brand_primary" />
</selector>
```

然後去設置為上述 drawable 的 tint:

```
android:drawable="@drawable/heart_selector"
android:drawableTint="@color/heart_color_selector"
```

...然後就發現 `cic_heart` 也被 `brand_primary` 給上色了。哭啊！

（環境是 `background` drawable. 可能其他 drawable 屬性也會發生。）

顯然 `@null` 沒有取消 tint. 那換成 transparent 呢？也還是沒用。

這時候就得調整 tint mode 了。交叉實驗 tint mode 後得到以下結果：

```
outline+tint: 正確運作的tint mode
src_in
src_atop
Multiply

colored+transparent: 正確運作的tint mode
add
src_over
Screen
src_atop
```

交叉比對可知，需要用 `src_atop` 才可完成這個需求。

改寫一下 color selector:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- Use it with "src_atop" tint mode -->
    <item android:color="@android:color/transparent" android:state_activated="true" />
    <item android:color="@android:color/transparent" android:state_checked="true" />
    <item android:color="@color/brand_primary" />
</selector>
```

drawable 使用：

```
android:drawable="@drawable/heart_selector"
android:drawableTint="@color/heart_color_selector"
android:drawableTintMode="src_atop"
```





