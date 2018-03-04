# SMS 服務

> SMS 也是在小產品中是常見的 Account System 的驗證方式，這樣的驗證方法在大多數地區，比 Email 更具有**「實名性質」**，在 Firebase Authentication 中提供了這樣的驗證方式

在 Parse 服務平台中，Account System 並沒有直接設計 SMS 認證環節，但由於 User 資料表也具有可擴充性，所以可以透過自行設計來達成

在 Firebase 平台中，雖然 Authentication 由官方提供了 SMS 驗證服務，但驗證服務為收費項目，此外如果想寄送客製化內容的簡訊，還是必須透過外部的服務商提供

### 目錄

* [常見 SMS 服務商](#service-provider)
* [申請 Twilio 服務](#twilio)
* [在 Parse Cloud Code 中使用 Twilio 服務寄送 SMS](#cloudcode)

### 常見 SMS 服務商 {#service-provider}

> 注意：製表日期 2018 Mar，服務商可能隨時調整服務內容

常見的 SMS 服務商中，只有 Amazon SNS 提供了 100 則的免費額度。以下提供常見服務價格計算機，以及價格比較表（美金），僅供參考
* [Amazon SNS 價格計算機](https://aws.amazon.com/tw/sns/sms-pricing/)
* [Twilio 價格介紹](https://www.twilio.com/sms/pricing/tw)
* [Plivo 價格介紹](https://www.plivo.com/pricing/TW/#!sms)
* [Nexmo 價格介紹](https://www.nexmo.com/products/sms/pricing)

| 服務 | 電信 | 收費 |
| --- | --- |
| Amazon SNS | 台灣大哥大 | $0.04612 |
| Amazon SNS | 台灣之星、威寶 | $0.0438 |
| Amazon SNS | 其他 | $0.06997 |
| Twilio | - | $0.0540 |
| Plivo | 亞太電信 | $0.0380 |
| Plivo | 中華電信 | $0.0360 |
| Plivo | 台灣之星、威寶 | $0.0300 |
| Plivo | 其他 | $0.0350 |
| Nexmo | - | $0.0533 |

### 申請 Twilio 服務 {#twilio}
### 在 Parse Cloud Code 中使用 Twilio 服務寄送 SMS {#cloudcode}
