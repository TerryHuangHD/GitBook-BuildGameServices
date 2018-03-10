# 簡易的回合制多人遊戲系統 - 透過 Parse

### TurnBasedGame 資料表設計

| 欄位 | 類型 | 解釋 | 範例 |
| --- | --- | --- | --- |
| ID | String | 遊戲 ID，供辨識使用 | ${GAME ID} |
| Participants | Array | 玩家清單 | [${USER ID},...] |
| CurrentPlayer | String | 當前回合玩家 | ${USER ID} |
| NextDeadline | Number | 當前回合玩家的期限 | ${UNIX TIMESTAMP} |
| GameData | Object | 遊戲資料 | ${GAME DATA} |
| GameStatus | String | 遊戲狀態 | Active <br> Finish... |

* **配對系統** 建立遊戲資料表

* 透過系統推送 **換手** 通知

* 任意玩家可隨時取得最新 **遊戲資料表**

* 當局玩家 **更新遊戲資料**，送出 **換手** 命令

* 當局玩家完成遊戲後，將 **更新遊戲資料**，送出 **完成遊戲** 命令

