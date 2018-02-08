# Setup: Hosting

### Menu

* Service Provider
* Parse Adapter Setting
* Hint: Simple Static Hosting with Parse Service

> 範例選用：Parse Server 嫁接 GridStore

如果是輕度使用，比如遊戲存放個人圖示，可選擇與 Parse Sever 透過 Adapter 直接嫁接 GridFS，或是透過伺服器直供。如果是開放型、大型靜態資料，建議可以做獨立的檔案伺服架構，或是簡單選用 GCS/S3 + CDN