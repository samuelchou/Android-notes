# Custom Error

某些場合，你可能會想要 Retrofit 返回自定義的 Error/Exception.

以下 SOF 或許會有所幫助：

{% embed url="https://stackoverflow.com/a/71433403/9735961" %}

{% embed url="https://stackoverflow.com/a/35056951/9735961" %}



## RxJava 的場合

{% embed url="https://stackoverflow.com/a/38825746/9735961" %}

如果你是使用 RxJava 版本，那一切更簡單

因為 Error 會吐入你設定的 Observable.

你也可以加上額外的 extension function, 來讓這一切更簡單些：

{% embed url="https://medium.com/@janczar/http-errors-with-kotlin-rx-and-retrofit-34e905aa91dd" %}

不過這個寫法仍須於每次使用 API 時額外呼叫。



## Converter

目前看起來最正統的作法。不過感覺步驟繁瑣。尚待研究...

{% embed url="https://medium.com/@nacro711072/android-retrofit-convertfactory-69329de7718f" %}



## 不會 work 的場合

1. Interceptor: 只能用來處理 Success Response. 處理轉換 Exception 的過程未知。若直接於 `intercept` 方法拋出錯誤，該錯誤（在 Rx 的場合）會導致 Crash.





