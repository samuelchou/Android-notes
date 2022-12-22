# 傳遞與解析資料

## 開啟並傳遞至 Activity

傳遞

```kotlin
Intent(context, TargetActivity::class.java).apply {
    putExtra(KEY_STRING, value)
    // or by bundle
    putExtras(bundle)
}
```

接收／解析

```kotlin
// in Activity
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val bundle = intent.extras
}
```

## 開啟並傳遞至 Fragment

傳遞

```kotlin
TargetFragment().apply {
    arguments = bundle
}
```

接收／解析

```kotlin
// in Fragment
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val bundle = arguments
}
```

## 從 Activity 回傳

開啟與接收者



在對象中設置



## 從 Fragment 回傳：到 Fragment

透過 `targetFragment` 觸發 `onActivityResult`

{% embed url="https://stackoverflow.com/a/34527080/9735961" %}

開啟與接收者

```kotlin
// in Fragment
val dialog = MyDialog()
dialog.setTargetFragmet(this, REQUEST_CODE)
dialog.show(parentFragmentManager, SOME_TAG)


override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    if (requestCode == REQUEST_CODE) {
        // do something...
        if (resultCode == SOME_RESULT_CODE) {
            val bundel = data?.extras
        }
    }
}
```

在對象中設置

```kotlin
val bundle = Bundle() // put data inside it

Intent().apply {
    putExtras(bundle)
}.let {
    getTargetFragment().onActivityResult(getTargetRequestCode(), SOME_RESULT_CODE, it)
}
```

## 從 Fragment 回傳：到 Activity

感覺可以參考到 Fragment 的操作來處理？

* 不行： `fragment().getActivity()` 會回傳 `FragmentActivity` , 而 `fragmentActivity.onActivityResult()` 是 `protected` 方法....

