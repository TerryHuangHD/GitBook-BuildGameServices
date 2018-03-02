# 在 Parse 上設計簡易的成就系統

### Achievement 資料表設計

|  欄位 | 類型 | 解釋 | 範例 |
| --- | --- | --- | --- |
| ID |  String | 辨識用 ID | achievement_hunt_master |
| NAME |  String | 顯示的名稱 | 獵龍大師 |
| DESCRIPTION |  String | 顯示的描述 |  完成獵殺龍類 10 隻 |
| ICON |  File | 圖示 | < File Object > |
| IS_HIDDEN | Bool | 是否是隱藏式成就 | False |
| IS_INCREMENTAL | Bool | 是否是累進式成就 | True |
| ORDER |  Number | 成就顯示的順序 | 1 |
| VERSION |  Number | 成就的版本 | 1 |

### Achievement 紀錄資料
