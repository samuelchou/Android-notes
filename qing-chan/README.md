---
description: RecyclerView, Adapter, 與其相關系列
---

# 清單

## Epoxy: 高端 RecyclerView

Made by Airbnb.

{% embed url="https://github.com/airbnb/epoxy" %}

{% embed url="https://www.slideshare.net/ssuser72c3b0/epoxy-78330870" %}

優勢／特色：

* 由大型企業 Airbnb 開發與維護的 Android List 解決方案
  * 豐富的 wiki
  * 大量網路文章
* 使用大量標記子來進行自動化生成與套用
* 支援 data binding (儘管是以他們自己的格式呈現)

### 基本觀念

三大物件：

1. `EpoxyModel`: 就是 item 啦
2. `EpoxyController` : 控制輸入的資料，與顯示的 item
3. `EpoxyRecyclerView` : 建議配合的 RecyclerView

### 使用方式一： RecyclerView 直接指派

```kotlin
epoxyRecyclerView.withModels { 
    header { 
        id("header") 
        title("My Photos")
        description("My album description!") 
    }
    photos.forEach {
        photoView {
            id(it.id())
            url(it.url())
        }
    }

    if (loadingMore) loaderView { id("loader") }
}
```

### 使用方式二： Adapter 處理資料

```kotlin
val adapter = EpoxyAdaper()
val item: EpoxyModel
val items: Collection<EpoxyModel>

// add single one
adapter.addModel(item)
adapter.notifyModelChanged(item) // something like original adapter

// or add a list
adapter.addModels(items)
adapter.notifyModelsChanged()
```

想要增強效能（或方便 input 整批清單），可以開啟 `enableDiffing` :

```kotlin
class DiffingEpoxyAdapter: EpoxyAdapter() {
    init {
        enableDiffing()
    }
    
    fun updateList(items: Collection<EpoxyModel>) {
        removeAllModels()
        addModels(items)
        notifyModelsChanged()
    }
}

val adapter = DiffingEpoxyAdapter()
val items: Collection<EpoxyModel>
adapter.updateList(items)
```

### Data Binding

{% embed url="https://github.com/airbnb/epoxy/wiki/Data-Binding-Support" %}

步驟

1. 匯入專門庫，並開啟 `databinding` 專案部署屬性
2. 使用標記子來自動轉換 layout 生成 model: `@EpoxyDataBindingPattern(rClass = R::class, layoutPrefix = "item")` 或者 `@EpoxyDataBindingLayouts({R.layout.header_view})`
   1. &#x20;你可以開一份專門檔案標記一個空class: `object EpoxyDataBindingPatterns`
3. 執行 Build Project, 此時 Epoxy 應該會自動生成 Model, 格式為 `(PrefixRemoved)NameBindingModel_`
   1. ex. `item_simple_one.xml` + `layoutPrefix = "item"` -> `SimpleOneBindingModel_`
   2. 該 BindingModel 也是 `EpoxyModel` 的一種
4. ...（尚未完成？）

### 使用 Epoxy 實作加載更多 Load More / Endless Scroll

{% embed url="https://github.com/airbnb/epoxy/wiki/Infinite-Scroll" %}

{% embed url="https://github.com/donglua/EpoxyLoadMoreDemo" %}





