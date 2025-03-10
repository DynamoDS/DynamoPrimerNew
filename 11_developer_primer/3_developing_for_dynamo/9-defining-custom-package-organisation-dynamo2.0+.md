# 사용자 지정 패키지 구성 정의(Dynamo 2.0+)

패키지에 원하는 레이아웃을 구현하는 것은 패키지에 포함할 노드의 유형에 따라 달라집니다. NodeModel에서 파생된 노드, ZeroTouch 노드, 사용자 지정 노드는 모두 분류를 정의하는 프로세스가 약간씩 다릅니다. 이러한 노드 유형을 동일한 패키지 내에서 혼합하여 사용할 수 있지만, 아래에 설명된 전략을 결합해야 합니다.

## NodeModel
NodeModel 라이브러리는 기본적으로 클래스 구조를 기반으로 구성됩니다.
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
노드는 다음과 같이 애드온 아래에 있습니다.
```
SampleLibraryUI/Examples/MyNodeModel
```

아래와 같이 클래스 또는 생성자에서 NodeCategory 속성을 사용하여 카테고리를 재지정할 수도 있습니다.
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

이제 노드는 다음과 같이 애드온 아래에 위치하게 됩니다.
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

ZeroTouch 라이브러리도 기본적으로 클래스 구조를 기반으로 구성됩니다.

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

노드는 다음과 같이 애드온 아래에 있습니다.

```
MyZTLibrary/Utilities/doubleValue
```

Dynamo 사용자 지정 XML 파일을 사용하여 클래스 구조 위치를 재지정할 수도 있습니다.
- XML 파일은 적절하게 이름을 지정하고 패키지의 `extra` 폴더에 넣어야 합니다
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

## 사용자 지정 노드

사용자 지정 노드는 (새로운 사용자 지정 노드 대화상자를 사용하여) 노드 생성 시 지정된 `Category Name`을 기반으로 구성됩니다.  

**경고!** <br>
노드 이름 또는 카테고리에서 점 표기법을 사용하면 중첩된 하위 카테고리가 추가로 생성됩니다. `.`은 추가적인 계층을 결정하는 구분 기호 역할을 합니다. 이는 Dynamo 2.0 라이브러리의 새로운 동작입니다.

![사용자 지정 노드 특성](images/custom-node-properties.jpg)

카테고리 이름은 나중에 .dyf 파일(XML 또는 JSON)에서 업데이트할 수 있습니다

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

## 패키지 노드 마이그레이션 전략

패키지 작성자가 새로운 릴리즈에서 기존 노드 이름을 변경하기로 결정한 경우 이전 이름을 가진 노드가 포함된 그래프를 마이그레이션할 수 있는 방법을 제공해야 합니다. 이는 다음과 같은 방법으로 수행할 수 있습니다.

**ZeroTouch** 노드는 다음과 같은 패키지 `bin` 폴더에 있는 `Namespace.Migrations.XML` 파일을 사용합니다.

`MyZeroTouchLib.MyNodes.SayHello`를 `MyZeroTouchLib.MyNodes.SayHelloRENAMED`로 변경
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**NodeModel에서 파생된 노드**는 다음과 같은 클래스에서 `AlsoKnownAs` 속성을 사용합니다.

`SampleLibraryUI.Examples.DropDownExample`을 `SampleLibraryUI.Examples.DropDownExampleRENAMED`로 변경
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