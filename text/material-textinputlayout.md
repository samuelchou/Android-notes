# Material TextInputLayout

## 基礎

{% embed url="https://guides.codepath.com/android/Working-with-the-EditText" %}

{% embed url="https://jc7003.pixnet.net/blog/post/361007618-android-text-fields-%E5%AF%A6%E4%BD%9C" %}

## 常駐 Label (標題固定上方，不遮罩輸入框)

設定 `app:expandedHintEnabled="false"` 並設定 `hint` 即可。

![](<../.gitbook/assets/截圖 2021-12-28 上午11.12.47.png>)



## 設置 error 左邊有個 drawable

在 error 左側設置一個 drawable

![](<../.gitbook/assets/image (5).png>)

可以透過映射獲得 error TextView, 再透過 Compound Drawable 組裝上去：

{% embed url="https://gist.github.com/qaz10102030/4705cf8a3f1ce902f37eaa9675bc3af1" %}

或者 使用 Kotlin 擴充語法：

```kotlin
/**
 * Calls [TextInputLayout.setError] with additional error drawable setup.
 * Result: drawable will be put in left of error text.
 */
fun TextInputLayout.setErrorWithStartDrawable(errorText: CharSequence?, errorDrawable: Drawable?) {
    error = errorText
    if (errorDrawable == null) return

    val tv: TextView = findViewById(R.id.textinput_error)

    // Make drawable match TextView height.
    tv.measure(0, 0)
    errorDrawable.setBounds(0, 0, tv.measuredHeight, tv.measuredHeight)

    // Make drawable color the same as TextView.
    errorDrawable.colorFilter = PorterDuffColorFilter(errorCurrentTextColors, PorterDuff.Mode.SRC_IN)

    tv.setCompoundDrawables(errorDrawable, null, null, null)
}

/**
 * Calls [TextInputLayout.setError] with additional error drawable setup.
 * Result: drawable will be put in left of error text.
 */
fun TextInputLayout.setErrorWithStartDrawable(
    errorText: CharSequence?, @DrawableRes errorRes: Int?,
) {
    setErrorWithStartDrawable(errorText, errorRes?.let { ContextCompat.getDrawable(context, it) })
}
```



## 左側標籤 / 欄位文字

左側秀出一個 drawable

![](<../.gitbook/assets/image (3).png>)

{% embed url="https://stackoverflow.com/q/34679382/9735961" %}

左側秀出一個欄位標題

![](../.gitbook/assets/image.png)

{% embed url="https://stackoverflow.com/q/54513250/9735961" %}



