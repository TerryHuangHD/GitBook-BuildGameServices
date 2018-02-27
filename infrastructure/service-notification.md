# TODO: Notification 服務

Game Services 中，設計得當的 Notification 能有效的增加 retention，通常有三種使用情境：本機端推送、雲端個別推送、雲端大量推送

* 本機端推送，可用於已經預知時間與內容的提醒，可完全在無連網情形之下發起通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
* 雲端個別推送，通常是用於非計畫性的小事件，比如：有朋友丟了悄悄話給你
* 雲端大量推送，通常用於不定時的廣播事件，比如：營運公司手動開始了一個活動

### 目錄

* 設定 Android FCM Setup
* 設定 iOS APN Setup
* Parse 推送服務設定與測試
* Advanced: Target Segmentation
* Android 本機端推送
* iOS 本機端推送

### 待續
* Firebase 推送服務設定與測試