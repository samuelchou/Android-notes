# 標記

{% embed url="https://developers.google.com/maps/documentation/android-sdk/marker" %}

標記是免費的。

```kotlin
val googleMap: GoogleMap

val pos = LatLng(25.037894, 121.546766)
lateinit var putMarker: Marker

MarkerOptions().position(pos).title("我的位置")
    .run {
        googleMap.addMarker(this)
    }.let { marker ->
        putMarker = marker // save for later use
    }
```

### 自定義圖形標記

你也可以在地圖上標記自定義圖形。

```kotlin
val googleMap: GoogleMap
lateinit var putMarker: Marker

val pos = LatLng(25.037894, 121.546766)
val myDrawableDes = context.getDrawable(R.drawable.shape_bolt)?.toBitmapDescriptor() ?: return

MarkerOptions().position(pos).title("我的位置")
    .icon(myDrawableDes).anchor(0.5f, 0.5f)
    .run {
        googleMap.addMarker(this)
    }.let { marker ->
        putMarker = marker // save for later use
    }

private fun Drawable.toBitmapDescriptor(): BitmapDescriptor {
    setBounds(0, 0, intrinsicWidth, intrinsicHeight)
    val bitmap = Bitmap.createBitmap(intrinsicWidth, intrinsicHeight, Bitmap.Config.ARGB_8888)
    draw(Canvas().also { it.setBitmap(bitmap) })
    return BitmapDescriptorFactory.fromBitmap(bitmap)
}
```

{% hint style="info" %}
anchor的單位是比例（0-1, Float）。

圖形預設 anchor 為 (0.5f, 0f), 也就是大頭針底部中央位置。如果想要讓座標位於圖形正中央，可以設定為 (0.5f, 0.5f).
{% endhint %}

### 在地圖上顯示文字標籤（文字造型標記）

可以參考使用這個 util:

{% embed url="http://googlemaps.github.io/android-maps-utils/" %}

或者...你也可以繪製 ChipDrawable, 然後設定上 `MarkerOptions` 做使用。

```kotlin
val googleMap: GoogleMap
lateinit var putMarker: Marker

val pos = LatLng(25.037894, 121.546766)
val chipDrawable: ChipDrawable // remember to set text inside

MarkerOptions().position(pos).title("我的位置")
    .icon(chipDrawable).anchor(0.5f, 0.5f)
    .run {
        googleMap.addMarker(this)
    }.let { marker ->
        putMarker = marker // save for later use
    }
```

### 處理標記點擊事件

點擊事件一次只會觸發一個標記。由 Z-index 最高的標記獲得點擊事件。

```kotlin
lateinit var putMarker: Marker

// put marker on map, and set it to putMarker

// deal with marker click event
googleMap.setOnMarkerClickListener { m ->
    if (m == putMarker) {
        // do something
        return@setOnMarkerClickListener true // no need to trigger map default interaction
    }
    false // normal click, trigger map interaction: focus & show info
}
```

