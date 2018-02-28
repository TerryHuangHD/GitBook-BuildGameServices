# Parse 推送服務設定與測試

### Parse Server Push Adapter 設定

若要透過 Parse 來推送兩個平台的通知，需分別取得推送所需要的資訊（前面有介紹），Android 須取得 **Sender ID** 以及**伺服器金鑰**，iOS 須取得 **development.p12** 以及 **production.p12** 兩個檔案，並上傳至伺服器中

```
var server = new ParseServer({
  // ...
  push: {
    android: {
      senderId: '',  // 取得的 FCM Sender ID 
      apiKey: ''     // 取得的 FCM 伺服器金鑰
    },
    ios: [
      {
        pfx: '/path/to/production.p12',  // 正式環境 p12 憑證檔案位置
        passphrase: '',                  // 憑證密碼（沒有的話可免去此欄位）
        bundleId: '',                    // Bundle ID
        production: true                 // 正式環境設定
      },
      {
        pfx: '/path/to/development.p12', // 開發環境 p12 憑證檔案位置 
        passphrase: '',                  // 憑證密碼（沒有的話可免去此欄位）
        bundleId: '',                    // Bundle ID
        production: false                // 開發環境設定
      }
    ]  
  }
});
```

### 將 token 存放至 Installation

### 使用 Dashboard 進行推送