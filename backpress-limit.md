---
description: 返回鍵是安卓內建按鍵之一，有特殊的限制方式。
---

# 限制返回鍵 BackPress

在不同地方操作，會有不同的限制方法。

## Activity: 覆寫 onBackPressed

```kotlin
override fun onBackPressed() {
    if (isLimited) {
        // do something to tell user...
    } else {
        super.onBackPressed()
    }
}
```

## Fragment: 使用 OnBackPressedCallback

```kotlin
val backPressInterceptor = object : OnBackPressedCallback(false) {
    override fun handleOnBackPressed() {
        Log.d(TAG, "handleOnBackPressed: back press intercepted. (Won't let user back)")
    }
}

override fun onViewCreated() {
    // ...
    activity?.onBackPressedDispatcher?.run {
        Log.d(TAG, "onBackPressedDispatcher: accessing on back press of activity. setup interceptor...")
        addCallback(viewLifecycleOwner, backPressInterceptor)
    } ?: run {
        Log.w(TAG, "onBackPressedDispatcher: failed getting activity back press. Interceptor won't work.")
    }
}

private fun limitUserBackPress(isLimited: Boolean) {
    backPressInterceptor.isEnabled = isLimited
}

```

## Dialog: setCancelable

```kotlin
private fun limitUserBackPress(isLimited: Boolean) {
    // (DialogFragment)
    setCancelable(isLimited)
}
```

