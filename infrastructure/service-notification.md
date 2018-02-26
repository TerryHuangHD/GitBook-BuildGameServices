# Notification 服務

雲端推送服務通常是非集中型的使用方式（規律性的通知會透過本地端推送）。重要性、負荷較低的服務，可直接透過 Parse Server 伺服器直接進行推送即可。若有進階的需求，比如：大量、批次、定時、追蹤...等，也可選用其他較專業的推送平台，如：OneSignal

Game Services 中，設計得當的 Notification 能有效的增加 retention，通常有三種使用情境：本機端推送、雲端個別推送、雲端大量推送

本機端推送，可用於已經預知時間與內容的提醒，可完全在無連網情形之下發起通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
雲端個別推送，通常是用於非計畫性的小事件，比如：有朋友丟了悄悄話給你
雲端大量推送，通常用於不定時的廣播事件，比如：營運公司手動開始了一個活動

### 目錄

* [部署與偵錯](#deploy)
* [支援功能](#function)
* [範例：在 Parse 服務中架構簡易的 Cloud Code 部署機制](#parse_deploy)

### 部署與偵錯 {#deploy}


### Menu

* Parse Installation Data
* Android FCM Setup
* iOS APN Setup
* Hint: Dashboard Test
* Advanced: Target Segmemtation


  > Android 選用：Parse Server 嫁接 Android GCM/FCM
  
  > iOS 選用：Parse Server 嫁接 APN push notification