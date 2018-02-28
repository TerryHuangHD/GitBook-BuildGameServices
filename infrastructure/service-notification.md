# Notification 服務

> Game Services 中，設計得當的 Notification 能有效的增加 retention，通常有三種使用情境：**本機端推送**、**雲端個別推送**、**雲端大量推送**
* 本機端推送，可用於已經預知時間與內容的提醒，可完全在無連網情形之下發起通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
* 雲端個別推送，通常是用於非計畫性的小事件，比如：有朋友丟了悄悄話給你
* 雲端大量推送，通常用於不定時的廣播事件，比如：營運公司手動開始了一個活動

在使用 Notification 服務時，通常包含了兩種類型：「雲端推送型通知」與「本機端通知」。在接下來的設定中，將會介紹「雲端推送型通知」與雲端服務平台的設定。想要能使用推送型通知，首先需要在使用者端程式中，設定推送服務，接下來會介紹 Android 平台 Firebase Cloud Messaging（以下簡稱 FCM）以及 iOS 平台 Apple Push Notification service（以下簡稱 APNs）。並會在後來的範例中，介紹 Parse 推送服務設定與測試，以及 Segmentation 的方法

### 目錄

* [設定 Android FCM](#android)
* [設定 iOS APNs](#ios)
* [主題：Parse 推送服務設定與測試](service-notification/parse-push-notification.md)
* [主題：Parse 推送服務 Segmentation](service-notification/parse-push-notification-segmentation.md)
* [主題：Android 本機端推送](service-notification/android-notification-local.md)
* [主題：iOS 本機端推送](service-notification/ios-notification-local.md)

### 設定 Android FCM {#android}

1. 在 root-level build.gradle 加入
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

2. 在 app-level build.gradle 加入 Dependency
```
dependencies {
        // Add Dependency
        compile 'com.google.firebase:firebase-core:11.8.0'
        compile 'com.google.firebase:firebase-messaging:11.8.0'
}
// ADD THIS AT THE BOTTOM
apply plugin: 'com.google.gms.google-services'
```

3. 在 AndroidManifest.xml 新增兩個 Service，名叫 MyFirebaseInstanceIDService 與 MyFirebaseMessagingService，分別處理 FCM 註冊的 Token 更新，以及處理 FCM 收到的訊息，
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

4. 新增 MyFirebaseInstanceIDService.java 檔案，處理 FCM 註冊的 Token 更新
```
@Override
public void onTokenRefresh() {
        String refreshedToken = FirebaseInstanceId.getInstance().getToken();
        // refreshedToken 就是伺服器 push 時所需要的 Token
}
```
> [按我可參考完整範例](https://github.com/firebase/quickstart-android/blob/master/messaging/app/src/main/java/com/google/firebase/quickstart/fcm/MyFirebaseInstanceIDService.java)

5. 新增 MyFirebaseMessagingService.java 檔案，處理 FCM 收到的訊息
```    
@Override
public void onMessageReceived(RemoteMessage remoteMessage) {
        // 將 remoteMessage 內容產生成為 Notification，或是觸發其他邏輯
}
```
> [按我可參考完整範例](https://github.com/firebase/quickstart-android/blob/master/messaging/app/src/main/java/com/google/firebase/quickstart/fcm/MyFirebaseMessagingService.java)


### 設定 iOS APNs {#ios}



