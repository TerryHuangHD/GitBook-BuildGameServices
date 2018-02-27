# 在 Parse 服務中架構簡易的 Cloud Code 部署機制

### 申請建立一個 Git private repository

  ![](/assets/bitbucket private repository.png)
  
  > 需要免費 private repository，可以考慮  Bitbucket

### 在 Development 環境開發，並將 Cloud Code Commit 到此 repository

### 透過 Git 把將 Cloud Code 部署到 Parse Server

  * 登入 Parse Server 主機，移到 Parse 資料夾
  * 將 repository 直接部署至 cloud 資料夾（記得將 Git 位址改成您的位置）
  ```
  sudo git clone https://USER@GIT_SERVER/REPOSITORY.git cloud
  ```
  
  * 重啟 Parse 服務
  ```
  sudo service PARSE restart
  ```

### 建立部署用的 Script 檔案

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

### 建立一個端口來觸發此 Script

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
  
  * 往後，每當您交付了 Cloud Code 程式之後，便可透過簡單的指令來完成部署（記得將網址替換為您的網域）
  ```
  https://parseServer.ddns.net/deploy
  ```