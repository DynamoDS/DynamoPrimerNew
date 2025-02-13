# Defining Custom Package Organisation (Dynamo 2.0+)

Achieving a desired layout for your package depends on the types of nodes you will be including in your package.  Node Model derived nodes, ZeroTouch nodes, and Custom nodes all have a slightly different process for defining the categorization.  You can mix and match these node types within the same package, but it will require a combination of the strategies outlined below.

## NodeModel
NodeModel libraries are organized based on the class structure by default.
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
Node will be located in Add-ons under:
```
SampleLibraryUI/Examples/MyNodeModel
```

You can also override the category by using the NodeCategory attribute on the class or in the constructor as shown below.
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

The node will now be located in Add-ons under:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

ZeroTouch libraries are also organized based on the class structure by default.

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

Node will be located in Add-ons under:

```
MyZTLibrary/Utilities/doubleValue
```

You can also override the class structure location by using a Dynamo Customization XML file.
- The XML file must be named accordingly and be included in the `extra` folder of the package
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

## CustomNodes

Custom nodes are organized based on the specified `Category Name` during the nodes creation (using the new Custom Node dialog box).  

**WARNING!** <br>
Using dot-notation in node Names or Categories will result in additional nested sub-categories.  The `.` will work as a delimiter to determine the additional hierarchy.  This is new behavior in the library for Dynamo 2.0.

![Custom Node Properties](images/custom-node-properties.jpg)

The Category Name can later be updated in the .dyf file (XML or JSON)

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

## Package Node Migration Strategies

When a package author decides to rename previously existing node in a new release they should provide a means for migrating graphs that contain nodes with the old names.  This can be accomplished in the following ways...

**ZeroTouch** nodes use a `Namespace.Migrations.XML` file located in the packages `bin` folder such as:

`MyZeroTouchLib.MyNodes.SayHello` to `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**NodeModel derived nodes** use the `AlsoKnownAs` attribute on the class such as:

`SampleLibraryUI.Examples.DropDownExample` to `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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