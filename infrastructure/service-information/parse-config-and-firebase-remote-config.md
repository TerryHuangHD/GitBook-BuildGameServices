# Parse Config 與 Firebase Remote Config 服務介紹

### Parse Config

Parse Config 提供了在 Parse 上存儲 single configuration 來遠程配置應用程式的方法。可簡單的透過 Dashboard 來變更數值，也支援透過 REST API 來修改值

* 取得 Config

```
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  https://YOUR.PARSE-SERVER.HERE/parse/config
```

* 設定 Config，使用 Master Key

```
curl -X PUT \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-Master-Key: ${MASTER_KEY}" \
  -d "{\"params\":{\"winningNumber\":43}}"
  https://YOUR.PARSE-SERVER.HERE/parse/config
```

* 支援的資料格式
  * String
  * Number
  * Date
  * JSONArray
  * JSONObject
  * File
  * GeoPoint


* 限制
  * 100 個數值
  * 總和 128KB

### Firebase Remote Config

Firebase Remote Config 是一個雲端資料服務，可讓您動態的啟用功能、改變數值，不需要使用者重新下載應用程式。Firebase Remote Config 支援針對不同群體提供不同的數值，用來幫助應用進行 A/B 測試

* 支援的群體目標條件

| 條件類型 | 比較法 | Value |
| --- | --- | --- |
| App | == | iOS: CFBundleIdentifier <br> Android: applicationId |
| 版本 | 完全吻合 <br> 包含 <br> 不包含 <br> regular expression | |
| OS 類型 | == | iOS <br> Android |
| 隨機比例 | ≤, > | 0-100 |
| 聽眾群 | == | Google Analytics for Firebase 中定義的聽眾群 |
| 地區、國家 | == | 一個、多個地區或國家 |
| 裝置語言 | == | 一個、多個語言 |
| 屬性 | **文字**： <br> 完全吻合 <br> 包含 <br> 不包含 <br> regular expression <br><br> **數字**： <br> =, ≠, >, ≥, <, ≤ | Google Analytics for Firebase 中定義的使用者屬性 |


* 支援的資料解譯格式
  * Boolean
  * Data(Byte[])
  * Double
  * Long
  * String


* 限制
  * 2000 個數值
  * 100 個條件
  * 總和 500,000 字元