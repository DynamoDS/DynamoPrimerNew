# 设置自己的 Python 模板

使用 Dynamo 2.0，我们可以指定在首次打开 Python 窗口时要使用的默认模板 `(.py extension)`。这是一个渴望已久的请求，因为这可加快 Dynamo 内 Python 的使用。通过使用模板，我们可以在计划开发自定义 Python 脚本时准备好默认导入。

此模板的位置位于 Dynamo 安装的 `APPDATA` 位置。

这通常如下所示：`( %appdata%\Dynamo\Dynamo Core\{version}\ )`。

![](../images/8-3/3/pythontemplates-appdatafolderlocation.jpg)

### 设置模板

为了能够利用此功能，我们需要在 `DynamoSettings.xml` 文件中添加以下行。_（在记事本中编辑）_

![](../images/8-3/3/pythontemplates-dynamosettingsxmlfile.png)

在我们看到 `<PythonTemplateFilePath />` 的位置，只需将其替换为以下内容：

```
<PythonTemplateFilePath>
<string>C:\Users\CURRENTUSER\AppData\Roaming\Dynamo\Dynamo Core\2.0\PythonTemplate.py</string>
</PythonTemplateFilePath>
```

{% hint style="warning" %} _注意：将 CURRENTUSER 替换为您的用户名_ {% endhint %}

接下来，我们需要使用要内置的功能构建模板。在本例中，我们在使用 Revit 时嵌入 Revit 相关的导入和一些其他典型项目。

您可以开始一个空白记事本文档，并在其中粘贴以下代码：

``` py
import clr

clr.AddReference('RevitAPI')
from Autodesk.Revit.DB import *
from Autodesk.Revit.DB.Structure import *

clr.AddReference('RevitAPIUI')
from Autodesk.Revit.UI import *

clr.AddReference('System')
from System.Collections.Generic import List

clr.AddReference('RevitNodes')
import Revit
clr.ImportExtensions(Revit.GeometryConversion)
clr.ImportExtensions(Revit.Elements)

clr.AddReference('RevitServices')
import RevitServices
from RevitServices.Persistence import DocumentManager
from RevitServices.Transactions import TransactionManager

doc = DocumentManager.Instance.CurrentDBDocument
uidoc=DocumentManager.Instance.CurrentUIApplication.ActiveUIDocument

#Preparing input from dynamo to revit
element = UnwrapElement(IN[0])

#Do some action in a Transaction
TransactionManager.Instance.EnsureInTransaction(doc)

TransactionManager.Instance.TransactionTaskDone()

OUT = element
```

完成后，将此文件在 `APPDATA` 位置中另存为 `PythonTemplate.py`。

### 之后的 Python 脚本行为

完成定义 Python 模板后，Dynamo 会在每次放置 Python 节点时查找该模板。如果找不到，它将看起来像默认的 Python 窗口。

![](../images/8-3/3/pythontemplates-beforesetuptemplate.jpg)

如果找到 Python 模板（如我们的 Revit 模板），则您会看到所有内置的默认项目。

![](../images/8-3/3/pythontemplates-aftersetuptemplate.jpg)

有关此出色附加功能（由 Radu Gidei 提供）的其他信息可以在此处找到。https://github.com/DynamoDS/Dynamo/pull/8122
