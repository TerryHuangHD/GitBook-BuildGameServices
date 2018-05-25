# 設計簡易的即時多人遊戲傳輸通道 - 透過 Firebase Realtime Database

## 可靠性通道：嘗試設計多玩家遊戲動作序列

* 透過節點紀錄每一筆玩家邏輯動作，由玩家 **push** 每一筆動作資料，由伺服器端接收的順序為準，再返回各玩家的程式中作動

| 根目錄 | 自動 | 資料 |
| --- | --- | --- |
| Game ID / Action / | ${Auto} / | UserID: 玩家 ID，可用於辨識   Action: 動作   Value: 數值   Time: 動作時間   ... |

* 玩家透過 push 傳送命令資料

```text
FIRDatabaseReference* ref = [[[FIRDatabase database] reference] child:@"Game/UUID GAME ID/Action"];
NSString *key = [ref childByAutoId].key;
NSDictionary *action = @{
                       @"UserID": @"USER ID A",
                       @"Action": "Hit",
                       @"Value": 50,
                       @"Time": [FIRServerValue timestamp]};
NSDictionary *childUpdates = @{[@"/" stringByAppendingString:key]: action};
[_ref updateChildValues:childUpdates];
```

* 起始玩家，透過監聽 onChildAdded，取得命令序列

```text
FIRDatabaseReference* ref = [[[FIRDatabase database] reference] child:@"Game/UUID GAME ID/Action"];
FIRDatabaseHandle handle = [ref observeEventType:FIRDataEventTypeChildAdded
                                       withBlock:^(FIRDataSnapshot* _Nonnull snapshot) {
                                           if (snapshot.value != [NSNull null]) {
                                               // 監聽 ChildAdded
                                           }
                                       }];
```

* 觀眾、重播系統，可取得節點 SnapShot 重建遊戲歷程

```text
FIRDatabaseReference* ref = [[[FIRDatabase database] reference] child:@"Game/UUID GAME ID/Action"];
[ref observeSingleEventOfType:FIRDataEventTypeValue
                    withBlock:^(FIRDataSnapshot* _Nonnull snapshot) {
                        if (snapshot.value != [NSNull null]) {
                            // 取得 Action Sanpshot
                        }
                    }
              withCancelBlock:^(NSError* _Nonnull error){
                  // 失敗
              }];
```

## 不可靠性通道：嘗試設計公頻聊天系統

* 透過節點單一筆資料紀錄最新發言內容，透過監聽 **onUpdate** 事件來取得新的對話，由使用者端形成對話歷史。這樣的優點是，**不用特意儲存與刪除大量的聊天記錄**

| 根目錄 | 資料 |
| --- | --- |
| Game ID / Chat / Public / | UserID: 玩家 ID，用於辨識   Name: 玩家姓名   Image: 玩家圖片   Message: 玩家聊天內容   Time: 發言時間    ... |

* 透過 REST API 或是 SDK（不開啟 Offline Capabilities）來進行發言嘗試

```text
FIRDatabaseReference* ref = [[[FIRDatabase database] reference] child:@"Game/UUID GAME ID/Chat/Public"];
NSDictionary* message = @{
    @"UserID" : @"USER ID A",
    @"Name" : @"USER NAME",
    @"Image" : @"IMAGE URL",
    @"Time" : [FIRServerValue timestamp]
};
[_ref updateChildValues:message];
```

* 玩家顯示介面綁定監聽 onUpdate

```text
FIRDatabaseReference* ref = [[[FIRDatabase database] reference] child:@"Game/UUID GAME ID/Chat/Public"];
FIRDatabaseHandle handle = [ref observeEventType:FIRDataEventTypeChildChanged
                                       withBlock:^(FIRDataSnapshot* _Nonnull snapshot) {
                                           if (snapshot.value != [NSNull null]) {
                                               // 監聽最新聊天訊息
                                           }
                                       }];
```

