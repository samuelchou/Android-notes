# Badge: 小紅點、未讀標記

Android 官方有推出 BadgeDrawable, 讓你可以在各種 drawable 上增加小紅點（或數字）。

{% embed url="https://material.io/develop/android/supporting/badge" %}

{% embed url="https://developer.android.com/reference/com/google/android/material/badge/BadgeDrawable" %}

真夠酷的。

{% embed url="https://www.jianshu.com/p/7f067cdad638" %}



## 在 MenuItem / ActionItem 使用 BadgeDrawable

{% embed url="https://stackoverflow.com/q/65597372/9735961" %}

NOTE: Material 1.3.0 以後，可以直接透過 BadgeUtils 來設定：

{% embed url="https://github.com/material-components/material-components-android/releases/tag/1.3.0" %}



```kotlin
val badge: BadgeDrawable

BadgeUtils.attachBadgeDrawable(badge, toolbar, R.id.menu_item_id)
```





