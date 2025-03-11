# Definición de la organización de paquetes personalizados (Dynamo 2.0+)

Para obtener el diseño deseado para el paquete, hay que tener en cuenta los tipos de nodo que se incluirán en el paquete. Los nodos derivados de NodeModel, los nodos ZeroTouch y los nodos personalizados requieren un proceso ligeramente diferente para definir la categorización. Puede mezclar y combinar estos tipos de nodos dentro del mismo paquete, pero tendrá que emplear una combinación de las estrategias que se describen a continuación.

## NodeModel
Las bibliotecas de NodeModel se organizan por defecto en función de la estructura de clases.
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
El nodo se ubicará en Complementos en:
```
SampleLibraryUI/Examples/MyNodeModel
```

También puede modificar la categoría mediante el atributo NodeCategory en la clase o en el constructor, como se muestra a continuación.
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

El nodo ahora se encontrará en Complementos en:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

Las bibliotecas de ZeroTouch también se organizan por defecto en función de la estructura de clases.

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

El nodo se ubicará en Complementos en:

```
MyZTLibrary/Utilities/doubleValue
```

También puede modificar la ubicación de la estructura de clases mediante un archivo XML de personalización de Dynamo.
- El archivo XML debe denominarse como corresponde y se debe incluir en la carpeta `extra` del paquete
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

## Nodos personalizados

Los nodos personalizados se organizan en función del `Category Name` especificado durante la creación del nodo (mediante el nuevo cuadro de diálogo Nodo personalizado).  

**ADVERTENCIA** <br>
El uso de la notación de puntos en los nombres o categorías de nodos genera subcategorías anidadas adicionales. El `.` funciona como un delimitador para determinar la jerarquía adicional. Este comportamiento es nuevo en la biblioteca de Dynamo 2.0.

![Propiedades de nodo personalizado](images/custom-node-properties.jpg)

El nombre de categoría se puede actualizar posteriormente en el archivo .dyf (XML o JSON).

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

## Estrategias para la migración de nodos de paquete

Cuando un autor de un paquete decide cambiar el nombre de un nodo existente anteriormente en una nueva versión, debe proporcionar un medio para migrar los gráficos que contengan nodos con los nombres antiguos. Esto se puede lograr de las siguientes maneras...

Los nodos **ZeroTouch** utilizan un archivo `Namespace.Migrations.XML` ubicado en la carpeta `bin` de los paquetes, como por ejemplo:

`MyZeroTouchLib.MyNodes.SayHello` a `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

Los **nodos derivados de NodeModel** utilizan el atributo `AlsoKnownAs` en la clase, como por ejemplo:

`SampleLibraryUI.Examples.DropDownExample` a `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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