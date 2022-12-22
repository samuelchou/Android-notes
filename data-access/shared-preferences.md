# Shared Preferences



{% embed url="https://developer.android.com/training/data-storage/shared-preferences" %}

## 基礎

使用 SharedPreferences 只要：

1. 獲取 preferences 實體 by context
2. 讀取或設定值

簡單吧。

SharedPreferences 有以下限制：

1. 存取類別有限：僅限儲存 Boolean, Int, Long, Float, String, Set\<String>
   1. (Custom) Class 可以透過套件（如 Gson 等）展平成 String / 從 String 讀取
2. 初始化一定要有 Context 才能運行
   1. 請小心持有範圍，不要讓 Context 外洩

## 獲得 Preferences 實體

```kotlin
context.getSharedPreferences("com.your.app.preference", Context.MODE_PRIVATE)
```

其中第一項參數是 preferences 清單命名。一般建議加上app命名前綴字，避免撞到其他命名。

{% hint style="info" %}
你可以帶入不同命名來獲得不同的清單。
{% endhint %}

或者，也可以透過 Android X 套件

```groovy
implementation 'androidx.preference:preference-ktx:1.2.0'
```

```
PreferenceManager.getDefaultSharedPreferences(context)
```

或者，如果你想要取得 by Activity 的 preference:

```kotlin
activity.getPreferences(Context.MODE_PRIVATE)
```



## 讀取值

依照 type 不同使用不同方法。

需注意：需提供預設值。**preference 不接受 null 存入 / 預設值**。

```kotlin
val pref: SharedPreferences

val boolVal = pref.getBoolean("keyBool", false)
val intVal = pref.getInt("keyInt", 0)
val longVal = pref.getLong("keyLong", 0L)
val floatVal = pref.getFloat("keyFloat", 0.0f)
val stringVal = pref.getString("keyString", null) // 唯二可以傳 null 
val stringSetVal: Set<String> = pref.getStringSet("keyStringSet", null) // 唯二可以傳 null 
```



## 新增、編輯、刪除

流程：

1. 取得該 preference 的 editor
2. 放入更新的值
3. 儲存

依照 type 不同使用不同方法。

```kotlin
val editor = pref.edit()

editor.putBoolean("keyBool", true)
editor.putInt("keyInt", 25)
editor.putLong("keyLong", 1000L)
editor.putFloat("keyFloat", 11.11f)
editor.putString("keyString", "set string")
editor.putStringSet("keyStringSet", setOf("st1", "st2"))

editor.apply() // important! // async task
// editor.commit() // another option: sync task
```

刪除的話比較簡單一些：

```kotlin
val editor = pref.edit()

editor.remove("key") // remove single pref set

editor.clear() // remove ALL pref on this pref

editor.apply() // important! // async task
// editor.commit() // another option: sync task
```



## 特定 Class 在 Pref 的存取

只要定義好特定 Class 如何「扁平」成能接受的類別（例如 String ），然後讀取 / 寫入時進行轉換即可。

網路普遍推薦使用 gson 處理這個過程：

{% embed url="https://stackoverflow.com/a/18463758/9735961" %}

```groovy
implementation 'com.google.code.gson:gson:2.8.8'
```

```kotlin
val pref: SharedPreferences
val gson = Gson()
val key = "keyObj"

// to read
val typeToken = object : TypeToken<MyObject>() {}.type
val default: MyObject
val myObj: MyObject = if (pref.contains(key)) {
    gson.fromJson(pref.getString(key, gson.toJson(default), type)
} else default

// to write
pref.edit().apply {
    putString(key, gson.toJson(myObj))
    apply()
}

// to remove
pref.edit().remove(key).apply()

```





