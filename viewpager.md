# ViewPager



## ViewPager2

### 基礎

很簡單：設定有幾頁，設定如何生成 Fragment, 即可完成。

{% embed url="https://ithelp.ithome.com.tw/articles/10247841" %}

{% embed url="https://www.jianshu.com/p/25aa5cacbfb9" %}





### With BottomNavigationView

{% embed url="https://www.jianshu.com/p/41d0e33bb674" %}



### 無限滑動

{% embed url="https://medium.com/mobile-app-development-publication/android-bi-direction-infinite-viewpager-2-scrolling-1a729e4ee773" %}

{% embed url="https://stackoverflow.com/a/59018969/9735961" %}







## 與 ViewPager 聯合使用時可能造成變數尚未初始化 (Null)

{% embed url="https://stackoverflow.com/q/33477846/" %}

{% embed url="https://stackoverflow.com/q/20280811/" %}

這是一個古老的 bug = =

### 解決方法

1. 在 `newInstance()` 裡面連帶處理變數的初始化。
2. 如果可以，將部分變數的宣告加上 `final` 關鍵字（例如 `List` 系列）。

