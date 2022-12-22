# Retrofit

Retrofit 透過標記子來為你自動實作網路請求，你無須在意底層實作方式，只要專注於 interface 的設定、 client 的初始化、資料的返回處理。示意：

```kotlin
interface MyAPIService {
    @GET("user/{id}")
    fun getUser(@Path("id") userId: String): Call<User>

    @GET("user")
    fun getUsers(): Call<List<User>>
    
    @PUT("user/{id}")
    fun updateUser(@Path("id") userId: String): Call<User>
    
    @POST("user/{id}")
    fun uploadNewItem(@Path("id") userId: String, @Body item: Item): Call
    
    @DELETE("user/{id}/{itemId}")
    fun deleteItem(@Path("id") userId: String, @Path("itemId") itemId: String): Response
}


```



{% embed url="https://bng86.gitbooks.io/android-third-party-/content/retrofit.html" %}

{% embed url="https://ithelp.ithome.com.tw/articles/10188660" %}

{% embed url="https://givemepass.medium.com/retrofit-%E6%93%8D%E4%BD%9C%E6%95%99%E5%AD%B8-26c7851ec154" %}



