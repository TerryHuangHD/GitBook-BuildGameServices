# 簡介

這個系列的名字為「Backend of Indie Game: 自建獨立遊戲服務」，是敝團隊這次開發遊戲『呆呆戰學校』的心得筆記，和其他 Indie Game 開發者一樣，我們這次開發過程中，嘗試在資源有限的狀況下，用簡易的方法來自建線上遊戲服務。希望能對其他 Indie 開發者能有所幫助。

後續內容將使用 Parse + Firebase 服務為基底，從最基礎的「Infrastructure 架構」進行介紹與「Step by Step 架設教學」，接著會介紹在此架構下，各種常見的「遊戲後端」服務設計與心得分享。希望能夠為 Indie Game 帶來更多的元素。

p.s. 作者懶癌末期，所以可能很多內容會以問答來提供，如果有任何細節問題，請不要客氣來信討論

# 大綱介紹

* Infrastructure
  * 依照步驟簡單的建立所需要的基礎服務，包含 Parse Service\(Domain, SSL, Database, Parse Server/Dashboard\) 架設以及  Firebase 服務設定
* Game Service
  * 介紹常見的遊戲服務，實作上遇到的各種挑戰或考量
* General System
  * 介紹一些~~\(好用\)~~泛用的基礎系統設計，可以方便用來開發成各種想要的遊戲服務

# 適合對象

* 小型獨立遊戲開發者
* 前 Parse 服務關閉的受害者（淚
* ~~時間太多的人~~

# 作者簡介

Terry Huang \(@kmshiori\) 目前任職 LiRise Games，業餘電動玩家，Overwatch 打不上 3000 分求大神帶 \(kmshiori\#3943\)

LiRise Games 是位於台南的 Indie Game Studio，隨時都在徵人，歡迎~~\(入坑\)~~對於遊戲開發有興趣的人來聊聊，或是與我們聯繫

# 書中目前尚不包含的內容

* SQL Sharding
* Database/Server HA
* Docker/k8s
* ~~黃金屋，顏如玉~~



