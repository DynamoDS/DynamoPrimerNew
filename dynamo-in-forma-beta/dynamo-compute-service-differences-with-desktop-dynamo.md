# Dynamo 计算服务与 Desktop Dynamo 的差异

此页面重点介绍在编写要在 Dynamo 计算服务云上下文中执行的 Dynamo 程序时应注意的差异。

## 什么是 DaaS？

DaaS、Dynamo 即服务、Dynamo 计算服务等均指同一件事：Dynamo 的核心运行时在云上下文中执行。这意味着您的图形不会在您的计算机上执行。DaaS 目前只能通过适用于 Forma 的 Dynamo Player 扩展访问，在该扩展中，用户可以上载和管理在桌面环境中创建的 `.dyn` 文件、通过该扩展运行同事共享的 `.dyn` 文件，或者使用 Autodesk 提供的预加载 `.dyn` 例程作为示例。

由于您的图形运行在此云上下文中，而不是在您的计算机上，因此 DaaS 目前无法直接使用传统的 Dynamo 主机上下文（Revit、Civil 3D 等）。如果要在图形中使用这些程序中的类型，则需要使用 `Data.Remember` 节点或其他图形内序列化技术将它们序列化（保存）到图形中。这些工作流与在 Revit 中为衍生式设计编写图形时需要使用的工作流类似。

## 哪个版本的 Dynamo 正在执行我的代码？

该版本基于 3.x，并根据 Dynamo 的开源主分支经常更新。

## 此版本的 Dynamo 中提供了哪些软件包/节点？

* 大多数核心节点，请参阅下一节了解一些特定限制。
* `DynamoFormaBeta` 用于与 Forma API 交互的软件包。
* `VASA` 用于体素化/高效分析。
* `MeshToolKit` 用于网格操纵。在 Dynamo 3.4 中，“网格”工具包也现成可用。
* `RefineryToolkit` 适用于允许进行碰撞检测、视图距离、最短路径、等轴测等的有用算法。

## 在为 DaaS 编写图表时，我应该注意什么？

* Python 节点将不起作用。这些_当前_无法执行。
* 无法使用自定义软件包。
* UI 节点的 UI/视图层不会执行。我们预计这不是核心功能的问题，但如果您发现与具有自定义 UI 的节点相关的错误，最好记住这一点。
* 仅限 Windows 的功能将不起作用。例如，如果尝试使用 Windows 注册表或 WPF，这将失败。
* 不会加载视图扩展。
* 文件系统节点不会很有用。在 DaaS 中运行时，在本地计算机上参考的任何文件将不存在。
* Excel/DSOffice 互操作节点将不起作用。Open XML 节点应该可以正常运行。
* 虽然可以调用 Forma API，但网络请求通常不起作用。

## 我该如何记住这一切？如果它发生变化怎么办？

* 将来，我们打算在 Dynamo for the Desktop 中提供工具，以便更轻松地确保图形在两个上下文中以相同方式运行。

## 这要花多少钱？

* 在此 Beta 版中，当前不收取计算时间费用。

## 我如何开始使用？

请查看[博客帖子](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/)、[YouTube 系列](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD)或 Forma Extension 中的样例，以便快速入门。这些将指导您完成：

* 获取对 Autodesk Forma 的访问。
* 在桌面上安装适用于 Dynamo 的 DynamoFormaBeta，以及在 Forma 中安装 Dynamo 扩展。
* 编写您的第一张图表。

## 安全性

* 请注意，共享图形存储在 Forma 中。
* 当前最大图表执行时间不到 30 分钟。此值可能会更改。
* 执行请求受到速率限制，因此，如果在太短的时间内发出许多计算请求，可能会看到错误。
