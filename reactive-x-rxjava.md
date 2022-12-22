# Reactive X (RxJava)



## 基礎使用

build.gradle (app) 匯入

```groovy
dependencies {
    // ...

    // Basic
    implementation 'io.reactivex.rxjava3:rxandroid:3.0.0'

    // If using Java
    implementation 'io.reactivex.rxjava3:rxjava:3.0.6'

    // If using Kotlin
    implementation 'io.reactivex.rxjava3:rxkotlin:3.0.1'

    // Optional
    implementation 'com.squareup.retrofit2:adapter-rxjava3:2.9.0' // For Retrofit
    implementation "androidx.room:room-rxjava3:2.4.0-beta01" // For Room

}
```



