# Definiowanie niestandardowej organizacji pakietów (Dynamo 2.0+) 

Osiągnięcie żądanego układu pakietu zależy od typów węzłów, które zostaną uwzględnione w pakiecie. Węzły pochodne Node Model, węzły ZeroTouch i węzły niestandardowe mają nieco inne procesy definiowania kategoryzacji. Możesz mieszać i zestawiać te typy węzłów w tym samym pakiecie, ale będzie to wymagało łączenia różnych strategii opisanych poniżej.

## NodeModel
Biblioteki NodeModel są domyślnie zorganizowane w oparciu o strukturę klas.
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
Węzeł będzie zlokalizowany w obszarze dodatków w ścieżce:
```
SampleLibraryUI/Examples/MyNodeModel
```

Kategorię można również nadpisać, używając atrybutu NodeCategory w klasie lub w konstruktorze, jak pokazano poniżej.
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

Węzeł będzie teraz znajdował się w obszarze dodatków w ścieżce:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

Biblioteki ZeroTouch są domyślnie organizowane w oparciu o strukturę klas.

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

Węzeł będzie zlokalizowany w obszarze dodatków w ścieżce:

```
MyZTLibrary/Utilities/doubleValue
```

Położenie struktury klas można również nadpisać za pomocą pliku XML dostosowywania dodatku Dynamo.
- Plik XML musi mieć odpowiednią nazwę i być umieszczony w folderze `extra` pakietu
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

## Węzły niestandardowe

Węzły niestandardowe są organizowane na podstawie wartości `Category Name` określonej podczas ich tworzenia (za pomocą nowego okna dialogowego Węzeł niestandardowy).  

**OSTRZEŻENIE!** <br>
Użycie zapisu kropkowego w nazwach lub kategoriach węzłów spowoduje utworzenie dodatkowych podkategorii zagnieżdżonych. Kropka `.` będzie pełnić rolę separatora określającego dodatkową hierarchię. Jest to nowe zachowanie w bibliotece dodatku Dynamo 2.0.

![Właściwości węzła niestandardowego](images/custom-node-properties.jpg)

Nazwę kategorii można później zaktualizować w pliku .dyf (XML lub JSON)

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

## Strategie migracji węzłów z pakietów

Gdy autor pakietu decyduje się zmienić nazwę wcześniej istniejącego węzła w nowej wersji, powinien zapewnić mechanizmy migracji wykresów zawierających węzły o starych nazwach. Można to zrealizować na następujące sposoby...

Węzły **ZeroTouch** używają pliku `Namespace.Migrations.XML` znajdującego się w folderze pakietów `bin`, na przykład:

`MyZeroTouchLib.MyNodes.SayHello` na `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**Węzły pochodne NodeModel** używają atrybutu `AlsoKnownAs` klasy, na przykład:

`SampleLibraryUI.Examples.DropDownExample` na `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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
