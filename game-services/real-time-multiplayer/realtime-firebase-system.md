# 設計簡易的即時多人遊戲傳輸通道 - 透過 Firebase Realtime Database

### 可靠性通道：嘗試設計多玩家遊戲動作序列

* 透過節點紀錄每一筆玩家邏輯動作，由玩家 **push** 每一筆動作資料，由伺服器端接收的順序為準，再返回各玩家的程式中作動

| 根目錄 | 自動 | 資料 |
| --- | --- | --- |
| Game ID / Action / | ${Auto} / | UserID: 玩家 ID，可用於辨識 <br> Action: 動作 <br> Value: 數值 <br> Time: 動作時間 <br> ... |

* 玩家透過 push 傳送命令資料

* 起始玩家，透過監聽 onChildAdded，取得命令序列

* 觀眾、重播系統，可取得節點 SnapShot 重建遊戲歷程

### 不可靠性通道：嘗試設計公頻聊天系統

* 透過節點單一筆資料紀錄最新發言內容，透過監聽 **onUpdate** 事件來取得新的對話，由使用者端形成對話歷史。這樣的優點是，**不用特意儲存與刪除大量的聊天記錄**

| 根目錄 | 資料 |
| --- | --- | 
| Game ID / Chat / Public / | UserID: 玩家 ID，用於辨識 <br> Name: 玩家姓名 <br> Image: 玩家圖片 <br> Message: 玩家聊天內容 <br> Time: 發言時間  <br> ... |

* 透過 REST API 來進行發言的一次性嘗試

* 玩家顯示介面綁定監聽 onUpdate