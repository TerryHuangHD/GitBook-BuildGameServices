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
  * ParseFile
  * ParseGeoPoint


* 限制
  * 100 個數值
  * 總和 128KB

### Firebase Remote Config