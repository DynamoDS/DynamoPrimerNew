# 在 Zero-Touch 节点中执行 Python 脚本 (C#)

### 在 Zero-Touch 节点中执行 Python 脚本 (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

如果您愿意使用 Python 编写脚本，并希望从标准 Dynamo Python 节点中获取更多功能，我们可以使用 Zero-Touch 创建自己的节点。让我们从一个简单示例开始，它允许我们将 Python 脚本作为字符串传递给 Zero-Touch 节点，在该节点中执行脚本并返回结果。本案例研究将建立在“快速入门”部分中的漫游和示例的基础上；如果您对创建 Zero-Touch 节点完全不熟悉，请参见这些漫游和示例。

![将执行 Python 脚本字符串的 Zero-Touch 节点](../../.gitbook/assets/python-case-study.png)

> 将执行 Python 脚本字符串的 Zero-Touch 节点

#### Python 引擎 <a href="#python-engine" id="python-engine"></a>

如果您从 CPython 迁移，PythonNet3 现在是默认引擎，带来更流畅的体验。在 Dynamo 4.0+ 中创建的所有新 Python 节点都以 PythonNet3 开始。

此节点依赖于 IronPython 脚本引擎的实例。为此，我们需要参照一些其他程序集。按照以下步骤操作，以在 Visual Studio 中设置基本模板：

* 创建新的 Visual Studio 类项目
* 添加对 `C:\Program Files (x86)\IronPython 2.7\IronPython.dll` 中 `IronPython.dll` 的参照
* 添加对 `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll` 中 `Microsoft.Scripting.dll` 的参照
* 在类中包含 `IronPython.Hosting` 和 `Microsoft.Scripting.Hosting` `using` 语句
* 添加一个私有的空构造函数，以防止将其他节点随软件包一起添加到 Dynamo 库中
* 创建一个将单个字符串作为输入参数的新方法
* 在此方法中，我们将实例化一个新的 Python 引擎，并创建一个空的脚本作用域。可以将此作用域想象为 Python 解释器实例中的全局变量
* 接下来，在引擎上调用 `Execute`，以将输入字符串和作用域作为参数传递
* 最后，通过在作用域上调用 `GetVariable` 并传递 Python 脚本中包含要返回的值的变量的名称，来检索并返回脚本的结果。（有关更多详细信息，请参见下面的示例）

以下代码提供了上述步骤的示例。构建解决方案将在项目的 bin 文件夹中创建一个新的 `.dll`。现在，此 `.dll` 可以作为软件包的一部分或通过导航到 `File < Import Library...` 以输入到 Dynamo 中

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Python 脚本将返回变量 `output`，这意味着我们在 Python 脚本中需要一个 `output` 变量。使用此样例脚本以在 Dynamo 中测试节点。如果您曾在 Dynamo 中使用过 Python 节点，则以下内容应该很熟悉。有关详细信息，请查看[入门手册的 Python 部分](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python)。

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### 多个输出 <a href="#multiple-outputs" id="multiple-outputs"></a>

标准 Python 节点的一个限制是它们只有一个输出端口；因此，如果我们要返回多个对象，我们必须构造一个列表并检索其中的每个对象。如果我们修改上面的示例以返回字典，则我们可以根据需要添加许多输出端口。有关字典的更多详细信息，请参见“进一步了解 Zero-Touch”中的“返回多个值”部分。

![此节点允许我们返回立方体的体积及其质心。](../../.gitbook/assets/python-multi-case-study.png)

> 此节点允许我们返回立方体的体积及其质心。

让我们通过以下步骤来修改前面的示例：

* 通过 NuGet 软件包管理器添加对 `DynamoServices.dll` 的参照
* 除了以前的程序集外，还包括 `System.Collections.Generic` 和 `Autodesk.DesignScript.Runtime`
* 修改我们方法的返回类型以返回一个包含我们输出的字典
* 必须从作用域中单独检索每个输出（考虑为较大的输出集设置一个简单的循环）

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

