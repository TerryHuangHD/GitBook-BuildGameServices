# Parse 服務架設

早期 Parse 以 BaaS 服務出線，提供了非常完整的後端服務，除了基本的資料存取服務之外，甚至包含了管理者介面、雲端程式、伺服器任務....等支援，完整滿足了小型產品、Indie Game 的需求，讓開發者能更專注在遊戲前端的開發。時至今日，Parse 雖然轉移成了 Self-Hosted 服務，但是透過許多外部服務的串接，大部分的功能還是得以實現。

Parse 的服務架構中，基礎上需包含「資料庫服務」、以及「Parse Server 服務」。Parse 架構支援了相當多樣的服務架設方法，以下列舉一些常見的方法。在接下來的範例中，將會演示「全虛擬機架設」，盡可能地提供步驟與說明。

* ### 本機端架設
  本機端架設法，提供了非常方便的方法來架設**「測試環境」**。Parse Server 透過簡易的 CLI 指令便可以在本機端開始服務。對應的資料庫，也提供了本機端的 mongodb runner，能直接架設本機端資料庫測試環境。當然也可以直接連接您的資料庫虛擬機，或是外部 Database as a Service（以下稱 DBaaS）服務
  
* ### 全虛擬機架設
  全虛擬機架設，是最完整的自架服務作法，在 Infrastructure as a Service（以下稱 IaaS）供應商中以虛擬機架設服務，常見於**「上線服務」**，各服務角色通常以 Dedicated Server 甚至是 Cluster 形式存在
  
* ### DBaaS 服務 + Parse 虛擬機
  除了全虛擬架設設法之外，也能將資料庫服務的部分連結外部資料庫服務提供商
  * 資料庫選擇，除了 MongoDB 之外，Parse 也支援了 PostgreSQL
  * 常見的 DBaaS 服務提供商有：[mLab](https://mlab.com/), [Amazon RDS](https://aws.amazon.com/tw/rds/postgresql/)
  
* ### 直接使用第三方 Parse BaaS 服務
  如果您使用在小型的專案中，也可以考慮提供完整 Parse BaaS 的服務供應商
  * 常見的 Parse SaaS 服務商有：[SashiDo](https://www.sashido.io/), [Back4App](https://www.back4app.com/)
  
* ### Docker
  Parse 以及 MongoDB 都有官方提供的 Docker Image 提供選用
  * [MongoDB Docker Image](https://github.com/docker-library/mongo)
  * [Parse Docker Image](https://hub.docker.com/r/parseplatform/parse-server/) 