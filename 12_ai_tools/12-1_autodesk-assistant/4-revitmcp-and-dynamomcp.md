# RevitMCP 和 DynamoMCP

Autodesk Assistant 的功能来自 MCP 服务器。每台服务器都为 Assistant 提供一组工具，每个工具在其所属的产品中执行一项作业。

在 Revit 中工作时，其中两个服务器很重要：**RevitMCP** 和 **DynamoMCP**。它们不是竞争性选项，您不会在它们之间做出选择。它们在不同的级别工作，Assistant 会使用符合您要求的级别。

### RevitMCP：直接模型操作

RevitMCP 作为 [Revit 公共 MCP 服务器](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm)提供，将一组精选的 Revit API 操作打包为 MCP 工具。每个工具都映射到模型中定义的操作：查找图元、检查参数和计数、批量编辑参数以及捕获视图快照。

这是工具集已经涵盖的任务的最短路径。一个请求变成一个调用，模型立即发生变化。没有要构建的图形，之后也没有要运行的内容。

### DynamoMCP：构建操作的工具

DynamoMCP 在上一级工作。其工具不直接执行建模操作。相反，它们编写 Dynamo 图形：放置节点、将它们关联在一起、设置值并运行结果。有关完整的工具列表，请参见[什么是 Dynamo MCP 服务器](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md)。

之所以这点很重要，是由节点的定义决定的。Dynamo 库中的每个节点本身是一个或多个 API 调用，已编写、测试和记录。软件包节点和 Python 节点的使用方式相同。因此，一小组图形创作工具可以访问整个 Dynamo 库。

其效果是 DynamoMCP 的工具产生新功能，而不仅仅是使用现有功能。Assistant 会从节点集合您请求的操作，而不是一个固定的操作列表。在实践中，写入图形的 MCP 工具的行为类似于创建更多工具的工具。

### 实践中的差异

假设您要求在一个级别上显示门标记。

- 若由 RevitMCP 处理，这项工作是直接调用，并且会显示标记。
- 若由 DynamoMCP 处理，Assistant 会放置收集门的节点，将它们过滤到您指定的级别，并创建标记，然后运行图形。

两条路线都标记了门。第二条路线也会给您在画布上留下一些东西。

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| 工具的作用 | 执行定义的 Revit 操作 | 构建并运行 Dynamo 图形 |
| 覆盖范围 | 工具集中包含的操作 | 库中的任何节点，包括软件包和 Python 节点 |
| 得到的结果 | 模型中的更改 | 模型中的更改，以及生成它的图形 |
| 最适合 | 一个明确定义的任务 | 要检查、调整、重用或共享的逻辑 |

### 为什么图形值得拥有

图形是工作的记录，而不仅仅是其结果。出现在画布上后，您可以：

- 读取它，看看到底做了什么。
- 更改值并再次运行它，而无需再次要求整个任务。
- 保存它并在下一个项目中运行它，或者交给同事。
- 将其发布到 Dynamo 播放器，以便不使用 Dynamo 的用户可以运行它。

第一个请求是时间用在了哪里。此后的每次运行几乎都是免费的。

{% hint style="info" %}
在 Dynamo 沙箱中，Assistant 仅会访问 DynamoMCP，因为没有可操作的 Revit 模型。在 Revit 中，两者都可用。
{% endhint %}

### 选择要问什么

您无需在提示中命名服务器。描述结果就足够了，然后 Assistant 将选择工具。

如果您特意想要一个图形，请说出来。诸如*“为我构建一个图形来标记标高 2 上的门”之类的提示*将告诉 Assistant 可重用版本才是重点，而不仅仅是标记的门。

### 延伸阅读

- [Autodesk Assistant（技术预览）](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) \- Assistant 在 Revit 中执行的操作（在 Revit 2027 帮助中）。
- [Revit 公共 MCP 服务器（技术预览）](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) \- MCP 的 Revit 侧（在 Revit 2027 帮助中）。
- [Autodesk Assistant：概述和常见问题解答](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) \- 哪些产品包含 Assistant，以及如何使用它。
