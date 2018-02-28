# Notification 服務

> Game Services 中，設計得當的 Notification 能有效的增加 retention，通常有三種使用情境：**本機端推送**、**雲端個別推送**、**雲端大量推送**
* 本機端推送，可用於已經預知時間與內容的提醒，可完全在無連網情形之下發起通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
* 雲端個別推送，通常是用於非計畫性的小事件，比如：有朋友丟了悄悄話給你
* 雲端大量推送，通常用於不定時的廣播事件，比如：營運公司手動開始了一個活動

在使用 Notification 服務時，通常包含了兩種類型：「雲端推送型通知」與「本機端通知」。在接下來的設定中，將會介紹「雲端推送型通知」與雲端服務平台的設定。想要能使用推送型通知，首先需要在使用者端程式中，設定推送服務，接下來會介紹 Android 平台 Firebase Cloud Messaging（以下簡稱 FCM）以及 iOS 平台 Apple Push Notification service（以下簡稱 APNs）。並會在後來的範例中，介紹 Parse 推送服務設定與測試，以及 Segmentation 的方法

### 目錄

* [設定 Android FCM Client](#android-client)
* [取得 Android FCM 外部推送所需資訊](#android-server)
* [設定 iOS APNs Client](#ios-client)
* [取得 iOS APNs 外部推送所需資訊](#ios-server)
* [主題：Parse 推送服務設定與測試](service-notification/parse-push-notification.md)
* [主題：Parse 推送服務 Segmentation](service-notification/parse-push-notification-segmentation.md)
* [主題：Android 本機端推送](service-notification/android-notification-local.md)
* [主題：iOS 本機端推送](service-notification/ios-notification-local.md)

### 設定 Android FCM Client {#android-client}

* 由於 FCM 為 Firebase 服務之一，所以須先建立 Firebase 專案

> https://console.firebase.google.com/

![](/assets/android fcm create project.png)

* 加入 Android 應用程式

![](/assets/android fcm create android app.png)

* 接續提示的步驟，下載並將 google-services.json 拉進專案

![](/assets/android fcm setup android app file.png)

* 接續直接進行 Firebase 以及 FCM 所需的設定，在 root-level build.gradle 加入

```
buildscript {
        // ...
        dependencies {
                // Add
                classpath 'com.google.gms:google-services:3.2.0' // google-services plugin
        }
}
allprojects {
        // ...
        repositories {
                // Add
                maven {
                        url "https://maven.google.com" // Google's Maven repository
                }
        }
}
```

* 在 app-level build.gradle 加入 Dependency

```
dependencies {
        // Add
        compile 'com.google.firebase:firebase-core:11.8.0'
        compile 'com.google.firebase:firebase-messaging:11.8.0'
}
// Add at bottom
apply plugin: 'com.google.gms.google-services'
```

* 在 AndroidManifest.xml 新增兩個 Service，名叫 MyFirebaseInstanceIDService 與 MyFirebaseMessagingService，分別處理 FCM 註冊的 Token 更新，以及處理 FCM 收到的訊息

```
<service
        android:name=".MyFirebaseMessagingService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
</service>
<service
        android:name=".MyFirebaseInstanceIDService">
        <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
</service>
```

* 新增 MyFirebaseInstanceIDService.java 檔案，處理 FCM 註冊的 Token 更新

```
@Override
public void onTokenRefresh() {
        String token = FirebaseInstanceId.getInstance().getToken();
        // token 需傳送到伺服器端紀錄，伺服器透過 token 推送訊息到這個裝置
}
```
> [按我可參考完整範例](https://github.com/firebase/quickstart-android/blob/master/messaging/app/src/main/java/com/google/firebase/quickstart/fcm/MyFirebaseInstanceIDService.java)

* 新增 MyFirebaseMessagingService.java 檔案，處理 FCM 收到的訊息

```    
@Override
public void onMessageReceived(RemoteMessage remoteMessage) {
        // 將 remoteMessage 內容產生成為 Notification，或是觸發其他邏輯
}
```
> [按我可參考完整範例](https://github.com/firebase/quickstart-android/blob/master/messaging/app/src/main/java/com/google/firebase/quickstart/fcm/MyFirebaseMessagingService.java)

* 至此便完成了 client 端設定。未來的章節將會介紹，將 token 送至伺服器保存，讓伺服器透過這個 token 傳送通知至此裝置。（已可透過 Firebase Console 中進行訊息推送的測試）

![](/assets/firebase fcm console android.png)

### 取得 Android FCM 外部推送所需資訊 {#android-server}

* 首先先前往 Firebase Console 並選擇您的專案

> https://console.firebase.google.com/

* 然後選取左上方設定圖樣，選擇專案設定

![](/assets/android fcm server setting.png)

* 然後選取 CLOUD MESSAGING 頁籤，便可獲得 **Sender ID** 以及**伺服器金鑰**

![](/assets/android fcm server setting cloud message.png)

* 之後將會使用 **Sender ID** 以及**伺服器金鑰** 來設定外部推送伺服器

### 設定 iOS APNs Client {#ios-client}

* 開啟專案，在專案設定中啟用 Push Notification

![](/assets/ios apn client app setting.png)

* 嘗試註冊遠端通知

```
- (void)applicationDidFinishLaunching:(UIApplication *)app {
        // 註冊遠端推送服務
        [[UIApplication sharedApplication] registerForRemoteNotifications];
}
```

* 實作函式來監聽註冊的結果

```
- (void)application:(UIApplication *)app didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)devToken {
        NSString * token = [[[[deviceToken description]
                stringByReplacingOccurrencesOfString: @"<" withString: @""]
                stringByReplacingOccurrencesOfString: @">" withString: @""]
                stringByReplacingOccurrencesOfString: @" " withString: @""];
        // 註冊成功，取得 token
        // token 需傳送到伺服器端紀錄，伺服器透過 token 推送訊息到這個裝置
}
 
- (void)application:(UIApplication *)app didFailToRegisterForRemoteNotificationsWithError:(NSError *)err {
        // 註冊失敗
}
```

* 至此便完成了 client 端設定。未來的章節將會介紹，將 token 送至伺服器保存，讓伺服器透過這個 token 傳送通知至此裝置。

### 取得 iOS APNs 外部推送所需資訊 {#ios-server}

* 須先建立 Certificate Signing Request 檔案，稍後會用在申請 push notification 憑證中使用。首先，開啟 Keychain Access，在選單中選擇 Certificate Assistant > Request a Certificate From a Certificate Authority

![](/assets/ios apn keychain access.png)

* 接著填入 Email 以及名稱，然後選擇 Save to disk。便會產生 .certSigningRequest 作為後續的使用

![](/assets/ios apn keychain access create.png)

* 接下來前往 Apple Developer Center 網站註冊一個 iOS App ID（如果已經有 App ID 則跳過此步驟）
 * 填入 Bundle ID
 * 勾選 Push Notifications

> https://developer.apple.com/account/ios/identifier/bundle/create

![](/assets/ios apn app id.png)

![](/assets/ios apn app services.png)

* 選擇建立完成的 App，然後點擊 Edit 進入編輯

![](/assets/ios apn select app.png)

* 前往 Push Notification 區塊，分別針對 Development 以及 Production 都 Create Certificate 並上傳一開始建立的 .certSigningRequest 檔案。完成後會分別獲得 **aps_development.cer** 以及 **aps.cer**

![](/assets/ios apn push configure edit.png)

* 回到本機端，雙擊 **aps_development.cer** 以及 **aps.cer** 將憑證安裝到電腦中

* 開啟 Keychain Access（可能安裝憑證時已經自動打開了），左方選擇 Certificates，右方可以方便找到剛剛安裝的兩個 Push Service 憑證，一個是 Development 另一個是正式環境。分別點右鍵，並選擇 Export，即可分別儲存成 development.p12 以及 production.p12 兩個檔案

![](/assets/ios apn push cert install.png)

* 之後將會使用 **development.p12** 以及 **production.p12** 這兩個檔案來設定外部推送伺服器