# 疑難排解



## 奇妙錯誤修復三本家

遇到不可名狀 / 莫名其妙的錯誤時，可以試著執行這三件事。

95%以上的未知錯誤都可以透過這三本家來排除。

* sync Gradle
* rebuild Project
* invalidate and restart

### 外傳主角

隨著 Android 11, 12 發布，有越來越多行為在裝置本機內產生。

如果嘗試三本家後仍然失敗。請嘗試外傳主角：

* 清除裝置本機的 App 快取



## 安裝 Plugin 失敗

之前是遇到 Kotlin Plugin update install failure. 叫我查看 Log, 而 Log 顯示

```
Plugin Kotlin was not installed: 
Cannot download '(some url from jetbrains)': 
Read timed out, response: 200 OK 
```

解決方法很簡單（可能大多 Plugin 都可以這樣處理）：到 Jetbrains 官網直接下載對應 plugin 的 zip 檔案，然後 Android Studio

```
Preferences -> Plugins -> Install Plugin from Disk
```

![](<../.gitbook/assets/截圖 2021-09-01 下午2.55.05.png>)

如此即可完成。

{% hint style="info" %}
如果你遭遇的是 Kotlin Plugin 下載失敗，可以到以下網站尋找並下載適合版本的 zip 檔案：

[https://plugins.jetbrains.com/plugin/6954-kotlin/versions/stable](https://plugins.jetbrains.com/plugin/6954-kotlin/versions/stable)
{% endhint %}



### lint: found fatal errors

如果你切換到某個版本（通常是 `release` ），且發現 build (Make Project) 會失敗。但是 lint 訊息非常隱晦：

<figure><img src="../.gitbook/assets/截圖 2022-09-12 下午6.58.05.png" alt=""><figcaption></figcaption></figure>

````
Lint found fatal errors while assembling a release target.
Fix the issues identified by lint, or create a baseline to see only new errors:
```
android {
    lint {
        baseline = file("lint-baseline.xml")
    }
}
```

For more details, see https://developer.android.com/studio/write/lint#snapshot?id=stc
````

而上方 `lintVitalRelease` 也非常隱晦：

```
Execution failed for task ':app:lintVitalRelease'.
> Lint found fatal errors while assembling a release target.

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.


// if Run with --info
> Task :app:lintVitalRelease FAILED
Caching disabled for task ':app:lintVitalRelease' because:
  Build cache is disabled
Task ':app:lintVitalRelease' is not up-to-date because:
  Task has not declared any outputs despite executing actions.
file or directory '/Users/STC/.android/lint', not found
:app:lintVitalRelease (Thread[Daemon worker Thread 7,5,main]) completed. Took 0.013 secs.
:app:mergeReleaseJniLibFolders (Thread[Execution worker for ':' Thread 5,5,main]) started.

Execution failed for task ':app:lintVitalRelease'.
> Lint found fatal errors while assembling a release target.

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --debug option to get more log output.
> Run with --scan to get full insights.
```

推薦修正方式：

1. 二話不說，**直接照著最下方建議操作補上 `baseline` 相關設定**：

```
android {
    // ...
    lint {
        baseline = file("lint-baseline.xml")
    }
    // ...
}
```

2\. (加上之後記得 Sync with Gradle)

3\. 然後 **build 2 次**。（第一次應該仍然會見到 error; 第二次則不會出現 error 了）

{% hint style="info" %}
推測成因：lint 在 (Android Studio) 構造場合，可能對判定逐漸嚴格。

有些錯誤於 Editor 中不會看出來。但是 lint 建造版本的時候會發現。
{% endhint %}

{% hint style="danger" %}
推測成因 2：只有 release 版本會出錯，是因為通常只有 release 版本會進行以下設定：

```
debuggable false
```

此設定會導致 lint 檢查變嚴格。
{% endhint %}



