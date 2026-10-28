# 什麼是 Dynamo MCP 伺服器

**MCP** 是 Model Context Protocol 的縮寫。它是一個開放標準，允許 AI 模型透過通用的通訊協定連接到軟體。MCP 伺服器會公開程式的工具和資料，讓 AI 用戶端可以讀取即時的背景資料、採取動作並傳回結構化的結果。

**DynamoMCP** 是 Dynamo 的內建 MCP 伺服器。它會將 Autodesk Assistant 連接到作用中的 Dynamo 工作階段 (圖表、節點、線路和工作區狀態)，讓 AI 能以您實際處理的內容為基礎提供協助。

對 Dynamo 使用者而言，這表示 Assistant 的功能不是只有回答一般性的問題。它可以檢查您開啟的圖表、協助疑難排解節點問題，並根據實際的工作階段內容 (而不是手動複製並貼上) 更新圖元區。

DynamoMCP 在 Dynamo 4.2.0 及更高版本中提供。

### 如何使用 DynamoMCP

您需透過 Autodesk Assistant 使用 DynamoMCP，沒有單獨的設定或獨立介面。Assistant 是一個入口，讓您提出問題、取得圖表相關協助，以及讓 AI 處理即時的 Dynamo 環境。

您在哪裡開啟 Assistant，取決於您工作的位置：

- **Dynamo Sandbox** — Dynamo 內的 Autodesk Assistant 提供 Dynamo MCP 工具。
- **Revit** — Revit 內的 Autodesk Assistant 提供 Dynamo MCP 工具。

無論是哪一種，您都是待在目前的工作階段中，使用相同的 Assistant 體驗存取 DynamoMCP。
### DynamoMCP 可以做什麼

DynamoMCP 提供 Assistant 一組**工具**。每個工具在 Dynamo 中執行一項工作，例如讀取圖元區、放置節點，或執行圖表。您完全不用自己呼叫這些工具，而是用通俗易懂的語言描述想要的內容，Assistant 就會挑選它需要的工具。

瞭解這些工具仍然很有用。它會告訴您 Assistant 能夠執行的動作，並協助您設定可以處理的要求。

{% hint style="info" %}
以 `get_` 開頭的工具名稱，只會讀取您工作階段中的資訊。其他所有工具都可以變更您的圖表。
{% endhint %}

#### 讀取目前的圖表

這些工具可讓 Assistant 在回答或做出變更之前，先查看您正在處理的作業。

| 工具 | 功能 |
| --- | --- |
| `get_workspace_info` | 列出圖元區上的內容：節點、線路、群組、註記和工作區詳細資料。 |
| `get_selection` | 顯示您已選取的節點、線路和群組。 |
| `get_node_connections` | 顯示節點每個埠連接的內容。 |
| `get_node_property` | 顯示節點設定，例如預覽、凍結、固定的預覽和標示。 |
| `get_node_lacing` | 顯示節點的交織設定。 |
| `get_installation_info` | 顯示您的 Dynamo 版本、您執行所在的主體程式，以及發行說明的連結。 |

*範例提示：「我的圖元區現在有什麼？」*

#### 尋找適當的節點

Dynamo 有數千個節點，包括已安裝套件中的節點。這些工具可協助 Assistant 選擇正確的節點，而不是靠猜測。

| 工具 | 功能 |
| --- | --- |
| `get_nodes_info` | 搜尋節點資源庫 (包括套件節點)，並傳回要放置的確切節點。 |
| `get_node_help` | 傳回節點的文件說明，以及指向其範例圖表的路徑 (如果有)。 |
| `get_type_schema` | 傳回 Dynamo 類型的預期資料格式，例如點、數字或顏色。 |

*範例提示：「我要使用哪個節點獲得一個曲面的面積？」*

#### 建立和編輯圖表

這些工具會在圖元區上執行工作。

| 工具 | 功能 |
| --- | --- |
| `create_nodes` | 在圖元區上放置一個或多個節點 (可以選擇起始值)。 |
| `set_node_value` | 設定輸入節點 (例如 Number、String、Code Block 和下拉式清單) 的值。 |
| `connect_nodes` | 將輸出埠連接到輸入埠。 |
| `disconnect_nodes` | 移除兩個節點之間的線路。 |
| `delete_node` | 移除圖元區中的節點。 |
| `rename_node` | 更名輸入節點和 Watch 節點，以清楚說明用途。函數節點會保留原始名稱。 |
| `select_nodes` | 選取圖元區上的節點，讓您查看 Assistant 指的是哪些節點。 |
| `set_node_property` | 開啟或關閉節點設定：預覽、凍結、固定的預覽、標示，以及輸入或輸出。 |
| `set_node_lacing` | 將交織設定為「自動」、「最短」、「最長」或「笛卡兒積」。 |
| `set_node_port_list_at_level` | 設定輸入埠上的清單層級。 |

