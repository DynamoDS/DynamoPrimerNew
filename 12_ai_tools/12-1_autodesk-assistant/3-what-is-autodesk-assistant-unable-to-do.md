# Autodesk Assistant 尚不能执行哪些操作

### 更新软件包

Autodesk Assistant 尚无法下载软件包并将其安装到 Dynamo 中

![](../../.gitbook/assets/Aa_Update.jpg)

如上图所示，Autodesk Assistant 无法修改、安装或卸载与 Dynamo 关联的外部软件包。它能够提供说明，以便您可以自己安装它们。


### 直接读取和写入文件

Autodesk Assistant 使用 Dynamo 图形文件：它可以打开图形、将其插入画布、运行并保存工作。它无法自行读取或写入硬盘上的其他文件。

在下面的示例中，有一些在画布上创建的几何图形。系统将要求 Autodesk Assistant 获取构成几何图形的点，并将其导出为 JSON。虽然 Autodesk Assistant 无法直接生成此 JSON，但它确实建议可以创建 Dynamo 脚本来执行此操作。

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

该建议是要记住的模式。图形外部的任何内容都是通过节点访问的，而不是通过 Assistant 本身访问的。

### 在没有图形的情况下更改 Revit 模型

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

适用于 Dynamo 的 Autodesk Assistant 与 Revit 模型没有直接连接。让它像请求 Revit 中的 Assistant 那样编辑模型，它会告诉您它不能。

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

它仍然可以更改 Revit 模型。它按照您的方式执行此操作：放置 Revit 节点，将它们关联在一起，然后模型在图形运行时发生更改。

![](../../.gitbook/assets/Aa_viewScript.jpg)

以这种方式工作会带来两件事：

- **在图形运行之前，不会有任何变化。**您可以先读取图形并调整值。只有在图形运行时，才会触及模型，无论是由您还是由 Assistant 作为您请求的一部分。
- **更改是可重复的。**图形将保留在画布上。保存它，然后可以在另一个模型上再次运行相同的任务，或与您的团队共享。

如果您想要进行单个、不涉及图形的直接编辑，请使用 Revit 中的 Autodesk Assistant。
