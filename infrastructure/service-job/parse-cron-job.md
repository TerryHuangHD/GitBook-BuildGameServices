# 在 Parse 上透過 Cron 配置常用的 Scheduler

### 建立執行 Cloud Job 的 Script 檔案

  * 編輯檔案，進入編輯模式
  ```
  sudo nano DAILY.sh
  ```

  * 加入 REST API 呼叫 Cloud Job
  ```
  curl -X POST
      -H 'X-Parse-Application-Id: appId'
      -H 'X-Parse-Master-Key: masterKey'
      https://my-parse-server.com/parse/jobs/myDailyJob
  ```
  
  * 編輯完成後按下［control］+［x］離開，然後輸入［y］再鍵入［enter］確定寫入到原檔案

  * 賦予 script 執行權限
  ```
  sudo chmod 755 DAILY.sh
  ```

  * 至此，透過執行此 script 檔案，便呼叫執行 Cloud Job

### 透過 cron 系統來定期呼叫此 script 檔案

* 使用命令，編輯 cron 檔案
```
sudo crontab -e
```

* 在編輯環境中按下 i 進入編輯模式，然後在下方加入一個排程，設定每日 00:00 執行此 script
```
0 0 * * * /path/to/DAILY.sh
```
> 更詳細的 crontab 說明[請參考此文件](http://linux.vbird.org/linux_basic/0430cron.php)

* 編輯完成後按下［esc］進入命令模式，然後輸入 **:wq** 再鍵入［enter］確定寫入並離開

至此，便完成了簡易的 Daily Runner。您可以透過相同的模式，把 Weekly, Hourly 甚至是 Minutely 都實作出來。此後便可以透過 Deploy Cloud Code/Job 來實作您要的功能