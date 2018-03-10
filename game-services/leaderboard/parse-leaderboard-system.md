# 設計簡易的公開排行榜系統 - 透過 Parse

以下的案例是以 **公開排行榜** 來設計初始概念，並不夠優化在正式環境中使用。在務實上，由於排行榜的資料使用存在很大的不對稱性，所以都會設計有預先計算以及快取的機制，來避免浪費大量的運算資源來 **重複讀取** 變動性不大的資訊

### Leaderboard 排行榜資料表設計

|  欄位 | 類型 | 解釋 | 範例 |
| --- | --- | --- | --- |
| ID |  String | 排行榜 ID，供辨識使用 | leaderboard_hunt_master |
| NAME |  String | 顯示的名稱 | leaderboard_highest_score_name <br> 多國語系鍵值，可於字串映射檔轉譯 <br> 最高分榜 |
| ICON |  File | 圖示 | < File Object > |
| IS_PUBLIC | Bool | 是否為公開排行榜 | True |
| DESCENDING | Bool | 排名方式以數值由大到小排列 | True |
| LASTING | String | 統計時段 | DAILY, WEEKLY, ALL |
| MAX_VALUE | Number | 合理數值上限 | 100000 |
| MIN_VALUE | Number | 合理數值下限 | 0.1 |
| VALUE_TYPE | String | 數值類別 | NORMAL, CONCURRENCY, TIME |
| ORDER |  Number | 排行榜顯示的順序 | 1 |
| APP_VERSION |  Number | 排行榜要求的程式版本 <br> 用來過濾舊版本程式不支援的成就 <br> 也方便開發、測試新的排行榜 | 1 |
| IS_DELETE | Bool | 是否已經刪除 | False |

> 多國語系的支援，可以透過本機端或是快取雲端字串映射來轉譯

* 新增排行榜：新增一筆排行榜的資料表，並將 APP_VERSION 設定為下一個 APP 版本編號。然後於新增多國語系的字串映射檔描述，以及結算時候的對應邏輯

* 修改排行榜：以此設計而言，可簡單的透過雲端資料的變更，可動態修改的欄位有：圖樣、合理數值、顯示順序...等等

* 移除排行榜：將排行榜 IS_DELETE 設定為 True 即可。不直接刪除，可避免部分資料相依性造成的錯誤

* 取得排行榜清單：將 **目前程式端支援** 以及 **未被刪除** 的排行榜取出，**依照順序排列**

```
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -G \
  --data-urlencode 'order=ORDER' \
  --data-urlencode 'where={"APP_VERSION":{"$lte":${CURRENT_APP_VERSION}},"IS_DELETE":false}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/Leaderboard
```

### LeaderboardRecord 排行榜紀錄表設計

|  欄位 | 類型 | 解釋 | 範例 |
| --- | --- | --- | --- |
| objectId | String | 此紀錄物件 ID | ${OBJECT ID} |
| USER | String <br> Pointer -> User | 玩家 ID | user_id_terry |
| LEADERBOARD | String <br> Pointer -> Leaderboard | 排行榜 ID | leaderboard_hunt_master |
| VALUE | Number | 排行數值 | 100 |
| TIME | Number <br> Date | 最新記錄創造時間 | ${UNIX_TIMESTAMP} |

* 取得玩家在特定排行榜自己的紀錄

```
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -G \
  --data-urlencode 'where={"USER":${USER_ID},"LEADERBOARD":${LEADERBOARD_ID}}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/LeaderboardRecord
```

* 取得玩家在特定公開排行榜自己的名次
> DESCENDING 以 $gt 取得比自己大的數量, ASCENDING 以 $lt 取得比自己小的數量。如果是有重複性數值的排行榜，通常會另外設計一個欄位來實作雙重排序因子的計數

```
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -G \
  --data-urlencode 'where={"LEADERBOARD":${LEADERBOARD_ID},"VALUE":{"$gt":${VALUE}}}' \
  --data-urlencode 'count=1' \
  --data-urlencode 'limit=0' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/LeaderboardRecord

```

* 取得特定公開排行榜榜單
> 以 -VALUE 或是 VALUE 來取得 DESCENDING 與 ASCENDING 的排序。limit 與 skip 可用來作 paging

```
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -G \
  --data-urlencode 'where={"LEADERBOARD":${LEADERBOARD_ID}}' \
  --data-urlencode 'order=-VALUE' \
  --data-urlencode 'limit=30' \
  --data-urlencode 'skip=0' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/LeaderboardRecord
```

* 新增排行榜紀錄

```
  curl -X POST \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"USER":${USER_ID},"LEADERBOARD":"leaderboard_highest_score_name","VALUE":${VALUE},"TIME":${NOW}}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/LeaderboardRecord
```

* 更新排行榜紀錄

```
curl -X PUT \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"VALUE":${VALUE},"TIME":${NOW}}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/LeaderboardRecord/${OBJECT ID}
```