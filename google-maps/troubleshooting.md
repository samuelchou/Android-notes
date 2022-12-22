# 疑難排解

## 無法開啟地圖（授權錯誤）

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
