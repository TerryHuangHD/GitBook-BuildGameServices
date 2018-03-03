# Email 服務

> Email 在小產品中是常見的 Account System 驗證方式，在 Parse Service 以及 Firebase Authentication 中都能夠整合這樣的驗證方式；除此之外，Email 也常設計為輔助的通知系統

在 Parse 服務平台中，您必須使用外部的 Email 服務，並設定到 Parse 服務上，才能用來作為 User 認證系統使用；目前官方維護的 adapter 有 Mailgun 服務，其他非官方套件可支援 Amazon SES 服務, Mandrill 服務, Postmark 服務, Sendgrid 服務, Sendinblue 服務, Mailjet 服務, Generic 連線, Sendmail 連線, SMTP 連線...等等

在 Firebase 平台中，雖然 Authentication 由官方提供了 Email 驗證服務，但如果想寄送客製化內容的郵件，還是必須透過外部的服務商提供

### 目錄

* [常見 Email 服務商](#service-provider)
* [申請 Mailgun 服務](#mailgun)
* [設定 Parse Mail 服務](#adapter)
* [在 Parse Cloud Code 中使用 Mailgun 服務寄送 Email](#cloudcode)

### 常見 Email 服務商 {#service-provider}

> 注意：製表日期 2018 Mar，服務商可能隨時調整服務內容

常見的 Mail 服務商中，有部分提供了定期免費額度，可供小型產品試用，以下提供參考

|  | Free | 
| --- | --- |
| Mailgun | 10k / month |
| Amazon SES | 62k / month <br> 須從 EC2 中託管的應用程式傳送 |
| SendGrid | 40,000 前 30 日 <br> 之後 100 / day |
| SendinBlue | 600 / day |

以下提供價格計算機，以及不同量級的服務價格（美金），僅供參考
* [Mailgun 價格計算機](https://www.mailgun.com/pricing-2)
* [Amazon SES 價格介紹](https://aws.amazon.com/tw/ses/pricing/)，[價格計算機](https://calculator.s3.amazonaws.com/index.html)
* [Mandrill 價格介紹](https://www.mandrill.com/pricing/)
* [Postmark 價格介紹](https://postmarkapp.com/pricing)
* [SendGrid 價格介紹](https://sendgrid.com/pricing/)
* [SendinBlue 價格介紹](https://www.sendinblue.com/pricing/)

|  | 40k | 250k | 1000k |
| --- | --- | --- | --- |
| Mailgun | $15 | $165 | $515 |
| Amazon SES | $4 | $25 | $100 |
| Mandrill | $40 | $200 | $720 |
| Postmark | $47.5 | $200 | $520 |
| SendGrid | $9.95 | ~$200 | ~$500 |
| SendinBlue | $25 | $173(350k)  | $603(3000k with free dedicated IP) |

### 申請 Mailgun 服務 {#mailgun}

### 設定 Parse Mail 服務 {#adapter}

### 在 Parse Cloud Code 中使用 Mailgun 服務寄送 Email {#cloudcode}

