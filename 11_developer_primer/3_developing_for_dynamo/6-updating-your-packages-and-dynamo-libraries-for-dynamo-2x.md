# Mettre à jour vos packages et bibliothèques Dynamo pour Dynamo 2.x 

### Introduction : <a href="#introduction" id="introduction"></a>

Dynamo 2.0 est une version majeure et certaines API ont été modifiées ou supprimées. L’un des changements les plus importants qui affectera les auteurs de nœuds et de packages est le passage au format de fichier JSON.

Dans la plupart des cas, les auteurs de nœuds Zero-Touch n’auront que peu ou pas de travail à faire pour que leurs packages fonctionnent dans la version 2.0.

Les nœuds d’interface utilisateur et les nœuds qui dérivent directement de NodeModel nécessiteront plus de travail pour fonctionner dans la version 2.x.

Les auteurs d’extensions peuvent également avoir des changements potentiels à faire selon la quantité d’API de Dynamo Core qu’ils utilisent dans leurs extensions.

***

### Règles générales relatives aux packages : <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* n’intégrez pas les fichiers .dll de Dynamo ou de Dynamo Revit dans votre package. Ces fichiers .dll seront déjà chargés par Dynamo. Si vous intégrez une version différente de celle que l’utilisateur a chargée _(p. ex. vous distribuez sur Dynamo Core 1.3 mais l’utilisateur exécute votre package sur Dynamo 2.0)_, des bogues d’exécution inattendus se produiront. Cela inclut les fichiers .dll tels que `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll` ;
* évitez d’intégrer et de distribuer des `newtonsoft.json.net` avec votre package si vous pouvez l’éviter. Ce fichier .dll sera également chargé par Dynamo 2.x. Le même problème que ci-dessus peut se produire ;
* évitez d’intégrer et de distribuer des `CEFSharp` avec votre package si vous pouvez l’éviter. Ce fichier .dll sera également chargé par Dynamo 2.x. Le même problème que ci-dessus peut se produire ;
* de manière générale, évitez de partager des dépendances avec Dynamo ou Revit si vous avez besoin de contrôler la version de cette dépendance.



### Problèmes courants : <a href="#common-issues" id="common-issues"></a>

1) Lors de l’ouverture d’un graphique, certains nœuds ont plusieurs ports portant le même nom, mais le graphique semble correct lors de l’enregistrement. Ce problème peut avoir plusieurs causes.

La cause principale est que le nœud a été créé à l’aide d’un constructeur qui a recréé les ports. Au lieu de cela, il aurait fallu utiliser un constructeur capable de charger les ports. Ces constructeurs sont généralement marqués `[JsonConstructor]`. _Voir les exemples ci-dessous_

![JSON endommagé](images/broken-json.jpg)

Cela peut se produire pour les raisons suivantes :

* il n’y avait tout simplement pas de `[JsonConstructor]` correspondant, ou il n’a pas été transmis par les fichiers `Inports` et `Outports` du fichier JSON .dyn ;
* deux versions de JSON.net ont été chargées simultanément dans le même processus, ce qui a entraîné une erreur au niveau de l’exécution .net, de sorte que l’attribut `[JsonConstructor]` n’a pas pu être utilisé correctement pour marquer le constructeur ;
* une version de DynamoServices.dll différente de la version actuelle de Dynamo a été incluse dans le package et provoque l’échec de l’exécution .net à identifier l’attribut `[MultiReturn]`, de sorte que les nœuds Zero-Touch marqués avec divers attributs ne pourront pas être appliqués. Il se peut qu’un nœud renvoie une seule sortie de dictionnaire au lieu de plusieurs ports.

2) Des nœuds sont complètement absents lors du chargement du graphique avec des erreurs dans la console.

* Cela peut se produire si votre désérialisation a échoué pour une raison quelconque. Il est recommandé de ne sérialiser que les propriétés dont vous avez besoin. Nous pouvons utiliser `[JsonIgnore]` sur les propriétés complexes que vous n’avez pas besoin de charger ou d’enregistrer pour les ignorer. Il s’agit de propriétés telles que `function pointer, delegate, action,` ou `event` etc. Elles ne doivent pas être sérialisées car elles échoueront généralement à la désérialisation et provoqueront une erreur d’exécution.


### Mise à niveau détaillée : <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Nœuds personnalisés 1.3 - > 2.0<a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Organiser les nœuds personnalisés dans librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Problèmes connus :

