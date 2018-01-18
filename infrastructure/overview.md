# 基礎服務架構介紹

![](/assets/Infrastructure.jpg "Infrastructure")

### Game Services 可能需要哪些基礎服務？

* #### 資料伺服器

  > 範例選用：Self-Hosted Parse

  在遊戲服務的範疇中，提供資料存放是最重要功能之一，比如：存放使用者資料、消費歷程...等等
  
  * Parse: 早期 Parse BaaS 服務提供了非常完整的後端服務，除了常見的資料存取服務之外，甚至包含了管理者介面、雲端程式、伺服器任務....等支援，完整滿足了小型產品、Indie Game 的需求，讓開發者能更專注在遊戲前端的開發。時至今日，Parse 雖然轉移成了 Self-Hosted 服務，但是透過許多外部服務的串接，大部分的功能還是得以實現
  
* #### 應用程式介面

  > 範例選用：Cloud Code on Parse
  
  除了資料的存放之外，伺服器往往也需要擁有邏輯操作的能力，並透過使用者或是排程來觸發，比如：排程統計每日的排行榜並給予獎勵...。面對這樣的需求，Cloud Code 在 Self-Hosted Parse 也被完整的支援了，Trigger, Webhook 也繼承了下來，可簡易的實作出遊戲中所需的伺服器 API，對於串接各種外部 Node.js package 也相當方便

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

* #### “即時性”服務

  > 範例選用：Firebase Realtime Database

  Firebase 早期變是純粹的即時資料庫服務，目前已擴充成完整的 BAAS 平台。強項在於能夠即時監聽雲端資料的變化，整合這樣的服務，可在遊戲中扮演關鍵的角色，提供遊戲更多互動性。除此之外，Firebase 更支援以 Node 為基礎的原子性操作，能讓即時遊戲服務更豐富，如：即時對弈、處理競爭條件...

### Self-Hosted Parse 基礎服務架設法

Parse 的服務架構中，基礎上需包含「資料庫服務」、以及「Parse Server 服務」。Parse 架構支援了相當多樣的服務架設方法，以下列舉一些常見的方法

* #### 本機端架設
  本機端架設法，提供了非常方便的方法來架設**「測試環境」**。Parse Server 透過簡易的 CLI 指令便可以在本機端開始服務。對應的資料庫，也提供了本機端的 mongodb runner，能直接架設本機端資料庫測試環境。當然也可以直接連接您的資料庫虛擬機，或是外部 Database as a Service（以下稱 DBaaS）服務
  
* #### 全虛擬機架設
  全虛擬機架設，是最完整的自架服務作法，在 Infrastructure as a Service（以下稱 IaaS）供應商中以虛擬機架設服務，常見於**「上線服務」**，各服務角色通常以 Dedicated Server 甚至是 Cluster 形式存在
  
* #### DBaaS 服務 + Parse 虛擬機
  除了全虛擬架設設法之外，也能將資料庫服務的部分連結外部資料庫服務提供商
  * 資料庫選擇，除了 MongoDB 之外，Parse 也支援了 PostgreSQL
  * 常見的 DBaaS 服務提供商有：[mLab](https://mlab.com/), [Amazon RDS](https://aws.amazon.com/tw/rds/postgresql/)
  
* #### 直接使用 Parse SaaS 服務
  如果您使用在小型的專案中，也可以考慮提供完整 Parse 服務的供應商
  * 常見的 Parse SaaS 服務商有：[SashiDo](https://www.sashido.io/), [Back4App](https://www.back4app.com/)
  
* #### Docker
  Parse 以及 MongoDB 都有官方提供的 Docker Image 提供選用
  * [MongoDB Docker Image](https://github.com/docker-library/mongo)
  * [Parse Docker Image](https://hub.docker.com/r/parseplatform/parse-server/) 

在本次的範例中，將會演示「全虛擬機架設」，盡可能地提供步驟與說明，並詳細的解說 Game Services 相關的套件設定