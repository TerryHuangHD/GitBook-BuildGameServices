# Android & iOS Network Connectivity

### Android 透過 ConnectivityManager 確認網路狀態訊息

* 宣告 ConnectivityManager，並讀取出 ActiveNetworkInfo

```
ConnectivityManager cm = (ConnectivityManager)context.getSystemService(Context.CONNECTIVITY_SERVICE);

NetworkInfo activeNetwork = cm.getActiveNetworkInfo();
boolean isConnected = activeNetwork != null && activeNetwork.isConnectedOrConnecting();

```

* 透過 activeNetwork.getType() 判斷網路類型

```
boolean isWiFi = activeNetwork.getType() == ConnectivityManager.TYPE_WIFI;
```

### iOS 透過 Reachability 確認並監聽網路狀態訊息

* 透過 iOS 官方 sample 提供的程式碼，將 Reachability 中 **Reachability.h** 與 **Reachability.m** 加入到自己的專案中

> https://developer.apple.com/library/content/samplecode/Reachability/Introduction/Intro.html

* 透過 currentReachabilityStatus 判斷網路狀態

```
// Router 可連性
Reachability* reach = [Reachability reachabilityForInternetConnection];

if ([reach currentReachabilityStatus] != NotReachable) {
    // Is Connect
}
```

```
//  Host 可連性
Reachability* reach = [Reachability reachabilityWithHostname:@"www.apple.com"];

if ([reach currentReachabilityStatus] != NotReachable) {
    // Is Connect
}
```

* 監聽網路變化

```
[[NSNotificationCenter defaultCenter] addObserver:self
    selector:@selector(appReachabilityChanged:)
    name:kReachabilityChangedNotification
    object:nil];
    
Reachability* reach = [Reachability reachabilityForInternetConnection];
[reach startNotifier];
```

```
(void)appReachabilityChanged:(NSNotification *)notification{
    Reachability *reach = [notification object];
    if([reach isKindOfClass:[Reachability class]]){
        NetworkStatus status = [reach currentReachabilityStatus];
        // Router 檢測類型
        if (reach == self.routerReachability) {
            if (status == NotReachable) {
                NSLog(@"routerReachability NotReachable");
            } else if (status == ReachableViaWiFi) {
                NSLog(@"routerReachability ReachableViaWiFi");
            } else if (status == ReachableViaWWAN) {
                NSLog(@"routerReachability ReachableViaWWAN");
            }
        }
        // Host 檢測類型
        if (reach == self.hostReachability) {
            NSLog(@"hostReachability");
            if ([reach currentReachabilityStatus] == NotReachable) {
                NSLog(@"hostReachability failed");
            } else if (status == ReachableViaWiFi) {
                NSLog(@"hostReachability ReachableViaWiFi");
            } else if (status == ReachableViaWWAN) {
                NSLog(@"hostReachability ReachableViaWWAN");
            }
        }
        
    }
}
```

## 透過 Firebase Realtime Database 監聽網路變化

```
// Android
DatabaseReference connectedRef = FirebaseDatabase.getInstance().getReference(".info/connected");
connectedRef.addValueEventListener(new ValueEventListener() {
  @Override
  public void onDataChange(DataSnapshot snapshot) {
    boolean connected = snapshot.getValue(Boolean.class);
    if (connected) {
      System.out.println("connected");
    } else {
      System.out.println("not connected");
    }
  }

  @Override
  public void onCancelled(DatabaseError error) {
    System.err.println("Listener was cancelled");
  }
});
```

```
// iOS: Objective-c
FIRDatabaseReference *connectedRef = [[FIRDatabase database] referenceWithPath:@".info/connected"];
[connectedRef observeEventType:FIRDataEventTypeValue withBlock:^(FIRDataSnapshot *snapshot) {
  if([snapshot.value boolValue]) {
    NSLog(@"connected");
  } else {
    NSLog(@"not connected");
  }
}];
```

```
// iOS: Swift
let connectedRef = Database.database().reference(withPath: ".info/connected")
connectedRef.observe(.value, with: { snapshot in
  if snapshot.value as? Bool ?? false {
    print("Connected")
  } else {
    print("Not connected")
  }
})
```