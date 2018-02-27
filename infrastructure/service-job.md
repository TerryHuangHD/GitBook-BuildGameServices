# TODO: Job 服務

目前在 Parse 以及 Firebase 服務中，都沒有直接支援 Job 的 Runner，因此需要特別的設計來做。在 Parse 中，可以定義 Job Function，可在 Dashboard 中手動進行命令執行，也會把每次執行的時間與紀錄保存下來，如需要自動化執行，則需要而外設計，將會在範例中介紹透過伺服器 Cron 來實作 Scheduler。在 Firebase 中，則需透過 App Engine Cron 透過 Cloud Pub/Sub 來觸發 Cloud Function，來實作 Job Runner

### 目錄

* Parse Job Function 設定
* Parse Job 透過 Dashboard 檢視與測試