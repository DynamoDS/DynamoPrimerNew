# Définition de l’organisation des packages personnalisés (Dynamo 2.0+) 

L’organisation de votre package dépend des types de nœuds que vous allez y inclure. Les nœuds dérivés du modèle de nœud, les nœuds Zero Touch et les nœuds personnalisés ont tous un processus de catégorisation légèrement différent. Vous pouvez mélanger et faire correspondre ces types de nœuds dans le même package, mais cela nécessitera une combinaison des stratégies décrites ci-dessous.

## NodeModel
Les bibliothèques NodeModel sont organisées en fonction de la structure de classe par défaut.
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
Le nœud se trouve dans les modules complémentaires sous :
```
SampleLibraryUI/Examples/MyNodeModel
```

Vous pouvez également remplacer la catégorie à l’aide de l’attribut NodeCategory dans la classe ou dans le constructeur, comme illustré ci-dessous.
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

Le nœud se trouve désormais dans les modules complémentaires sous :
```
NewSampleLibraryUI/Examples/MyNodeModel
```

## Zero Touch

Les bibliothèques Zero Touch sont également organisées en fonction de la structure de classe par défaut.

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

Le nœud se trouve dans les modules complémentaires sous :

```
MyZTLibrary/Utilities/doubleValue
```

Vous pouvez également remplacer l’emplacement de la structure de classe à l’aide d’un fichier XML Dynamo Customization.
- Vous devez nommer le fichier XML en conséquence et l’ajouter au dossier `extra` du package
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

## Nœuds personnalisés

Les nœuds personnalisés sont organisés en fonction du `Category Name` spécifié lors de la création des nœuds (à l’aide de la nouvelle boîte de dialogue Nœud personnalisé).  

**AVERTISSEMENT !** <br>
L’utilisation de la notation par points dans les noms ou catégories de nœuds entraîne la création de sous-catégories imbriquées supplémentaires. Le `.` agit comme un délimiteur pour déterminer la hiérarchie supplémentaire. Il s’agit d’un nouveau comportement dans la bibliothèque de Dynamo 2.0.

![Propriétés de nœuds personnalisés](images/custom-node-properties.jpg)

Vous pouvez mettre à jour le nom de la catégorie ultérieurement dans le fichier .dyf (XML ou JSON)

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

## Stratégies de migration des nœuds de package

Lorsqu’un créateur de package décide de renommer un nœud existant dans une nouvelle version, il doit migrer les graphes qui contiennent des nœuds avec les anciens noms. La migration peut être réalisée des manières suivantes...

**Les nœuds Zero Touch** utilisent un fichier `Namespace.Migrations.XML` situé dans le dossier `bin` des packages tel que :

`MyZeroTouchLib.MyNodes.SayHello` vers `MyZeroTouchLib.MyNodes.SayHelloRENAMED`
```XML
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>MyZeroTouchLib.MyNodes.SayHello</oldName>
    <newName>MyZeroTouchLib.MyNodes.SayHelloRENAMED</newName>
  </priorNameHint>
</migrations>
```

**Les nœuds dérivés de NodeModel** utilisent l’attribut `AlsoKnownAs` de la classe, par exemple :

`SampleLibraryUI.Examples.DropDownExample` vers `SampleLibraryUI.Examples.DropDownExampleRENAMED`
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
