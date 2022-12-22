# 安插固定的 Header

可以透過 Interceptor 設置。



範例：

{% embed url="https://github.com/yc0015139/LoginToParse/blob/master/app/src/main/java/dev/yc/logintoparse/data/service/generator/ServiceGenerator.kt?fbclid=IwAR35AgFjrncVf9Hm0tqlcqOdRLtp8c-hD6mzcwltqynX2dcYeJkHRJsxjj4" %}

{% embed url="https://github.com/yc0015139/LoginToParse/blob/master/app/src/main/java/dev/yc/logintoparse/data/AppContainer.kt?fbclid=IwAR35AgFjrncVf9Hm0tqlcqOdRLtp8c-hD6mzcwltqynX2dcYeJkHRJsxjj4" %}

以下示範總是會安插 bearer token 的 AuthInterceptor:

```kotlin
import okhttp3.Interceptor
import okhttp3.Response

/**
 * 自動處理 Header Auth 等資訊的 Interceptor.
 */
object AuthInterceptor : Interceptor {

    private var bearerToken = "Bearer UNDEFINED"

    /**
     * 設定共用的 header token.
     *
     * @param token server 直接提供的字串。（無須加上 "Bearer" 等前綴字）
     */
    fun setToken(token: String) {
        bearerToken = "Bearer $token"
    }

    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request().newBuilder()
            .addHeader("Authorization", bearerToken)
            .build()
        return chain.proceed(request)
    }
}
```





