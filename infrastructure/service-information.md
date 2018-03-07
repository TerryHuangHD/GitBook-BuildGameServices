# Information 服務

> 資訊服務為良好的使用體驗提供了基礎。產品中常見的資訊有
* Config（設定資訊服務）
* Connectivity（連線資訊服務）
* Time（時間資訊服務）
* Location（地點資訊服務）

**「設定資訊服務」**提供了使用者端或雲端，取得**變動性設定資訊**的便利性。目前在 Parse & Firebase 服務中皆有提供類似的功能，以下將會詳細介紹 **Parse Config** 與 **Firebase Remote Config** 服務

**「連線資訊服務」**大致上可分為兩種面向的需求，一種是使用者端**「網路連線狀態」**的資訊，另一種是**「使用者端與伺服器連線狀態」**資訊，以下將會介紹 Android & iOS 連線狀態取得方法，以及透過 Firebase Realtime Database 監聽與伺服器連線狀態的方法

**「時間服務」**提供了使用者端準確、同步的時間，在很多的服務中扮演重要的角色，以下將會介紹在 Parse 伺服器上透過 Express 提供 Time 服務，以及透過 Firebase Realtime Database 提供 Time 服務

**「地點資訊服務」**依照所需要的精準度，有各種實作方式，但由於隱私權日益受到重視，因此透過手機硬體取得權限與資訊的做法，常常會遇到很多挑戰，況且，大部分產品與遊戲中不需要過於精準的地理位址，因此以下介紹了透過 ip 來取得概略位置的方法，可在**不用取得額外權限**的前提下，取得有用資訊

### 目錄

* [主題：Parse Config 與 Firebase Remote Config 服務介紹](service-information/parse-config-and-firebase-remote-config.md) 
* [主題：Connectivity 取得與監聽](service-information/android-and-ios-network-connectivity.md)
* [主題：Time 服務介紹與實作方法](service-information/express-time-service.md)
* [主題：IP 資料庫能獲得什麼資料](service-information/ip-to-location.md)