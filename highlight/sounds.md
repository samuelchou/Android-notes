# 提示音 / 音效

想要在 Android App 播放音效，內建有三種方法：

1. Ringtone
2. SoundPool
3. MediaPlayer

相關教學：

{% embed url="https://style77125tech.pixnet.net/blog/post/152442867" %}

{% embed url="https://blog.csdn.net/foruok/article/details/52578930" %}



{% embed url="https://blog.csdn.net/zhangying1994/article/details/50851111" %}

{% embed url="https://www.twblogs.net/a/5b8447002b71775d1cd016fa" %}

## 音效播放種類

{% embed url="https://support.google.com/android/answer/9082609" %}

{% embed url="https://developer.android.com/reference/android/media/AudioAttributes.html" %}

Android 提供多了音效頻道來處理不同的聲音。

Android 已知選項：鈴聲，媒體，通話，鬧鐘，通知

Samsung 另外的選項：系統，Bixby voices

根據 AudioAttributes , 有以下選項可以選擇：

* `USAGE_UNKNOWN`
* `USAGE_MEDIA` → 預設值，使用媒體音量
* `USAGE_VOICE_COMMUNICATION`
* `USAGE_VOICE_COMMUNICATION_SIGNALLING`
* `USAGE_ALARM` → 使用「鬧鐘」音量
* `USAGE_NOTIFICATION` → 使用「通知」音量
* `USAGE_NOTIFICATION_RINGTONE` → 使用「鈴聲」音量
* `USAGE_NOTIFICATION_COMMUNICATION_REQUEST`
* `USAGE_NOTIFICATION_COMMUNICATION_INSTANT`
* `USAGE_NOTIFICATION_COMMUNICATION_DELAYED`
* `USAGE_NOTIFICATION_EVENT`
* `USAGE_ASSISTANCE_ACCESSIBILITY`
* `USAGE_ASSISTANCE_NAVIGATION_GUIDANCE`
* `USAGE_ASSISTANCE_SONIFICATION`
* `USAGE_GAME`
* `USAGE_VIRTUAL_SOURCE`
* `USAGE_ASSISTANT`
