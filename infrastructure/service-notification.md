# Notification 服務

> Game Services 中，設計得當的 Notification 能有效的增加 retention，通常有三種使用情境：**本機端推送**、**雲端個別推送**、**雲端大量推送**
* 本機端推送，可用於已經預知時間與內容的提醒，可完全在無連網情形之下發起通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
* 雲端個別推送，通常是用於非計畫性的小事件，比如：有朋友丟了悄悄話給你
* 雲端大量推送，通常用於不定時的廣播事件，比如：營運公司手動開始了一個活動

在使用 Notification 服務時，通常包含了兩種類型：「雲端推送型通知」與「本機端通知」。在接下來的設定中，將會介紹「雲端推送型通知」與雲端服務平台的設定。想要能使用推送型通知，首先需要在使用者端程式中，設定推送服務，接下來會介紹 Android 平台 Firebase Cloud Messaging（以下簡稱 FCM）以及 iOS 平台 Apple Push Notification service（以下簡稱 APNs）。並會在後來的範例中，介紹 Parse 推送服務設定與測試，以及 Segmentation 的方法

### 目錄

* [設定 Android FCM](#android)
* [設定 iOS APNs](#ios)

### 設定 Android FCM {#android}

### 設定 iOS APNs {#ios}



