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
| Mailgun | 10k / month <br> 須完成 domain 綁定 |
| Amazon SES | 62k / month <br> 須從 EC2 中託管的應用程式傳送 |
| SendGrid | 40,000 前 30 日 <br> 之後 100 / day <br> 需綁定信用卡選擇試用後消費計畫 |
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

* 前往 Mailgun 網站

> https://www.mailgun.com/

![](/assets/mail mailgun apply.png)

* 選擇 SignUp 填以下表格來進行註冊

![](/assets/mail mailgun apply form.png)

* 完成註冊之後，您已經可以開始使用

![](/assets/mail mailgun apply finish.png)

* 您可以在 Domain 中看見您目前擁有的寄信設定，包含 Domain 與 API Key

> https://app.mailgun.com/app/domains

![](/assets/mail mailgun domain detail.png)

* 建議新增自己的網域來完成 Custom Domain 設定，未完成設定的帳號會有以下的幾個限制
    * 每日僅可傳送 300 封 Email
    * 僅能寄送給授權的授信人
    * 無法以您的 Mail 位址寄出
    * 寄出的 Mail 會帶有 Mailgun 預設訊息

### 設定 Parse Mail 服務 {#adapter}

```
var server = ParseServer({
  ...otherOptions,
  // Enable email verification
  verifyUserEmails: true,

  // if `verifyUserEmails` is `true` and
  //     if `emailVerifyTokenValidityDuration` is `undefined` then
  //        email verify token never expires
  //     else
  //        email verify token expires after `emailVerifyTokenValidityDuration`
  //
  // `emailVerifyTokenValidityDuration` defaults to `undefined`
  //
  // email verify token below expires in 2 hours (= 2 * 60 * 60 == 7200 seconds)
  emailVerifyTokenValidityDuration: 2 * 60 * 60, // in seconds (2 hours = 7200 seconds)

  // set preventLoginWithUnverifiedEmail to false to allow user to login without verifying their email
  // set preventLoginWithUnverifiedEmail to true to prevent user from login if their email is not verified
  preventLoginWithUnverifiedEmail: false, // defaults to false

  // The public URL of your app.
  // This will appear in the link that is used to verify email addresses and reset passwords.
  // Set the mount path as it is in serverURL
  publicServerURL: 'https://example.com/parse',
  // Your apps name. This will appear in the subject and body of the emails that are sent.
  appName: 'Parse App',
  // The email adapter
  emailAdapter: {
    module: '@parse/simple-mailgun-adapter',
    options: {
      // The address that your emails come from
      fromAddress: 'parse@example.com',
      // Your domain from mailgun.com
      domain: 'example.com',
      // Your API key from mailgun.com
      apiKey: 'key-mykey',
    }
  },

  // account lockout policy setting (OPTIONAL) - defaults to undefined
  // if the account lockout policy is set and there are more than `threshold` number of failed login attempts then the `login` api call returns error code `Parse.Error.OBJECT_NOT_FOUND` with error message `Your account is locked due to multiple failed login attempts. Please try again after <duration> minute(s)`. After `duration` minutes of no login attempts, the application will allow the user to try login again.
  accountLockout: {
    duration: 5, // duration policy setting determines the number of minutes that a locked-out account remains locked out before automatically becoming unlocked. Set it to a value greater than 0 and less than 100000.
    threshold: 3, // threshold policy setting determines the number of failed sign-in attempts that will cause a user account to be locked. Set it to an integer value greater than 0 and less than 1000.
  },
  // optional settings to enforce password policies
  passwordPolicy: {
    // Two optional settings to enforce strong passwords. Either one or both can be specified. 
    // If both are specified, both checks must pass to accept the password
    // 1. a RegExp object or a regex string representing the pattern to enforce 
    validatorPattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.{8,})/, // enforce password with at least 8 char with at least 1 lower case, 1 upper case and 1 digit
    // 2. a callback function to be invoked to validate the password  
    validatorCallback: (password) => { return validatePassword(password) }, 
    doNotAllowUsername: true, // optional setting to disallow username in passwords
    maxPasswordAge: 90, // optional setting in days for password expiry. Login fails if user does not reset the password within this period after signup/last reset. 
    maxPasswordHistory: 5, // optional setting to prevent reuse of previous n passwords. Maximum value that can be specified is 20. Not specifying it or specifying 0 will not enforce history.
    //optional setting to set a validity duration for password reset links (in seconds)
    resetTokenValidityDuration: 24*60*60, // expire after 24 hours
  }
});
```

### 在 Parse Cloud Code 中使用 Mailgun 服務寄送 Email {#cloudcode}

