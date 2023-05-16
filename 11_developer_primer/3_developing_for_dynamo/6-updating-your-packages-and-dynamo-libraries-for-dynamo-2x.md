# Aktualizace balíčků a knihoven aplikace Dynamo pro aplikaci Dynamo 2.x

### Úvod: <a href="#introduction" id="introduction"></a>

Aplikace Dynamo 2.0 je hlavní verzí, ve které byla změněna nebo odstraněna některá rozhraní API. Jednou z největších změn, která se dotkne autorů uzlů a balíčků, je přechod na formát souborů JSON.

Obecně platí, že autoři uzlů Zero Touch nebudou mít se zprovozněním svých balíčků ve verzi 2.0 mnoho práce.

Zprovoznění uzlů uživatelského rozhraní a uzlů, které se odvozují přímo z uzlu NodeModel, bude ve verzi 2.x vyžadovat větší úsilí.

Autoři rozšíření mohou také provést některé potenciální změny – v závislosti na tom, kolik rozhraní API jádra aplikace Dynamo používají ve svých rozšířeních.

***

### Obecná pravidla pro tvorbu balíčků: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Nezahrnujte do svých balíčků knihovny DLL aplikace Dynamo nebo Dynamo Revit. Tyto knihovny DLL již budou načteny aplikací Dynamo. Pokud přibalíte jinou verzi, než má uživatel načtenou _(tj. distribuujete jádro aplikace Dynamo 1.3, ale uživatel spouští balíček v aplikaci Dynamo 2.0), bude docházet k neočekávaným chybám za běhu._ To zahrnuje knihovny DLL jako `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll` a `ProtoGeometry.dll`.
* Pokud se tomu můžete vyhnout, nezahrnujte do svého balíčku knihovnu `newtonsoft.json.net` a nedistribuujte ji. Tato knihovna DLL již bude načtena aplikací Dynamo 2.x. Může dojít ke stejnému problému jako výše.
* Pokud se tomu můžete vyhnout, nezahrnujte do svého balíčku knihovnu `CEFSharp` a nedistribuujte ji. Tato knihovna DLL již bude načtena aplikací Dynamo 2.x. Může dojít ke stejnému problému jako výše.
* Obecně se vyhněte sdílení závislostí s aplikací Dynamo nebo Revit, pokud potřebujete kontrolovat verzi dané závislosti.



### Běžné problémy: <a href="#common-issues" id="common-issues"></a>

1) Po otevření grafu mají některé uzly více portů se stejným názvem, ale při uložení vypadal graf v pořádku. Tento problém může mít několik příčin.

Obvyklou hlavní příčinou je, že uzel byl vytvořen pomocí konstruktoru, který znovu vytvořil porty. Místo toho měl být použit konstruktor, který porty načte. Tyto konstruktory jsou obvykle označeny `[JsonConstructor]` _viz příklady níže_.

![Poškozený JSON](images/broken-json.jpg)

K tomu může dojít z následujících důvodů:

* Jednoduše nebyl nalezen odpovídající konstruktor `[JsonConstructor]` nebo mu nebyly předány `Inports` a `Outports` ze souboru .dyn JSON.
* Do stejného procesu byly načteny dvě verze rozhraní JSON.net současně, což způsobilo chybu modulu runtime rozhraní .net, takže atribut `[JsonConstructor]` nebylo možné správně použít k označení konstruktoru.
* K balíčku byl přibalen soubor DynamoServices.dll s jinou verzí, než je aktuální verze aplikace Dynamo, což způsobuje, že modul runtime rozhraní .net nedokáže identifikovat atribut `[MultiReturn]`, takže u uzlů Zero Touch označených různými atributy nebudou tyto atributy použity. Může se stát, že uzel vrací jeden výstup slovníku místo více portů.

2) Při načítání grafu zcela chybí uzly a v konzoli se zobrazují chyby.

* K tomu může dojít, pokud se z nějakého důvodu nezdařila deserializace. Je vhodné serializovat pouze vlastnosti, které potřebujete. Chcete-li ignorovat složité vlastnosti, které není nutné načíst nebo uložit, můžete použít `[JsonIgnore]`. Jedná se o vlastnosti jako `function pointer, delegate, action,` nebo `event` atd. Tyto vlastnosti by neměly být serializovány, protože se je obvykle nepodaří deserializovat a způsobí chybu za běhu.


### Podrobné informace o aktualizaci: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Vlastní uzly1.3 -> 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Uspořádání vlastních uzlů v souboru librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Známé problémy:

* Shodný název vlastního uzlu a název kategorie na stejné úrovni v souboru librarie.js způsobuje neočekávané chování. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) – vyhněte se použití stejných názvů pro kategorie a uzly.
* Komentáře budou namísto řádkových komentářů převedeny na blokové komentáře.
* Krátké názvy typů budou nahrazeny úplnými názvy. Pokud jste například nezadali typ, při opětovném načtení vlastního uzlu se zobrazí `var[]..[]`, protože se jedná o výchozí typ.


