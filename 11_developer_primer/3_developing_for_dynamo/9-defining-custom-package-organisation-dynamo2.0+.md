# 定義自訂套件組織 (Dynamo 2.0+)

要讓套件達到想要的配置，取決於您要納入套件中的節點類型。NodeModel 衍生的節點、ZeroTouch 節點和自訂節點，定義分類方式的程序都略有不同。您可以在同一個套件中混合使用這些節點類型，但需要結合使用下面概述的策略。

## NodeModel
NodeModel 資源庫預設會根據類別結構進行整理。
```C#
namespace SampleLibraryUI.Examples
```
```C#
// Class Attribute
[NodeName("MyNodeModel")]
public class MyNewNodeModel : NodeModel

// or

// Constructor
public ButtonCustomNodeModel()
{
    this.Name = "MyNodeModel";
}

```
節點將位於附加元件的以下位置：
```
SampleLibraryUI/Examples/MyNodeModel
```

您也可以在類別上或在建構函式中使用 NodeCategory 屬性來取代該品類，如下所示。
```C#
// Class Attribute
[NodeCategory("NewSampleLibraryUI.Examples")]

// or

// Constructor
public ButtonCustomNodeModel()
{
    this.Category = "NewSampleLibraryUI.Examples";
}
```

節點現在將位於附加元件中的以下位置：
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

ZeroTouch 資源庫預設也會根據類別結構進行整理。

```C#
namespace MyZTLibrary
```

```C#
public class Utilities
{
    public double doubleValue(double num)
    {
        return num * 2;
    }
}
```

節點將位於附加元件的以下位置：

```
MyZTLibrary/Utilities/doubleValue
```

您也可以使用 Dynamo 自訂 XML 檔案取代類別結構位置。
- XML 檔案必須相應地命名，並放入套件的 `extra` 資料夾中
    - `PackageName_DynamoCustomization.xml`

```XML
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>MeshToolkit</name>
    </assembly>
    <namespaces>
        <!--Remap Namespaces-->
        <namespace name="Autodesk.Dynamo.MeshToolkit">
            <category>MeshToolkit</category>
        </namespace>
        <namespace name="Autodesk.Dynamo.MeshToolkit.Display">
                <category>Display</category>
        </namespace>
    </namespaces>
    <classes>
        <!--Remap Class Names-->
        <class name="Autodesk.Dynamo.MeshToolkit.Display.MeshDisplay" shortname="MeshDisplay"/>
        <class name="Autodesk.Dynamo.MeshToolkit.Mesh" shortname="Mesh"/>
    </classes>
</doc>

```

## 自訂節點

自訂節點是根據建立節點期間 (使用新的「自訂節點」對話方塊) 中指定的 `Category Name` 來進行整理。  

**警告！**<br>
在節點名稱或品類中使用點符號，將會產生額外的巢狀子品類。`.` 將用作分隔符號來決定其他階層。這是 Dynamo 2.0 資源庫中的新行為。

![自訂節點性質](images/custom-node-properties.jpg)

之後可在 .dyf 檔案 (XML 或 JSON) 中更新品類名稱

```JSON
{
  "Uuid": "85066088-1616-40b1-96e1-c33e685c6948",
  "IsCustomNode": true,
  "Category": "MyCustomNodes.Utilities.Actions",
  "Description": "This is an example custom nodes.",
  "Name": "doubleValue",
  "ElementResolver": {
    "ResolutionMap": {}
  },...
```

```XML
<Workspace Version="1.3.0.0000" X="100" Y="100" zoom="1.0000000" Description="This is an example custom nodes." Category="MyCustomNodes.Utilities.Actions" Name="doubleValue" ID="85066088-1616-40b1-96e1-c33e685c6948">
```

## 套件節點移轉策略

當套件作者決定在新版本中更名先前存在的節點時，他們應該提供方法來移轉包含舊名稱節點的圖表。可以透過以下方式完成...

**ZeroTouch** 節點使用套件 `bin` 資料夾中的 `Namespace.Migrations.XML` 檔案，例如：

`MyZeroTouchLib.MyNodes.SayHello` 變為 `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**NodeModel 衍生的節點**在類別上使用 `AlsoKnownAs` 屬性，例如：

`SampleLibraryUI.Examples.DropDownExample` 變為 `SampleLibraryUI.Examples.DropDownExampleRENAMED`
```C#
namespace SampleLibraryUI.Examples
{
    [NodeName("Drop Down Example")]
    [NodeDescription("An example drop down node.")]
    [IsDesignScriptCompatible]
    [AlsoKnownAs("SampleLibraryUI.Examples.DropDownExample")]
    public class DropDownExampleRENAMED : DSDropDownBase
    {
        ...
    }
{
```