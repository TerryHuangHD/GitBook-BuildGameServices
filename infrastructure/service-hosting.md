# TODO: Hosting 服務

> Hosting 提供了檔案形式的資源取得方式，在 Game Services 中最常見用途有：遊戲通用的資源，遊戲的更新檔案、使用者上傳的照片...等等。依照需求有各種實作和優化方式



> 如果是輕度使用，比如遊戲存放個人圖示，可選擇與 Parse Sever 透過 Adapter 直接嫁接 GridFS，或是透過伺服器直供。如果是開放型、大型靜態資料，建議可以做獨立的檔案伺服架構，或是簡單選用 GCS/S3 + CDN

### 目錄

* Parse File Adapter Setting
* 範例：透過 Express App 直接在 Server 提供 Hosting 服務
* 其他 Hosting 服務

### TODO