* un nom de nœud personnalisé et un nom de catégorie coïncidant au même niveau dans librarie.js provoque un comportement inattendu. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) : évitez d’utiliser les mêmes noms pour la catégorie et les nœuds ;
* les commentaires seront transformés en commentaires de bloc au lieu de commentaires de ligne ;
* les noms de type abrégés seront remplacés par des noms complets. Par exemple, si vous n’avez pas spécifié de type lorsque vous chargez à nouveau le nœud personnalisé, `var[]..[]` s’affiche, car il s’agit du type par défaut.


### Nœuds Zero-Touch 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* Dans Dynamo 2.0, les types Liste et Dictionnaire ont été divisés et la syntaxe pour créer des listes et des dictionnaires a été modifiée. Les listes sont initialisées à l’aide de `[]` tandis que les dictionnaires utilisent `{}`.\
 Si vous utilisiez auparavant l’attribut `DefaultArgument` pour marquer les paramètres de vos nœuds Zero-Touch et utilisiez la syntaxe de liste par défaut pour une liste spécifique telle que `someFunc([DefaultArgument("{0,1,2}")])`, ceci ne sera plus valable et vous devrez modifier l’extrait de code DesignScript pour utiliser la nouvelle syntaxe d’initialisation pour les listes.
* Comme indiqué ci-dessus, ne distribuez pas de fichiers .dll Dynamo avec vos packages. (`DynamoCore`, `DynamoServices`, etc.).


### Nœuds de modèle de nœud 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Les nœuds de modèle de nœud nécessitent le plus de travail pour passer à Dynamo 2.x. De manière générale, vous devez implémenter des constructeurs qui seront utilisés uniquement pour charger vos nœuds à partir de json, en plus des constructeurs nodeModel standard utilisés pour instancier de nouvelles instances de vos types de nœud. Pour les différencier, vous devez marquer les constructeurs de chargement à l’aide de l’attribut `[JsonConstructor]` de newtonsoft.Json.net.

Les noms des paramètres du constructeur doivent généralement correspondre aux noms des propriétés JSON, bien que ce mappage soit plus compliquée si vous remplacez les noms sérialisés à l’aide d’attributs [JsonProperty].\
 [Pour plus d’informations, reportez-vous à la documentation de Json.net.](https://www.newtonsoft.com/json/help/html/Introduction.htm)


#### Constructeurs JSON <a href="#json-constructors" id="json-constructors"></a>

La modification la plus courante requise pour mettre à jour les nœuds dérivés de la classe de base `NodeModel` (ou d’autres classes de base Dynamo Core, par exemple `DSDropDownBase`) est la nécessité d’ajouter un constructeur JSON à votre classe.

Votre constructeur original sans paramètre gérera toujours l’initialisation d’un nouveau nœud créé au sein de Dynamo (via la bibliothèque par exemple). Le constructeur JSON est requis pour initialiser un nœud désérialisé _(chargé)_ à partir d’un fichier .dyn ou .dyf enregistré.

Le constructeur JSON diffère du constructeur de base, car il possède des paramètres `PortModel` pour les `inPorts` et les `outPorts`, fournis par la logique de chargement JSON. L’appel pour enregistrer les ports pour le nœud n’est pas requis ici, car les données existent dans le fichier .dyn. Voici un exemple de constructeur JSON :

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Cette syntaxe `:base(Inports,outPorts){}` appelle le constructeur de base `nodeModel` et lui transmet les ports désérialisés.

Il n’est pas nécessaire de répéter dans ce constructeur toute logique spéciale qui existait dans le constructeur de la classe et qui impliquait l’initialisation de données spécifiques sérialisées dans le fichier .dyn _(par exemple la définition de l’enregistrement du port, la stratégie de combinaison, etc.)_, car ces valeurs peuvent être lues à partir du JSON.

C’est la principale différence entre le constructeur JSON et les constructeurs non-JSON pour vos nodeModels. Les constructeurs JSON sont appelés lors du chargement à partir d’un fichier et les données chargées sont transmises. D’autres éléments de la logique utilisateur doivent cependant être dupliqués dans le constructeur JSON _(par exemple, l’initialisation des gestionnaires d’événements pour le nœud ou l’association)_.

Des exemples sont disponibles dans le dépôt DynamoSamples -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) ou [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123).


#### Propriétés publiques et sérialisation <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Auparavant, un développeur pouvait sérialiser et désérialiser des données de modèle spécifiques dans le document XML à l’aide des méthodes `SerializeCore` et `DeserializeCore`. Ces méthodes existent toujours dans l’API, mais seront abandonnées dans une version ultérieure de Dynamo (un exemple est disponible [ici](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Avec l’implémentation JSON.NET, les propriétés `public` de la classe dérivée NodeModel peuvent être sérialisées directement dans le fichier .dyn. JSON.Net fournit plusieurs attributs pour contrôler la façon dont la propriété est sérialisée.

