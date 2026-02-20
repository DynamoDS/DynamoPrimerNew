# 更新 Dynamo 4.x 的软件包和 Dynamo 库

### 简介<a href="#introduction" id="introduction"></a>

本部分包含有关将图形、软件包和库移植到 Dynamo 4.x 时可能所遇到问题的信息。Dynamo 4.0 引入了：
* 重要性能的改进
* 稳定性和错误修复更新
* 代码库的现代化
* 删除了以前在 1.x 中标记为过时的 API
* 从 .NET 8 到 .NET 10 的主要运行时更新
* PythonNet 3 现在是所有新 Python 节点的默认 Python 引擎

.NET 10 迁移工作确保 Dynamo 与 Microsoft 的技术路线图保持一致，远早于 2026 年 11 月结束对 .NET 8 的支持。

启动 Dynamo 4.0 时，如果尚未更新，系统将要求您更新到 .NET 10。软件包作者需要将其项目更新为目标 .NET 10，以确保完全兼容。

在 Dynamo 4.0+ 中创建的所有新 Python 节点都以 PythonNet3 开始。无需担心向后兼容性：对于在多版本软件（例如，Revit 或 Civil 3D 2025/2026）中工作的用户，请在 Dynamo 3.3-3.6 中安装 PythonNet3 引擎软件包以保持兼容性。有关详细信息，可在[此处](https://dynamobim.org/dynamo-core-4-0-release/)找到。

在 1.x 中标记为过时的 API 和节点已在 Dynamo 4.0 中删除。您可以参考此处的[更改完整列表](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0)。

### 软件包兼容性<a href="#package-compatibility" id="package-compatibility"></a>

#### 在 Dynamo 4.x 中使用 Dynamo 2.x 和 3.x 软件包 
由于 Dynamo 4.x 现在在 .NET 10 运行时上运行，因此不能保证为 Dynamo 2.x (* .NET48*) 和 Dynamo 3.x（*使用 .NET 8*）构建的软件包能够在 Dynamo 4.x 中工作。尝试在 Dynamo 4.x 中下载从 Dynamo 版本低于 4.0 发布的软件包时，您会收到一条警告，指出该软件包来自较旧版本的 Dynamo。

**这并不意味着软件包将不工作**。这只是一个警告，指出可能存在兼容性问题；通常，最好检查是否有专为 Dynamo 4.x 构建的较新版本。

在加载软件包时，您还可能会在 Dynamo 日志文件中注意到此类警告。如果一切正常，可以忽略它。

#### 在 Dynamo 2.x 中使用 Dynamo 4.x 软件包 

为 Dynamo 4.x（*使用 .Net 10*）构建的软件包不太可能基于 Dynamo 2.x 运行。尝试在 Dynamo 2.x 中安装为 Dynamo 4.x 构建的软件包时，还会看到以下警告。

![软件包兼容性警告](images/6-2-packages-new-version-compatibility-warning.png)


#### 在 Dynamo 3.x 中使用 Dynamo 4.x 软件包 

为 Dynamo 4.x（*使用 .Net 10*）构建的软件包可以在 Dynamo 3.x 上运行，只要该软件包中使用的所有 API 都存在于 .NET 8 中即可。但不能保证它会奏效。尝试在 Dynamo 3.x 中安装为 Dynamo 4.x 构建的软件包时，还会看到以下警告。

![软件包兼容性警告](images/6-2-packages-compatibility-warning.png)

#### 软件包作者的最佳做法 
最佳做法是通过修改 .csproj，使项目同时支持 .NET 8 和 .NET 10。

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
这可确保：
* 支持仍基于 .NET 8 的 Revit 托管的 Dynamo 版本
* 与 .NET 10 上的单机版 Dynamo 4.x 兼容