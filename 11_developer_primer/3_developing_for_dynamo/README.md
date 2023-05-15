# 为 Dynamo 开发

无论用户的经验水平如何，Dynamo 平台是专为所有用户设计的，旨在使他们都能够成为参与者。有多个针对不同能力和技能水平的开发选项，每个选项都有其优势和劣势，具体取决于目标。下面，我们将概述不同的选项以及如何选择其中一个。

![三种开发环境](images/developing-for-dynamo.png)

> 三种开发环境：Visual Studio、Python 编辑器和代码块 DesignScript

#### 我有哪些选项？<a href="#what-are-my-options" id="what-are-my-options"></a>

Dynamo 的开发选项主要分为两类：_for_（面向）Dynamo 与 _in_（使用）Dynamo。可以将这两个类别视为如下所述：“in”Dynamo 表示使用 Dynamo IDE 创建要在 Dynamo 中使用的内容，而“for”Dynamo 表示使用外部工具创建要输入到 Dynamo 中以供使用的内容。尽管本手册重点介绍 _for_ Dynamo 开发，但下面介绍了适用于所有过程的资源。

#### For（面向）Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

这些节点允许最高程度的自定义。许多软件包都是使用此方法构建的，它对于参与 Dynamo 的源代码而言必不可少。本手册将介绍构建这些软件包的过程。

* Zero-Touch 节点
* NodeModel 派生节点
* 扩展

> 本 Primer 提供了有关[输入 Zero-Touch 库](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/5-zero-touch)的指南。

在下面的讨论中，Visual Studio 将用作 Zero-Touch 和 NodeModel 节点的开发环境。

![Visual Studio 界面](images/vs-devenv.jpg)

> 我们将要开发的项目的 Visual Studio 界面

#### In（使用）Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

尽管这些过程存在于可视化编程工作空间中并且相对简单，但它们都是用于自定义 Dynamo 的可行选项。本 Primer 涵盖了这些过程，并在[脚本编写策略](http://dynamoprimer.com/en/12\_Best-Practice/12-1\_Scripting-Strategies.html)一章中提供了脚本编写技巧和最佳实践。

*   代码块会在可视化编程环境中显示 DesignScript，从而支持灵活的文本脚本和节点工作流。代码块中的函数可由工作空间中的任何对象调用。

    > 下载代码块示例（单击鼠标右键，然后单击“另存为”），或在 [Primer](https://primer.dynamobim.org/07\_Code-Block/7-1\_what-is-a-code-block.html) 中查看详细漫游。
*   自定义节点是节点集合甚至整个图形的容器。它们是收集常用例程并与社区共享这些例程的有效方式。

    > 下载自定义节点示例（单击鼠标右键，然后单击“另存为”），或在 [Primer](https://primer.dynamobim.org/10\_Custom-Nodes/10-1\_Introduction.html) 中查看详细漫游。
*   Python 节点是可视化编程工作空间中的脚本接口，类似于代码块。Autodesk.DesignScript 库使用与 DesignScript 类似的点表示法。

    > 下载 Python 节点示例（单击鼠标右键，然后单击“另存为”），或在 [Primer](https://primer.dynamobim.org/10\_Custom-Nodes/10-4\_Python.html) 中查看详细漫游。

在 Dynamo 工作空间中开发是获得即时反馈的强大工具。

![在 Dynamo 工作空间中使用 Python 节点开发](images/python-example.jpg)

> 在 Dynamo 工作空间中使用 Python 节点开发

#### 每种方法有哪些优点/缺点？<a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Dynamo 的开发选项旨在解决自定义需求的复杂性。无论目标是在 Python 中编写递归脚本还是构建完全自定义的节点 UI，都有一些选项可用于实现仅涉及启动和运行所需内容的代码。

**Dynamo 中的代码块、Python 节点和自定义节点**

这些都是用于在 Dynamo 可视化编程环境中编写代码的简单选项。通过 Dynamo 可视化编程工作空间，可访问 Python、DesignScript，还可以在自定义节点内包含多个节点。

![代码块、Python 脚本和自定义节点](images/Development-Icons.png)

使用这些方法，我们可以：

* 开始编写 Python 或 DesignScript，而几乎无需设置。
* 将 Python 库输入到 Dynamo 中。
* 将代码块、Python 节点和自定义节点作为软件包的一部分与 Dynamo 社区共享。

**Zero-Touch 节点**

“Zero-Touch”是指一种用于输入 C# 库的简单点击方法。Dynamo 将读取 `.dll` 的公有方法，并将这些方法转换为 Dynamo 节点。可以使用 Zero-Touch 来开发您自己的自定义节点和软件包。

![Zero-Touch 节点](images/ZTImport.png)

使用此方法，我们可以：

* 输入不一定是为 Dynamo 开发的库，并自动创建一组新节点，如 Primer 中的 [A-Forge 示例](http://dynamoprimer.com/en/10\_Packages/10-5\_Zero-Touch.html)
* 编写 C# 方法，并在 Dynamo 中轻松将这些方法用作节点
* 通过软件包将 C# 库作为节点与 Dynamo 社区共享

**NodeModel 派生节点**

通过这些节点，可以更深入地了解 Dynamo 的结构。它们基于 `NodeModel` 类，并使用 C# 编写。尽管此方法提供了最大的灵活性和强大功能，但节点的大多数方面都必须明确定义，并且函数需要位于单独的程序集中。

![NodeModel 派生节点](images/Development-Icons-NodeModel.png)

使用此方法，我们可以：

* 使用滑块、图像、颜色等创建完全可自定义的节点 UI（例如 ColorRange 节点）
* 访问并影响 Dynamo 画布中发生的情况
* 自定义连缀
* 作为软件包载入到 Dynamo 中

#### 了解 Dynamo 版本控制和 API 更改 (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

由于 Dynamo 会定期更新，因此可能会对软件包使用的 API 的一部分进行更改。跟踪这些更改对于确保现有软件包继续正常工作而言非常重要。

API 更改是在 [Dynamo Github Wiki](https://github.com/DynamoDS/Dynamo/wiki/API-Changes) 上进行跟踪的。这包括对 DynamoCore、库和工作空间的更改。

![Dynamo API 更改文档](images/api-changes.jpg)

举例来说，2.0 版中即将发生的重大更改是从 XML 文件格式转换为 JSON 文件格式。NodeModel 派生节点现在需要 [JSON 构造函数](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node)，否则它们将无法在 Dynamo 2.0 中打开。

Dynamo 的 API 文档目前涵盖了核心功能：[http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI)

![API 文档](images/api-docs.jpg)

#### 在软件包中分发二进制文件的权限 <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

请注意，.dll 包含在要上传到软件包管理器的软件包中。如果软件包作者未创建 .dll，则他们必须有权共享它。

如果软件包包含二进制文件，则在下载时必须提示用户该软件包包含二进制文件。