Cet exemple qui spécifie un `PropertyName` se trouve [ici](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38) dans le dépôt Dynamo.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`


#### Convertisseurs : <a href="#converters" id="converters"></a>

**Remarque**\
 Si vous créez votre propre classe de convertisseur JSON.net, Dynamo ne dispose pas actuellement d’un mécanisme qui vous permet de l’injecter dans les méthodes de chargement et d’enregistrement. Ainsi, même si vous marquez votre classe avec l’attribut `[JsonConverter]`, elle ne peut pas être utilisée, à la place vous pouvez appeler votre convertisseur directement dans votre setter ou getter. _//À FAIRE : avoir la confirmation de cette limitation. Toute preuve est la bienvenue._

Un exemple qui spécifie une méthode de sérialisation pour convertir la propriété en chaîne est disponible [ici](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) dans le dépôt Dynamo.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`


#### Ignorer les propriétés <a href="#ignoring-properties" id="ignoring-properties"></a>

Les propriétés `public` qui ne sont pas destinées à la sérialisation doivent être accompagnées de l’attribut `[JsonIgnore]`. Lorsque les nœuds sont enregistrés dans le fichier .dyn, cela garantit que ces données sont ignorées par le mécanisme de sérialisation et qu’elles n’auront pas de conséquences inattendues lorsque le graphique sera ouvert à nouveau. Vous pouvez en trouver un exemple [ici](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45), dans le dépôt Dynamo.

***

#### Annuler/Rétablir<a href="#undoredo" id="undoredo"></a>

Comme indiqué ci-dessus, les méthodes `SerializeCore` et `DeserializeCore` étaient utilisées dans le passé pour enregistrer et charger des nœuds dans le fichier xml .dyn. En outre, elles étaient également utilisées pour enregistrer et charger l’état des nœuds pour annuler/rétabliret le **sont toujours !** Si vous souhaitez mettre en œuvre une fonctionnalité complexe Annuler/Rétablir pour votre nœud de l’interface utilisateur nodeModel, vous devrez mettre en œuvre ces méthodes et sérialiser dans l’objet document XML fourni en tant que paramètre de ces méthodes. Il s’agit d’un cas d’utilisation rare, sauf pour les nœuds complexes de l’interface utilisateur.


#### API des ports d’entrée et de sortie <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

L’enregistrement du port dans le constructeur du nœud est un phénomène courant dans les nœuds nodeModel concernés par les modifications de l’API 2.0. Si vous avez déjà regardé des exemples dans le dépôt Dynamo ou DynamoSamples, il est possible que vous ayez utilisé les méthodes `InPortData.Add()` ou `OutPortData.Add()`. Auparavant, dans l’API Dynamo, les propriétés publiques `InPortData` et `OutPortData` étaient marquées comme étant obsolètes. Dans la version 2.0, ces propriétés ont été supprimées. Les développeurs doivent maintenant utiliser les méthodes `InPorts.Add()` et `OutPorts.Add()`. En outre, ces deux méthodes `Add()` ont des signatures légèrement différentes :

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

comparé à

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Des exemples de code converti sont disponibles dans le dépôt Dynamo -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) ou [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281)

L’autre cas d’utilisation courant affecté par les modifications de l’API 2.0 est lié aux méthodes couramment utilisées dans la méthode `BuildAst()` pour déterminer le comportement du nœud en fonction de la présence ou de l’absence de connecteurs de port. Auparavant, `HasConnectedInput(index)` était utilisé pour valider un état de port connecté. Les développeurs doivent maintenant utiliser la propriété `InPorts[0].IsConnected` pour vérifier l’état de la connexion au port. Un exemple de ceci peut être trouvé dans [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) dans le dépôt Dynamo.


### Exemples : <a href="#examples" id="examples"></a>

Nous allons voir comment mettre à jour un nœud d’interface utilisateur 1.3 vers Dynamo 2.x.

