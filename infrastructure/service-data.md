# Data 服務

### 資料模型

| Parse | Firebase Realtime | Firebase Firestone |
| --- | --- | --- |
| 以多個 Class 為基礎，每個 Class 為 Object（資料）的集合 | 以節點為基礎，母節點即為一個 JSON 結構 | 以多個 Collection 為基礎，每個 Collection 為 Document（資料）的集合 |
| 可預先定義 Class 的結構格式，也可透過資料直接新增欄位，定義後的結構即具有約束性 | Schemaless，任意節點與資料皆沒有約束性  | 無預先定義 Collection 資料格式，即使在同一個 Collection 中的 Document 亦沒有相互約束性 |

### 資料格式類型
### 資料查詢、索引
### 資料寫入、原子性操作
### 資料可靠性、拓展性
### 權限控制
### 限制