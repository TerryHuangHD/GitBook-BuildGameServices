# 基礎服務架構介紹

![](/assets/Infrastructure.jpg "Infrastructure")

### Game Services 可能需要哪些基礎服務？

* #### 資料儲存

  在 Game Services 的範疇中，提供資料儲存是最重要功能之一，比如存放個人面向資料：使用者資料、消費歷程...，或是存放公用面向資料資：排行榜資料...等等。在實際的使用面上，各資料儲存系統都有其優缺點，比如：schemaless, security rule, indexing 能力, atomic operation 支援度...等等，也可能會同時使用多種資料儲存服務
  
* #### 應用程式介面
  
  應用程式介面除了提供基礎的資料存取介面之外，通常也用來實現伺服器端的邏輯。在目前新興的服務中，更會包含了各種連動能力（如: Database Trigger, Schedule/Job, Web Hook）

* #### Notification 服務

  Game Services 中，設計得當的 Notification 能夠增加 retention，通常有三種使用情境：本機端推送、雲端個別推送、雲端大量推送
    
  * 本機端推送，可用於已經預知時間與內容的提醒，可完全在無聯網情形之下通知，適用場景相當多，比如：每天晚上七點鐘的公會戰、能量已經恢復到滿格了...等等
  * 雲端個別推送，通常是用於突發性的小事件，比如：有朋友丟了悄悄話給你
  * 雲端大量推送，通常用於不定時的廣播事件，比如：遊戲公司開始一個營運活動

* #### Job 服務

  Job 服務，主要提供伺服端能依排程進行動作的能力，依照任務複雜度，可能會有不同的實作方法。在 Game Services 中，通常用來做定時的伺服器資料更新，比如：每日排行榜更新...等等

* #### Email 服務
  
  Email 在小產品中是常見的 Account System 的驗證方式，在 Parse Service 中整合了這樣的驗證方式

* #### SMS 服務

  SMS 也是在小產品中是常見的 Account System 的驗證方式，這樣的驗證方法比 Email 更具有實名性質，在 Firebase 中整合了這樣的驗證方式

* #### Hosting 服務
  
  Hosting 提供了檔案形式的資源存取方式，在 Game Services 中最常見用途有：遊戲通用的資源，遊戲的更新檔案、使用者上傳的照片...等等。依照需求有各種實作和優化方式

* #### Time 服務


* #### Realtime 服務

  > 範例選用：Firebase Realtime Database

  Firebase 早期變是純粹的即時資料庫服務，目前已擴充成完整的 BAAS 平台。強項在於能夠即時監聽雲端資料的變化，整合這樣的服務，可在遊戲中扮演關鍵的角色，提供遊戲更多互動性。除此之外，Firebase 更支援以 Node 為基礎的原子性操作，能讓即時遊戲服務更豐富，如：即時對弈、處理競爭條件...