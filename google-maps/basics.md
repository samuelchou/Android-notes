# 基礎控制

##

{% embed url="https://developers.google.com/maps/documentation/android-sdk/views" %}

### 設定位置與縮放

```kotlin
val googleMap: GoogleMap

val center = LatLng(25.037894, 121.546766)
googleMap.moveCamera(CameraUpdateFactory.newLatLng(centerPos))
googleMap.moveCamera(CameraUpdateFactory.zoomTo(14.7f))
```

