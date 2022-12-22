# 讀取 assets 檔案

## 使用 assets 須知

初步認識：

{% embed url="https://www.geeksforgeeks.org/assets-folder-in-android/" %}

範例：讀取文字檔或圖檔

{% embed url="https://xjaphx.wordpress.com/2011/10/02/store-and-use-files-in-assets/" %}

```kotlin
// assets > image > thumbnail.jpg
val fileNameWithPath = "image/thumbnail.jpg"
try {
    context.assets.open(fileNameWithPath).let {
        Drawable.createFromStream(it, null)
    }.let { myImageView.setImageDrawable(it) }
} catch (e: IOException) {
    Log.e(TAG, "Failed opening $fileNameWithPath", e)
}
```

### 注意事項

1. 使用 `assets.open()` 方法**需要輸入全部路徑，包含資料夾位置**。
   1. 開頭**不需要斜線**。



