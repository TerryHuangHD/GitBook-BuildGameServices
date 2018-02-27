# Information 服務

> 資訊服務為良好的使用體驗提供了基礎。產品中常見的資訊有
* Config: 全局性設定資訊服務
* Connectivity: 連線資訊服務
* Time: 時間資訊服務
* Location: 地點資訊服務

「全局性設定資訊服務」目前在 Parse & Firebase 中皆有原生提供，提供了使用者端、雲端同步設定與資料的便利性。「連線資訊服務」大致上可分為兩種程度上的需求，一種是「網路連線狀態」的資訊，另一種是「使用者端與伺服器連線狀態」資訊，以下將會分開介紹實作的辦法。「時間服務」提供了使用者端準確、同步的時間，在很多的服務中扮演重要的角色。「地點資訊服務」依照所需要的精準度，也有多種實作方式

### 目錄

* Parse Config 服務
* Firebase Remote Config 服務

* 使用者端 Network Connectivity
* 透過 Firebase 監聽 Server Connectivity
* Time: 確保使用者端準確、同步的時間
* Location: 取得使用者地理資訊