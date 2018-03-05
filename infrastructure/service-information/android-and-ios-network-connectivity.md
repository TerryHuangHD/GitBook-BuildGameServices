# Android & iOS Network Connectivity

### Android Client 網路狀態訊息

* 透過 ConnectivityManager 確認是否有連上網路

```
ConnectivityManager cm = (ConnectivityManager)context.getSystemService(Context.CONNECTIVITY_SERVICE);

NetworkInfo activeNetwork = cm.getActiveNetworkInfo();
boolean isConnected = activeNetwork != null && activeNetwork.isConnectedOrConnecting();

```

* 透過 activeNetwork.getType() 判斷網路類型

```
boolean isWiFi = activeNetwork.getType() == ConnectivityManager.TYPE_WIFI;
```

### iOS Client 網路狀態訊息

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
