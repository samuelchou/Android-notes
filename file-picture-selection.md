# 選取檔案或拍照



在 Activity Result API 推出之後，驚人地簡單：

{% embed url="https://juejin.cn/post/7082314521284444173" %}

{% embed url="https://www.holdapp.com/blog/how-to-access-files-in-android-storage-access-framework-and-activity-result-api" %}

## 選擇圖檔

```kotlin
// inside fragment or activity
val fileSelectLauncher =
    registerForActivityResult(ActivityResultContracts.GetContent()) { fileUri ->
        // do something...
    }

myButton.setOnClickListener {
    fileSelectLauncher.launch("image/*") // select image
}
```









