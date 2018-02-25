# API 服務

在 Parse 與 Firebase 的平台中，分別有 Cloud Code 與 Cloud Functions 的工具，用來擴充後端伺服器能力。我們接著把他們從「部署與偵錯」、「支援功能」...等等各方面進行簡單的比較

### 部署與偵錯

| Cloud Code | Cloud Functions |
| --- | --- |
| 需手動進行驗證、部署，並重啟 Parse Service 來作動 Cloud Code | 以 firebase-tools 完成驗證、部署、生效 |
| 可在本機端建立臨時的 Parse Service 環境，可透過 Dashboard 的 API Console 或是透過外部工具模擬 Trigger 進行測試 | 可透過 Cloud Functions shell 模擬各種 Trigger 與資料，或是透過 firebase serve 指令來模擬 HTTPS functions |

### 支援功能

|  | Cloud Code | Cloud Functions |
| --- | --- | --- |
| Data Trigger | BeforeSave<br>AfterSave<br>BeforeDelete<br>AfterDelete | onCreate<br>onUpdate<br>onDelete<br>onWrite |
| Find Trigger | BeforeFind | X |
| Authentication Trigger | 可透過監聽 User Class 來達成 | onCreate<br>onDelete |
| Analytics Trigger | X | Google Analytics event |
| Crashlytics Trigger | X | onNewDetected<br>onRegressed<br>onVelocityAlert |
| Storage Trigger | X | upload, update, delete 檔案或是資料夾 |
| Pub/Sub Trigger | X | onPublish |
| HTTP Trigger | O | O |
| External | Webhook | X |

### 範例：在 Parse 服務中架構簡易的 Deploy 機制

1. 申請建立一個 Git private repository

  ![](/assets/bitbucket private repository.png)
  
  > 需要免費 private repository，可以考慮  Bitbucket

2. 在 Development 環境開發，並將 Cloud Code Commit 到此 repository

3. 透過 Git 把將 Cloud Code deploy 到 Parse Server

4. 建立 Script 檔案

5. 建立一個端口來觸發此 Script
