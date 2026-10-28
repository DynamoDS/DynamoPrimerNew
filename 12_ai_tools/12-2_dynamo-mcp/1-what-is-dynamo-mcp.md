# 什么是 Dynamo MCP 服务器

**MCP** 代表模型上下文协议。它是一种开放标准，允许 AI 模型通过通用协议连接到软件。MCP 服务器公开程序的工具和数据，以便 AI 客户端可以读取实时上下文、采取行动并返回结构化结果。

**DynamoMCP** 是 Dynamo 的内置 MCP 服务器。它将 Autodesk Assistant 连接到活动的 Dynamo 会话（图形、节点、关联和工作空间状态），因此 AI 帮助以您实际正在处理的内容为基础。

对于 Dynamo 用户，这意味着 Assistant 的功能不仅仅是回答一般问题。它可以检查打开的图形，帮助解决节点问题，并根据真实会话上下文（而不是手动复制和粘贴）更新画布。

DynamoMCP 在 Dynamo 4.2.0 及更高版本中可用。

### 如何使用 DynamoMCP

您可以通过 Autodesk Assistant 使用 DynamoMCP，没有单独的设置或独立界面。Assistant 是提出问题、获取图形帮助以及让 AI 与实时 Dynamo 上下文配合使用的切入点。

您在何处打开 Assistant 取决于您在哪里工作：

- **Dynamo 沙箱** \- Dynamo MCP 工具在 Dynamo 内的 Autodesk Assistant 中可用。
- **Revit** \- Dynamo MCP 工具在 Revit 内的 Autodesk Assistant 中可用。

在这两种情况下，您将停留在当前会话中，并使用相同的 Assistant 体验访问 DynamoMCP。
### DynamoMCP 的用途

DynamoMCP 为 Assistant 提供一组**工具**。每个工具在 Dynamo 中执行一项工作，例如读取画布、放置节点或运行图形。您永远不会自己调用这些工具。您用通俗易懂的语言描述您想要什么，然后 Assistant 会选择所需的工具。

了解这些工具仍然很有用。它会告诉您 Assistant 能够执行的操作，并帮助您表达可以执行的请求。

{% hint style="info" %}
以 `get_` 开头的工具名称仅会从会话中读取信息。所有其他工具都可以更改您的图形。
{% endhint %}

#### 读取当前图形

这些工具可让 Assistant 在回答或进行更改之前查看您正在处理的内容。

| 工具 | 功能 |
| --- | --- |
| `get_workspace_info` | 列出画布上的内容：节点、关联、组、注释和工作空间详细信息。 |
| `get_selection` | 报告选定的节点、关联和组。 |
| `get_node_connections` | 逐个端口显示一个节点的关联对象。 |
| `get_node_property` | 报告节点设置，例如预览、冻结、固定预览和标签。 |
| `get_node_lacing` | 报告节点的连缀设置。 |
| `get_installation_info` | 报告 Dynamo 版本、在其上运行的主机以及指向发行说明的链接。 |

*提示示例：“我的画布上现在有什么？”*

#### 找到正确的节点

Dynamo 有数千个节点，包括来自已安装软件包的节点。这些工具可帮助 Assistant 选择正确的工具，而不是猜测。

| 工具 | 功能 |
| --- | --- |
| `get_nodes_info` | 搜索节点库（包括软件包节点），并返回要放置的确切节点。 |
| `get_node_help` | 返回节点的书面帮助，以及其示例图形（如果存在）的路径。 |
| `get_type_schema` | 返回 Dynamo 类型（例如“点”、“数字”或“颜色”）的预期数据格式。 |

*示例提示：“使用哪个节点来获取曲面的面积？”*

#### 构建和编辑图形

这些工具在画布上完成工作。

