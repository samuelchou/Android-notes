---
description: 一些關於 Preferences 的技巧。
---

# Preferences 設定元件

此頁是探討一般 Android 設定頁建議呈現的 Preferences View 元件系列。

如需了解 Android 如何儲存 key-value, 請參考 [Shared Preferences](data-access/shared-preferences.md).



{% embed url="https://developer.android.com/guide/topics/ui/settings" %}

{% embed url="https://stackoverflow.com/questions/43412495/how-to-use-preference-fragment" %}

## 種類

### Preference

最基礎的 Preference.

基本上只用來當 View 使用，除非你手動設置互動方法。（點擊或長按之類）

### SwitchPreference

開關版 Preference. 簡單，直覺，好用，大概是最常見的一種。



### CheckboxPreference

另一種樣貌的 SwitchPreference.



### ListPreference

清單 Preference, 按下後會跳出一個 List 供你選擇。

{% hint style="danger" %}
`ListPreference` 只允許字串格式 `string`.
{% endhint %}

{% hint style="danger" %}
`ListPreference` **一定要**同時設置 `entries` 與 `entryValues`, 且必須一對一，不可多或少。

前者是顯示、後者是儲存的值。
{% endhint %}

需要注意的是預設並**不會直接顯示**選擇的值。

{% hint style="info" %}
想要顯示選擇值，可以透過手動指派 `summary` 屬性。

或者可以設置 `useSimpleSummaryProvider` 來直接顯示。
{% endhint %}



### SeekBarPreference

讓使用者以拖拉方式控制整數 integer 的儲存。

{% hint style="danger" %}
截至 ktx:1.1.1 版本，`max` 屬性仍然**只能**透過 `android:` 前綴字指定。
{% endhint %}





## 技巧

### 在值變更前詢問

使用 `OnPreferenceChangeListener` 進行阻擋（變更前跳出視窗並回傳 `false` ）。

```kotlin
val switchPref = findPreference<SwitchPreference>("DEMO_KEY_SWITCH")
switchPref.setOnPreferenceChangeListener { _, newValue ->
    // direct change when turn off
    if (newValue == false) return@setOnPreferenceChangeListener true
    AlertDialog.Builder(context)
        .setMessage("Are you sure to turn it on?")
        .setPositiveButton("Yes") { _, _ ->
            switchPref.isChecked = true
        }
        .setNegativeButton("Maybe Later", null)
        .show()
    false // don't turn on immediately.
}

```