### Uzly Zero Touch 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* V aplikaci Dynamo 2.0 byly rozděleny typy seznamů a slovníků a byla změněna syntaxe pro vytváření seznamů a slovníků. Seznamy jsou inicializovány pomocí `[]`, zatímco slovníky používají `{}`.\
 Pokud jste dříve používali atribut `DefaultArgument` k označení parametrů v uzlech Zero Touch a používali syntaxi seznamu pro výchozí nastavení konkrétního seznamu, například `someFunc([DefaultArgument("{0,1,2}")])`, toto již nebude platné a bude nutné upravit úryvek jazyka DesignScript, aby používal novou inicializační syntaxi pro seznamy.
* Jak bylo uvedeno výše, nedistribuujte knihovny DLL aplikace Dynamo s balíčky (`DynamoCore`, `DynamoServices` atd.).


### Uzly Node Model 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Uzly Node Model vyžadují při aktualizaci na verzi Dynamo 2.x nejvíce práce. Na vysoké úrovni budete kromě běžných konstruktorů nodeModel, které se používají k vytváření instancí nových instancí typů uzlů, muset implementovat konstruktory, které budou použity pouze k načtení uzlů ze JSON. K jejich rozlišení označte konstruktory načítání pomocí `[JsonConstructor]`, což je atribut z rozhraní newtonsoft.Json.net.

Názvy parametrů v konstruktoru by obecně měly odpovídat názvům vlastností JSON. Toto mapování se však komplikuje, pokud přepíšete názvy, které jsou serializovány, pomocí atributů [JsonProperty].\
 [Další informace naleznete v dokumentaci k rozhraní Json.net.](https://www.newtonsoft.com/json/help/html/Introduction.htm)


#### Konstruktory JSON <a href="#json-constructors" id="json-constructors"></a>

Nejčastější změnou vyžadovanou k aktualizaci uzlů odvozených ze základní třídy `NodeModel` (nebo jiných základních tříd jádra aplikace Dynamo, tj. `DSDropDownBase`), je nutnost přidat do třídy konstruktor JSON.

Původní bezparametrický konstruktor bude stále schopen inicializovat nový uzel vytvořený v aplikaci Dynamo (například prostřednictvím knihovny). Konstruktor JSON je vyžadován k inicializaci uzlu, který je deserializován _(načten)_ z uloženého souboru .dyn nebo .dyf.

Konstruktor JSON se liší od základního konstruktoru tím, že obsahuje parametry `PortModel` pro `inPorts` a `outPorts`, které jsou poskytovány logikou načítání JSON. Volání k registraci portů pro uzel zde není vyžadováno, protože data existují v souboru .dyn. Příklad konstruktoru JSON vypadá následovně:

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Tato syntaxe `:base(Inports,outPorts){}` volá základní konstruktor `nodeModel` a předává mu deserializované porty.

Jakékoli speciální logiky, které existovaly v konstruktoru třídy a zahrnují inicializaci specifických dat serializovaných do souboru .dyn _(například nastavení registrace portů, strategie vázání atd.)_, není nutné v tomto konstruktoru opakovat, protože tyto hodnoty lze načíst ze JSON.

Jedná se o hlavní rozdíl mezi konstruktorem JSON a jinými konstruktory, pro vaše uzly nodeModel. Konstruktory JSON jsou vyvolány při načítání ze souboru a jsou jim předána načtená data. Ostatní uživatelská logika však musí být duplikována v konstruktoru JSON _(například inicializace ovladačů událostí pro uzel nebo připojování)_.

Příklady naleznete v úložišti DynamoSamples - > [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) nebo [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123).


#### Veřejné vlastnosti a serializace <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Dříve mohl vývojář serializovat a deserializovat konkrétní data modelu do dokumentu XML pomocí metod `SerializeCore` a `DeserializeCore`. Tyto metody v rozhraní API stále existují, ale v budoucí verzi aplikace Dynamo budou vyřazeny (příklad naleznete [zde](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Díky implementaci rozhraní JSON.NET lze nyní vlastnosti `public` odvozené třídy NodeModel serializovat přímo do souboru .dyn. Rozhraní JSON.Net poskytuje několik atributů pro řízení způsobu serializace vlastností.

Tento příklad, který určuje `PropertyName`, naleznete [zde](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38) v úložišti Dynamo.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`


#### Převodníky: <a href="#converters" id="converters"></a>

**Poznámka**\
 Pokud vytvoříte vlastní třídu převodníku JSON.net, aplikace Dynamo v současné době nemá mechanismus, který by umožňoval jeho začlenění do metod načítání a ukládání, takže i když svou třídu označíte atributem `[JsonConverter]`, nemusí být použita – místo toho můžete volat převodník přímo v metodě setter nebo getter. _//Toto omezení je třeba potvrdit. Všechny důkazy jsou vítány._

Příklad, který určuje metodu serializace pro převod vlastnosti na řetězec, naleznete [zde](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) v úložišti Dynamo.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`


#### Ignorování vlastností <a href="#ignoring-properties" id="ignoring-properties"></a>

Vlastnostem `public`, které nejsou určeny pro serializaci, musí mít přidán atribut `[JsonIgnore]`. Při uložení uzlů do souboru .dyn je zajištěno, že tato data budou serializačním mechanismem ignorována a nezpůsobí neočekávané následky při dalším otevření grafu. Příklad si můžete prohlédnout [zde](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) v úložišti Dynamo.

***

#### Vrácení a opakování změn <a href="#undoredo" id="undoredo"></a>

Jak je uvedeno výše, metody `SerializeCore` a `DeserializeCore` se v minulosti používaly k ukládání a načítání uzlů do souboru xml s příponou .dyn. Kromě toho se také používaly a **stále používají** k ukládání a načítání stavu uzlu pro operace Zpět/Znovu. Pokud chcete pro uzel nodeModel uživatelského rozhraní implementovat složité funkce Zpět/Znovu, bude nutné tyto metody implementovat a serializovat do objektu dokumentu XML, který je těmto metodám poskytován jako parametr. To by měl být vzácný případ použití s výjimkou složitých uzlů uživatelského rozhraní.


#### Rozhraní API vstupních a výstupních portů <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

Jednou z běžných událostí v uzlech nodeModel, kterých se týkají změny rozhraní API verze 2.0, je registrace portu v konstruktoru uzlu. Při prohlížení příkladů v úložišti Dynamo nebo DynamoSamples jste dříve mohli zaznamenat použití metod `InPortData.Add()` nebo `OutPortData.Add()`. V rozhraní API aplikace Dynamo byly dříve veřejné vlastnosti `InPortData` a `OutPortData` označeny jako zastaralé. Ve verzi 2.0 byly tyto vlastnosti odstraněny. Vývojáři by nyní měli používat metody `InPorts.Add()` a `OutPorts.Add()`. Kromě toho mají tyto dvě metody `Add()` mírně odlišné signatury:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

vs.

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Příklady převedeného kódu naleznete zde v úložišti Dynamo -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) nebo [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281).

