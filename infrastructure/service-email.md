# Email 服務

> Email 在小產品中是常見的 Account System 驗證方式，在 Parse Service 以及 Firebase Authentication 中都能夠整合這樣的驗證方式；除此之外，Email 也常設計為輔助的通知系統

在 Parse 服務平台中，您必須使用外部的 Email 服務，並設定到 Parse 服務上，才能用來作為 User 認證系統使用；目前官方維護的 adapter 有 Mailgun 服務，其他非官方套件可支援 Amazon SES 服務, Mandrill 服務, Postmark 服務, Sendgrid 服務, Sendinblue 服務, Mailjet 服務, Generic 連線, Sendmail 連線, SMTP 連線...等等

在 Firebase 平台中，雖然 Authentication 由官方提供了 Email 驗證服務，但如果想寄送客製化內容的郵件，還是必須透過外部的服務商提供

### 目錄

* 常見 Email 服務商
* 申請 Mailgun 服務
* [設定 Parse Mail 服務](#adapter)
* [在 Parse Cloud Code 中使用 Mailgun 服務寄送 Email](#cloudcode)