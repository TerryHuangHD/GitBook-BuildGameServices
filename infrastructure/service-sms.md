# SMS 服務

> SMS 也是在小產品中是常見的 Account System 的驗證方式，這樣的驗證方法在大多數地區，比 Email 更具有**「實名性質」**，在 Firebase Authentication 中提供了這樣的驗證方式

在 Parse 服務平台中，Account System 並沒有直接設計 SMS 認證環節，但由於 User 資料表也具有可擴充性，所以可以透過自行設計來達成

在 Firebase 平台中，雖然 Authentication 由官方提供了 SMS 驗證服務，但驗證服務為收費項目，此外如果想寄送客製化內容的簡訊，還是必須透過外部的服務商提供

### 目錄

* 常見 SMS 服務商
* 申請 Twilio 服務
* [範例：在 Parse Cloud Code 中使用 Twilio 服務寄送 SMS](service-sms/parse-sms-cloud-code.md)
