# LoadMoreAdapter

## 構思

長什麼樣子比較好？

```kotlin
val adapter = LoadMoreAdapter<MyItem>(recyclerView).apply {
	preloadThreshold = 5

	setOnLoadMoreListener(object: OnLoadMoreListener {
		override fun onLoadMore() {
			loadMoreDataIntoAdapter()
		}
	})

	override fun canLoadMore(): Boolean {
		return getItemCount >= 30
	}
}

fun loadMoreDataIntoAdapter() {
	adapter.submitList(itemList)

	adapter.finishLoadMore()
}

```

如何吃個 EasyBindingAdapter?

```kotlin
val adapter = object : EasyBindingAdapter<MyItem, MyItemBinding> {
	override fun onBind(item: MyItem, binding: MyItemBinding) {
	}
}
```

```java
List<MyItem> dataList = new ArrayList<MyItem>();
EasyRecyclerViewAdapter adapter = new 
	EasyRecyclerViewAdapter<MyItem>(context, R.layout.item, dataList) {
	@Override
	public void bindData(View itemView, int position, MyItem data) {
		// do the bind
	}
}
```

設定 EasyBindingLoadMoreAdapter?

```kotlin
// ListAdapter
// -> LoadMoreAdapter
// ----> EasyBindingLoadMoreAdapter

// LoadMoreAdapter
val adapter = LoadMoreAdapter<MyItem>().apply {
	preloadThreshold = 5

	override fun onLoadMore() {
		loadMoreDataIntoAdapter()
	}

	override fun canLoadMore(): Boolean {
		return getItemCount >= 30
	}

	setupWithRecyclerView(recyclerView, layoutManager)
}

fun loadMoreDataIntoAdapter() {
	adapter.submitList(itemList)

	adapter.finishLoadMore()
}
```

## 結果

參考這個 repo 吧：

{% embed url="https://github.com/samuelchou/UI-Lab" %}

{% embed url="https://github.com/samuelchou/UI-Lab/tree/master/app/src/main/java/studio/ultoolapp/uilab/view/component" %}



