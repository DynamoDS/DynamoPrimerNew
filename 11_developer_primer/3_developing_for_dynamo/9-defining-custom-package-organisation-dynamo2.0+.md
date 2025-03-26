# Definieren einer benutzerdefinierten Paketorganisation (Dynamo 2.0+) 

Das gewünschte Layout für Ihr Paket hängt von den Blocktypen ab, die Sie in das Paket aufnehmen. Vom Blockmodell abgeleitete Blöcke, ZeroTouch-Blöcke und benutzerdefinierte Blöcke weisen alle einen leicht unterschiedlichen Prozess zum Definieren der Kategorisierung auf. Sie können diese Blocktypen innerhalb desselben Pakets mischen und anpassen. Dies erfordert jedoch eine Kombination der unten beschriebenen Strategien.

## NodeModel
NodeModel-Bibliotheken sind vorgabemäßig basierend auf der Klassenstruktur organisiert.
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
Der Block befindet sich in Add-Ons unter:
```
SampleLibraryUI/Examples/MyNodeModel
```

Sie können die Kategorie auch überschreiben, indem Sie das NodeCategory-Attribut für die Klasse oder im Konstruktor verwenden, wie unten gezeigt.
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

Der Block befindet sich nun in Add-Ons unter:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

ZeroTouch-Bibliotheken sind vorgabemäßig ebenfalls basierend auf der Klassenstruktur organisiert.

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

Der Block befindet sich in Add-Ons unter:

```
MyZTLibrary/Utilities/doubleValue
```

Sie können den Speicherort der Klassenstruktur auch mithilfe einer XML-Datei für die Dynamo-Anpassung überschreiben.
- Die XML-Datei muss entsprechend benannt und im Ordner `extra` des Pakets enthalten sein.
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

Benutzerdefinierte Blöcke werden während der Blockerstellung auf Grundlage des angegebenen `Category Name` organisiert (mithilfe des neuen Dialogfelds Benutzerdefinierter Block).  

**WARNUNG!** <br>
Die Verwendung der Punktnotation in Blocknamen oder -kategorien führt zu zusätzlichen verschachtelten Unterkategorien. `.` dient als Trennzeichen, um die zusätzliche Hierarchie festzulegen. Dies ist ein neues Verhalten in der Bibliothek für Dynamo 2.0.

![Eigenschaften von benutzerdefinierten Blöcken](images/custom-node-properties.jpg)

Der Kategoriename kann später in der DYF-Datei (XML oder JSON) aktualisiert werden

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

## Strategien für die Paketblock-Migration

Wenn ein Paketautor beschließt, zuvor vorhandene Blöcke in einer neuen Version umzubenennen, sollte er eine Möglichkeit zum Migrieren von Diagrammen bereitstellen, die Blöcke mit den alten Namen enthalten. Dies kann auf folgende Weise erreicht werden...

**ZeroTouch**-Blöcke verwenden eine `Namespace.Migrations.XML`-Datei, die sich im Ordner `bin` der Pakete befindet, z. B.:

`MyZeroTouchLib.MyNodes.SayHello` bis `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**Von NodeModel abgeleitete Blöcke** verwenden das `AlsoKnownAs`-Attribut für die Klasse, z. B.:

`SampleLibraryUI.Examples.DropDownExample` bis `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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
