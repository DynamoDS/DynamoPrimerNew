# 提取請求 

Dynamo 取決於社群的創意和承諾，Dynamo 團隊鼓勵貢獻者探索各種可能性、測試想法，並參與社群做出回饋。雖然我們非常鼓勵創新，但只有在變更能讓 Dynamo 更容易使用，並且符合本文件中定義的準則時，我們才會合併變更。我們不會合併只有少量改進的變更。

#### 提取請求的期望 <a href="#pull-request-expectations" id="pull-request-expectations"></a>

Dynamo 團隊希望提取請求遵循一些準則：

* 遵循我們的[程式碼撰寫標準](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)和[節點命名標準](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards)
* 加入新功能時需納入單元測試
* 修正錯誤時，請加入一個能突顯目前行為是如何造成問題的單元測試
* 將討論集中在一個問題。如果出現新主題或相關主題，請建立新問題。

不要這樣做的準則：

* 做出大量超出我們預期的提取請求。請改為提出問題並開始討論，以便我們可以在您投入大量時間之前就方向達成共識。
* 認可不是您撰寫的程式碼。如果您發現一些程式碼很適合加入 Dynamo，請在繼續之前，先提出問題並開始討論。
* 提交會改變授權相關檔案或標頭的 PR。如果您認為這些 PR 有問題，請提出問題，我們很樂於討論。
* 在沒有提出問題並且先跟我們討論的情況下增加 API 內容。

#### 填寫提取請求樣板 <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

提交提取請求時，請使用[預設的 PR 樣板](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md)。在提交您的 PR 之前，請確定清楚描述目的，並且確認所有聲明都真實：

* 提出此 PR 之後，程式碼庫會處於更佳狀態
* 根據[標準](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)製作文件
* 此 PR 所包含的測試階層是適當的
* 向使用者顯示的字串 (如果有) 已擷取到 `*.resx` 檔案中
* 使用自助服務 CI 通過所有測試
* 使用者介面變更的快照 (如果有)
* 對 API 的變更遵循[語意版本管理](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions)，並記錄在 [API 變更](https://github.com/DynamoDS/Dynamo/wiki/API-Changes)文件中。

Dynamo 團隊將為您的提取請求指定適當的審閱者。

#### 提取請求審閱流程 <a href="#pull-request-review-process" id="pull-request-review-process"></a>

提交提取請求後，您可能需要在審閱過程中持續參與。請注意以下審閱準則：

* Dynamo 團隊每個月開會一次，檢閱最舊到最新的提取請求。
* 如果審閱過的提取請求需要擁有者進行變更，PR 的擁有者有 30 天時間可以回覆。如果該 PR 在下一次會議之前沒有任何活動，則會被團隊關閉，或視其實用性由團隊中的某人接管。
* 提取請求應使用 Dynamo 的預設 PR 樣板
* 不會審閱未完整填寫 Dynamo PR 樣板且確信所有聲明為真的提取請求。

#### 精選 Dynamo Revit 認可內容 <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

由於市場上有多個版本的 Revit，因此您可能需要精選您的變更來放入特定的 DynamoRevit Release 分支，以便不同版本的 Revit 可以挑選新功能。在審閱過程中，貢獻者要依照 Dynamo 團隊指定，負責精選其審閱過的內容認可到其他 DynamoRevit 分支。
