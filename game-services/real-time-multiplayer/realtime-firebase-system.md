# 設計簡易的即時多人遊戲傳輸通道 - 透過 Firebase Realtime Database

### 可靠性通道



### 不可靠性通道

* 嘗試設計公頻聊天系統，透過單一節點一筆資料紀錄最新發言內容，透過監聽 **onUpdate** 事件來取得新的對話，由使用者端形成對話歷史。這樣的優點是，**不用特意儲存與刪除大量的聊天記錄**

| 根目錄 | 資料 |
| --- | --- | 
| Game ID / Chat / Public | UserID: 玩家 ID，可用於辨識 <br> Name: 玩家姓名 <br> Image: 玩家圖片 <br> Message: 玩家聊天內容 <br> Time: 發言時間 |

* 透過 REST API 來進行發言的一次性嘗試

* 玩家顯示介面綁定監聽 onUpdate