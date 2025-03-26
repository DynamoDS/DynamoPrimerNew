# Definizione dell'organizzazione di pacchetti personalizzati (Dynamo 2.0 e versioni successive)

La realizzazione del layout desiderato per il pacchetto dipende dai tipi di nodi che verranno inclusi nel pacchetto. I nodi derivati da NodeModel, i nodi zero-touch e i nodi personalizzati hanno tutti un processo leggermente diverso per la definizione della categorizzazione. È possibile combinare questi tipi di nodi all'interno dello stesso pacchetto, ma sarà necessaria una combinazione delle strategie descritte di seguito.

## NodeModel
Per default, le librerie NodeModel vengono organizzate in base alla struttura delle classi.
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
Il nodo si troverà in Moduli aggiuntivi in:
```
SampleLibraryUI/Examples/MyNodeModel
```

È inoltre possibile eseguire l'override della categoria utilizzando l'attributo NodeCategory nella classe o nel costruttore come mostrato di seguito.
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

Il nodo si troverà ora in Moduli aggiuntivi in:
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## Zero-touch

Per default, anche le librerie zero-touch vengono organizzate in base alla struttura delle classi.

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

Il nodo si troverà in Moduli aggiuntivi in:

```
MyZTLibrary/Utilities/doubleValue
```

È inoltre possibile sostituire la posizione della struttura delle classi utilizzando un file XML di personalizzazione di Dynamo.
- Il file XML deve essere denominato di conseguenza ed essere incluso nella cartella `extra` del pacchetto.
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

I nodi personalizzati vengono organizzati in base al parametro `Category Name` specificato durante la creazione dei nodi (utilizzando la nuova finestra di dialogo Nodo personalizzato).  

**AVVISO** <br>
L'utilizzo della notazione con il punto nei nomi o nelle categorie dei nodi determinerà ulteriori sottocategorie nidificate. Il segno `.` fungerà da delimitatore per determinare la gerarchia aggiuntiva. Questo è un nuovo comportamento nella libreria per Dynamo 2.0.

![Proprietà dei nodi personalizzati](images/custom-node-properties.jpg)

Il nome della categoria può essere successivamente aggiornato nel file .dyf (XML o JSON)

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

## Strategie di migrazione dei nodi del pacchetto

Quando l'autore di un pacchetto decide di rinominare un nodo precedentemente esistente in una nuova release, dovrebbe fornire un mezzo per eseguire la migrazione dei grafici che contengono nodi con i vecchi nomi. Questa operazione può essere eseguita nei seguenti modi:

I nodi **zero-touch** utilizzano un file `Namespace.Migrations.XML` che si trova nella cartella `bin` dei pacchetti, ad esempio:

Da `MyZeroTouchLib.MyNodes.SayHello` a `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**I nodi derivati da NodeModel** utilizzano l'attributo `AlsoKnownAs` nella classe, ad esempio:

Da `SampleLibraryUI.Examples.DropDownExample` a `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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