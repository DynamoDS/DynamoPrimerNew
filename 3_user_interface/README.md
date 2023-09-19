# 用户界面

### 用户界面概述

Dynamo 的用户界面 (UI) 分为五个主要区域。我们将在此简要介绍概述，然后在以下各部分中进一步介绍工作空间和库。

![](images/userinterface-ui.jpg)

> 1. 菜单
> 2. 工具栏
> 3. 库
> 4. 工作空间
> 5. 执行栏

### 菜单

![](../.gitbook/assets/userinterface-menu\(1\).jpg)

以下是 Dynamo 应用程序基本功能的菜单。与大多数 Windows 软件一样，前两个菜单涉及文件管理、选择操作和内容编辑。其余菜单则更加特定于 Dynamo。

#### Dynamo 菜单

常规信息和设置位于 **“Dynamo”** 下拉菜单中。

![](images/userinterface-dynamomenu.jpg)

> 1. 关于 - 了解计算机上安装的 Dynamo 版本。
> 2. 同意收集可用性数据 - 这允许您选择加入或退出共享您的用户数据以改进 Dynamo。
> 3. 首选项 - 包括定义应用程序的小数点精度和几何图形渲染质量等设置。
> 4. 退出 Dynamo

#### 帮助

如果遇到问题，请查看 **“帮助”** 菜单。可以通过 Internet 浏览器访问其中一个 Dynamo 参考网站。

![](images/userinterface-helpmenu.jpg)

> 1. 快速入门 - 简要介绍如何使用 Dynamo。
> 2. 互动指南 -
> 3. 示例 - 参考示例文件。
> 4. Dynamo 词典 - 包含所有节点文档的资源。
> 5. Dynamo 网站 - 在 GitHub 上查看 Dynamo 项目。
> 6. Dynamo Project Wiki - 访问 Wiki 以了解如何使用 Dynamo API、支持库和工具进行开发。
> 7. 显示开始页面 - 在文档内时返回 Dynamo 开始页面。
> 8. 报告错误 - 在 GitHub 上打开问题。

### 工具栏

Dynamo 工具栏包含一系列按钮，可快速处理文件以及访问“Undo [Ctrl + Z]”和“Redo [Ctrl + Y]”命令。最右侧是另一个按钮，它将输出工作空间快照，这对于文档编制和共享非常重要。

* ![](images/userinterface-newfile.jpg) 新建 - 创建新的 .dyn 文件
* \![](<images/userinterface-open(1) (1) (1).jpg>) 打开 - 打开现有 .dyn（工作空间）或 .dyf（自定义节点）文件
* ![](images/userinterface-save.jpg) 保存/另存为 - 保存活动的 .dyn 或 .dyf 文件
* ![](images/userinterface-undo.jpg) 撤消 - 撤消上一个操作
* ![](images/userinterface-redo.jpg) 重做 - 重做下一个操作
* ![](images/userinterface-screenshot.jpg) 将工作空间输出为图像 - 将可见工作空间输出为 PNG 文件

### 库

Dynamo 库是功能库的集合，每个库都包含按类别分组的节点。它包含在默认安装 Dynamo 期间添加的基本库，随着我们继续介绍其用法，我们将演示如何通过自定义节点和其他软件包扩展基本功能。[2-library.md](2-library.md "提及")部分将介绍有关如何使用它的更详细指导。

![](images/userinterface-library.jpg)

### 工作空间

在工作空间中，我们可以编写可视化程序，您还可以更改其“预览”设置以从此处查看三维几何图形。有关更多详细信息，请参见 [1-workspace.md](1-workspace.md "提及")。

![](images/userinterface-workspace.gif)

### 执行栏

从此处运行 Dynamo 脚本。单击“执行”按钮上的下拉图标，可在不同模式之间切换。

![](images/userinterface-executionbar.gif)

* 自动：自动运行脚本。更改会实时更新。
* 手动：仅当单击“运行”按钮时，脚本才会运行。对更改复杂且“繁重”的脚本很有用。
* 周期：默认情况下，此选项灰显。仅当使用 _DateTime.Now_ 节点时可用。可以将图形设置为按指定的间隔自动运行。

![](images/userinterface-executionbarDateTimenode.jpg)
