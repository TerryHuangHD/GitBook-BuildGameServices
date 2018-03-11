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

```
curl -X POST \
-H "X-Parse-Application-Id: ${APPLICATION_ID}" \
-H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
-H "Content-Type: application/json" \
-d '{"Participants":[${USER_OBJ_A}...],"CurrentPlayer": ${USER_ID_A},"NextDeadline":${TIMESTAMP},"GameData":${DATA_OBJ},"GameStatus":"Active"}' \
https://YOUR.PARSE-SERVER.HERE/parse/classes/TurnBasedGame
```

* 透過 Cloud Code 系統推送 **換手** 通知給特定玩家

> 可在 Installation 與 User 中互相綁定資料，用來作 Target Segmentation

```
var query = new Parse.Query(Parse.Installation)
query.equalTo('user', {'__type': 'Pointer', 'className': 'User', 'objectId': ${USER ID}})
Parse.Push.send({
  where: query,
  data: {
    alert: 'TURN'
  }
}, { useMasterKey: true }).then(
  function () {
    response.success()
  },
  function (error) {
    response.error(error)
  }
)
```

* 任意玩家可隨時取得最新 **遊戲資料表**

* 當局玩家 **更新遊戲資料**，送出 **換手** 命令

* 當局玩家完成遊戲後，將 **更新遊戲資料**，送出 **完成遊戲** 命令

