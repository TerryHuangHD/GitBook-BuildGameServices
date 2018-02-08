# 基礎服務架構介紹

![](/assets/Infrastructure.jpg "Infrastructure")

### Game Services 可能需要哪些基礎服務？

* #### 資料儲存

  在 Game Services 的範疇中，提供資料儲存是最重要功能之一，比如存放個人面向的使用者資料、消費歷程，或是存放公用面向的排行榜資料...等等。在實際的使用面上，各資料儲存系統都有其優缺點，比如：schemaless, security rule, indexing 能力, atomic operation 支援度...等等，也可能會同時使用多種資料儲存服務
  
* #### 應用程式介面
  
  應用程式介面除了提供基礎的資料存取介面之外，通常實作也需要擁有實現伺服器端邏輯的能力。在目前新興的服務中，更會包含了各種連動能力（如: Database Trigger, Schedule/Job, Web Hook）

* #### Notification 服務

  > Android 選用：Parse Server 嫁接 Android GCM/FCM
  
  > iOS 選用：Parse Server 嫁接 APN push notification

  Game Services 中，雲端推送服務通常是非集中型的使用方式（規律性的通知會透過本地端推送）。重要性、負荷較低的服務，可直接透過 Parse Server 伺服器直接進行推送即可。若有進階的需求，比如：大量、批次、定時、追蹤...等，也可選用其他較專業的推送平台，如：OneSignal

* #### Job 服務

  > 範例選用：Parse Cloud Job 嫁接伺服器 Cron

  Job 服務，主要提供伺服端能依照時間進行動作的能力，依照所需要的任務複雜度，可能會有不同的選擇性。常見的 Game Services，通常用來做定時的伺服器資料更新，比如：每日排行榜...。簡易的需求可透過伺服器 Cron 系統便可簡單達成。如果有進階的需求，像是 priority, queue ...等，可選用 kue 來做

* #### Email 服務

  > Parse Server 選用: mailgun
  
  > Cloud Code 選用: AWS SES
  
  目前 Parse Server 將 Email 服務整合在帳號系統的認證環節中使用，可選用簡易的 AWS SES 服務。如果還有更大量的批次寄送或是更進階的行銷需求，也可考慮更專業、便於管理的電郵服務，比如 mailgun

* #### SMS 服務

  > 目前 Parse 附帶的 Account System 並沒有直接設計 SMS 認證環節，須自行設計
  
  > Cloud Code 選用: Twilio
  
  SMS 服務通常很類似，大部分都是透過 REST API 來提供服務，可依照服務範圍、開發習慣~~、\(價格\)~~進行選擇。常見的服務提供者有：Twilio, AWS SNS

* #### Hosting 服務

  > 範例選用：Parse Server 嫁接 GridStore

  如果是輕度使用，比如遊戲存放個人圖示，可選擇與 Parse Sever 透過 Adapter 直接嫁接 GridFS，或是透過伺服器直供。如果是開放型、大型靜態資料，建議可以做獨立的檔案伺服架構，或是簡單選用 GCS/S3 + CDN

* #### Time 服務


* #### Realtime 服務

  > 範例選用：Firebase Realtime Database

  Firebase 早期變是純粹的即時資料庫服務，目前已擴充成完整的 BAAS 平台。強項在於能夠即時監聽雲端資料的變化，整合這樣的服務，可在遊戲中扮演關鍵的角色，提供遊戲更多互動性。除此之外，Firebase 更支援以 Node 為基礎的原子性操作，能讓即時遊戲服務更豐富，如：即時對弈、處理競爭條件...