```
using System;
using System.Collections.Generic;
using Dynamo.Graph.Nodes;
using CustomNodeModel.CustomNodeModelFunction;
using ProtoCore.AST.AssociativeAST;
using Autodesk.DesignScript.Geometry;

namespace CustomNodeModel.CustomNodeModel
{
    [NodeName("RectangularGrid")]
    [NodeDescription("An example NodeModel node that creates a rectangular grid. The slider randomly scales the cells.")]
    [NodeCategory("CustomNodeModel")]
    [InPortNames("xCount", "yCount")]
    [InPortTypes("double", "double")]
    [InPortDescriptions("Number of cells in the X direction", "Number of cells in the Y direction")]
    [OutPortNames("Rectangles")]
    [OutPortTypes("Autodesk.DesignScript.Geometry.Rectangle[]")]
    [OutPortDescriptions("A list of rectangles")]
    [IsDesignScriptCompatible]
    public class GridNodeModel : NodeModel
    {
        private double _sliderValue;
        public double SliderValue
        {
            get { return _sliderValue; }
            set
            {
                _sliderValue = value;
                RaisePropertyChanged("SliderValue");
                OnNodeModified(false);
            }
        }
        public GridNodeModel()
        {
            RegisterAllPorts();
        }
        public override IEnumerable<AssociativeNode> BuildOutputAst(List<AssociativeNode> inputAstNodes)
        {
            if (!HasConnectedInput(0) || !HasConnectedInput(1))
            {
                return new[] { AstFactory.BuildAssignment(GetAstIdentifierForOutputIndex(0), AstFactory.BuildNullNode()) };
            }
            var sliderValue = AstFactory.BuildDoubleNode(SliderValue);
            var functionCall =
              AstFactory.BuildFunctionCall(
                new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid),
                new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });

            return new[] { AstFactory.BuildAssignment(GetAstIdentifierForOutputIndex(0), functionCall) };
        }
    }
}
```

Pour charger et enregistrer correctement cette classe `nodeModel` dans la version 2.0, il suffit d’ajouter un jsonConstructor pour gérer le chargement des ports. Nous passons simplement les ports au constructeur de base et cette implémentation est vide.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Remarque : n’appelez pas `RegisterPorts()` ou une variante de ce paramètre dans votre JsonConstructor. Cela utilisera les attributs des paramètres d’entrée et de sortie de votre classe de nœuds pour construire de nouveaux ports ! **Ce n’est pas ce que nous souhaitons**, puisque nous voulons utiliser les ports chargés qui sont passés au constructeur.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

Cet exemple ajoute le constructeur JSON le moins chargé possible. Mais qu’en est-il si nous avons besoin d’une logique de construction plus complexe, comme la mise en place d’écouteurs pour la gestion des événements à l’intérieur du constructeur. L’échantillon suivant, tiré du \
[dépôt DynamoSamples](https://github.com/DynamoDS/DynamoSamples), est lié ci-dessus dans la `JsonConstructors Section` de ce document.

Voici un constructeur plus complexe pour un nœud d’interface utilisateur :

```
 public ButtonCustomNodeModel()
        {
            // When you create a UI node, you need to do the
            // work of setting up the ports yourself. To do this,
            // you can populate the InPorts and the OutPorts
            // collections with PortData objects describing your ports.
            InPorts.Add(new PortModel(PortType.Input, this, new PortData("inputString", "a string value displayed on our button")));

            // Nodes can have an arbitrary number of inputs and outputs.
            // If you want more ports, just create more PortData objects.
            OutPorts.Add(new PortModel(PortType.Output, this, new PortData("button value", "returns the string value displayed on our button")));
            OutPorts.Add(new PortModel(PortType.Output, this, new PortData("window value", "returns the string value displayed in our window when button is pressed")));

            // This call is required to ensure that your ports are
            // properly created.
            RegisterAllPorts();

            // Listen for input port disconnection to trigger button UI update
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;

            // The arugment lacing is the way in which Dynamo handles
            // inputs of lists. If you don't want your node to
            // support argument lacing, you can set this to LacingStrategy.Disabled.
            ArgumentLacing = LacingStrategy.Disabled;

            // We create a DelegateCommand object which will be 
            // bound to our button in our custom UI. Clicking the button 
            // will call the ShowMessage method.
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);

            // Setting our property here will trigger a 
            // property change notification and the UI 
            // will be updated to reflect the new value.
            ButtonText = defaultButtonText;
            WindowText = defaultWindowText;
        }
```

Lorsque nous ajoutons un constructeur JSON pour charger ce nœud à partir d’un fichier, nous devons recréer une partie de cette logique, mais notez que nous n’incluons pas le code qui crée les ports, définit la combinaison ou les valeurs par défaut des propriétés que nous pouvons charger à partir du fichier.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Notez que les autres propriétés publiques qui ont été sérialisées dans le JSON comme `ButtonText` et `WindowText` ne devront pas être ajoutées en tant que paramètres explicites au constructeur. Elles sont définies automatiquement par JSON.net en utilisant les setters pour ces propriétés.
