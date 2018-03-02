# 在 Parse 上設計簡易的成就系統

### Achievement 資料表設計

|  欄位 | 類型 | 解釋 | 範例 |
| --- | --- | --- | --- |
| ID |  String | 辨識用 ID | achievement_hunt_master |
| NAME |  String | 顯示的名稱 | achievement_hunt_master_name <br> 多國語系鍵值，可於使用者端轉譯 <br> 獵龍大師 |
| DESCRIPTION |  String | 顯示的描述 | achievement_hunt_master_description <br> 多國語系鍵值，可於使用者端轉譯 <br> 完成獵殺龍類 10 隻 |
| ICON |  File | 圖示 | < File Object > |
| IS_HIDDEN | Bool | 是否是隱藏式成就 | False |
| AMOUNT | Number | 成就所需數量 <br> 若 > 1 則為累進式成就 | 10 |
| ORDER |  Number | 成就顯示的順序 | 1 |
| VERSION |  Number | 成就的版本 <br> 用來過濾舊版本程式不支援的成就 <br> 也可方便新成就的開發與測試 | 1 |
| IS_DELETE | Bool | 是否已經刪除 | False |

> 在這範例中，我們將多國語系的支援，實作在使用者端的字典檔案中，雲端僅存放鍵值。並透過版本控管來避免顯示尚未支援的成就

### Achievement 紀錄資料
