# 清單：下拉與上滑

## SwipeRefreshLayout 拉動刷新

官方套件：支援下拉刷新。

{% embed url="https://developer.android.com/training/swipe" %}

{% embed url="https://thumbb13555.pixnet.net/blog/post/312844960" %}

{% embed url="https://mnya.tw/cc/word/1520.html" %}

### 依賴庫

{% embed url="https://developer.android.com/jetpack/androidx/releases/swiperefreshlayout" %}

2020/07 起，改使用 androidx 結構：

```groovy
dependencies {
    implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.1.0"
}
```

### 基礎用法

當成 ViewGroup 包裹即可使用：

```markup
<androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
    <!-- something to refresh -->
</androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```

監聽刷新事件：

```kotlin
swipeRefreshLayout.setOnRefreshListener {
    // do something
}
```

關閉刷新（傳遞完成刷新事件）：

```kotlin
swipeRefreshLayout.setRefreshing(false)
```

## RecyclerView OnScrollListener 抓底 上滑加載

{% embed url="https://stackoverflow.com/a/33941753/9735961" %}

### 使用 Adapter Footer 的版本

{% embed url="https://www.jianshu.com/p/a1f378da7c9f" %}

{% embed url="https://medium.com/@programmerasi/how-to-implement-load-more-in-recyclerview-3c6358297f4" %}

{% embed url="https://blog.csdn.net/cunchi4221/article/details/107477237" %}

### 如果你發現 Load More VH 會留下空白...

試著 follow 這篇，重設 VH.itemView 的 LayoutParams:

{% embed url="https://stackoverflow.com/a/46342024/9735961" %}

```kotlin
holder.itemView.layoutParams = RecyclerView.LayoutParams(0, 0)
```









