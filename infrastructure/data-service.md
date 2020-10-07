# Data 服務

> 在 Game Services 的範疇中，提供資料服務是最重要功能之一，比如存放個人面向資料：使用者資料、消費歷程...，或是存放公用面向資料資：排行榜資料...等等。在實際的使用面上，各資料儲存系統都有其優缺點，比如：schemaless, security rule, indexing 能力, atomic operation 支援度...等等，也可能會同時使用多種資料儲存服務

常見的 BaaS 服務中，在建立或申請好服務的同時，通常就帶有 Data 服務。我們接著把 Parse, Firebase Realtime Database, Firebase Cloud Firestore 資料服務從「資料模型」、「資料格式類型」、「資料查詢與索引」、「資料原子性操作」、「資料可靠性與拓展性」、「權限控制」、「使用性限制」...等等各方面進行比較，方便在設計各種功能時，能簡易的找到適合的選項

## 資料模型 <a id="model"></a>

| Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- |
| 以多個 Class 為基礎，每個 Class 為 Object（資料）的集合 | 以節點為基礎，母節點即為一個 JSON 結構 | 以多個 Collection 為基礎，每個 Collection 為 Document（資料）的集合 |
| 可預先定義 Class 的結構格式，也可透過資料直接新增欄位，經定義後的結構即具有約束性 | Schemaless，任意節點與資料皆沒有約束性（可透過設定 Database Rules 來驗證資料） | 無法預先定義 Collection 資料格式，即使在同一個 Collection 中的 Document 亦沒有相互約束性 |

* Parse 的 Data Model 最類似傳統的 RDBMS，如果您有相關的使用經驗，應該沒有太大的轉移成本

## 資料格式類型 <a id="type"></a>

| Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- |
| String | String | String |
| Number | Number | Number |
| Boolean | Boolean | Boolean |
| Object | Map &lt; String, Object &gt; | Object |
| Array | List &lt; Object &gt; | Array |
| DateTime |  | Timestamp |
| GeoPoint |  | Geopoint |
| Polygon |  |  |
| File |  |  |
| Pointer |  | Reference |
| Relation |  |  |
|  | Null | Null |

* 三個資料服務，都能夠完整對應 JSON 格式
* 地理位置 GeoPoint，僅在 Parse 以及 Cloud Firestore 支援
* 指標資料 Pointer, Reference，僅在 Parse 以及 Cloud Firestore 支援
* 檔案指標 File，僅在 Parse 支援。其餘服務可搭配 Cloud Storage 自行設計資料格式
* 多邊形資料 Polygon，僅在 Parse 支援

## 資料查詢與索引 <a id="index"></a>

|  | Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- | :--- |
| Compound Query | O | X（僅能對單一屬性做 Sort 或 Filter） | O |
| Auto Index | O（可自行於資料庫優化） | X（需手動設定） | O |
| GeoPoint Index | O | X | O |
| Query Optimization | O（支援 Select Field） | X | O（可 Query SubCollections） |
| Live Listening | P（須透過 Live Query） | O | O |

## 資料原子性操作 <a id="atom"></a>

| Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- |
| 僅支援 Number, Array 的增減操作 | 針對整個資料庫特定節點執行 Transaction | 可 Batch 多個寫入指令來執行 Transaction |

## 資料可靠性與拓展性 <a id="reliable"></a>

| Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- |
| 取決於資料庫可靠性 | 可用於「上線服務」的成熟產品，有 SLA | 還在 Beta 中（2018 Feb） |
|  | 資料有區域可用性 | 具有全球化的可靠性與拓展能力 |
|  | 超過上限時須手動 Shard（100k同時連線，1k次寫入/秒） | 自動化拓展 |

## 權限控制 <a id="permission"></a>

| Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- |
| 支援 Class-Level Permissions 與 Access Control Lists，可分別針對整個 Class 或特定 Object 進行權限控管 | 僅能透過 Database Rules 來規範可存取的使用者或資料驗證 | 可透過 Security Rules 與 Identity and Access Management 控管使用者端與伺服器端權限 |

## 使用性限制 <a id="limit"></a>

| Parse | Realtime Database | Cloud Firestore |
| :--- | :--- | :--- |
|  | 同時連線數 100,000/資料庫 | 寫入限制 2.5 MiB/秒/資料庫 |
|  | 同時監聽回應數 100,000/資料庫 | 同時連線數 100,000/資料庫（Beta 限制） |
|  | 單一 Cloud Function 寫入數量 1000 | Subcollection 深度 100 |
|  | 事件驅動寫入大小 1 MB | 文件大小 1 MiB |
|  | 傳送至 Cloud Function 資料 10 MB/秒 | 單一欄位大小 1 MiB - 89 bytes |
|  | 單一回應文件大小 256 MB | API Request 大小 10 MiB |
|  | Query 執行時間限制 15 分鐘 | 單一文件寫入 1/秒 |
|  | 單一寫入資料庫大小 SDK 16 MB, REST 256 MB | 單一 Collection 中寫入文件數量 500/秒 |
|  | Simultaneous Write 64 MB/分鐘 | Transaction Commit 寫入文件數量 500 |
|  |  | Transaction 執行時間限制 270秒 |

* 不列舉 naming, index 限制

