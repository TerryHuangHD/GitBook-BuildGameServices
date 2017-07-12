# 簡介

> 獨立遊戲（英語：Independent Game或Indie Game）開發者沒有遊戲公司或遊戲發行商提供的薪資，必須獨力負擔開發過程中的所有花費。相對地，開發者可以決定遊戲的走向，做自己想做的遊戲，往往可以推出嶄新觀點的作品，而不用受制於遊戲公司或市場。 - wikipedia

本系列名為「Indie Game Backend 錦囊妙計：自建遊戲服務」，意指在面向 Indie Game 開發者，在資源有限以及對後端工程可能較不熟悉的狀況下，提供簡易的方法來自建遊戲服務。

後續內容將使用 Parse + Firebase 服務為基底，從最基礎的「Infrastructure 架構」進行介紹與「架設教學」，接著會介紹在此架構下，各式各樣的「遊戲後端」服務設計與心得分享。希望能夠讓同為 Indie Game 開發者的大家，帶來更多的元素。

# 作者簡介

Terry Huang

@kmshiori

David Lin

攔路中中路無敵歡迎單挑

LiRise is Hiring

# 適合對象

* 獨立遊戲開發者
* 原本使用 Parse 服務平台者
* 專注於遊戲內容，對於傳統後端服務較不熟悉者（如：資料庫、Rest API...）
* 希望能在獨立遊戲中加入更多元素的開發者

# 大綱介紹

> 本系列內容面向獨立遊戲環境使用，因此不包含 Scaling, HA 環節，亦不包含 DevOps, CI/CD 相關內容



