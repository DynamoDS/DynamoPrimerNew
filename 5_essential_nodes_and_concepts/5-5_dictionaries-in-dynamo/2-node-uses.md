# 字典節點

Dynamo 2.0 公開各種字典節點供我們使用。這包括_建立、動作和查詢_節點。

![]

#### Create

`Dictionary.ByKeysValues`Dictionary.ByKeysValues 將使用提供的值和鍵建立字典。 _(項目數目會是最短的清單輸入)_

#### Action

2\. `Dictionary.Components` Dictionary.Components 將產生輸入字典的元件。 _(這是建立節點的反轉過程)。_

3\. `Dictionary.RemoveKeys` Dictionary.RemoveKeys 將產生新的字典物件但移除輸入鍵。

4\. `Dictionary.SetValueAtKeys` Dictionary.SetValueAtKeys 將根據輸入鍵與值產生新字典，以取代對應鍵處的目前值。

5\. `Dictionary.ValueAtKey` Dictionary.ValueAtKey 會傳回輸入鍵處的值。

#### 計數

6\. `Dictionary.Count` Dictionary.Count 會告訴您字典中有多少個鍵值對。

7\. `Dictionary.Keys` Dictionary.Keys 會傳回字典中目前儲存哪些鍵。

8\. `Dictionary.Values` Dictionary.Values 會傳回字典中目前儲存哪些值。

字典的整體相關資料是處理索引和清單的舊方法的重要替代方法。
