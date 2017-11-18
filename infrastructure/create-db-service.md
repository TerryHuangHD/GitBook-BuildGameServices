# 建立資料庫服務

### 目錄

* [概述說明](#intro)
* [建立資料庫伺服器虛擬機](#db-instance)
* [建立資料庫服務](#db-service)
* [啟用資料庫身份驗證](#db-service-enable-auth)
* [建立並設定 Parse 資料庫](#db-parse)
* [其他事項](#other)

### 概述說明 {#intro}

Parse Service 在常見的配置上，包含「資料庫服務」用以存放資料，「Parse 伺服器」作為服務端口，「Parse Dashboard」提供簡單的管理之用。在這次的範例中，將會演示建立以 [Google Cloud Platform（下稱 GCP）](https://cloud.google.com/)的 Compute Engine「虛擬機」為基礎的方式，逐步架設 Parse 所需的「資料庫服務」。當然，您也可以選擇喜好的其他選項：
* IAAS 服務可依需求、價格選用合適的服務，如：[AWS](https://aws.amazon.com/), [Linode](https://www.linode.com/)
* 資料庫選擇，除了 MongoDB 之外，Parse 也支援了 PostgreSQL
* 市面上也有現成的 DB 服務提供商，如：[mLab](https://mlab.com/), [Amazon RDS](https://aws.amazon.com/tw/rds/postgresql/)
* [MongoDB](https://github.com/docker-library/mongo) 有官方的 Docker Image 供選用

### 建立資料庫伺服器虛擬機 {#db-instance}

* 在 [GCP 的 Compute Engine](https://console.cloud.google.com/compute) 控制介面中，點擊「建立」，新增 VM Instance

![](/assets/Compute Engine Create.png)

* 設定 Instance 相關資訊
 * 名稱：自行命名
 * 區域：可參考 Google 提供的[服務地點說明](https://cloud.google.com/about/locations/)來決定，亞太地區 Cloud Compute 可選擇：新加坡(asia-southeast1)	、台灣(asia-east1)、東京(asia-northeast1)、雪梨(australia-southeast1)
 * 機器類型：依需求決定，範例選用微型機器
 * 開機磁碟：依需求決定，範例使用預設磁碟大小
 * 作業系統：範例選用 Ubuntu 14.04 LTS

![](/assets/Compute Engine VM Setup.png)

* 等待建立完成後，便可透過網頁直接開啟 SSH 模擬器來登入主機

![](/assets/Compute Engine Instance Crate.png)
![](/assets/Compute Engine SSH.png)

### 建立資料庫服務 {#db-service}

目前 Parse Server [支援 MongoDB 2.6.X, 3.0.X or 3.2.X](http://docs.parseplatform.org/parse-server/guide/#prerequisites)，這次演示選用 3.2 版來安裝。MongoDB 針對 LTS 版本的 Ubuntu 有長期的支援，如: 12.04 LTS (precise), 14.04 LTS (trusty), 16.04 LTS (xenial)。接下來透過以下命令在 SSH 安裝 MongoDB

* 匯入 MongoDB GPG 公開金鑰

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
```

* 建立 MongoDB 檔案清單

```
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
```

* 更新本地端套件資料庫

```
sudo apt-get update
```

* 安裝 MongoDB 套件

```
sudo apt-get install -y mongodb-org=3.2.17 mongodb-org-server=3.2.17 mongodb-org-shell=3.2.17 mongodb-org-mongos=3.2.17 mongodb-org-tools=3.2.17
```

* 將 MongoDB 鎖定目前版本

```
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```

### 啟用資料庫身份驗證 {#db-service-enable-auth}

* 登入本地端資料庫

```
mongo 127.0.0.1
```

* 切換至 admin 資料庫

```
use admin
```

* 新增最高管理員帳號。如果想知道更多關於預設的資料庫角色，可[到此觀看](https://docs.mongodb.com/manual/reference/built-in-roles/)

```
db.createUser(
  {
    user: "名稱",
    pwd: "密碼",
    roles: [ { role: "dbOwner", db: "admin" } ]
  }
)
```

* 將資料庫授權於此帳號

```
db.auth("帳號", "密碼")
```

* 離開資料庫連線，回到主機 SSH

```
exit
```

* 編輯 MongoDB 設定檔案，將預設值設定為啟用身份驗證

```
echo "security:" | sudo tee -a /etc/mongod.conf
echo " authorization: \"enabled\"" | sudo tee -a /etc/mongod.conf
```

* 重新啟動資料庫服務

```
sudo service mongod restart
```

### 建立並設定 Parse 資料庫 {#db-parse}

* 以建立的管理者登入資料庫服務

```
mongo 127.0.0.1 -u "test" -p "12345" --authenticationDatabase "admin"
```

### 補充說明 {#other}

SQL Security
transparent_hugepage

