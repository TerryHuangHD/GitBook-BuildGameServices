# Information 服務

> 資訊服務為良好的使用體驗提供了基礎。產品中常見的資訊有
* Config（設定資訊服務）
* Connectivity（連線資訊服務）
* Time（時間資訊服務）
* Location（地點資訊服務）

「設定資訊服務」提高了使用者端或雲端，取得變動性設定資訊的便利性。目前在 Parse & Firebase 中皆有提供，以下將會詳細介紹 Parse Config 與 Firebase Remote Config 服務

「連線資訊服務」大致上可分為兩種程度上的需求，一種是使用者端「網路連線狀態」的資訊，另一種是「使用者端與伺服器連線狀態」資訊，以下將會分開介紹實作的辦法

「時間服務」提供了使用者端準確、同步的時間，在很多的服務中扮演重要的角色

「地點資訊服務」依照所需要的精準度，有各種實作方式；由於隱私權日益受到重視，因此透過手機端硬體取得權限與資訊的做法，常會遇到很多挑戰，且大部分產品與遊戲中不需要過於精準的位址，因此以下介紹了透過 ip 來取得概略位置的方法，可在不用取得而外權限同意的前提下，取得有用資訊

### 目錄

* Parse Config 與 Firebase Remote Config 服務
* Android & iOS Network Connectivity
* 透過 Firebase Realtime Database 監聽 Server Connectivity
* 透過 Express 提供 Time 服務
* 透過 Firebase Realtime Database 提供 Time 服務
* 透過 ip 取得概略地理位置資訊