# API 服務

在 Parse 與 Firebase 的平台中，分別有 Cloud Code 與 Cloud Functions 的工具，用來擴充後端伺服器能力。我們接著把他們從「部署與偵錯」、「支援功能」...等等各方面進行簡單的比較。並會在後來的範例中，展示一個簡單的 Parse Cloud Code 部署方法。

### 目錄

* [部署與偵錯](#deploy)
* [支援功能](#function)
* [範例：在 Parse 服務中架構簡易的 Cloud Code 部署機制](#parse_deploy)

### 部署與偵錯 {#deploy}

| Cloud Code | Cloud Functions |
| --- | --- |
| 需手動進行驗證、部署，並重啟 Parse Service 來作動 Cloud Code | 以 firebase-tools 完成驗證、部署、生效 |
| 可在本機端建立臨時的 Parse Service 環境，可透過 Dashboard 的 API Console 或是透過外部工具模擬 Trigger 進行測試 | 可透過 Cloud Functions shell 模擬各種 Trigger 與資料，或是透過 firebase serve 指令來模擬 HTTPS functions |

### 支援功能 {#function}

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

### 範例：在 Parse 服務中架構簡易的 Cloud Code 部署機制 {#parse_deploy}

1. 申請建立一個 Git private repository

  ![](/assets/bitbucket private repository.png)
  
  > 需要免費 private repository，可以考慮  Bitbucket

2. 在 Development 環境開發，並將 Cloud Code Commit 到此 repository

3. 透過 Git 把將 Cloud Code 部署到 Parse Server

  * 登入 Parse Server 主機，移到 Parse 資料夾
  * 將 repository 直接部署至 cloud 資料夾（記得將 Git 位址改成您的位置）
  ```
  sudo git clone https://USER@GIT_SERVER/REPOSITORY.git cloud
  ```
  
  * 重啟 Parse 服務
  ```
  sudo service PARSE restart
  ```
4. 建立部署用的 Script 檔案

  * 編輯檔案，進入編輯模式
  ```
  sudo nano DEPLOY.sh
  ```

  * 加入 前往 Cloud Code Folder 指令
  ```
  cd /PATH/TO/PARSE/cloud
  ```

  * 加入 cloud code pull 指令（記得將 Git 位址改成您的位置，並加上密碼）
  ```
  sudo git pull https://USER:PASSWORD@GIT_SERVER/REPOSITORY.git master
  ```

  * 加入 Parse 服務重開指令
  ```
  sudo service PARSE restart
  ```

  * 編輯完成後按下［control］+［x］離開，然後輸入［y］再鍵入［enter］確定寫入到原檔案

  * 賦予 script 執行權限
  ```
  sudo chmod 755 DEPLOY.sh
  ```

  * 至此，透過執行此 script 檔案，便可完成 pull 更新 Cloud Code 並重啟服務來作動
5. 建立一個端口來觸發此 Script

  * 編輯 PARSE 的 express app
  ```
  sudo nano PARSE/app.js
  ```

  * 把端口設定加入對應位置（app 為 express instance）
  ```
  app.use('/deploy', function(req, res) {
    var exec = require('child_process').exec;
    var cmd = 'DEPLOY.sh';
    exec(cmd, function(error, stdout, stderr) {});
    res.send('DEPLOY');
  });
  ```
  * 編輯完成後按下［control］+［x］離開，然後輸入［y］再鍵入［enter］確定寫入到原檔案

  * 重啟服務
  ```
  sudo service PARSE restart
  ```
  
  * 此時便可透過網頁端口來觸發伺服器端的更新與作動（記得將網址替換為您的網域）
  ```
  https://parseServer.ddns.net/deploy
  ```
6. 至此，您便可以在更新 Cloud Code 之後，透過簡單的指令來完成部署