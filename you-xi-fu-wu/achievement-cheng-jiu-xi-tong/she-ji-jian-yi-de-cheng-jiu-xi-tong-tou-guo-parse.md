# 設計簡易的成就系統 - 透過 Parse

## Achievement 成就資料表設計

| 欄位 | 類型 | 解釋 | 範例 |
| :--- | :--- | :--- | :--- |
| ID | String | 成就 ID，供辨識使用 | achievement\_hunt\_master |
| NAME | String | 顯示的名稱 | achievement\_hunt\_master\_name   多國語系鍵值，可於字串映射檔轉譯   獵龍大師 |
| DESCRIPTION | String | 顯示的描述 | achievement\_hunt\_master\_description   多國語系鍵值，可於字串映射檔轉譯   完成獵殺龍類 10 隻 |
| ICON | File | 圖示 | &lt; File Object &gt; |
| IS\_HIDDEN | Bool | 是否是隱藏式成就 | False |
| AMOUNT | Number | 成就所需數量   若 &gt; 1 則為累進性成就 | 10 |
| ORDER | Number | 成就顯示的順序 | 1 |
| APP\_VERSION | Number | 成就要求的程式版本   用來過濾舊版本程式不支援的成就   也方便開發、測試新的成就 | 1 |
| IS\_DELETE | Bool | 是否已經刪除 | False |

> 多國語系的支援，可以透過本機端或是快取雲端字串映射來轉譯

* 新增成就：新增一筆成就的資料表，並將 APP\_VERSION 設定為下一個 APP 版本編號。然後於新增多國語系的字串映射檔描述，以及實作其邏輯上的對應（比如：何時會觸發更新、完成成就進度）
* 修改成就：以此設計而言，可簡單的透過雲端資料的變更，更新圖樣、隱藏性、所需數量、顯示順序...等等
* 移除成就：將成就 IS\_DELETE 設定為 True 即可。不直接刪除，可避免部分資料相依性造成的錯誤
* 取得成就清單：將 **目前程式端支援** 以及 **未被刪除** 的成就取出，**依照順序排列**

```text
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -G \
  --data-urlencode 'order=ORDER' \
  --data-urlencode 'where={"APP_VERSION":{"$lte":${CURRENT_APP_VERSION}},"IS_DELETE":false}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/Achievement
```

## AchievementRecord 成就紀錄表設計

| 欄位 | 類型 | 解釋 | 範例 |
| :--- | :--- | :--- | :--- |
| objectId | String | 此紀錄物件 ID | ${OBJECT ID} |
| USER | String   Pointer -&gt; User | 玩家 ID | user\_id\_terry |
| ACHIEVEMENT | String   Pointer -&gt; Achievement | 成就 ID | achievement\_hunt\_master |
| AMOUNT | Number | 成就已經完成的數量 | 1 |
| IS\_FINISH | Bool | 成就是否已經完成 | False |

> 在這設計中，將 **成就進度** 與 **是否完成成就** 分開紀錄，可避免成就設定被更改造成的錯誤判讀

* 取得成就紀錄清單

```text
curl -X GET \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -G \
  --data-urlencode 'where={"USER":${USER_ID}}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/AchievementRecord
```

* 更新成就進度

```text
curl -X PUT \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"AMOUNT":${VALUE}}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/AchievementRecord/${OBJECT ID}
```

* 完成累進性成就

```text
curl -X PUT \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"AMOUNT":${VALUE},"IS_FINISH":true}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/AchievementRecord/${OBJECT ID}
```

* 完成一次性成就

```text
  curl -X POST \
  -H "X-Parse-Application-Id: ${APPLICATION_ID}" \
  -H "X-Parse-REST-API-Key: ${REST_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"USER":${USER_ID},"ACHIEVEMENT":"achievement_hunt_master","AMOUNT":${VALUE},"IS_FINISH":true}' \
  https://YOUR.PARSE-SERVER.HERE/parse/classes/AchievementRecord
```

