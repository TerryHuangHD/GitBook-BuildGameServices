# 建立  Parse 服務

### 目錄

* [概述說明](#intro)
* [建立虛擬機](#parse-vm)
* [申請與設定網域對應](#parse-dns)
* [申請與設定 SSL 憑證](#parse-ssl)
* [建立並設定 Parse Server 服務](#parse-server)
* [建立並設定 Parse Dashboard 服務](#parse-dashboard)
* [補充說明](#supply)

### 概述說明 {#intro}

Parse Service 在常見的配置上，包含**「資料庫服務」**用以存放資料，**「Parse 伺服器」**作為服務端口，**「Parse Dashboard」**提供簡單的管理之用。在這次的範例中，將會演示建立以 [Google Cloud Platform（下稱 GCP）](https://cloud.google.com/)的 Compute Engine「虛擬機」為基礎的方式，逐步架設 Parse 所需的**「Parse 伺服器」**、**「Parse Dashboard」**
 > IAAS 服務可依需求、價格選用合適的服務，如：[AWS](https://aws.amazon.com/), [Linode](https://www.linode.com/)
 >
 > **大部分的 IAAS 服務商，通常都有提供免費試用計畫**，比如：[GCP 免費項目](https://cloud.google.com/free/), [AWS 免費方案](https://aws.amazon.com/tw/free/)

### 建立虛擬機 {#parse-vm}

* 在 [GCP 的 Compute Engine](https://console.cloud.google.com/compute) 控制介面中，點擊「建立」，新增 VM Instance

![](/assets/Parse Create.png)

* 設定 Instance 相關資訊
 * 名稱：自行命名
 * 區域：與資料庫同網域（因範例中資料庫伺服器網路預設開放 internal 連線）
 * 機器類型：依需求決定，範例選用微型機器
 * 開機磁碟：依需求決定，範例使用預設磁碟大小
 * 作業系統：範例選用 Ubuntu 14.04 LTS

![](/assets/Parse Setup.png)

* 防火牆設定，開啟允許 HTTP & HTTPS

![](/assets/Parse Firewall.png)

* 點開「管理、磁碟、網路、SSH 金鑰」，切換到「網路」頁籤，在「外部 IP」的選項中，點擊「建立 IP 位址」，填入名稱進行預約，讓此主機在未來即使重新啟動，都能綁定至此外部 IP

![](/assets/Parse External Ip.png)

* 等待建立完成後，便完成了一台虛擬機的啟用。爾後可以在此畫面，管理所有建立的虛擬機。也可透過後方的按鈕，直接透過 SSH 登入到每一台虛擬機

![](/assets/Parse Instance Create.png)

### 申請與設定網域對應 {#parse-dns}

由於安全性的考量，Parse 服務端口建議使用 SSL 連線，因此，必須先申請所想要使用的網域，並設定其 DNS Resource Record。市售的網域註冊服務很多，除了較為特別的網域名稱會被抬高價格之外，大部分的費用都差異不大（[按我查看 Google Domains 價格](https://support.google.com/domains/answer/6010092?hl=en)），建議可以挑選大型服務來購買，比如：[Google Domains](https://domains.google), [GoDaddy](https://godaddy.com)。以下將會演示，在 [noip](https://www.noip.com) 申請免費域名並設定 DNS 的過程

> noip 免費服務只能申請幾個限定網域的子網域，如果是要使用在正式產品，還是建議註冊一個漂亮的網域名稱吧

* 到[官方網站](https://www.noip.com/)，輸入網域並註冊帳號。範例中選了 ddns.net 網域，註冊了 parseServer 的子域名

![](/assets/No IP Create.png)

* 完成帳號註冊與 Email 確認

![](/assets/Confirm No IP Account.png)

* 到[設定畫面](https://my.noip.com/#!/dynamic-dns)，將網域的 IP 位址設定為剛剛建立的 Parse 虛擬機預約的外部 IP

![](/assets/My No IP Hostnames.png)

* 至此，您已經完成了網域以及其 DNS 設定。接下只需靜待 DNS Resource Record 更新，便可透過這個網域來連接設定好的 IP 位址。

* （選用）您可以透過本機端的終端機命令列來確認目前的 DNS 轉址，請記得將命令中的  DOMAIN_NAME 替換成您申請的網址，比如：parseServer.ddns.net

 * Windows
  
 ```
 ping DOMAIN_NAME
 ```
 
 * macOS
 
 ```
 ping -c 4 DOMAIN_NAME
 ```

### 申請與設定 SSL 憑證 {#parse-ssl}

接下來將會演示，如何申請免費的 [Let's Encrypt](https://letsencrypt.org/) SSL 憑證，與設定自動更新的過程。透過 SSH 登入 Parse 虛擬機，然後開始以下的步驟

* 安裝 mini-httpd，用來進行網域所有權驗證
 
 ```
 sudo apt-get install mini-httpd -y
 ```

* 設定 mini-httpd

 * 您可以手動編輯設定檔案，將 /etc/default/mini-httpd 中改成 START=1，以及 /etc/mini-httpd.conf 中改成 host=0.0.0.0。或是透過以下的 script 來自動寫入設定檔案（測試於：mini-httpd_1.19）

 ```
 echo "START=1" | sudo tee /etc/default/mini-httpd
 echo "DAEMON_OPTS=\"-C /etc/mini-httpd.conf\"" | sudo tee -a /etc/default/mini-httpd
 
 echo "host=0.0.0.0" | sudo tee /etc/mini-httpd.conf
 echo "port=80" | sudo tee -a /etc/mini-httpd.conf
 echo "user=nobody" | sudo tee -a /etc/mini-httpd.conf
 echo "chroot" | sudo tee /etc/mini-httpd.conf
 echo "data_dir=/usr/share/mini-httpd/html" | sudo tee -a /etc/mini-httpd.conf
 echo "pidfile=/var/run/mini-httpd.pid" | sudo tee -a /etc/mini-httpd.conf
 ```

* 啟動 mini-httpd 服務
 
 ```
 sudo /etc/init.d/mini-httpd start
 ```

* 此時可透過瀏覽器來測試 mini-httpd 是否正常運作

![](/assets/Mini Httpd.png)

* 增加 Ubuntu 套件來源，過程中需按下［Enter］來同意繼續
 
 ```
 sudo add-apt-repository ppa:certbot/certbot
 ```

* 安裝 Let's Encrypt 服務
 
 ```
 sudo apt-get update
 sudo apt-get install letsencrypt -y
 ```

* 輸入以下命令，來驗證網域所有權，並取得 SSL 憑證

 > 請將 DOMAIN_NAME 替換成您申請的網域名稱，EMAIL 替換成您的聯絡用 Email
 
 ```
 sudo letsencrypt certonly -a webroot --agree-tos --webroot-path=/usr/share/mini-httpd/html -d DOMAIN_NAME -m EMAIL
 ```

* 驗證過程中，會詢問你是否願意分享 Email 給 [Electronic Frontier Foundation](https://zh.wikipedia.org/wiki/%E7%94%B5%E5%AD%90%E5%89%8D%E5%93%A8%E5%9F%BA%E9%87%91%E4%BC%9A)，您可以輸入 Y/N 來同意/不同意。接著便會進入核發程序，成功的話，便會顯示 SSL 存放的位置。至此，您已經擁有您網域 SSL 所需要使用的憑證

 > Let's Encrypt 核發的憑證都只有 3 個月的有效期，將會在[補充說明](#supply)中示範如何自動更新憑證

![](/assets/Parse SSL.png)

* 關閉並刪除 mini-httpd 服務
 
 ```
 sudo /etc/init.d/mini-httpd stop
 sudo apt-get purge mini-httpd* -y
 ```

### 建立並設定 Parse Server 服務 {#parse-server}

* 安裝 Node.js
 
 ```
 curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
 sudo apt-get install nodejs -y
 ```

* 透過 npm 安裝 express-generator 的 package
 
 ```
 sudo npm install express-generator -g
 ```

* 透過 express generator 建立基本的 express app 到 PARSE 資料夾，然後進到該資料夾中完成 npm 安裝

 > PARSE 可替換任意資料夾名稱

 ```
 express PARSE
 cd PARSE
 sudo npm install
 ```

* 繼續在此資料夾中，透過 npm 安裝 parse-server 需要的 package

 ```
 sudo npm install parse-server express
 ```

* 編寫 app.js 檔案，來完成 Parse Server 服務架設在 express 上

 ```
 (to be continued)
 ```

### 建立並設定 Parse Dashboard 服務 {#parse-dashboard}

### 補充說明 {#supply}

* 自動更新 Let's Encrypt SSL 憑證，運用伺服器 cron 系統，在每週日淩晨三點進行憑證更新以及重新啟動 Parse 服務

 > 請注意伺服器時區，預設是 Etc/UTC 時間

 ```
 echo "0 3 * * 7 root /usr/bin/letsencrypt renew" | sudo tee -a /etc/crontab
 echo "0 3 * * 7 root service parse restart" | sudo tee -a /etc/crontab
 ```