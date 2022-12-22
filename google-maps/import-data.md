# 外部資料匯入處理



## String to Polyline

{% embed url="https://stackoverflow.com/a/38536525/9735961" %}

有時候你可能會從 API 拿到類似這樣的資料：

```
"polyline": "wpxsDps|eQs@?@f@?h@?b@|@CVB"
```

這其實是代表 polyline 座標路徑的字串內容。

推薦使用官方推出的額外依賴庫進行解析：

{% embed url="https://github.com/googlemaps/android-maps-utils" %}

```groovy
// in build.gradle (:app)
dependencies {
    implementation 'com.google.maps.android:android-maps-utils:2.4.0'
}
```

使用 PolyUtils 進行解析：

```kotlin
val encodedPath: String
val path: List<LatLng> = PolyUtil.decode(encodedPath)
val encodedBackPath: String = PolyUtil.encode(path)
```







