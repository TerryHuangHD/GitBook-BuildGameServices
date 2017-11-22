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

Parse Service 在常見的配置上，包含**「資料庫服務」**用以存放資料，**「Parse 伺服器」**作為服務端口，**「Parse Dashboard」**提供簡單的管理之用。在這次的範例中，將會演示建立以 [Google Cloud Platform（下稱 GCP）](https://cloud.google.com/)的 Compute Engine「虛擬機」為基礎的方式，逐步架設 Parse 所需的**「Parse 伺服器」**、**「Parse Dashboard」**。當然，您也可以選擇其他選項：
* IAAS 服務可依需求、價格選用合適的服務，如：[AWS](https://aws.amazon.com/), [Linode](https://www.linode.com/)
* **大部分的 IAAS 服務商，通常都有提供免費試用計畫**，比如：[GCP 免費項目](https://cloud.google.com/free/), [AWS 免費方案](https://aws.amazon.com/tw/free/)
* 網路上直供 Parse 服務商，如：[SashiDo](https://www.sashido.io/), [Back4App](https://www.back4app.com/)
* Parse 有官方的 [Docker Image](https://hub.docker.com/r/parseplatform/parse-server/) 供選用

### 建立虛擬機 {#parse-vm}

* 在 [GCP 的 Compute Engine](https://console.cloud.google.com/compute) 控制介面中，點擊「建立」，新增 VM Instance

![](/assets/Parse Create.png)

* 設定 Instance 相關資訊
 * 名稱：自行命名
 * 區域：可參考 Google 提供的[服務地點說明](https://cloud.google.com/about/locations/)來決定，亞太地區 Cloud Compute 可選擇：新加坡(asia-southeast1)	、台灣(asia-east1)、東京(asia-northeast1)、雪梨(australia-southeast1)
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

由於安全性的考量，Parse 服務端口建議使用 SSL 連線，因此，必須先申請並設定所想要使用的網域。市售的網域註冊服務很多，除了特別的網域之外，大部分的費用都大同小異，建議可以挑選大型服務來購買，比如：[Google Domains](https://domains.google), [GoDaddy](https://godaddy.com)。以下將會演示，在 [noip](https://www.noip.com) 申請免費域名並設定 DNS 的過程

> noip 免費服務只能申請幾個限定網域的子網域，如果是要使用在正式產品，還是建議註冊一個漂亮的網域名稱吧

* 到[官方網站](https://www.noip.com/)，註冊帳號與網域

![](/assets/No IP Create.png)

* 完成確認信

![](/assets/Confirm Your No IP Account   kmshiori gmail.com   Gmail.png)

* 到[設定畫面](https://my.noip.com/#!/dynamic-dns)，將網域的 IP 位址設定為剛剛建立的 Parse 虛擬機預約的外部 IP

![](/assets/My No IP Hostnames.png)



### TEST {#TEST}