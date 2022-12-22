# 清單 RecyclerView



## 炫技：演示資料

{% embed url="https://www.eeaseries.com/2021/09/android-tools-attributes-reference.html" %}



## 疑難排解

### IllegalArgumentException: Tmp detached view should be removed from RecyclerView before it can be recycled

如果你使用 RecyclerView, 某天在 Firebase Crashlytics 上可能會看到突如其來的錯誤訊息：

```
Fatal Exception: java.lang.IllegalArgumentException: Tmp detached view should be removed from RecyclerView before it can be recycled: YourYiewHolder{c81d962 position=0 id=-1, oldPos=-1, pLpos:-1 update tmpDetached no parent} androidx.recyclerview.widget.RecyclerView{94df30a VFED..... ........ 0,171-1016,332 #7f08017b app:id/recyclerViewId}, adapter:your.custom.RecyclerViewAdapter@dfdf354, layout:androidx.recyclerview.widget.LinearLayoutManager@c3c62fd, context:SomeActivityOrFragmentContextWrapper@776b702
       at androidx.recyclerview.widget.RecyclerView$Recycler.recycleViewHolderInternal(RecyclerView.java:6439)
       at androidx.recyclerview.widget.RecyclerView.removeAnimatingView(RecyclerView.java:1456)
       at androidx.recyclerview.widget.RecyclerView$ItemAnimatorRestoreListener.onAnimationFinished(RecyclerView.java:12699)
       at androidx.recyclerview.widget.RecyclerView$ItemAnimator.dispatchAnimationFinished(RecyclerView.java:13199)
       at androidx.recyclerview.widget.SimpleItemAnimator.dispatchAddFinished(SimpleItemAnimator.java:302)
       at androidx.recyclerview.widget.DefaultItemAnimator$5.onAnimationEnd(DefaultItemAnimator.java:247)
       at android.view.ViewPropertyAnimator$AnimatorEventListener.onAnimationEnd(ViewPropertyAnimator.java:1111)
       ...
```

推測成因可能有多個，不過主要跟 stable Id 或動畫演出有關。

{% embed url="https://stackoverflow.com/q/30078834/9735961" %}

{% embed url="https://blog.csdn.net/lylddingHFFW/article/details/102930560" %}

{% embed url="https://github.com/airbnb/epoxy/issues/689" %}

這裡的網友留言感覺特別關鍵：

{% hint style="info" %}
Basically I think it's RecyclerView not liking that **you hide it while it's still animating.**\
\---- [AndroidDeveloperLB](https://github.com/AndroidDeveloperLB) at Epoxy Issue #689
{% endhint %}

{% embed url="https://issuetracker.google.com/issues/148720682" %}



#### 可能有效的解法

移除 RecyclerView 的 Animator

```kotlin
recyclerView.itemAnimator = null
```

同樣思路：不要使用 `animateLayoutChanges`

```markup
<SomeBaseViewGroup
    animateLayoutChanges="true" 
    >
    <!--^ Don't use it anymore -->
    <androidx.recyclerview.widget.RecyclerView />
</SomeBaseViewGroup>
```

取消 Adapter 的 stable Id

```kotlin
// adapter.setHasStableIds(true) // remove it
adapter.setHasStableIds(false) // or set to false...?
```



