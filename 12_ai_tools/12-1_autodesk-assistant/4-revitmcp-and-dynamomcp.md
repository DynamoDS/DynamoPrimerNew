# RevitMCP 和 DynamoMCP

Autodesk Assistant 的功能來自 MCP 伺服器。每部伺服器都會提供 Assistant 一組工具，每個工具都在其所屬的產品中執行一項工作。

當您在 Revit 中工作時，這當中的 **RevitMCP** 和 **DynamoMCP** 兩部伺服器很重要。它們不是相互競爭的選項，您不用在它們之間做選擇。它們在不同的層級運作，Assistant 會使用能符合您要求的伺服器。

### RevitMCP：直接操作模型

RevitMCP 以 [Revit 公用 MCP 伺服器的方式提供](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm)，將一組精選的 Revit API 作業包裝成 MCP 工具。每個工具都對映到模型中一個事先定義好的動作：尋找元素、檢查參數和計數、批量編輯參數，以及擷取視圖快照。

使用工具集，可以最快速完成一項工作。一個要求變成一個呼叫，而且模型會立即變更。不需要建立圖表，之後也不需要執行內容。

### DynamoMCP：建立作業的工具

DynamoMCP 是在較高的層級運作。它的工具不會直接執行模型動作，而是編寫 Dynamo 圖表：放置節點、將節點連接在一起、設定值，然後執行結果。請參閱[什麼是 Dynamo MCP 伺服器](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md)，以取得完整的工具清單。

這之所以重要，是因為這與節點的本質有關。Dynamo 資源庫中的每個節點本身都是一個或多個 API 呼叫，這些呼叫已經撰寫好、經過測試並成為文件。套件節點和 Python 節點都以相同的方式提供。因此，只要透過幾個圖表編寫工具，就可以使用整個 Dynamo 資源庫。

結果就是，DynamoMCP 的工具可以產生新功能，而不僅僅只是使用既有功能。Assistant 不是使用固定的動作清單，而是將多個節點組合出您要求的動作。實際上，撰寫圖表的 MCP 工具，就像是建立出更多工具的工具。

### 實際上的差異

假設您要求將一個樓層的門加上標籤。

- 如果由 RevitMCP 處理，工作是直接進行呼叫，標籤就會出現。
- 如果由 DynamoMCP 處理，Assistant 會放置收集門的節點、將門篩選到您指定的樓層、建立標籤，然後執行圖表。

兩條路線都將門加上標籤。第二條路線也會在圖元區留下一些東西。

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| 工具的作用 | 執行定義好的 Revit 作業 | 建立並執行 Dynamo 圖表 |
| 使用範圍 | 工具集中包括的作業 | 資源庫中的任何節點，包括套件與 Python 節點 |
| 您獲得的內容 | 模型中的變更 | 模型中的變更，加上產生變更的圖表 |
| 最適合 | 單一、明確定義的工作 | 您想要檢查、調整、重複使用或分享的邏輯 |

### 為什麼推薦使用圖表

圖表包含運作的記錄，而不只有結果。圖元區出現圖表後，您可以：

- 檢視圖表，看看確切執行的動作。
- 變更值並再次執行，無須再次要求執行整個工作。
- 儲存圖表後在下一個專案中執行，或將圖表交給同事。
- 將圖表發佈到 Dynamo 播放器，讓不使用 Dynamo 的人可以執行。

第一個要求需要花費較長時間。之後每次執行幾乎不太需要等待。

{% hint style="info" %}
在 Dynamo Sandbox 中，Assistant 只能聯絡 DynamoMCP，因為沒有 Revit 模型可作用。在 Revit 中，則可聯絡兩者。
{% endhint %}

### 選擇要求的內容

您不需要在提示中指定伺服器。只要描述結果，Assistant 會自行選擇工具。

如果您需要圖表，就要指明。例如*「為我建立一個圖表，為樓層 2 的門加上標籤」*提示，會告訴 Assistant 重點是可重複使用的版本，而不只是加上標籤的門。

### 詳細資訊

- [Autodesk Assistant (技術預覽)](https://help.autodesk.com/cloudhelp/2027/CHT/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) — Assistant 在 Revit 中的作用，詳見 Revit 2027 說明。
- [Revit 公用 MCP 伺服器 (技術預覽)](https://help.autodesk.com/cloudhelp/2027/CHT/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) — MCP 的 Revit 端，詳見 Revit 2027 說明。
- [Autodesk Assistant：概述和常見問題](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) — 哪些產品包含 Assistant，以及如何使用。
