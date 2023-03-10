# 加密



某些場合，你會希望傳輸有進行加密以符合安全規範。\
Retrofit 也有提供加密方式，主要為使用 Interceptor 攔截器。

```kotlin
import okhttp3.OkHttpClient


val myCryptoStrategy: CryptoStrategy

val httpClient = OkHttpClient.Builder()
    .addInterceptor(EncryptionInterceptor(myCryptoStrategy))
    .addInterceptor(DecryptionInterceptor(myCryptoStrategy))
    .build()

val myRetrofitAPIService = Retrofit.Builder()
    .baseUrl("https://my-awesome-server/")
    .client(httpClient)
    .build()
    .create(MyAPIService::class.java)
```

```kotlin
import java.io.IOException

interface CryptoStrategy {
    @Throws(IOException::class)
    fun encrypt(data: String): String

    @Throws(IOException::class)
    fun decrypt(data: String): String
}
```

```kotlin
import android.util.Log
import okhttp3.*
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.RequestBody.Companion.toRequestBody
import okhttp3.ResponseBody.Companion.toResponseBody
import okio.Buffer
import java.io.IOException

class EncryptionInterceptor(private val cryptoStrategy: CryptoStrategy) : Interceptor {
    companion object {
        private const val TAG = "EncryptionInterceptor"
    }

    @Throws(IOException::class)
    override fun intercept(chain: Interceptor.Chain): Response {
        val oldRequest: Request = chain.request()

        val newRequest: Request = oldRequest.body?.let { body ->
            Log.v(TAG, "intercept: received encrypt request.")
            val mediaType: MediaType = "text/plain; charset=utf-8".toMediaType()

            // Encryption
            val encryptedBodyStr: String = try {
                Buffer().let { buffer ->
                    body.writeTo(buffer)
                    buffer.readUtf8()
                }.let {
                    cryptoStrategy.encrypt(it)
                }.also {
                    Log.v(TAG, "intercept: successfully encrypted: " +
                            "[${cryptoStrategy.javaClass.simpleName}] -> $it")
                }
            } catch (e: Exception) {
                Log.e(TAG, "intercept: failed encrypting $body", e)
                ""
            }
            val encryptedBody: RequestBody = encryptedBodyStr.toRequestBody(mediaType)
            oldRequest.newBuilder()
                    .header("Content-Type", encryptedBody.contentType().toString())
                    .header("Content-Length", encryptedBody.contentLength().toString())
                    .method(oldRequest.method, encryptedBody)
                    .build()
        } ?: run {
            Log.v(TAG, "intercept: original body is null. (Nothing to encrypt.)")
            oldRequest
        }
        return chain.proceed(newRequest)
    }
}
```

```kotlin
class DecryptionInterceptor(private val cryptoStrategy: CryptoStrategy) : Interceptor {
    companion object {
        private const val TAG = "DecryptionInterceptor"
    }

    @Throws(IOException::class)
    override fun intercept(chain: Interceptor.Chain): Response {
        val response: Response = chain.proceed(chain.request())
        if (response.isSuccessful) {
            Log.v(TAG, "intercept: received decrypt request.")
            response.body?.let { body ->
                val mediaType: MediaType = response.header("Content-Type").let {
                    if (it.isNullOrEmpty()) "application/json" else it
                }.toMediaType()

                // Decryption
                val bodyString = try {
                    body.string()
                } catch (e: Exception) {
                    e.printStackTrace()
                    ""
                }
                val decryptedString: String = try {
                    bodyString.let {
                        cryptoStrategy.decrypt(it)
                    }.also {
                        Log.v(TAG, "intercept: successfully decrypted: " +
                                "[${cryptoStrategy.javaClass.simpleName}] -> $it")
                    }
                } catch (e: Exception) {
                    Log.e(TAG, "intercept: failed decrypting $bodyString", e)
                    bodyString
                }

                return response.newBuilder()
                        .body(decryptedString.toResponseBody(mediaType))
                        .build()
            } ?: run {
                Log.v(TAG, "intercept: original body is null. (Nothing to decrypt.)")
                return response
            }
        }
        return response
    }
}
```

 





{% embed url="https://www.jianshu.com/p/25525e8180e6" %}

{% embed url="https://stackoverflow.com/a/37649405/9735961" %}

{% embed url="https://stackoverflow.com/a/40175319/9735961" %}



{% embed url="https://medium.com/swlh/okhttp-interceptors-with-retrofit-2dcc322cc3f3" %}

{% embed url="https://gist.github.com/balrampandey19/96bdc04174e58ba3dd03c0aa6f7345af" %}



