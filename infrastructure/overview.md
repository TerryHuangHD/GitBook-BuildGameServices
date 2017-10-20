# 整體架構介紹

* 以 Parse 架構實踐「主要資料儲存」以及各式「伺服器邏輯」
* 以 Firebase 服務實踐「即時性」以及「原子性」操作

「圖：架構」

### 為什麼選擇自架遊戲服務？

* 跨平台
  * iOS Game Center / Google Play Game Services 目前都沒有跨平台支援\(Play Game Services for iOS is deprecated\)
* 擁有資料伺服器
  * 自行擁有資料庫伺服器，可簡易的在全球各服務區提供較優質的服務
  * 部分國家，對於服務資料位置有限制
* 彈性較高可客製化較多樣的遊戲服務
  * 相比於其他第三方 Game Service，自行架設遊戲服務，可以~~\(耍任性\)~~支援多樣性的伺服器邏輯

### 為什麼選擇使用 Parse & Firebase 作為後端服務？

這樣的選擇有其歷史原因，早期 Parse 服務提供了非常完整的後端服務，而 Firebase 則是純粹的即時資料庫服務；時至今日，Parse 能夠 Self-Hosted，反而更能支援資料自主性。

、多樣化取用、便於管理與呈現資料、更彈性的權限管控考量下，將資料儲存與伺服器邏輯實踐於 Parse 平台；而為了取用更完整的即時通訊能力、更完整支援的交易處理能力，將即時服務實踐在 Firebase 平台

|  | Parse | Firebase |
| :--- | :--- | :--- |
|  |  |  |
|  |  |  |
|  |  |  |



# 服務架設

1. 架設/註冊 Parse 服務
   1. IAAS Solution
   2. SAAS Solution
2. 註冊 Firebase 服務
3. 將架構加到遊戲中



