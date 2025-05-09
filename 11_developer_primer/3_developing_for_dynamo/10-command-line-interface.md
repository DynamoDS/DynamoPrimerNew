# Dynamo 命令行界面

`-o, -O, --OpenFilePath` 指示 Dynamo 打开命令文件并运行它在此路径中包含的命令。仅当从 DynamoSandbox 运行时，才支持此选项。  

`-c, -C, --CommandFilePath` 指示 Dynamo 打开命令文件并运行它在此路径中包含的命令。仅当从 DynamoSandbox 运行时，才支持此选项。  

`-v, -V, --Verbose` 指示 Dynamo 将其执行的所有评估输出到指定路径处的 XML 文件。  

`-g, -G, --Geometry` 指示 Dynamo 将所有计算中的几何图形输出到此路径上的 JSON 文件。  

`-h, -H, --help` 获得一些帮助。  

`-i, -I, --Import` 指示 Dynamo 将部件作为节点库输入。此参数应该是通向单个 `.dll` 的文件路径。如果要导入多个 `.dlls`，请用空格分隔列出它们：`-i 'assembly1.dll' 'assembly2.dll'`。  

`--GeometryPath` 包含 ASM 的目录的相对或绝对路径。提供后，系统不会在硬盘中搜索 ASM，而是直接从此路径加载。  

`-k, -K, --KeepAlive` Keepalive 模式，使 Dynamo 进程保持运行状态，直到加载的扩展将其关闭。  

`--HostName` 确定与主机关联的 Dynamo 变化。  

`-s, -S, --SessionId` 确定 Dynamo 主机分析会话 ID。  

`-p, -P, --ParentId` 标识 Dynamo 主机分析父对象 ID。  

`-x, -X, --ConvertFile` 与 `-O` 标志结合使用时，将从指定路径打开 `.dyn` 文件并将其转换为 `.json`。文件将具有 `.json` 扩展名，并与原始文件位于同一目录中。  

`-n, -N, --NoConsole` 不要依赖控制台窗口在 Keepalive 模式下与 CLI 交互。  

`-u, -U, --UserData` 使用 CLI 指定 PathResolver 要使用的用户数据文件夹。  

`--CommonData` 使用 CLI 指定 PathResolver 要使用的公共数据文件夹。  

`--DisableAnalytics` 在进程使用寿命内禁用 Dynamo 中的分析。  

`--CERLocation` 指定位于磁盘上的崩溃错误报告工具。  

`--ServiceMode` 指定服务模式启动。  



#### 原因 
 您可能出于各种原因而要从命令行控制 Dynamo，这些原因可能包括： 
 
 * 自动执行多个 Dynamo 运行
 * 测试 Dynamo 图形（使用 DynamoSandbox 时，另请查看 -c）
 * 按特定顺序运行 Dynamo 图形序列
 * 编写运行多个命令行执行的批处理文件
 * 编写另一个程序来控制和自动执行 Dynamo 图形的运行，并利用这些计算结果执行一些很酷的操作

#### 内容
命令行界面 (DynamoCLI) 是对 DynamoSandbox 的补充。它是一个 DOS/终端命令行实用程序，旨在为运行 Dynamo 提供命令行参数的便利。在第一次实现中，它不是独立的运行，它必须从 Dynamo 二进制文件所在的文件夹中运行，因为它依赖于与沙盒相同的核心 DLL。它不能与其他内部版本的 Dynamo 互操作。

有四种方法可以运行 CLI：从 Dos 提示符、从 Dos 批处理文件，以及作为 Windows 桌面快捷方式（其路径已修改为包含指定的命令行标志）。Dos 文件规范可以是完全限定的或相对的，并且还支持映射驱动器和 URL 语法。也可以使用 Mono 构建它，并从终端在 Linux 或 Mac 上运行。

该实用程序支持 Dynamo 软件包，但不能加载自定义节点 (dyf)，只能加载独立图表 (dyn)。

在初步测试中，CLI 实用程序支持 Windows 的本地化版本，并且您可以使用大写 ASCII 字符指定 filespec 参数。

可以通过 DynamoCLI.exe 应用程序访问 CLI。此应用程序允许用户或其他应用程序通过使用命令字符串调用 DynamoCLI.exe 与 Dynamo 评估模型进行交互。这可能如下所示：
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
此命令将告知 Dynamo 在 *“C：\\someReallyCoolDynamoFile.Dyn”* 打开指定的文件，而不绘制任何 UI，然后运行它。图形完成运行后，Dynamo 将退出。 

**2.1 版中的新功能**：DynamoWPFCLI.exe 应用程序。此应用程序通过添加几何图形 (-g) 选项支持 DynamoCLI.exe 应用程序支持的所有内容。DynamoWPFCLI.exe 应用程序仅适用于 Windows。

#### 重要说明

* 与 DynamoCLI 交互的首选方法是通过命令提示符界面。
* 此时，您需要从 [Dynamo 版本] 文件夹内的安装位置运行 DynamoCLI。CLI 需要访问与 Dynamo 相同的 .dll，因此不应移动它。
* 您应该能够执行当前在 Dynamo 中打开的图形，但这可能会导致意外的附带后果。
* 所有文件路径都完全符合 DOS 要求，因此相对路径和完全限定路径应该起作用，但请确保将路径用引号括起来 *“C:path\\to\\file.dyn”* 
* DynamoCLI 是一项新功能，目前正在不断变化：**CLI 目前仅加载标准 Dynamo 库的子集**，如果图形未正确执行，请注意这一点。这些库在[此处](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28)指定 
* 当前不提供 std 输出，如果没有遇到错误，CLI 将在运行完成后退出。
 
#### 方法

`-o` 您可以在将运行图形的无头模式下打开指向 .dyn 的 Dynamo。

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v` 当 Dynamo 在无头模式下运行时（当我们已使用 `-o` 打开文件时）可以使用，此标志将迭代图表中的所有节点，并将其输出值转储为简单的 XML 文件。由于 `--ServiceMode` 标志可以强制 Dynamo 运行多个图表解算，因此输出文件将保存发生的每个解算的值。

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
XML 输出文件的格式如下：
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` 当 Dynamo 在无头模式下运行时（当我们已使用 `-o` 打开文件时）可以使用，此标志将生成图表，然后将生成的几何图形转储为 JSON 文件。 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
JSON 几何图形文件将具有以下形式：

 待定 - 正在处理中

`-h` 使用此项可获取可能选项的列表

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

-i 标志可以多次使用，以导入尝试打开的图形需要运行的多个部件。

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

-l 标志可用于在不同的区域设置下运行 Dynamo。但通常，区域设置不会影响图表结果

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

--GeometryPath 标志可用于将 DynamoSandbox 或 CLI 指向特定的 ASM 二进制文件集 - 像这样使用

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

或

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

-k 标志可用于使 Dynamo 进程保持运行状态，直到载入的扩展将其关闭。

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

--HostName 标志可用于标识与主机关联的 Dynamo 变体。

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

或

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

-s 标志可用于标识 Dynamo 主机分析会话 ID

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

-p 标志可用于标识 Dynamo 主机分析父对象 ID

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
