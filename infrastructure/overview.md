# 整體架構介紹

* 以 Parse 架構實踐「主要資料儲存」以及各式「伺服器邏輯」
* 以 Firebase 服務實踐「即時性」以及「原子性」操作

「圖：架構」

### 為什麼選擇自架遊戲服務？

* 跨平台
  * iOS Game Center / Google Play Game Services 目前都沒有跨平台支援\(Play Game Services for iOS is deprecated\)
* 資料伺服器自主性
  * 自行擁有資料庫伺服器，可簡易的在全球各服務區提供較優質的服務
  * 部分國家，對於服務資料位置有限制
* 彈性較高可客製化較多樣的遊戲服務
  * 相比於其他第三方 Game Service，自行架設遊戲服務，可以~~\(耍任性\)~~支援多樣性的伺服器邏輯

### 為什麼選擇這些後端服務？

* Parse
  * 這樣的選擇有其歷史原因，早期 Parse 服務提供了非常完整的後端服務，完整滿足了 Indie Game 開發者在後端以及其他伺服器服務的需求。時至今日，Parse 雖然轉移成了 Self-Hosted，卻也剛好能達到資料自主性的需求。除此之外 Parse 在資料多樣化取用、管理與呈現資料、彈性的權限管控都有很好的表現
* Firebase
  * Firebase 早期是純粹的即時資料庫服務 Realtime Database，可在遊戲的互動環節中，扮演重要的角色。
  * 目前 Firebase 已擴充成完整的 BAAS 平台
* AWS SES
  * 作為簡易的註冊訊息，以 AWS SES 是可以應付得來的。如果還有更大量或是更進階的需求，也可考慮現成的電郵服務，比如 mailgun

# 服務架設

1. 架設/註冊 Parse 服務
   1. IAAS Solution
   2. SAAS Solution
2. 註冊 Firebase 服務
3. 將架構加到遊戲中



