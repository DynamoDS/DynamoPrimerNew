# 在 Dynamo 中管理和更新依赖项

### 此 Wiki 何时适用

在使用新功能或仅更新现有依存关系时，应先评估以下内容，然后再将新的依存关系引入 Dynamo 存储库。

### 注意事项

1. 新的或更新的依存关系的许可是什么 - 只有一些开源许可在未与 ADSK 法律部门协商的情况下获得批准。
   * 解析许可后，确保依存关系和版本记录在内部 wiki 中。
   * 如果许可为 `LGPL`、`GPL` 或 `Apache`，则必须将许可文件复制到 Dynamo 内部版本的 _“Open Source Licenses”_ 子文件夹中。
   * 如果许可为 `LGPL`，则必须将所有第三方组件的完整源代码及其相应开源许可的文本副本上传到 [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. 如果是更新，许可类型是否与先前版本不同？
3. 依存关系是否跨平台？
   * 它是否具有本机组件（如 `CEFSharp` 或 `ImageMagick`）？这将使跨平台部署变得更加困难
   * 它是否有仅适用于 Windows 的参照？如果是这样，它不应作为 DynamoCore 或 Dynamo 的其他跨平台部分（模型层）的依存关系。
4. 依存关系是否与其所有必需的依存关系一起正确捆绑到内部版本上的 bin 文件夹中？
   * 如果更新，某些文件是否会因更新而被删除？此版本的 Dynamo 是否适用于主机产品的单点版本？如果是这样，需要保留旧的二进制文件，直到全球发布年份，以支持补丁安装程序。请参见[此处](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me)。
5. 依存关系或其依存关系树是否与 Dynamo 中的其他现有依存关系冲突？
6. 依存关系或其依存关系树是否与在流程中集成 Dynamo 产品（Revit、Civil 等）中的现有依存关系冲突 - **这一点很重要，因为除非预先完成工作，否则只有在集成时才能发现这些问题。**
