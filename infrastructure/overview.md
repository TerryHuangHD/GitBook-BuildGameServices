# 整體架構介紹

「圖：架構」

### 為什麼選擇自架遊戲服務？

* 跨平台
  * iOS Game Center / Google Play Game Services 目前都沒有跨平台支援，只能選用外部遊戲服務，如：Photon, AppWarp
* 資料伺服器自主性
  * 自行擁有資料庫伺服器，可有較高的使用彈性，比如：行銷，跨產品設計
  * 可簡易的在全球各服務區提供較優質的服務
  * 在某些國家，對於資料伺服器位置有所限制（應該不是 Indie Game Issue）
* 滿足遊戲邏輯多樣性
  * 與第三方遊戲服務比較，自行架設遊戲服務，較能實現更多客製化需求的配對機制、遊戲模式、互動方式

### Game Services 可能需要哪些基礎服務？

* 資料伺服器
  * 選用：Self-Hosted Parse
  * 早期 Parse BAAS 服務提供了非常完整的後端服務，甚至包含了管理者介面、雲端程式、伺服器任務....等支援，完整滿足了 Indie Game 的需求，讓開發者能更專注在遊戲的開發。時至今日，Parse 雖然轉移成了 Self-Hosted 服務，但是透過許多外部服務的串接，大部分的功能還是得以實現。
* 應用程式介面
  * 選用：Cloud Code on Parse
  * Cloud Code 在 Self-Hosted Parse 也被完整的支援了，Trigger, Webhook 也繼承了下來，可簡易的實作出遊戲中所需的伺服器 API，也便於串接各種 Node.js package。
* “即時性”服務
  * 選用：Firebase Realtime Database
  * Firebase 早期變是純粹的即時資料庫服務，目前已擴充成完整的 BAAS 平台。能夠即時監聽雲端資料的變化，可在遊戲中扮演關鍵的角色，提供遊戲更多互動性。
  * 除此之外，Firebase 更支援以 Node 為基礎的原子性操作，能讓即時遊戲服務更豐富，如：即時對弈、處理競爭條件...
* Notification 服務
  * Android 選用：Parse Server 嫁接 Android GCM/FCM
  * iOS 選用：Parse Server 嫁接 APN push notification
  * Game Services 中，雲端推送服務通常是分散式的使用方式（規律性的通知會透過本地端推送）。若是以這樣的需求為前提，可直接透過 Parse Server 伺服器直接進行推送即可。若有進階的需求，比如：大量、批次、定時、追蹤...等，也可選用其他較專業的推送平台，如：OneSignal
* Job 服務
  * 選用：Parse Cloud Job 嫁接伺服器 Cron
  * 依照所需要的任務複雜度，可能會有不同的選擇性，以常見的 Game Services，通常用來做定時的伺服器資料更新，比如：每日排行榜...，透過伺服器 Cron 便可簡單達成。如果有進階的需求，像是 priority, queue ...等，可選用 kue 來做
* Email 服務
  * 選用：Parse Server 嫁接 mailgun
  * 目前 Parse 將此 Email 服務，整合在帳號系統的認證環節中使用，可選用簡易的 AWS SES。如果還有更大量的批次寄送或是更進階的行銷需求，也可考慮更專業、便於管理的電郵服務，比如 mailgun
* SMS 服務
  * 目前 Parse 附帶的 Account System 並沒有直接支援 SMS 認證服務
  * 這類型服務大同小異，大部分都是透過 REST API 來傳送，可依照服務商服務範圍、開發習慣~~、\(價格\)~~進行選擇。常見的服務提供者有：Twilio, AWS SNS
* Hosting 服務
  * 選用：Parse Server 嫁接 GridStore
  * 如果是遊戲輕度使用，比如存放個人圖示，可選擇與 Parse Sever 透過 Adapter 直接嫁接 GridFS。
  * 如果是開放型、大型靜態資料，建議可以做獨立的檔案伺服架構，如：簡單選用 GCS/S3 + CDN。

# 服務架設

1. 架設/註冊 Parse 服務
   1. IAAS Solution
   2. SAAS Solution
2. 註冊 Firebase 服務
3. 將架構加到遊戲中



