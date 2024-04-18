# 更新 Dynamo 3.x 的软件包和 Dynamo 库以及 NET8

### 简介<a href="#introduction" id="introduction"></a>

本部分包含有关将图形、软件包和库移植到 Dynamo 3.x 时可能所遇到问题的信息。

Dynamo 3.0 是一个主版本，一些 API 已更改或已删除。对于作为 Dynamo 3.x 的开发人员或用户的您而言，可能产生影响的最大变化是迁移到 .NET8。

Dotnet/.NET 是运行时，支持 Dynamo 编写所用的 C# 语言。我们已将此运行时以及 Autodesk 生态系统的其余部分一起更新到现代版本。

可以在[我们的博客贴子](https://dynamobim.org/dynamo-on-net-8/)中了解详细信息。
***

### 软件包兼容性<a href="#package-compatibility" id="package-compatibility"></a>

#### 在 Dynamo 3.x 中使用 Dynamo 2.x 软件包 
由于 Dynamo 3.x 现在基于 .NET8 运行时运行，因此并不能保证为 Dynamo 2.x（*使用 .NET48*）构建的软件包能够在 Dynamo 3.x 中工作。尝试在 Dynamo 3.x 中下载从 Dynamo 版本低于 3.0 发布的软件包时，您会收到一条警告，指出该软件包来自较旧版本的 Dynamo。 

**这并不意味着软件包将不工作**。这只是一个警告，指出可能存在兼容性问题；通常，最好检查是否有专为 Dynamo 3.x 构建的较新版本。

在加载软件包时，您还可能会在 Dynamo 日志文件中注意到此类警告。如果一切正常，可以忽略它。

#### 在 Dynamo 2.x 中使用 Dynamo 3.x 软件包 

为 Dynamo 3.x（*使用 .Net8*）构建的软件包不太可能基于 Dynamo 2.x 运行。如果您正在使用较旧版本的 Dynamo，则下载为更高版本的 Dynamo 构建的软件包时，还会看到警告。


