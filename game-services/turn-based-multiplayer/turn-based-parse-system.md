# 簡易的回合制多人遊戲系統 - 透過 Parse

### TurnBasedGame 資料表設計

| 欄位 | 類型 | 解釋 | 範例 |
| --- | --- | --- | --- |
| ID | String | 遊戲 ID，供辨識使用 | ${GAME ID} |
| Participants | Array | 玩家清單 | [${USER ID},...] |
| CurrentPlayer | String <br> Pointer -> User | 當前回合玩家 | ${USER ID} |
| NextDeadline | Number | 當前回合玩家的期限 | ${UNIX TIMESTAMP} |
| GameData | Object | 遊戲資料 | ${GAME DATA} |
| GameStatus | String | 遊戲狀態 | Active <br> Finish |
