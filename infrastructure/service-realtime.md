# Realtime 服務

> 即時服務在有些遊戲中，扮演了非常關鍵的角色，比如：即時賽局的匹配、即時對弈...等等；對於一般遊戲也能增加互動性：比如：即時互動、聊天...等等

在 Parse 服務平台中，Realtime 服務需透過 Live Query 來達成，支援 Android 與 iOS 雙平台，可針對 Query 進行監聽。在 Firebase 平台中，Realtime Database 以及 Cloud Firestore 都原生提供了 Realtime 服務，廣泛地支援了多個平台和語言，除了都可針對特定資料進行監聽服務之外，Firestore 也支援了 Query 監聽

### 目錄

* [Parse Live Query 設定與介紹](#parse-live-query)
* [監聽 Firebase Realtime Database](#firebase-realtime)
* [監聽 Firebase Cloud Firestore](#firebase-firestore)

### Parse Live Query 設定與介紹 {#parse-live-query}

Parse Query 是 Parse 的關鍵功能之一。它允許使用者指定某些條件來取得想要的資料。但是，Parse Query 僅支持 Pull Mode，不適用於需要 realtime 服務的程式。因此 Parse 推出了 Parse LiveQuery，能夠讓您直接訂閱原本的 Parse Query，一旦訂閱之後，當 Parse Query 的匹配成果出現變動，服務器就會主通通知客戶端

* 支援事件
  * create: 新增了一個的 Parse Object 符合 Parse Query 條件
  * enter: 原本存在的 Parse Object 經過更新之後 符合 Parse Query 條件
  * update: 原本符合 Parse Query 條件的 Parse Object 更新後也符合 Parse Query 條件
  * leave: 原本符合 Parse Query 條件的 Parse Object 更新後不符合 Parse Query 條件
  * delete: 刪除了原本符合 Parse Query 條件的 Parse Object

* 伺服器端設定變更

```
{
  "appId": 'my_app_id',
  "masterKey": 'my_master_key',
  // ...
  "liveQuery": {
    "classNames": ['Test', 'TestAgain']  // Live Query 要支援的 Class
  }
}
```

* 除此之外，還需要把掛載 Parse 的 http(s)Server 拿來創建一個 Live Query Server

```
// 原本 Express
let httpServer = require('http').createServer(app);
httpServer.listen(port);

// 新增 Live Query Server
var parseLiveQueryServer = ParseServer.createLiveQueryServer(httpServer);
```

> LiveQuery 服務器的 ws protocol 會沿用 http(s)Server 監聽的主機名和端口。例如，如果 http(s)Sever 正在偵聽 localhost:8080，則 LiveQuery 服務器的 ws protocol 為 ws://localhost:8080/

### 監聽 Firebase Realtime Database {#firebase-realtime}

### 監聽 Firebase Cloud Firestore {#firebase-firestore}