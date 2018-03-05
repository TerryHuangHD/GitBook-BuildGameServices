# Parse Live Query 設定

### 伺服器端設定變更

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

### Android Client 設定

* 在 gradle 設定檔案中新增 dependency

```
dependencies {
  compile 'com.parse:parse-livequery-android:1.0.4'
}
```

* 針對特定的 Query 進行 Subscribe

```
ParseQuery<Message> parseQuery = ParseQuery.getQuery(Message.class);

ParseLiveQueryClient parseLiveQueryClient = ParseLiveQueryClient.Factory.getClient();
SubscriptionHandling<ParseObject> subscriptionHandling = parseLiveQueryClient.subscribe(parseQuery);
```

* 監聽所有事件

```
subscriptionHandling.handleEvents(new SubscriptionHandling.HandleEventsCallback<ParseObject>() {
    @Override
    public void onEvents(ParseQuery<ParseObject> query, SubscriptionHandling.Event event, ParseObject object) {
        // HANDLING all events
    }
})
```

* 監聽特定事件

```
subscriptionHandling.handleEvent(SubscriptionHandling.Event.CREATE, new SubscriptionHandling.HandleEventCallback<ParseObject>() {
    @Override
    public void onEvent(ParseQuery<ParseObject> query, ParseObject object) {
        // HANDLING create events
    }
})
```

### iOS Client 設定

* 在 pod 設定檔案中新增 dependency

```
pod 'ParseLiveQuery'
```

* 針對特定的 Query 進行 Subscribe

```
let myQuery = Message.query()!.where(....)
let subscription: Subscription<Message> = Client.shared.subscribe(myQuery)
```

* 監聽所有事件

```
subscription.handleEvent { query, event in
    // HANDLING all events
}
```

* 監聽特定事件

```
subscription.handle(Event.created) { query, object in
    // HANDLING create events
}
```