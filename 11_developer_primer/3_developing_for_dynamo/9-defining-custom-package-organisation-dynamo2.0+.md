# Определение пользовательской организации пакетов (Dynamo 2.0 или более поздней версии) 

Требуемая компоновка пакета зависит от типов узлов, которые будут включены в пакет. Существуют различия в процессах упорядочивания производных узлов NodeModel, узлов ZeroTouch и пользовательских узлов. В один пакет могут входить разные типы узлов, но для этого потребуется комбинация стратегий, описанных ниже.

## NodeModel
По умолчанию библиотеки NodeModel организованы на основе структуры классов.
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
Узел будет расположен в разделе «Надстройки» по пути:
```
SampleLibraryUI/Examples/MyNodeModel
```

Кроме того, категорию можно переопределить с помощью атрибута NodeCategory в классе или в конструкторе, как показано ниже.
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

Теперь узел будет располагаться в разделе «Надстройки» по пути:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

Библиотеки ZeroTouch также по умолчанию организуются на основе структуры классов.

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

Узел будет расположен в разделе «Надстройки» по пути:

```
MyZTLibrary/Utilities/doubleValue
```

Кроме того, можно также переопределить расположение структуры классов с помощью файла XML адаптации Dynamo.
- Файл XML должен иметь соответствующее имя и содержаться в папке `extra` пакета
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

## Пользовательские узлы

Пользовательские узлы организуются на основе имени категории `Category Name`, указанного при создании с помощью нового диалогового окна «Пользовательский узел».  

**ПРЕДУПРЕЖДЕНИЕ** <br>
Запись через точку в именах или категориях узлов приведет к созданию дополнительных вложенных подкатегорий. В качестве разделителя для определения дополнительной иерархии будет использоваться `.`. Это новое поведение в библиотеке для Dynamo 2.0.

![Свойства пользовательского узла](images/custom-node-properties.jpg)

Имя категории можно будет позже изменить в файле DYF (XML или JSON)

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

## Стратегии переноса узлов пакета

Когда автор пакета решает переименовать ранее существующий узел в новой версии, он должен предоставить средства для переноса графиков, содержащих узлы со старыми именами. Это можно сделать следующими способами.

В узлах **ZeroTouch** используется файл `Namespace.Migrations.XML`, расположенный в папке `bin` пакета, например:

`MyZeroTouchLib.MyNodes.SayHello` в `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**Производные узлы NodeModel** используют атрибут `AlsoKnownAs` класса следующим образом:

`SampleLibraryUI.Examples.DropDownExample` в `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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