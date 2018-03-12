# 簡介

**「自己打造遊戲後端服務」~~Step by Step~~**，是 LiRise Games 在開發**『呆呆夥伴』**系列遊戲時，後端開發的心得筆記；我們這次的開發過程中，嘗試在有限資源的狀況下，**用簡單的方法來「自建遊戲服務」，提供跨平台的「遊戲體驗」**，希望我們的經驗能對其他開發者有所幫助。後續內容將會依序介紹，各式遊戲相關的 BaaS 和其他外部服務，接著會在此基礎架構下，介紹各種常見的 Game Services 的設計與心得。目標是讓開發者透過介紹，就能簡單的建立自己管理的後端系統

> 作者患有嚴重的懶癌，如果發現偷工減料，歡迎靠邀作者

![](/assets/Simple Layer 169.jpg)

# 大綱介紹

* Infrastructure
  * 介紹常見的 BaaS 服務架設、設定（Parse, Firebase...），整合其他外部服務（Notification, Email, SMS, Time...），構建 Game Services 所需的底層架構
* Game Services
  * 介紹常見的遊戲服務，實作上的各種形式，可能遇到的各種挑戰或考量（Account, Invitation, Achievement, Leaderboard, Queue & Pairing, Turned-Based Multiplayer, Real-time Multiplayer, Saved Games, Event & Quest & Challenges）

# 自己打造遊戲服務

* 簡易的跨平台
  * iOS Game Center 與 Google Play Game Services，由於帳號綁定的關係，大部分發力在各自的平台，支援的服務也略有不同。所以跨平台遊戲，通常是選用完整的第三方服務為主，再輔以各平台服務進行部分的優化。自建遊戲服務便可脫離帳號綁定的約束，在輕量化的遊戲中實現更簡易的跨平台體驗
* 靈活性
  * 資料可有較高的使用彈性，比如：跨產品設計，行銷用途
  * 各遊戲子服務可依照需求，採用互相綁定或各自獨立的設計
* 便於客製化、優化
  * 與泛用型的遊戲服務比較，自建的遊戲服務能實踐更多客製化功能，比如：配對機制、遊戲模式、互動方式
  * 可簡易的在全球各服務區提供在地化的服務，也可針對需求與使用狀況來優化服務
* 資料伺服器自主性
  * 在某些~~（長官任期較長）~~國家，對於資料伺服器位置有所限制
* 缺點
  * 得自行管理機器，處理 Availability, Failover, Optimization, Scaling...等等問題

# 適合對象

* 小型產品、獨立遊戲開發者
* 前 Parse 服務關閉的受害者（淚

# 作者簡介

Terry Huang \(@kmshiori\) ，業餘電動玩家，Overwatch 打不上 3000，求大神帶 \(BattleNet: kmshiori\#3943)。絕地求生吃雞和吃肯德基一樣簡單，歡迎~~（加入戰隊）~~一起遊戲 \(Steam: kmshiori@gmail.com)。或是加個好友認識一下 \(FB: kmshiori@gmail.com)

LiRise Games 是位於台南的 Indie Game Studio，隨時都在徵人中，歡迎對於遊戲開發、企劃、行銷有興趣的人來聊聊，或是直接與我們聯繫 \([service@lirise.com](mailto:service@lirise.com)\)