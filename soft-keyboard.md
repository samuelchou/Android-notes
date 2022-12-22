# 軟鍵盤的互動處置

優質好文：Vander 的 CSDN 文章

{% embed url="https://blog.csdn.net/l540675759/article/details/74528641" %}

## 設置鍵盤完成動作

有些時候，你會希望軟鍵盤可以自動更換 Enter 鍵為其他功能，例如「換到下一行」、「自動送出」等

此時，網路一般會建議你設置 `imeOption` 與 `EditorActionListener` :

```kotlin
// in xml: editText set android:imeOptions="actionDone"
editText.setOnEditorActionListener { v, actionId, _ ->
    if (actionId == EditorInfo.IME_ACTION_DONE) {
        v.clearFocus()
        binding.btnConfirm.performClick()
        true
    } else false
}
```

以上，即可運作。

### 注意事項

如果該 EditText 同時還使用了 `digits` 來排除輸入字元，可能會導致監聽失效：

{% embed url="https://stackoverflow.com/q/42480576/9735961" %}

解法是設置 `singeLine` 屬性：

```kotlin
// in xml: editText set android:imeOptions="actionDone"
// if android:digits is set, should also add android:singleLine="true" to keep it work
editText.setOnEditorActionListener { v, actionId, _ ->
    if (actionId == EditorInfo.IME_ACTION_DONE) {
        v.clearFocus()
        binding.btnConfirm.performClick()
        true
    } else false
}
```

很神秘吧（笑）

## 監聽鍵盤隱藏

需要自定義 EditText, 並覆寫 `onKeyPreIme` 方法：

```kotlin
@Override
public boolean onKeyPreIme(int keyCode, KeyEvent event) {
  if (event.getKeyCode() == KeyEvent.KEYCODE_BACK) {
    // Do your thing.
    return true;  // So it is not propagated.
  }
  return super.dispatchKeyEvent(event);
}
```

{% embed url="https://stackoverflow.com/a/4390592/9735961" %}

##



