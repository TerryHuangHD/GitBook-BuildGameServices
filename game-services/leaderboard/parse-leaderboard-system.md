# 在 Parse 上設計簡易的排行榜系統

### Leaderboard 成就資料表設計

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
