# Time 服務介紹與實作方法

## 透過第三方 API

市面上有眾多提供時間的 API，大部分頭提供有 REST API 可供存取，以下是一家免費服務商的範例，透過 API 取得目前的 Unixtimestamp

* 送出請求

```text
http://www.convert-unix-time.com/api?timestamp=now
```

* 取得回應

```text
{  
   "localDate":"Monday 5th March 2018 02:22:11 PM",
   "utcDate":"Monday 5th March 2018 02:22:11 PM",
   "format":"l jS F Y h:i:s A",
   "returnType":"json",
   "timestamp":1520259731,
   "timezone":"UTC",
   "daylightSavingTime":false,
   "url":"http:\/\/www.convert-unix-time.com?t=1520259731"
}
```

## 透過 Express 直接在 Parse Server 提供 Time 服務

若您架設有 Parse 伺服器，也可以透過伺服器直接掛載一個端口來讓使用者取得伺服器時間

* 將 /time 掛載時間值回傳

```text
var app = express();
// ...
app.use('/time', function(req, res){
    res.end("" + new Date().getTime());
});
```

## 透過 Firebase Realtime Database 取得 Server Time

若您的服務架設在 Firebase Realtime Database 之上，您也可以透過伺服器 Clock Skew 功能，來取得本地端時間與伺服器端時間的時間差，即可推估精準的伺服器時間

```text
// Android
DatabaseReference offsetRef = FirebaseDatabase.getInstance().getReference(".info/serverTimeOffset");
offsetRef.addValueEventListener(new ValueEventListener() {
  @Override
  public void onDataChange(DataSnapshot snapshot) {
    double offset = snapshot.getValue(Double.class);
    double estimatedServerTimeMs = System.currentTimeMillis() + offset;
  }

  @Override
  public void onCancelled(DatabaseError error) {
    System.err.println("Listener was cancelled");
  }
});
```

```text
// iOS: Objective-c
FIRDatabaseReference *offsetRef = [[FIRDatabase database] referenceWithPath:@".info/serverTimeOffset"];
[offsetRef observeEventType:FIRDataEventTypeValue withBlock:^(FIRDataSnapshot *snapshot) {
  NSTimeInterval offset = [(NSNumber *)snapshot.value doubleValue];
  NSTimeInterval estimatedServerTimeMs = [[NSDate date] timeIntervalSince1970] * 1000.0 + offset;
  NSLog(@"Estimated server time: %0.3f", estimatedServerTimeMs);
}];
```

```text
// iOS: Swift
let offsetRef = Database.database().reference(withPath: ".info/serverTimeOffset")
offsetRef.observe(.value, with: { snapshot in
  if let offset = snapshot.value as? TimeInterval {
    print("Estimated server time in milliseconds: \(Date().timeIntervalSince1970 * 1000 + offset)")
  }
})
```

