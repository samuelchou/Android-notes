# 鏡頭控制

{% embed url="https://developers.google.com/maps/documentation/android-sdk/views" %}

## 設定位置與縮放

```kotlin
val googleMap: GoogleMap

val center = LatLng(25.037894, 121.546766)
googleMap.moveCamera(CameraUpdateFactory.newLatLng(centerPos))
googleMap.moveCamera(CameraUpdateFactory.zoomTo(14.7f))
```



## 控制鏡頭涵蓋所有座標

透過 `LatLngBounds` 來達成這件事：

```kotlin
val origin: LatLng
val dest: LatLng

val latLngBounds = LatLngBounds.builder()
    .include(origin).include(dest)
    .build()
googleMap.moveCamera(
    CameraUpdateFactory.newLatLngBounds(latLngBounds, 800, 800, 0)
)
```

{% hint style="info" %}
除了指定寬高以外，你也可以使用提供指定 padding 的&#x20;

`CameraUpdateFactory.newLatLngBounds(latlngBounds, padding)`
{% endhint %}









