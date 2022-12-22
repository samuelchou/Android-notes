# 繪圖 / 繪製形狀

{% embed url="https://developers.google.com/maps/documentation/android-sdk/shapes" %}

你也可以在地圖上繪製形狀，諸如線、多邊形、圓形等。

```kotlin
val googleMap: GoogleMap

// draw circle
val pos = LatLng(25.037894, 121.546766)
val circle = CircleOptions().center(pos).radius(500.0) // meter
    .strokeWidth(5f)
    .strokeColor(context.getColor(R.color.brand_primary))
    .fillColor(context.getColor(R.color.brand_light))

googleMap.addCircle(circle)
```

注意繪製不會自動幫你調整成半透明。請自行調整顏色透明度。
