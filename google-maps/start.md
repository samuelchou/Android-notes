# 開始使用

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

### 設置並獲取

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
