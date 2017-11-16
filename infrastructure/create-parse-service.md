# 建立  Parse 服務

### 目錄

* [概述說明](#intro)
* [建立資料庫伺服器虛擬機](#db-instance)
* [建立資料庫服務](#db-service)
* [建立並設定 Parse 資料庫](#db-parse)
* [補充說明](#supply)

### 概述說明 {#intro}

Parse 服務在常用的配置上，會包含「資料庫服務」存放資料，「Parse 伺服器」作為服務端口，「Parse Dashboard」提供簡單的管理之用。在這次的範例中，將會演示建立以 [Google Cloud Platform（下稱 GCP）](https://cloud.google.com/)的 Compute Engine「虛擬機」為基礎的架設方式。
* IAAS 服務可依需求、價格選用合適的服務，如：[AWS](https://aws.amazon.com/), [Linode](https://www.linode.com/)
* 資料庫的部分，目前 Parse 已支援 MongoDB & PostgreSQL。也可考慮現成的 DB 服務，如：[mLab](https://mlab.com/)
* 求方便的話，可直接選用 Parse Service Provider，如：[SashiDo](https://www.sashido.io/), [Back4App](https://www.back4app.com/)

### 建立資料庫伺服器虛擬機 {#db-instance}

* 在 [GCP 的 Compute Engine](https://console.cloud.google.com/compute) 控制介面中，點擊「建立」，新增 VM Instance

![](/assets/Compute Engine Create.png)

* 設定 Instance 相關資訊
 * 名稱：自行命名
 * 區域：可參考 Google 提供的[服務地點說明](https://cloud.google.com/about/locations/)來決定，亞太地區有：新加坡(asia-southeast1)	、台灣(asia-east1)、東京(asia-northeast1)、雪梨(australia-southeast1)
 * 機器類型：依需求決定
 * 開機磁碟：可更改作業系統映像檔案，範例選用 Ubuntu 16.04 LTS

![](/assets/Compute Engine VM Setup.png)

* 等待建立完成後，便可透過網頁直接開啟 SSH 模擬器來登入主機

![](/assets/Compute Engine Instance Crate.png)
![](/assets/Compute Engine SSH.png)

### 建立資料庫服務 {#db-service}

### 建立並設定 Parse 資料庫 {#db-parse}

### 補充說明 {#supply}
