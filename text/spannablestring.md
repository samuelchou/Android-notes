# SpannableString 文字魔術師

可以針對文字中的其中一段進行變化：上色、改變大小、改變字體、甚至插入圖片！

{% embed url="https://northbei.gitbooks.io/android_development_note/content/anything/spannablestring.html" %}

{% embed url="https://www.jianshu.com/p/f004300c6920" %}



## 快速觀念

1. `SpannableString` 可以視為 `String` 與 `Span` 的組合
2. `Span` 與文字組合時，**必須附加在現有文字上**
   1. 換言之，就算你是插入圖片 `ImageSpan`, 也**必須先有 text** 才能取代。
3. `SpannableStringBuilder` 可以 append 項目，但是只能 append `String` （精確來說是 `CharSequence` ）
   1. 換言之，你無法 append `Span` （請參照第一點）&#x20;



