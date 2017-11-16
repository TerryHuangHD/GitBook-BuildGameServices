# 建立  Parse 服務

### 目錄

* [概述說明](#intro)
* [建立資料庫伺服器虛擬機](#db-instance)
* [建立資料庫服務](#db-service)
* [建立並設定 Parse 資料庫](#db-parse)

### 概述說明 {#intro}

* Parse 服務在常用的配置上，會包含「資料庫服務」存放資料，「Parse 伺服器」作為服務端口，「Parse Dashboard」用於簡單的管理之用。
* 在這次的範例中，將會演示建立以 Google Cloud Platform(下稱 GCP) 的 Compute Engine「虛擬機」為基礎的架設方式。可以需求，選用合適的 IAAS 服務商，或考慮現成的 DB 服務，甚至是直接選用 Parse Service Provider

### 建立資料庫伺服器虛擬機 {#db-instance}

* 在 [GCP 的 Compute Engine](https://console.cloud.google.com/compute) 建立資料庫伺服器 VM Instance

![](/assets/Compute Engine Create.png)

* 設定 Instance 相關資訊，如：名稱、區域、機器類型、開機磁碟。這次選用 Ubuntu 16.04 LTS

![](/assets/Compute Engine VM Setup.png)

* 建立完成後便可透過網頁直接開啟 SSH 模擬器來登入主機

![](/assets/Compute Engine Instance Crate.png)
![](/assets/Compute Engine SSH.png)

### 建立資料庫服務 {#db-service}

### 建立並設定 Parse 資料庫 {#db-parse}