*範例提示：「加入數字滑棒，並連接到圓的半徑輸入。」*

#### 組織和記錄圖表

工作圖表不一定是易讀的圖表。這些工具會處理配置、分組、顏色和註記。

| 工具 | 功能 |
| --- | --- |
| `create_group` | 將一組節點分組。 |
| `create_groups` | 透過一個步驟建立多個群組。 |
| `ungroup_by_guid` | 移除群組。當中的節點會留在圖元區上。 |
| `ungroup_by_title` | 依標題移除群組。當中的節點會留在圖元區上。 |
| `get_available_group_styles` | 列出您可以使用的群組型式，包括偏好中的自訂型式。 |
| `assign_group_styles` | 套用群組型式，例如「輸入」、「動作」、「輸出」和「審閱」。 |
| `get_notes` | 列出圖表中的註記。 |
| `create_notes` | 加入說明圖表各部分內容的註記。 |
| `update_notes` | 編輯註記的文字，或移動註記。 |
| `delete_notes` | 移除註記。 |
| `set_note_state` | 將註記固定到節點，讓兩者一起移動，或取消固定。 |
| `auto_layout_workspace` | 使用內建的 Dynamo 自動配置，重新排列節點。 |
| `set_workspace_description` | 將圖表的描述寫入檔案的性質中。 |

*範例提示：「依輸入、動作和輸出將此圖表分組並顯示不同顏色，然後加入簡短註記。」*

#### 執行圖表並檢查結果

| 工具 | 功能 |
| --- | --- |
| `run_workspace` | 執行目前的圖表。在自動模式下，圖表在每次變更時都會重新執行，因此略過此步驟。 |
| `get_workspace_warnings` | 列出節點，包含警告或錯誤、訊息文字和建議的後續步驟。圖表必須先執行。 |
| `get_node_output_values_shapes` | 總結圖表中一個節點或每個節點的輸出值。 |
| `get_node_output_value` | 針對太大而無法總結的結果，一次一頁讀取單一節點的完整輸出。 |

*範例提示：「執行圖表，然後告訴我 List.GetItemAtIndex 節點出現警告的原因。」*

#### 使用圖表檔案

Assistant 也可以處理磁碟上的圖表，而不只是您已開啟的圖表。

| 工具 | 功能 |
| --- | --- |
| `new_workspace` | 開啟新的空白圖表。如果目前圖表有未儲存的變更，系統會要求您先儲存或捨棄變更。 |
| `save_workspace` | 儲存目前的圖表。如果未提供位置，則會儲存到「我的文件」中的 「Dynamo」資料夾。 |
| `edit_graph_from_path` | 開啟已儲存的圖表進行編輯，但不執行。 |
| `insert_graph_from_path` | 將儲存的圖表合併到目前的圖元區。這就是帶入節點範例圖表的方式。 |
| `run_graph_from_path` | 使用您提供的選擇性輸入值執行儲存的圖表。 |
| `run_graph_from_json` | 執行以 Dynamo JSON 格式提供的圖表，磁碟上沒有檔案。 |
| `get_graph_info_from_path` | 讀取已儲存圖表的輸入、輸出和中繼資料，但不開啟圖表。 |
| `get_graph_info_from_json` | 從以 Dynamo JSON 格式提供的圖表中讀取相同的詳細資料。 |
| `get_graphs_info_from_path` | 掃描圖表的資料夾，並顯示每個圖表包含的內容。 |
| `get_available_graphs_folders` | 列出 Dynamo 知道且可能包含圖表的資料夾。 |

*範例提示：「瀏覽我的 Dynamo 圖表資料夾，告訴我哪些圖表放置牆。」*

{% hint style="warning" %}
Assistant 可以變更您的圖表。請在要求進行大量編輯之前，先儲存您的工作，並在儲存覆蓋既有檔案之前先檢閱結果。
{% endhint %}
