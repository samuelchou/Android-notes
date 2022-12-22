# 日期與時間

比起 `java.util.Date`, 更推薦使用 Java 8 的 `java.time.LocalDate`.

{% embed url="https://www.baeldung.com/java-8-date-time-intro" %}

{% embed url="https://kucw.github.io/blog/2020/6/java-date" %}

## 向下相容

Java 8 預設只能在 SDK 26 / Android O / Android 8.0 以上使用。

如果想要在這之前的版本使用，請參閱官方教學。官方推薦 desugaring 後即可使用：

{% embed url="https://developer.android.com/studio/write/java8-support#library-desugaring" %}

其實很簡單， gradle 修改一下即可：

```groovy
android {
    defaultConfig {
        // Required when setting minSdkVersion to 20 or lower
        multiDexEnabled true
    }

    compileOptions {
        // Flag to enable support for the new language APIs
        coreLibraryDesugaringEnabled true
        // Sets Java compatibility to Java 8
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    coreLibraryDesugaring 'com.android.tools:desugar_jdk_libs:1.1.5'
}
```





