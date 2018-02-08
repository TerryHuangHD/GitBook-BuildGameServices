# Setup: Notification

### Menu

* Parse Installation Data
* Android FCM Setup
* iOS APN Setup
* Hint: Dashboard Test
* Advanced: Target Segmemtation



  > Android 選用：Parse Server 嫁接 Android GCM/FCM
  
  > iOS 選用：Parse Server 嫁接 APN push notification
  
  雲端推送服務通常是非集中型的使用方式（規律性的通知會透過本地端推送）。重要性、負荷較低的服務，可直接透過 Parse Server 伺服器直接進行推送即可。若有進階的需求，比如：大量、批次、定時、追蹤...等，也可選用其他較專業的推送平台，如：OneSignal

