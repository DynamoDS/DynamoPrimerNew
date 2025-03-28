# Definir a organização de pacotes personalizados (Dynamo 2.0 e superior)

A obtenção de um layout desejado para o pacote depende dos tipos de nós que você incluirá no pacote. Os nós derivados do modelo de nó, os nós ZeroTouch e os nós personalizados têm um processo ligeiramente diferente para definir a categorização. É possível misturar e combinar esses tipos de nó dentro do mesmo pacote, mas isso exigirá uma combinação das estratégias descritas abaixo.

## NodeModel
Por padrão, as bibliotecas NodeModel são organizadas com base na estrutura de classes.
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
O nó ficará localizado em Complementos em:
```
SampleLibraryUI/Examples/MyNodeModel
```

Também é possível substituir a categoria usando o atributo NodeCategory na classe ou no construtor, como mostrado abaixo.
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

O nó agora ficará localizado em Complementos em:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## ZeroTouch

As bibliotecas ZeroTouch também são organizadas com base na estrutura de classes por padrão.

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

O nó ficará localizado em Complementos em:

```
MyZTLibrary/Utilities/doubleValue
```

Também é possível substituir a localização da estrutura da classe usando um arquivo XML de personalização do Dynamo.
- O arquivo XML deve ser nomeado adequadamente e ser incluído na pasta `extra` do pacote
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

Os nós personalizados são organizados com base no `Category Name` especificado durante a criação dos nós (usando a caixa de diálogo Novo nó personalizado).  

**AVISO** <br>
O uso da notação de ponto em nomes de nós ou categorias resultará em subcategorias aninhadas adicionais. O `.` funcionará como um delimitador para determinar a hierarquia adicional. Esse é um novo comportamento na biblioteca para o Dynamo 2.0.

![Propriedades do nó personalizado](images/custom-node-properties.jpg)

O nome da categoria pode ser atualizado posteriormente no arquivo .dyf (XML ou JSON)

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

## Estratégias de migração de nós do pacote

Quando o autor de um pacote decide renomear um nó existente anterior em uma nova versão, ele deve fornecer um meio para migrar gráficos que contêm nós com os nomes antigos. Isso pode ser feito das seguintes maneiras:

Os nós **ZeroTouch** usam um arquivo `Namespace.Migrations.XML` localizado na pasta `bin` dos pacotes, como:

`MyZeroTouchLib.MyNodes.SayHello` para `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

Os **nós derivados do NodeModel** usam o atributo `AlsoKnownAs` na classe, como:

`SampleLibraryUI.Examples.DropDownExample` para `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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
