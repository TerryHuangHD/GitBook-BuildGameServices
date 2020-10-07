# 設計簡易的 Dispatcherless 自動配對機制 - 透過 Firebase Realtime Database

遊戲配對系統中，直接規劃 **配對者（Dispatcher）** 來執行配對任務是最直接的解決辦法，但耗費較高也比較繁瑣，以下的設計，透過使用者端發起 Transaction 機制來作動配對邏輯，再另外監聽單場遊戲節點來確認遊戲配對狀態與起始資料，可在不需而外 Dispatcher 的狀況下進行配對，適用在固定玩家數量的遊戲類型

## 排隊結構

* 透過 **子結點** 實作 **群組配對**，並將玩家屬性寫入排隊資料，配對邏輯可以進行 **屬性配對**

| 根目錄 | 群組 | 資料 |
| :--- | :--- | :--- |
| Queue / | Group ID / | \[   Room ID: 已開啟的房間號碼   User\[ \]: 已經報名的玩家，及其屬性   Create: 房間創立時間   \] |

* 玩家進行自動配對

> 其中 Queue/GroupA 為特定群組，以群組為節點存放等待資訊，也以此節點為 Transaction 執行的標的。其中 game\_id, score, low, high 為玩家屬性，實際使用應為變化數值

```text
FIRDatabaseReference* pair_ref = [[[FIRDatabase database] reference] child:@"Queue/GroupA"];
[pair_ref runTransactionBlock:^FIRTransactionResult* _Nonnull(FIRMutableData* _Nonnull currentData) {
    NSMutableArray* datas = currentData.value;

    if (!datas || [datas isEqual:[NSNull null]]) {
        // Queue 空白，將自己排入 Queue
        NSDictionary* data = @{
            @"game_id" : "UUID GAME ID",    // 新建遊戲房的 ID
            @"score" : 100,                 // 玩家目前積分
            @"low" : 80,                    // 配對玩家的積分下限
            @"high" : 120,                  // 配對玩家的積分上限
        };

        datas = [[NSMutableArray alloc] initWithObjects:data, nil];
    } else {
        BOOL pair = NO;

        for (NSDictionary* data in datas) {
            if ([[data objectForKey:@"score"] intValue] >= 80
                && [[data objectForKey:@"score"] intValue] <= 120
                && 100 >= [[data objectForKey:@"low"] intValue]
                && 100 <= [[data objectForKey:@"high"] intValue]) {
                // 配對成功 Pair，將等待訊息移除，目標前往加入 [data objectForKey:@"game_id"]
                [datas removeObject:data];
                break;
            }
        }

        if (!pair) {
            // 配對失敗，將自己排入 Queue
            NSDictionary* data = @{
                @"game_id" : "UUID GAME ID",
                @"score" : 100,
                @"low" : 80,
                @"high" : 120,
            };

            [datas addObject:data];
        }
    }
    [currentData setValue:datas];

    return [FIRTransactionResult successWithValue:currentData];
}
    andCompletionBlock:^(NSError* _Nullable error, BOOL committed, FIRDataSnapshot* _Nullable snapshot) {
        if (error || !committed) {
            // 配對失敗
        } else {
            // 成功配對，進入下個階段，前往 game
        }
    }
    withLocalEvents:NO];
```

## 遊戲結構

* 透過 Player 來監聽整體的 **配對狀態**，滿員時即配對完成。Init 則是設計為開場預設資料，統一委由開局者或是關局者來寫入即可

| 根目錄 | - | 資料 |
| :--- | :--- | :--- |
| Game ID / Player / | Player Id / | User   Image   ... // 遊戲中使用到的玩家基本資料 |
| Game ID / Init / |  | RandomSeed   ... // 遊戲中共同設定資料 |

* 配對完成後，加入遊戲。可以相同方法寫入初始值

> Player ID A 與 user\_json\_dic 為此玩家的專屬 ID 及其玩家資料，使用上應替換為玩家數值

```text
FIRDatabaseReference* game_ref = [[[FIRDatabase database] reference] child:@"UUID GAME ID/Player"];

NSData* user_json_string = [[NSString stringWithUTF8String:user_json] dataUsingEncoding:NSUTF8StringEncoding];
NSDictionary* user_json_dic = [NSJSONSerialization JSONObjectWithData:user_json_string options:0 error:nil];

NSDictionary* player = @{
                         @"Player ID A" : user_json_dic,
                         };
[game_ref setValue:player];
```

* 監聽遊戲的配對狀態

```text
FIRDatabaseReference* listen_ref = [[[FIRDatabase database] reference] child:@"Game/UUID GAME ID"];
FIRDatabaseHandle handle = [listen_ref observeEventType:FIRDataEventTypeValue
                                              withBlock:^(FIRDataSnapshot* _Nonnull snapshot) {
                                                  if (snapshot.value != [NSNull null]) {
                                                      // 監聽遊戲房間副本
                                                  }
                                              }];
```