| 工具 | 功能 |
| --- | --- |
| `create_nodes` | 使用可选的起始值将一个或多个节点放置在画布上。 |
| `set_node_value` | 设置输入节点的值，例如“数字”、“字符串”、“代码块”和下拉列表。 |
| `connect_nodes` | 将输出端口关联到输入端口。 |
| `disconnect_nodes` | 删除两个节点之间的关联。 |
| `delete_node` | 从画布中移除节点。 |
| `rename_node` | 重命名输入和 Watch 节点，以便明确其用途。功能节点保留其原始名称。 |
| `select_nodes` | 在画布上选择节点，以便您可以查看 Assistant 表示的节点。 |
| `set_node_property` | 打开或关闭节点设置：预览、冻结、固定预览、标签以及输入或输出。 |
| `set_node_lacing` | 将连缀设置为“自动”、“最短”、“最长”或“笛卡尔积”。 |
| `set_node_port_list_at_level` | 在输入端口上设置列表级别。 |

*提示示例：“添加一个数字滑块，并将其关联到我的圆的半径输入。”*

#### 组织和记录图形

工作图形并不总是可读的图形。这些工具处理布局、分组、颜色和注释。

| 工具 | 功能 |
| --- | --- |
| `create_group` | 围绕一组节点放置一个组。 |
| `create_groups` | 在一个步骤中创建多个组。 |
| `ungroup_by_guid` | 删除组。它内部的节点保持在画布上。 |
| `ungroup_by_title` | 按标题删除组。它们内部的节点保持在画布上。 |
| `get_available_group_styles` | 列出可以使用的组样式（包括首选项中的自定义样式）。 |
| `assign_group_styles` | 应用组样式，例如“输入”、“操作”、“输出”和“审阅”。 |
| `get_notes` | 列出图形中的注释。 |
| `create_notes` | 添加说明图形的各个部分的注释。 |
| `update_notes` | 编辑或移动注释文本。 |
| `delete_notes` | 删除注释。 |
| `set_note_state` | 将注释固定到节点，使两者一起移动，或取消固定注释。 |
| `auto_layout_workspace` | 使用内置 Dynamo 自动布局重新排列节点。 |
| `set_workspace_description` | 将图形的描述写入文件的属性。 |

*示例提示：“按输入、操作和输出对此图形进行分组和颜色编码，然后添加简短注释。”*

#### 运行图形并检查结果

| 工具 | 功能 |
| --- | --- |
| `run_workspace` | 运行当前图形。在自动模式下，图形已在每次更改时重新运行，因此将跳过此步骤。 |
| `get_workspace_warnings` | 列出具有警告或错误的节点、消息文本和建议的后续步骤。图形必须先运行。 |
| `get_node_output_values_shapes` | 汇总图形中一个节点或每个节点的输出值。 |
| `get_node_output_value` | 读取单个节点的完整输出，一次读取一页，以获取太大而无法汇总的结果。 |

*示例提示：“运行图形并告诉我为什么 List.GetItemAtIndex 节点显示警告。”*

#### 使用图形文件

Assistant 还可以处理磁盘上的图形，而不仅仅是您打开的图形。

| 工具 | 功能 |
| --- | --- |
| `new_workspace` | 打开新的空图形。如果当前图形有未保存的更改，系统将要求您先保存还是放弃这些更改。 |
| `save_workspace` | 保存当前图形。如果没有选择位置，它将保存到“文档”中的 Dynamo 文件夹。 |
| `edit_graph_from_path` | 打开已保存的图形进行编辑，而不运行它。 |
| `insert_graph_from_path` | 将保存的图形合并到当前画布中。这就是引入节点示例图形的方式。 |
| `run_graph_from_path` | 使用您提供的可选输入值运行已保存的图形。 |
| `run_graph_from_json` | 运行作为 Dynamo JSON 提供的图形，在磁盘上没有文件。 |
| `get_graph_info_from_path` | 读取已保存图形的输入、输出和元数据，而不将其打开。 |
| `get_graph_info_from_json` | 从 Dynamo JSON 提供的图形中读取相同的详细信息。 |
| `get_graphs_info_from_path` | 扫描图形文件夹并报告每个文件夹包含的内容。 |
| `get_available_graphs_folders` | 列出 Dynamo 知道的、可能包含图形的文件夹。 |

*示例提示：“浏览我的 Dynamo 图形文件夹，并告诉我哪些图形放置了墙。”*

{% hint style="warning" %}
Assistant 可以更改图形。请求进行大量编辑之前保存您的工作，并在保存现有文件之前查看结果。
{% endhint %}
