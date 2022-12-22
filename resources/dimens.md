# dimens 與各種數值



## 在程式中獲取 dp (to px)

有時候可能會需要透過程式設置或變更一些介面長度，例如 padding, margin 等。

不過大多時候直接輸入整數是 px. （例如 `setBounds` 函式）

推薦作法：使用 dimens, 並配合 resources 便捷方法。

{% embed url="https://stackoverflow.com/a/35360836/9735961" %}

in dimens.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="margin_for_custom_view">12dp</dimen>
</resources>
```

in program

```kotlin
val marginInPx: Int = resources.getDimensionPixelOffset(R.dimen.margin_for_custom_view)
```