我们还向样例 Python 脚本添加了一个额外输出变量 (`output2`)。请记住，这些变量可以使用任何合法的 Python 命名约定；为了清晰起见，在本例中已严格使用输出。

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### PythonNet3 已知限制和解决方法<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

下面是使用 PythonNet3 时的一些已知限制和解决方法

* .NET 集合不会自动转换为 Python 列表
    * 在使用 ```len()```、索引或迭代之前，必须使用 ```list(...)``` 显式转换 ```.NET``` 数组或集合。
* 泛型 .NET 方法可能需要显式类型参数
    * 除非您手动指定泛型类型而不是依赖自动推理，否则某些方法（例如 ```GroupBy```）将失败。
* 无法通过 ```dir()``` 发现扩展方法或自动完成扩展方法
    * 扩展方法在显式调用时可能仍然有效，但它们不会出现在自检或代码补全中。
* 不支持 DataTable 扩展方法
    * 导入 ```System.Data.DataTableExtensions``` 失败；不能直接使用这些帮助程序方法。
* 某些 Dynamo 核心方法在 PythonNet3 中的行为方式不同
    * 由于收集处理更加严格，某些功能（例如列表展平）可能无法按预期工作。
* 如果 Python 类继承自 .NET 类型，则无法在节点之间传递这些类
    * 派生自 .NET 类型或接口的类无法在 Python 节点之间安全地传递。
* Python ```set()``` 不接受某些 .NET 对象
    * 类似 ```InvalidElementId``` 的对象必须过滤掉或使用 .NET 集合进行处理。
* 频繁的 ```print()``` 调用会导致内存增长
    * 避免在循环或长时间运行的脚本中大量使用 ```print()```。
* Dynamo 和 Python 之间的词典互操作性受到限制
    * Dynamo 词典和 Python 词典不能完全互换，可能需要手动转换。
* 获取指定对象的运行 COM 实例的方法 ```Marshal.GetActiveObject()``` 不再可用
    * 如果您知道所使用的文件的路径，请使用 ```BindToMoniker```。
    * 使用类结构 ```Marshal.GetActiveObject()``` 以 C# 编写库代码

#### 从 CPython3 移植到 PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo 会自动将 CPython 节点移植到 PythonNet 3。以下是具体情况：

> 1. 将自动创建原始文件的备份副本。
> 2. 所有 CPython 节点（包括使用 CPython 的自定义节点）都将转换为 PythonNet3。 
> 3. 一则提示通知可让您知道已迁移的节点数。
> 4. 保存时，您将看到一条提醒，提示您的 Python 节点现在将使用 PythonNet3。同样，无需担心向后兼容性：对于在多版本软件（例如，Revit 或 Civil 3D 2025/2026）中工作的用户，请在 Dynamo 3.3-3.6 中安装 PythonNet3 引擎软件包以保持兼容性。 

#### 从 IronPython2 移植到 PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

如果图形使用 IronPython 引擎，则不会自动迁移。 

如果安装了匹配的 IronPython 软件包，则图形将正常运行。如果缺少该软件包，您将在“工作空间参照”扩展中看到一条依存关系警告，要求您下载该软件包。您可以通过重新安装软件包继续使用 IronPython。但是，由于 IronPython 已经好几年没有更新了，而且 Dynamo 很久没有在 Dynamo 中积极支持这些引擎了，因此我们强烈建议您移植到 PythonNet3，以确保您的图形能够持续可靠地运行。虽然 DynamoIronPython2.7 和 DynamoIronPython3 将继续在 Dynamo Package Manager 上作为软件包提供，但它们将不再由 Dynamo 团队维护。 

在这种情况下，您可以使用的移植选项是利用 Python 编辑器中提供的移植助手进行节点移植。  

有关移植的详细信息，请参见此[博客](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/)