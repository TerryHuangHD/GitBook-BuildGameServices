# 整體架構介紹

「圖：架構」

### 為什麼選擇自架遊戲服務？

* 跨平台
  * iOS Game Center / Google Play Game Services 目前都沒有跨平台支援\(Play Game Services for iOS is deprecated\)，只能選用外部遊戲服務，如：Photon, AppWarp
* 資料伺服器自主性
  * 自行擁有資料庫伺服器，可有較高的使用彈性
  * 可簡易的在全球各服務區提供較優質的服務；再某些國家，對於資料伺服器位置有所限制
* 彈性較高可客製化較多樣的遊戲服務
  * 與第三方遊戲服務比較，自行架設遊戲服務，較能實現更多客製化需求的配對、遊戲模式

### Game Service 需要哪些後端基礎服務？

* 資料伺服器
  * 選用：Self-Hosted Parse
  * 早期 Parse BAAS 服務提供了非常完整的後端服務，完整滿足了 Indie Game 的需求，讓開發者能更專注在遊戲的開發。時至今日，Parse 雖然轉移成了 Self-Hosted 服務，但是透過許多外部服務的串接，大部分的功能還是得以實現。
* 應用程式介面
* “即時性”服務
  * Firebase 早期變是純粹的即時資料庫服務 Realtime Database，可在遊戲中提供即時互動、即時對弈、即時同步...功能
  * 目前 Firebase 已擴充成完整的 BAAS 平台
* Notification 服務
  * 如果作為簡易、零星的通知使用，使用 Parse Server 伺服器直接進行推送即可。若有進階的需求，也可選用其他較專業的推送平台，如：OneSignal
* Job 服務
  * 依照任務複雜度，可能會有不同的選擇性，較簡易的任務，可透過伺服器 Cron 來達成。如果有進階的需求，像是 priority, queue ...等，可選用 kue 來做
* Email 服務
  * 如果僅僅作為 Game Services 中註冊相關環節的使用，以 AWS SES 是足以應付得來的。如果還有更大量或是更進階的需求，也可考慮現成的電郵服務，比如 mailgun
* SMS 服務
  * 這類型服務大同小異，可依照開發者習慣~~\(價格\)~~進行選擇
* Hosting 服務

# 服務架設

1. 架設/註冊 Parse 服務
   1. IAAS Solution
   2. SAAS Solution
2. 註冊 Firebase 服務
3. 將架構加到遊戲中



