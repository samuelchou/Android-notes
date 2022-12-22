# 製作自己的套件 (library)

## 建議通用規則

### 版本號命名： SemVer

如果你的 Library 會以任何形式發佈，那麼都建議加上版本號。

目前國際通用的版本號命名法則為 SemVer (語意化版本):

{% embed url="https://semver.org/lang/zh-TW/" %}

規則很簡單： `主版號.次版號.修訂號` . That's it.

```
1.2.3
^ this is a SemVer
```

## 第一步：生成 Library

{% embed url="https://developer.android.com/studio/projects/android-library" %}



## 發布通用教學

{% embed url="https://www.slideshare.net/kewang/library-80280143" %}



## Jitpack

{% embed url="https://muchone.pixnet.net/blog/post/404053121-android--%E8%8F%9C%E9%B3%A5%E7%AD%86%E8%A8%98--%E5%A6%82%E4%BD%95%E8%87%AA%E8%A3%BDandroid-library%E4%B8%A6%E4%B8%94%E9%80%8F" %}

### 限制

* 無法使用自定義網域：你必須自行命名

{% embed url="https://stackoverflow.com/questions/34434555/set-up-custom-domain-name-in-jitpack" %}



