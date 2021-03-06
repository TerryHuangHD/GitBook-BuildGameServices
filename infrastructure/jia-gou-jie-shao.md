# 架構介紹

![Infrastructure](../.gitbook/assets/infrastructure2.jpg)

## Game Services 可能需要哪些基礎服務？

* **Data 服務**

  在 Game Services 的範疇中，提供資料服務是最重要功能之一，比如存放個人面向資料：使用者資料、消費歷程...，或是存放公用面向資料資：排行榜資料...等等。在實際的使用面上，各資料儲存系統都有其優缺點，比如：schemaless, security rule, indexing 能力, atomic operation 支援度...等等，也可能會同時使用多種資料儲存服務

* **API 服務**

  應用程式介面除了提供基礎的資料存取介面之外，通常也用來實現伺服器端的邏輯。在目前新興的服務中，更會包含了資料情境連動的能力（如: Database Trigger, Schedule/Job, Web Hook）

* **Notification 服務**

  Game Services 中，設計得當的 Notification 能有效的增加 retention，通常有三種使用情境：**本機端推送**、**雲端個別推送**、**雲端大量推送**

  * 本機端推送，可用於已經預知時間與內容的提醒，可完全在無連網情形之下發起通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
  * 雲端個別推送，通常是用於非計畫性的小事件，比如：有朋友丟了悄悄話給你
  * 雲端大量推送，通常用於不定時的廣播事件，比如：營運公司手動開始了一個活動

* **Job 服務**

  Job 服務，主要提供伺服端**「依照排程進行動作」**的能力，通常也比一般的 API 函式需要更多的執行時間。在 Game Services 中，通常用來做定時的伺服器資料更新，比如：每日排行榜更新...等等

* **Email 服務**

  Email 在小產品中是常見的 Account System 驗證方式，在 Parse Service 以及 Firebase Authentication 中都能夠整合這樣的驗證方式；除此之外，Email 也常設計為輔助的通知系統

* **SMS 服務**

  SMS 也是在小產品中是常見的 Account System 的驗證方式，這樣的驗證方法在大多數地區，比 Email 更具有**「實名性質」**，在 Firebase Authentication 中提供了這樣的驗證方式

* **Hosting 服務**

  Hosting 提供了檔案形式的資源取得方式，在 Game Services 中最常見用途有：遊戲通用的資源，遊戲的更新檔案、使用者上傳的照片...等等。依照需求有各種實作和優化方式

* **Realtime 服務**

  即時服務在有些遊戲中，扮演了非常關鍵的角色，比如：即時賽局的匹配、即時對弈...等等；對於一般遊戲也能增加互動性：比如：即時互動、聊天...等等

* **Information 服務**

  資訊服務為良好的使用體驗提供了基礎。產品中常見的資訊有

  * Config（設定資訊服務）
  * Connectivity（連線資訊服務）
  * Time（時間資訊服務）
  * Location（地點資訊服務）

