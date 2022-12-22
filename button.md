# Button



## 著色(半)透明色彩

請使用 `app:backgroundTint:`

```xml
<Button
    app:backgroundTint="@color/my_transparent_color" />
```

此時會發現仍然有陰影。需要移除 `stateListAnimator` （透過重新指派）：

{% embed url="https://stackoverflow.com/a/39122825/9735961" %}

```xml
<Button
    android:stateListAnimator="@null"
    app:backgroundTint="@color/my_transparent_color" />
```

上述操作**同樣適用**於 Material Button:

```xml
<com.google.android.material.button.MaterialButton
    android:stateListAnimator="@null"
    app:backgroundTint="@color/my_transparent_color" />
```







