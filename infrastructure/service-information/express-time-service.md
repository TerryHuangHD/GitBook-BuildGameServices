# Time 服務介紹與實作方法

### 透過第三方 API

http://www.convert-unix-time.com/api?timestamp=now

### 透過 Express 直接在 Parse Server 提供 Time 服務

```
var app = express();
// ...
app.use('/time', function(req, res){
    res.end("" + new Date().getTime());
});
```

### 透過 Firebase Realtime Database 取得 Server Time


