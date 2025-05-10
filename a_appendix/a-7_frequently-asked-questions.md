# 常见问题解答

## 如何使用 Dynamo 内部版本

### 每日构建与稳定构建

按照传统，Autodesk 团队的 Dynamo 会通过在每次提交时发布每日内部版本，以及在系统测试和发布周期后发布稳定内部版本，来保持快速迭代的步伐。我们的团队希望重新启动每日稳定版，以便用户可以控制 DynamoCore 在其磁盘上的本地解压缩位置，这样用户可以放心使用它，而不会影响用于其他 ADSK 产品的 Dynamo。有一些自然的候选者可用于此目的，包括 .nupkg、.zip 文件，或用户可以选择安装路径或其他选项的专用安装程序。

考虑到我们的目标是以最简单的方式为用户提供最新代码，我们决定提供一个 .zip 文件，其中包含 DynamoCore 二进制文件和 Dynamo 沙盒，无需 Revit 即可使用（有一些约束）。

### Dynamo Zip 内部版本

#### 定义和来源

DynamoCoreRuntime zip 版本是在自动生成期间创建的 DynamoCore 二进制文件的快照。

您应该能够在解压缩的文件夹中启动 DynamoSandbox.exe，以便使用最少设置的 Dynamo。

#### 所需组件

| Dynamo Version | Microsoft Visual C++ | DirectX                         |   |   |   |   |
| -------------- | -------------------- | ------------------------------- | - | - | - | - |
| 2.0 - 2.6      | 2015 Redistributable | 10                              |   |   |   |   |
| 2.7            | 2019 Redistributable | 11/12 (included with windows 10 |   |   |   |   |
| >=2.8          | 2019 Redistributable | 11/12 (included with windows 10 |   |   |   |   |

**Microsoft DirectX，它也可以从我们的 Dynamo Github 存储库中公开获得** [**此处**](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)

**7zip，用于解压缩软件包** [**此处**](https://sparanoid.com/lab/7z/download.html)

**Microsoft Visual C++ 2015-2024 Redistributable (x64)** [**链接**](https://aka.ms/vs/17/release/vc_redist.x64.exe)

**可选组件**

几何图形库（它仅适用于特定的 Autodesk 建模工具，如 Revit、Civil 3D、Advance Steel 等）

### 疑难解答

如果解压缩内部版本后根本无法启动 DynamoSandbox.exe，请确保使用 [7zip](https://sparanoid.com/lab/7z/download.html) 解压缩内部版本。如果您对您的计算机有权限，也可以在提取它_之前_手动取消阻止 .zip 归档。

![](images/a-7/dynamo-builds-1.png)

如果缺少任何必需的组件，使用 Dynamo 时可能会遇到问题，并且 UI 的某些部分可能无法加载。

以下面的屏幕截图为例，在没有 GPU 的干净 Windows 10 VM 上解压缩我们的内部版本，计算机缺少两个必需的组件。这在 Dynamo 控制台中有所指示。

![](images/a-7/dynamo-builds-2.png)

**安装 DirectX**

请按照此处的 Microsoft 说明检查是否已安装 DirectX。如果没有，可以在[此处](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)从 Dynamo Github 存储库打开 DXSETUP.exe。看到下面的对话框后，请随时单击“下一步”以将 DirectX 安装到默认位置。

![](images/a-7/dynamo-builds-3.png)

**安装 Microsoft Visual C++ 2015-2024 Redistributable (x64)**

请在[此处](https://aka.ms/vs/17/release/vc_redist.x64.exe)下载最新版本。然后您应该能够在浏览器下载位置中运行名为 vc_redist.x64.exe 的安装程序。看到以下对话框后，请随时单击“安装”以将此组件放置在默认位置。

![](images/a-7/dynamo-builds-4.png)

从上面的链接安装两个必需的组件后，重新启动 DynamoSandbox.exe，您应该会看到以下结果：

![](images/a-7/dynamo-builds-5.png)

**缺少 3D 图形。**

在第一次运行沙盒时，您可能还会遇到图形问题，您可以按照此处的标准图形问题常见问题解答进行操作：

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

通常，在使用 DynamoSandbox.exe 时，可能需要对显卡强制使用高性能 GPU 模式

_NVIDIA 控制面板示例：_

![](images/a-7/dynamo-builds-6.png)

**安装 WebView2 运行时**

目前，后续 Dynamo 模块将使用 WebView2 组件：文档浏览器、导览和库，因此为了确保 Dynamo 的这一部分正确显示 Web 内容，我们需要安装 WebView2 Evergreen 运行时安装程序（您需要验证计算机中是否已安装或需要安装）。

这是安装 WebView2 运行时的链接：[https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/?form=MA13LH#download-section)

![](images/a-7/dynamo-builds-7.png)

应该安装的（只是其中之一）是 Evergreen Bootstrapper 或 Evergreen Standalone Installer，第一个下载 1.50 MB 安装程序，第二个下载 130 MB 安装程序。

在安装运行时后，Dynamo 的后续组件应该能够正常工作：

![](images/a-7/dynamo-builds-8.png)

**Dynamo Excel 节点问题**

可以参考此[文章](https://www.autodesk.com.cn/support/technical/article/caas/sfdcarticles/sfdcarticles/CHS/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html)进行诊断。

### Dynamo 内部版位置

稳定版本

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

每日内部版和稳定版本

[https://dynamobuilds.com/](https://dynamobuilds.com/)
