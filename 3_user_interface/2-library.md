# 库

该库中包含所有已加载的节点，包括安装时附带的 10 个默认类别节点以及任何额外加载的自定义节点或软件包。该库中的节点在库、类别和子类别（如果适用）中按层次进行组织。

![](<images/3-2/library - library UI.jpg>)

* 基本节点：默认安装时随附。
* 自定义节点：将常用例程或特殊图形存储为自定义节点。还可以与社区共享自定义节点
* 软件包管理器中的节点：已发布自定义节点的集合。

我们将浏览[节点层次结构](3-3\_dynamo\_libraries.md#library-hierarchy-for-categories)类别、介绍如何[从库中快速搜索](3-3\_dynamo\_libraries.md#quick-search-in-library)，以及了解其中一些[常用节点](3-3\_dynamo\_libraries.md#frequently-used-nodes)。

### 类别的库层次结构

浏览这些类别是了解我们可以向“工作空间”添加的内容层次的最快方法，也是查找之前尚未使用的新节点的最佳方式。

通过单击菜单展开每个类别及其子类别来浏览库

{% hint style="info" %}
“几何图形”是开始探索的最佳菜单，因为它们包含最多数量的节点。{% endhint %}

![](<images/3-2/library  - modified and resize library categories.jpg>)

> 1. 库
> 2. 种类
> 3. 子类别
> 4. 节点

这些选项会根据节点是 **“创建”** 数据、执行 **“操作”** 还是 **“查询”** 数据，来对同一子类别中的节点进行进一步分类。

* ![](<images/3-2/user interface - create.jpg>) **创建**：从头开始创建或构建几何图形。例如，圆。
* ![](<images/3-2/user interface - action.jpg>) **操作**：对对象执行操作。例如，缩放圆。
* ![](<images/3-2/user interface - query.jpg>) **查询**：获取已存在对象的特性。例如，获取圆的半径。

将鼠标光标悬停在节点上，即可显示除其名称和图标以外的更多详细信息。这使我们可以快速了解节点的作用、所需输入内容以及输出内容。

![](<images/3-2/user interface - node description.jpg>)

> 1. 描述 - 节点的纯语言描述
> 2. 图标 - 库菜单中图标的较大版本
> 3. 输入 - 名称、数据类型和数据结构
> 4. 输出 - 数据类型和结构

### 在库中快速搜索

如果您相对具体地了解要添加到工作空间的节点，请在 **“搜索”** 字段中键入内容以查找所有匹配的节点。

通过单击要添加的节点进行选择，或按 Enter 键将亮显的节点添加到工作空间的中心。

![](<images/3-2/user interface - search.jpg>)

#### 按层次结构搜索

除了使用关键字尝试查找节点之外，我们还可以在“搜索”字段或代码块中键入以句点分隔的层次结构（这使用 _Dynamo 文本语言_）。

每个库的层次结构都会反映在添加到工作空间的节点名称中。

以 `library.category.nodeName` 格式键入“库”层次结构中节点位置的不同部分会返回不同结果

* `library.category.nodeName`

![](<images/3-2/library - search by hierarchy geometry point by coordinates (1).jpg>)

* `category.nodeName`

![](<images/3-2/library - search by hierarchy 2 point by coordinates.jpg>)

* `nodeName`或`keyword`

![](<images/3-2/library - search by hierarchy 3 by coordinates.jpg>)

通常，工作空间中的节点名称将以 `category.nodeName` 格式进行呈现，但存在一些明显例外，尤其是在“输入”和“视图”类别中。

小心类似的命名节点，并注意类别差异：

* 大多数库中的节点将包括类别格式

![](<images/3-2/library - node category differences 1.jpg>)

* `Point.ByCoordinates` 和 `UV.ByCoordinates` 有相同名称，但来自不同类别

![](<images/3-2/library - node category differences 2.jpg>)

* 值得注意的例外情况包括内置函数、Core.Input、Core.View 和运算符

![](<images/3-2/library - node category differences 3.jpg>)

### 常用节点

Dynamo 的基本安装中包含数百个节点，哪些节点对于开发可视化程序至关重要？我们将重点介绍以下节点：定义程序的参数（**“Input”**）、查看节点操作的结果（**“Watch”**）以及通过快捷方式定义输入或功能（**“Code Block”**）。

#### 输入节点

“Input”节点是可视化程序的用户（无论是自己还是他人）与关键参数交互的主要手段。以下是核心库中提供的一些节点：

| 节点 |                                                | 节点 |                                                |
| -------------- | ---------------------------------------------- | -------------- | ---------------------------------------------- |
| 布尔运算 | ![](<images/3-2/library - boolean.jpg>) | 编号 | ![](<images/3-2/library - number.jpg>) |
| 字符串 | ![](<images/3-2/library - string.jpg>) | 数字滑块 | ![](<images/3-2/library - number slider.jpg>) |
| 目录路径 | ![](<images/3-2/library - directory path.jpg>) | 整数滑块 | ![](<images/3-2/library - integer slider.jpg>) |
| 文件路径 | ![](<images/3-2/library - file path.jpg>) |                |                                                |

#### Watch 和 Watch3D

“Watch”节点对于管理流经可视化程序的数据至关重要。将鼠标光标悬停在节点上，即可通过**节点数据预览**查看节点的结果。

![](<images/3-2/library - node preview.jpg>)

它有助于在 **“Watch”** 节点中保持其显示

![](<images/3-2/library - watch node.jpg>)

或者，通过 **“Watch3D”** 节点查看几何图形结果。

![](<images/3-2/library - watch3d node.gif>)

这两个节点均位于核心库的“视图”类别中。

{% hint style="info" %}
提示：如果可视化程序中包含许多节点，则三维预览有时可能会分散注意力。请考虑取消选中“设置”菜单中的“显示背景预览”选项，然后使用“Watch3D”节点预览几何图形。
{% endhint %}

#### 代码块

Code Block 节点可用于定义代码块，其中各行用分号隔开。 这可以像 `X/Y` 一样简单。

我们还可以将“代码块”用作定义“数字输入”或调用另一个节点功能的快捷方式。执行此类操作的语法遵循 Dynamo 文本语言（即 [DesignScript](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md)）的命名约定。

下面是有关在脚本中使用“代码块”的简单演示（带有说明）。

![](<images/3-2/library - code block demo.gif>)

1. 双击以创建“Code Block”节点
2. `Circle.ByCenterPointRadius(x,y);`Type
3. 在工作空间上单击以清除选择内容，应会自动添加 `x` 和 `y` 输入。
4. 创建“Point.ByCoordinates”节点和“数字滑块”，然后将它们连接到“代码块”的输入。
5. 执行可视化程序的结果在三维预览中会显示为圆
