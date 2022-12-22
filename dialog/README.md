# 元件雜談： Dialog

## 基礎

推薦使用 `androidx.appcompat.app.AlertDialog`

```kotlin
import androidx.appcompat.app.AlertDialog

val dialog = AlertDialog.Builder(context)
    .setTitle(R.string.msg_title_info)
    .setMessage("Hello")

dialog.show()
```

## 客製化 Layout

{% embed url="https://stackoverflow.com/a/18799229/9735961" %}

使用 `setView` 方法即可辦到：

```kotlin
val customView = EditText(activity)

dialog.setView(customView)
```

一般會更建議生成一個 custom layout:

```kotlin
val customView = layoutInflater.inflate(R.layout.dialog_custom, null)

dialog.setView(customView)
```

### 使 Custom Layout 融入 Dialog

AlertDialog 有內建的 padding. 使用 custom layout 時會忽略。但有時候會覺得這樣很醜。

這時候就可以使用 `?dialogPreferredPadding`&#x20;

{% embed url="https://stackoverflow.com/a/37976060/9735961" %}

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:paddingStart="?dialogPreferredPadding"
        android:paddingEnd="?dialogPreferredPadding"
        android:orientation="vertical">
        <!-- put your custom layout here... -->
</LinearLayout>
```





