# 地圖



一般推薦直接使用 Google Maps.

## 收費注意事項

{% hint style="info" %}
注意！Google Maps 自 2018 年後不再是完全免費。

高級功能，例如搜尋、街景、路線規劃等，需要計價。
{% endhint %}

請參照官方收費表：

{% embed url="https://developers.google.com/maps/billing-and-pricing/pricing" %}

## 教學

請參考官方教學：

{% embed url="https://developers.google.com/maps/documentation/android-sdk/map-with-marker" %}



### 範例專案

官方專案： Map With Marker

{% embed url="https://github.com/googlemaps/android-samples/tree/main/tutorials/kotlin/MapWithMarker" %}



## 新增到你的專案

其實可以用 New > Google Maps Fragment. 相關設置還挺齊全的。



### gradle 設置

專案層級 gradle:

```
plugins {
    // ...
    id 'com.google.android.libraries.mapsplatform.secrets-gradle-plugin' version '2.0.1' apply false
}
```

應用層級 gradle:

```
plugins {
    // ...
    id 'com.google.android.libraries.mapsplatform.secrets-gradle-plugin'
}

dependencies {
    // ...
    implementation 'com.google.android.gms:play-services-maps:18.0.2'
}
```



### Manifest 引入參數

需要提供專屬的 API Key.

一般推薦到 local.properties 下方放入：

```
MAPS_API_KEY=aLittleBitLongerNuafodsnu88q-faysm98Key
```

AndroidManifest:

```
<application
    ...>
    <!-- ... -->
    <meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="${MAPS_API_KEY}" />
</application>
```

### 設置並獲取&#x20;

準備一個 layout 裝載 SupportMapFragment:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.fragment.app.FragmentContainerView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/map"
    android:name="com.google.android.gms.maps.SupportMapFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

然後 inflate 它。

完成後嘗試獲取，並要求進行 getMapAsync:

```kotlin
(fragmentManager.findFragmentById(R.id.map) as SupportMapFragment?)?.let {
    prepareMap(it)
} ?: run {
    Timber.e("No Map Fragment Found!")
}

private fun prepareMap(mapFragment: SupportMapFragment) = mapFragment.getMapAsync { googleMap ->
    // do whatever you want with googleMap instance
}
```



## 基礎控制

{% embed url="https://developers.google.com/maps/documentation/android-sdk/views" %}



### 設定位置與縮放

```kotlin
val googleMap: GoogleMap

val center = LatLng(25.037894, 121.546766)
googleMap.moveCamera(CameraUpdateFactory.newLatLng(centerPos))
googleMap.moveCamera(CameraUpdateFactory.zoomTo(14.7f))
```



## 控制項與限制操作

{% embed url="https://developers.google.com/maps/documentation/android-sdk/controls" %}

```kotlin
val googleMap: GoogleMap

// avoid user using gesture to zoom in
googleMap.uiSettings.isZoomGesturesEnabled = false
// and so on...
```





## 標記

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



## 繪圖 / 形狀

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



## 疑難排解

### 無法開啟地圖（授權錯誤）

持續跳出以下錯誤？

```
E/Google Maps Android API: Authorization failure.  Please see https://developers.google.com/maps/documentation/android-api/start for how to correctly set up the map.
E/Google Maps Android API: In the Google Developer Console (https://console.developers.google.com)
    Ensure that the "Google Maps Android API v2" is enabled.
    Ensure that the following Android Key exists:
        API Key: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    	Android Application (<cert_fingerprint>;<package_name>): 12:34:56:78:...:3F;your.app.name
```

但是都到 Cloud Console 確認過 Google Maps 服務有開啟？

請到你的 properties 檔案，把

```
MAPS_API_KEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
```

改成：

```
MAPS_API_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```





