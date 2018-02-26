# TODO: SMS 服務

SMS 也是在小產品中是常見的 Account System 的驗證方式，這樣的驗證方法在大部分國家，比 Email 更具有「實名性質」，在 Firebase 中整合了這樣的驗證方式

> SMS 服務通常很類似，大部分都是透過 REST API 來提供服務，可依照服務範圍、開發習慣~~、\(價格\)~~進行選擇。常見的服務提供者有：Twilio, AWS SNS

> 目前 Parse 附帶的 Account System 並沒有直接設計 SMS 認證環節，須自行設計

### 目錄

* 常見服務商
* 範例：在 Parse Cloud Code 中使用 Twilio 服務寄送 SMS

### TODO
* Firebase Authentication SMS Verification 設定