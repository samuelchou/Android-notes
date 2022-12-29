# 虛擬機問題



## 無法啟動

壞掉了：

<figure><img src="../.gitbook/assets/截圖 2022-12-29 下午6.20.10.png" alt=""><figcaption></figcaption></figure>

怎麼辦？

### 摘要

以下提供幾種方法

1. Cold Boot
2. 檢查更新
3. 重新安裝套件
4. 全部重設



### Cold Boot

就選擇右邊的「...」 > Cold Boot



### 檢查更新

執行 Android Studio > Check for Updates, 然後把需要更新的鬼東西全部更新一遍。



### 重新安裝套件

特別針對 Emulators 系列套件更新 / 重新安裝

{% embed url="https://stackoverflow.com/a/71333540/9735961" %}

### 全部重設

砍掉虛擬機檔案，然後重新安裝。

{% embed url="https://youtu.be/bOURCXjmXlg" %}

### Bonus: 挖掘錯誤訊息

{% embed url="https://stackoverflow.com/a/72732551/9735961" %}

開啟 zsh(terminal) 並且輸入：

```
~/Library/Android/sdk/tools/emulator -avd {AVD_ID}
```

{% hint style="info" %}
或者可能要使用：

[https://stackoverflow.com/a/52496987/9735961](https://stackoverflow.com/a/52496987/9735961)

```
~/Library/Android/sdk/emulator/emulator -avd {AVD_ID}
```
{% endhint %}



其會顯示訊息，告訴你問題出在哪。

我曾經遇過的訊息有：

```
Could not launch '/Users/STC/Library/Android/sdk/emulator/qemu/darwin-x86_64/qemu-system-x86_64': No such file or directory
```

```
PANIC: Missing emulator engine program for 'x86' CPU.
```

```
PANIC: Avd's CPU Architecture 'x86_64' is not supported by the QEMU2 emulator on aarch64 host.
```