Další běžný případ použití, který je ovlivněn změnami rozhraní API verze 2.0, se týká metod běžně používaných v metodě `BuildAst()` k určení chování uzlu na základě přítomnosti nebo nepřítomnosti konektorů portů. Dříve se k ověření stavu připojeného portu používala metoda `HasConnectedInput(index)`. Vývojáři by nyní měli kontrolovat stav připojení portu pomocí vlastnosti `InPorts[0].IsConnected`. Příklad naleznete v souboru [ColourRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) v úložišti Dynamo.


### Příklady: <a href="#examples" id="examples"></a>

Nyní si ukážeme aktualizaci uzlu uživatelského rozhraní verze 1.3 na verzi Dynamo 2.x.

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

Vše, co je potřeba udělat s touto třídou `nodeModel`, aby se správně načítala a ukládala ve verzi 2.0, je přidat konstruktor jsonConstructor, který se stará o načítání portů. Jednoduše předáme porty základnímu konstruktoru a tato implementace je prázdná.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Poznámka: Nevolejte v konstruktoru JsonConstructor funkci `RegisterPorts()` nebo některou její variantu – ta by použila atributy vstupních a výstupních parametrů třídy uzlů k vytvoření nových portů! **To je nežádoucí chování**, protože chceme používat načtené porty, které jsou předány konstruktoru.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

Tento příklad přidává minimální možné načítání konstruktoru JSON. Co když však potřebujeme provést složitější konstrukční logiku, například nastavit některé podprocesy naslouchání pro obsluhu událostí uvnitř konstruktoru. Další ukázka převzatá z\
 [úložiště DynamoSamples](https://github.com/DynamoDS/DynamoSamples) je uvedena výše v části `JsonConstructors Section` tohoto dokumentu.

Zde je složitější konstruktor pro uzel uživatelského rozhraní:

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

Když přidáme konstruktor JSON pro načtení tohoto uzlu ze souboru, je nutné znovu vytvořit část této logiky, ale všimněte si, že nezadáváme kód, který vytváří porty, nastavuje vázání nebo nastavuje výchozí hodnoty vlastností, které lze ze souboru načíst.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Všimněte si, že další veřejné vlastnosti, které byly serializovány do souboru JSON, jako `ButtonText` a `WindowText`, nebudou přidány jako explicitní parametry ke konstruktoru – jsou automaticky nastaveny rozhraním JSON.net pomocí metod setter pro tyto vlastnosti.
