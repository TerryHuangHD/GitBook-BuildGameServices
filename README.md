# This Book Is Under Construction

---

# 簡介

這個系列的名字為「Backend of Indie Game: 自建獨立遊戲服務」，是 Lirise Games 團隊這次開發遊戲『呆呆戰學校』的心得筆記；和其他 Indie Game 開發者一樣，我們這次的開發過程中，嘗試在資源有限的狀況下，用簡易的方法來建立跨平台的「線上遊戲服務」。希望我們的經驗能對其他 Indie Game 開發者有所幫助。

後續內容將使用 Parse 服務為中心，進階整合其他服務（Notification, Real-time, SMS...）作為最基礎，接著會在此基礎架構下，介紹各種常見的 Game Services 的設計與心得。

p.s. 作者患有嚴重的懶癌，所以可能很多內容會以~~（偷工減料）~~簡單的的方式來呈現，如果需要詳細說明，請告訴我

![](/assets/Simple Layer.jpg)

# 大綱介紹

* Infrastructure
  * 依照步驟簡單的建立所需要的基礎服務，包含 Parse Service 架設，以及其他服務的設定，如：API, Realtime, Notification, Job, Email, SMS, Hosting
* Game Services
  * 介紹常見的遊戲服務，實作上遇到的各種挑戰或考量。包含：Account, Invitation, Achievement, Leaderboard, Queue & Pairing, Turned-Based Multiplayer, Real-time Multiplayer, Saved Games, Event & Quest & Challenges
* General System
  * 介紹一些泛用的基礎系統設計，可以方便用來開發成各種想要的遊戲服務：如：Security Tunnel, Counter System, Tag System, Storage System

# 自建遊戲服務的優點

* 簡易的跨平台實現
  * iOS Game Center 與 Google Play Game Services，由於帳號綁定的關係，大部分發力在各自的平台，支援的服務也略有不同。所以跨平台遊戲，通常是選用完整的第三方服務為主，再輔以各平台服務進行部分的優化
* 資料伺服器自主性
  * 自行擁有資料庫伺服器，可有較高的使用彈性，比如：跨產品設計，行銷用途
  * 在某些~~（強國）~~國家，對於資料伺服器位置有所限制
* 便於客製化、優化
  * 與泛用型的遊戲服務比較，自建的遊戲服務能實踐更多客製化功能，比如：配對機制、遊戲模式、互動方式
  * 可簡易的在全球各服務區提供在地化的服務
  * 可簡易針對需求提供最優化的服務
* ~~離職的時候可以體驗 rm -rf 一鍵清空伺服器的快感~~

# 自建遊戲服務的缺點

* 得自行管理機器，處理 Failover, Optimization, Scaling...等問題
* ~~來來來，喝杯酒我告訴你~~

# 適合對象

* 小型產品、獨立遊戲開發者
* 前 Parse 服務關閉的受害者（淚

# 作者簡介

Terry Huang \(@kmshiori\) ，業餘電動玩家，Overwatch 打不上 3000，絕地求生還沒吃過雞，求大神帶 \(BattleNet: kmshiori\#3943, Steam: kmshiori@gmail.com)。

LiRise Games 是位於台南的 Indie Game Studio，隨時都在徵人中，歡迎對於遊戲開發、企劃、行銷有興趣的人來聊聊，或是直接與我們聯繫 \([service@lirise.com](mailto:service@lirise.com)\)

# 書中尚不包含的內容

* Docker/k8s
* SQL/AP Sharding
* Database/Server HA
* ~~黃金屋，顏如玉~~