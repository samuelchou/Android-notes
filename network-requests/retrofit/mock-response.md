# 模擬伺服器回應

有時候，你會想要模擬伺服器回應，觀察程式碼邏輯是否有正確運轉。

Retrofit 提供了相對完整的模擬功能。

## 簡介

1. 使用 `okhttp3.mockwebserver` 捆包來完成模擬伺服器的架設與測試。
2. 對應現有 OkHttp / Retrofit 架構的改動。
3. `MockWebServer` 的操作。

## 在單元測試中使用 MockWebServer 模擬回應

{% embed url="https://ithelp.ithome.com.tw/articles/10227214" %}

可以輕鬆在單元測試中用附加檔案回應 API, 完成測試。

### 引用

build.gradle (app):

```
dependencies {
    // ...

    testImplementation 'com.squareup.okhttp3:mockwebserver:4.9.1'

    // ...
}
```

（未完成...）

## 在應用環境中使用 MockWebServer



（未完成）自己看專案的 code 吧。

重點：

1. MockWebServer 並不能直接套用到應用內。需要應用放寬權限 (AndroidManifest 修正)
2. MockWebServer 的模擬回應 **並不總是會回應給**（設置後的）**最近一次請求**！

