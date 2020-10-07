# Leaderboard 排行榜系統

排行榜系統本質上就是透過比較與競爭來獲得成就感的系統，對於 Hardcore 玩家而言，通常會在爭取 **公開排行榜** 的名次上獲得樂趣。對於 Casual 玩家而言，則是在 **朋友排行榜** 之間競爭更有樂趣

## 目錄

* [常見類型或特性](./#type)
* [排行榜系統情境](./#general)
* [主題：設計簡易的公開排行榜系統 - 透過 Parse](she-ji-jian-yi-de-gong-kai-pai-hang-bang-xi-tong-tou-guo-parse.md)

## 常見類型或特性 <a id="type"></a>

| 依據 | 類型與說明 |
| :--- | :--- |
| 排行參與者 | 公開排行榜：固定群體的排行榜，每個相同條件者看到的數據相同   社交排行榜：依照每個人的朋友清單訂製的排行榜 |
| 統計時間 | 日榜   週榜   全時榜 |
| 排行方式 | 降序   升序 |

## 排行榜系統情境 <a id="general"></a>

* 遊戲設計者預先設立**一個或多個排行榜**
* 遊戲設計者設立排行榜時需要確立屬性
  * 名稱：通常是簡單描述排行的標的，比如：最高積分榜
  * 圖示：顯示上能簡單代表這一個排行榜
  * 排行理論：數值由小到大排名，還是由大到小排名
  * 統計時段：常見的是每天、每週、全時段的紀錄排行
  * 合理數值：預先設計合理範圍，可排除無意義或是作弊的數值
  * 順序：通常是依照普及度排序，越多人都參與得到的通常會先出現
* 玩家有特定的入口可以觀看**所有的排行榜**
* 玩家在遊戲關卡結算時將排行數值**送到伺服器端取得結果**
  * 是否刷新個人成績
    * 如果是，則更新成績
    * 如果不是，也會回傳玩家在該排行榜的成績
  * 最新的個人排行資料
* 玩家在刷新排行榜成績紀錄時，介面通常設計有明顯的提示
* 玩家通常在刷新排行榜成績紀錄時，會設計獲得獎勵
