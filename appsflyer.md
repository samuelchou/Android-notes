---
description: 你的專案為了營運目的，可能有導入 AppsFlyer. 部分 AppsFlyer 功能相當強大且好用。
---

# AppsFlyer





## OneLink Management

AppsFlyer 整合了 OneLink 功能。

{% embed url="https://dev.appsflyer.com/hc/docs/unified-deep-linking-udl" %}

參數項目：

1. short url: 就是這個 OneLink 最後長的樣子。
2. Deep link value: 在 App 內**透過 AppsFlyer 套件解析取得**的 link. 可以用來做頁面導向。
3. Additional deep link value: 在 App 內**透過 AppsFlyer 套件解析取得**的附加參數。
4. Android and iOS fallback - Add URI scheme to launch the app: 上述執行動作失敗時會嘗試**讓 App （或裝置，如果沒裝 App 的話）開啟**的連結。

{% hint style="danger" %}
OneLink 同個 link **只能提供(同)一組 scheme** 給雙平台。

請確保你的雙平台 App 使用同樣的 scheme, 例如 `my.app://...`
{% endhint %}

範例程式碼：

```kotlin
// inside Main -> onCreate
// for a link like https://myapp.onelink.me/tW22/page?id=abcde
// with additional value: campaign-123
// redirect to my.app://page?id=abcde
AppsFlyerLib.getInstance().subscribeForDeepLink { result ->
    if (result.status != DeepLinkResult.Status.FOUND) {
        // The link is not from AppsFlyer service; or encountered error.
        return@subscribeForDeepLink
    }
    val appsFlyerDeepLink = result.deepLink
    val deepLinkValue = appsFlyerDeepLink.deepLinkValue // -> my.app://page
    val idParam = appsFlyerDeepLink.getStringValue("id") // -> abcde
    val additionalDeepLinkValue = 
        appsFlyerDeepLink.getStringValue("deep_link_sub1") // -> campaign-123
    val uri = Uri.parse(deepLinkValue)
        .buildUpon()
        .appendQueryParameter("id", idParam)
        .build()
    Log.d(TAG, "get appsflyer deeplink from OneLink, uri=$uri")
    startDeeplink(uri)
}
```





