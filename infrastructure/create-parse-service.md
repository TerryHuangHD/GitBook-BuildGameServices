# 建立  Parse 服務

### 目錄

* [概述說明](#intro)
* [建立資料庫伺服器虛擬機](#db-instance)
* [建立資料庫服務](#db-service)
* [建立並設定 Parse 資料庫](#db-parse)
* [建立 Parse 伺服器虛擬機](#parse-instance)
* [建立並設定 Parse Server 服務](#parse-server)
* [建立並設定 Parse Dashboard 服務](#parse-dashboard)
* [補充說明](#supply)

### 概述說明 {#intro}

Parse 服務在常用的配置上，會包含「資料庫服務」存放資料，「Parse 伺服器」作為服務端口，「Parse Dashboard」提供簡單的管理之用。在這次的範例中，將會演示建立以 [Google Cloud Platform（下稱 GCP）](https://cloud.google.com/)的 Compute Engine「虛擬機」為基礎的方式，架設完整的服務。當然，您也可以選擇您喜好的架設服務方式：
* IAAS 服務可依需求、價格選用合適的服務，如：[AWS](https://aws.amazon.com/), [Linode](https://www.linode.com/)
* 資料庫的部分，目前 Parse 已支援 MongoDB & PostgreSQL。也可考慮現成的 DB 服務，如：[mLab](https://mlab.com/), [Amazon RDS](https://aws.amazon.com/tw/rds/postgresql/)
* 目前也有服務商直供 Parse 服務，如：[SashiDo](https://www.sashido.io/), [Back4App](https://www.back4app.com/)
* [MongoDB](https://github.com/docker-library/mongo) 與 [Parse](https://hub.docker.com/r/parseplatform/parse-server/) 目前也都有 Docker Image 供選用

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

> 目前 Parse Server [支援 MongoDB 2.6.X, 3.0.X or 3.2.X](http://docs.parseplatform.org/parse-server/guide/#prerequisites)，這次選用 3.2.7 版來安裝。

> MongoDB 針對 LTS 版本的 Ubuntu 有長期的支援，如: 12.04 LTS (precise), 14.04 LTS (trusty), 16.04 LTS (xenial)

* Import MongoDB public GPG Key

{% codetabs name="Python", type="py" -%}
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
{%- endcodetabs %}

* Create a list file for MongoDB

{% codetabs name="Python", type="py" -%}
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
{%- endcodetabs %}

### 建立並設定 Parse 資料庫 {#db-parse}

### 建立 Parse 伺服器虛擬機 {#parse-instance}

### 建立並設定 Parse Server 服務 {#parse-server}

### 建立並設定 Parse Dashboard 服務 {#parse-dashboard}

### 補充說明 {#supply}

SQL Security
transparent_hugepage

