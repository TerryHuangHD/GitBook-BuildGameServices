# API 服務

在 Parse 與 Firebase 的平台中，分別有 Cloud Code 與 Cloud Functions 的工具，用來擴充後端伺服器能力。

### 部署與偵錯

| Cloud Code | Cloud Functions |
| --- | --- |
| 需手動進行驗證、部署，並重啟 Parse Service 來作動 Cloud Code | 以 firebase-tools 完成驗證、部署、生效 |
| 可在本機端建立臨時的 Parse Service 環境，可透過 Dashboard 的 API Console 或是透過外部工具模擬 Trigger 進行測試 | 可透過 Cloud Functions shell 模擬各種 Trigger 與資料，或是透過 firebase serve 指令來模擬 HTTPS functions |


### 支援功能

|  | Cloud Code | Cloud Functions |
| --- | --- | --- |
| 常用 Trigger | BeforeSave<br>AfterSave<br>BeforeDelete<br>AfterDelete | onCreate<br>onUpdate<br>onDelete<br>onWrite |
| 其他 Trigger | | |


* 環境設定
* 編輯與部屬
* Hint: Dashboard Test