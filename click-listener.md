# 點擊監聽

## 基礎

View 都含有基礎的 Click Listener 可以使用。Kotlin 提供更方便的語法：

```kotlin
val btn: Button
btn.setOnClickListener {
    Log.d(TAG, "btn: clicked!")
}
```



## 點擊外面

{% embed url="https://stackoverflow.com/q/6685690/9735961" %}

兩種可用作法：「外模」佈局監聽，與點擊事件計算。

### 「外模」佈局監聽

簡單來說，就是在想關注的 View 外面設置一個 Group, 以此監聽。

需要注意的是，記得設置 `clickable` 屬性。

```xml
<LinearLayout
    android:id="@+id/layoutRoot"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:clickable="true"
    android:orientation="vertical">
    
    <!-- Other component -->
    <Button
        android:id="@+id/btn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```

```kotlin
btn.setOnClickListener {
    Log.d(TAG, "btn: clicked!")
}

layoutRoot.setOnClickListener {
    Log.d(TAG, "layoutRoot: clicked outside the btn!")
}
```

但這個缺點不少，例如 view group 必須**涵蓋夠大的點擊範圍**才能抓到所有點擊事件。另外 view group **margin** 範圍也不會套用監聽。

### 點擊事件計算

Android 提供了方法達成以下事項：

1. 點擊事件的包裝 `MotionEvent`
2. Activity 可以監聽並攔截點擊事件 `onTouchEvent`
3. 索取 View 在目前畫面上的範圍座標 `getGlobalVisibleRect`
4. 判斷點是否在範圍內 `Rect.contains`

透過以上能力的組合，我們可以做到一件事：

**監聽點擊事件，並判斷點擊範圍是否位於指定 View 內**。

```kotlin
fun MotionEvent.isInsideView(view: View): Boolean {
    val viewArea = Rect().also { view.getGlobalVisibleRect(it) }
    return viewArea.contains(rawX.toInt(), rawY.toInt())
}

// In your activity
override fun dispatchTouchEvent(ev: MotionEvent?): Boolean {
    val result = super.dispatchTouchEvent(ev)
    if (result.not() && ev?.isInsideView(btn) == false) {
        Log.d(TAG, "layoutRoot: clicked outside the btn!")
    }
    return result
}
```

{% hint style="info" %}
原先 `dispatchTouchEvent` 回傳值為「是否偵測到有關此點擊事件的實作（並消耗之）」，例如：點到其他設置了 ClickListener 的 View.

如果希望不論是否觸發其他動作事件都要執行，請將 if 中的 result 的判斷移除。
{% endhint %}



