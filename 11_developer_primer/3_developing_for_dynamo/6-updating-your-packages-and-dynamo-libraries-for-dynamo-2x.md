# Aktualisieren der Pakete und Dynamo-Bibliotheken für Dynamo 2.x

### Einführung: <a href="#introduction" id="introduction"></a>

Dynamo 2.0 ist eine Hauptversion, und einige APIs wurden geändert oder entfernt. Eine der größten Änderungen, die sich auf Block- und Paket-Autoren auswirken wird, ist der Wechsel zum JSON-Dateiformat.

Generell werden Zero-Touch-Block-Autoren wenig bis gar keine Arbeit damit haben, ihre Pakete in der Version 2.0 zu verwenden.

Benutzeroberflächen-Blöcke und Blöcke, die direkt aus NodeModel abgeleitet werden, erfordern mehr Arbeit, damit sie in der Version 2.x ausgeführt werden können.

Erweiterungsautoren müssen möglicherweise auch einige Änderungen vornehmen, je nachdem, wie viele der Dynamo Core-APIs sie in ihren Erweiterungen verwenden.



### Allgemeine Paket-Regeln: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Bündeln Sie Dynamo- oder Dynamo Revit-DLL-Dateien nicht zusammen mit Ihrem Paket. Diese DLL-Dateien sind bereits von Dynamo geladen worden. Wenn Sie eine andere Version bündeln, als der Benutzer geladen hat _(d. h., Sie verteilen zum Beispiel Dynamo Core 1.3, der Benutzer führt Ihr Paket jedoch unter Dynamo 2.0 aus)_, treten mysteriöse Laufzeitfehler auf. Dazu gehören DLL-Dateien wie `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll`.
* Bündeln und verteilen Sie `newtonsoft.json.net` nach Möglichkeit nicht zusammen mit Ihrem Paket. Diese DLL-Datei ist ebenfalls bereits von Dynamo 2.x geladen. Das gleiche Problem wie oben kann auftreten.
* Bündeln und verteilen Sie `CEFSharp` nach Möglichkeit nicht zusammen mit Ihrem Paket. Diese DLL-Datei ist ebenfalls bereits von Dynamo 2.x geladen. Das gleiche Problem wie oben kann auftreten.
* Im Allgemeinen sollten Sie die Freigabe von Abhängigkeiten für Dynamo oder Revit vermeiden, wenn Sie die Version dieser Abhängigkeit steuern müssen.

### Häufige Probleme: <a href="#common-issues" id="common-issues"></a>

1) Beim Öffnen eines Diagramms haben einige Blöcke mehrere Anschlüsse mit demselben Namen, das Diagramm sah beim Speichern jedoch einwandfrei aus. Dieses Problem kann mehrere Ursachen haben.

Die häufigste Fehlerursache ist, dass der Block mit einem Konstruktor erstellt wurde, der die Anschlüsse neu erstellt hat. Stattdessen hätte ein Konstruktor verwendet werden müssen, der die Anschlüsse geladen hat. Diese Konstruktoren sind gewöhnlich mit `[JsonConstructor]` gekennzeichnet. _Beispiele finden Sie unten_.

![Beschädigte JSON-Datei](images/broken-json.jpg)

Dies kann folgende Ursachen haben:

* Es gab einfach keinen passenden `[JsonConstructor]`, oder der Datei wurden `Inports` und `Outports` aus der JSON-DYN-Datei nicht übergeben.
* Es wurden zwei Versionen von JSON.net gleichzeitig in denselben Prozess geladen, was einen .NET-Laufzeitfehler verursachte, sodass das Attribut `[JsonConstructor]` nicht ordnungsgemäß verwendet werden konnte, um den Konstruktor zu markieren.
* Die Datei DynamoServices.dll mit einer anderen Version als die aktuelle Dynamo-Version wurde mit dem Paket gebündelt. Dadurch kann ein .NET-Laufzeitfehler beim Ermitteln des Attributs `[MultiReturn]` auftreten, sodass die mit verschiedenen Attributen markierten Zero-Touch-Blöcke diese nicht anwenden können. Sie werden möglicherweise feststellen, dass ein Block eine einzelne Wörterbuchausgabe anstelle mehrerer Anschlüsse zurückgibt.

2) Beim Laden des Diagramms fehlen Blöcke vollständig, und es treten Fehler in der Konsole auf.

* Dies kann auftreten, wenn die Deserialisierung aus irgendeinem Grund fehlgeschlagen ist. Es empfiehlt sich, nur die benötigten Eigenschaften zu serialisieren. Wir können `[JsonIgnore]` für komplexe Eigenschaften verwenden, die Sie nicht laden oder speichern müssen, um sie zu ignorieren. Eigenschaften wie `function pointer, delegate, action,` oder `event` usw. Diese sollten nicht serialisiert werden, da sie in der Regel nicht deserialisiert werden können und einen Laufzeitfehler verursachen.

### Aktualisierung im Detail: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Benutzerdefinierte Blöcke 1.3 - > 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Organisieren von benutzerdefinierten Blöcken in library.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Bekannte Probleme:

* Ein gleicher benutzerdefinierter Blockname und Kategoriename auf derselben Ebene in library.js führt zu unerwartetem Verhalten. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653): Vermeiden Sie die Verwendung derselben Namen für Kategorie und Blöcke.
* Kommentare werden in Blockkommentare anstatt in Zeilenkommentare umgewandelt.
* Kurze Typnamen werden durch vollständige Namen ersetzt. Wenn Sie beispielsweise beim erneuten Laden des benutzerdefinierten Blocks keinen Typ angegeben haben, wird `var[]..[]` angezeigt, da dies der Vorgabetyp ist.

### Zero-Touch-Blöcke 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* In Dynamo 2.0 wurden Listen- und Wörterbuchtypen getrennt, und die Syntax zum Erstellen von Listen und Wörterbüchern wurde geändert. Listen werden mit `[]` initialisiert, während Wörterbücher `{}` verwenden.\
 Wenn Sie zuvor das Attribut `DefaultArgument` verwendet haben, um Parameter auf den Zero-Touch-Blöcken zu markieren, und die Listensyntax verwendet haben, um eine bestimmte Liste wie `someFunc([DefaultArgument("{0,1,2}")])` als Vorgabe zu verwenden, ist dies nicht mehr gültig. Sie müssen dann das DesignScript-Snippet ändern, um die neue Initialisierungssyntax für Listen zu verwenden.
* Wie oben erwähnt, sollten Sie Dynamo-DLL-Dateien nicht mit Ihren Paketen verteilen (`DynamoCore`, `DynamoServices` usw.).

### NodeModel-Blöcke 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

NodeModel-Blöcke erfordern die meiste Arbeit bei der Aktualisierung auf Dynamo 2.x. Auf höherer Ebene müssen Sie Konstruktoren implementieren, die nur zum Laden der Blöcke aus JSON verwendet werden, zusätzlich zu den regulären NodeModel-Konstruktoren, die zum Instanziieren neuer Instanzen der Blocktypen verwendet werden. Um zwischen diesen zu unterscheiden, markieren Sie die Konstruktoren für die Ladezeit mit `[JsonConstructor]`, einem Attribut von newtonsoft.Json.net.

Die Namen der Parameter im Konstruktor sollten im Allgemeinen mit den Namen der JSON-Eigenschaften übereinstimmen. Diese Zuordnung wird jedoch komplizierter, wenn Sie die Namen überschreiben, die mithilfe von [JsonProperty]-Attributen serialisiert werden.\
 [Weitere Informationen finden Sie in der Dokumentation zu Json.net.](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### JSON-Konstruktoren <a href="#json-constructors" id="json-constructors"></a>

Die häufigste Änderung, die beim Aktualisieren von Blöcken erforderlich ist, die von der `NodeModel`-Basisklasse (oder anderen Dynamo Core-Basisklassen, z. B. `DSDropDownBase`) abgeleitet wurden, ist die Notwendigkeit, Ihrer Klasse einen JSON-Konstruktor hinzuzufügen.

Die Initialisierung eines neuen Blocks, der in Dynamo erstellt wurde (z. B. über die Bibliothek), ist weiterhin durch den ursprünglichen parameterlosen Konstruktor möglich. Der JSON-Konstruktor ist erforderlich, um einen Block zu initialisieren, der aus einer gespeicherten DYN- oder DYF-Datei deserialisiert _(geladen)_ wird.

Der JSON-Konstruktor unterscheidet sich vom Basiskonstruktor dadurch, dass er über `PortModel`-Parameter für `inPorts` und `outPorts` verfügt, die von der JSON-Ladelogik bereitgestellt werden. Der Aufruf zum Registrieren der Anschlüsse für den Block ist hier nicht erforderlich, da die Daten in der DYN-Datei vorhanden sind. Ein JSON-Konstruktor sieht beispielsweise wie folgt aus:

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Diese Syntax `:base(Inports,outPorts){}` ruft den `nodeModel`-Basiskonstruktor auf und übergibt die deserialisierten Anschlüsse an diesen.

Spezielle Logik im Klassenkonstruktor, die die Initialisierung bestimmter Daten umfasst, die in die DYN-Datei serialisiert werden _(z. B. Festlegen der Anschlussregistrierung, Vergitterungsstrategie usw.)_, muss in diesem Konstruktor nicht wiederholt werden, da diese Werte aus dem JSON-Konstruktor gelesen werden können.

Dies ist der Hauptunterschied zwischen dem JSON-Konstruktor und Nicht-JSON-Konstruktoren für Ihre NodeModels. JSON-Konstruktoren werden beim Laden aus einer Datei aufgerufen und erhalten geladene Daten. Andere Benutzerlogik muss jedoch im JSON-Konstruktor dupliziert werden _(z. B. Initialisieren von Ereignis-Steuerprogrammen für den Block oder Anhängen)_.

Beispiele finden Sie hier im DynamoSamples-Repository -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) oder [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123).

#### Öffentliche Eigenschaften und Serialisierung <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Bisher konnte ein Entwickler bestimmte Modelldaten über die `SerializeCore`- und `DeserializeCore`-Methode in das XML-Dokument serialisieren und deserialisieren. Diese Methoden sind weiterhin in der API vorhanden, werden jedoch in einer zukünftigen Version von Dynamo nicht mehr unterstützt (ein Beispiel finden Sie [hier](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Mit der JSON.NET-Implementierung können jetzt `public`-Eigenschaften in der von NodeModel abgeleiteten Klasse direkt in die DYN-Datei serialisiert werden. JSON.Net stellt mehrere Attribute bereit, mit denen die Serialisierung der Eigenschaft gesteuert wird.

Dieses Beispiel mit der Angabe eines `PropertyName` finden Sie [hier](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38) im Dynamo-Repository.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### Konverter: <a href="#converters" id="converters"></a>

**Anmerkung**\
 Wenn Sie eine eigene JSON.net-Konverterklasse erstellen, verfügt Dynamo derzeit über keinen Mechanismus, mit dem Sie diese in die Lade- und Speichermethoden einlesen können. Daher kann die Klasse auch dann nicht verwendet werden, wenn Sie sie mit dem `[JsonConverter]`-Attribut markieren. Sie können den Konverter stattdessen direkt in Ihrem Setter oder Getter aufrufen. _//ZU ERLEDIGEN: Bestätigung dieser Einschränkung erforderlich. Alle Nachweise sind willkommen._

Ein Beispiel, in dem eine Serialisierungsmethode zum Konvertieren der Eigenschaft in eine Zeichenfolge angegeben ist, finden Sie [hier](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) im Dynamo-Repository.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### Ignorieren von Eigenschaften <a href="#ignoring-properties" id="ignoring-properties"></a>

Für `public`-Eigenschaften, die nicht für die Serialisierung vorgesehen sind, muss das Attribut `[JsonIgnore]` hinzugefügt werden. Wenn die Blöcke in der DYN-Datei gespeichert werden, wird dadurch sichergestellt, dass diese Daten vom Serialisierungsmechanismus ignoriert werden. Beim erneuten Öffnen des Diagramms treten dann keine unerwarteten Auswirkungen auf. Ein Beispiel dafür finden Sie [hier](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) im Dynamo-Repository.



#### Rückgängig/Wiederholen <a href="#undoredo" id="undoredo"></a>

Wie bereits erwähnt, wurden in der Vergangenheit die Methoden `SerializeCore` und `DeserializeCore` verwendet, um Blöcke zu speichern und in die XML-DYN-Datei zu laden. Außerdem wurden sie **und werden weiterhin** auch zum Speichern und Laden des Blockstatus zum Rückgängigmachen und Wiederherstellen verwendet. Wenn Sie komplexe Funktionen zum Rückgängigmachen und Wiederherstellen für den NodeModel-Benutzeroberflächen-Block implementieren möchten, müssen Sie diese Methoden implementieren und in das XML-Dokumentobjekt serialisieren, das als Parameter für diese Methoden bereitgestellt wird. Dies sollte ein selten auftretender Anwendungsfall sein, mit Ausnahme von komplexen Benutzeroberflächen-Blöcken.

#### APIs für Ein- und Ausgabeanschlüsse <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

Ein häufiges Vorkommen in NodeModel-Blöcken, die von 2.0-API-Änderungen betroffen sind, ist die Anschlussregistrierung im Blockkonstruktor. Wenn Sie sich die Beispiele im Dynamo- oder DynamoSamples-Repository ansehen, werden Sie bisher die Verwendung der Methode `InPortData.Add()` oder `OutPortData.Add()` gefunden haben. In der Dynamo-API wurden die öffentlichen `InPortData`- und `OutPortData`-Eigenschaften bisher als veraltet markiert. In Version 2.0 wurden diese Eigenschaften entfernt. Entwickler sollten jetzt die Methoden `InPorts.Add()` und `OutPorts.Add()` verwenden. Darüber hinaus haben diese beiden `Add()`-Methoden leicht unterschiedliche Signaturen:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

im Vergleich zu

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Beispiele für konvertierten Code finden Sie hier im Dynamo-Repository -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) oder [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281).

Der andere häufige Anwendungsfall, der von den 2.0-API-Änderungen betroffen ist, bezieht sich auf die Methoden, die häufig in der Methode `BuildAst()` verwendet werden, um das Blockverhalten basierend auf dem Vorhandensein oder Fehlen von Anschlussverbindungen zu bestimmen. Zuvor wurde `HasConnectedInput(index)` verwendet, um einen verbundenen Anschlussstatus zu validieren. Entwickler sollten nun die Eigenschaft `InPorts[0].IsConnected` verwenden, um den Anschlussverbindungsstatus zu überprüfen. Ein Beispiel hierfür finden Sie in [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) im Dynamo-Repository.

### Beispiele: <a href="#examples" id="examples"></a>

Gehen wir nun die Schritte für die Aktualisierung eines 1.3-Benutzeroberflächen-Blocks auf Dynamo 2.x durch.

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

Damit die `nodeModel`-Klasse in der Version 2.0 ordnungsgemäß geladen und gespeichert wird, müssen wir einfach nur einen jsonConstructor hinzufügen, der das Laden der Anschlüsse handhabt. Wir übergeben die Anschlüsse einfach an den Basiskonstruktor, und diese Implementierung ist leer.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Anmerkung: Rufen Sie `RegisterPorts()` oder eine Variante davon nicht in Ihrem JsonConstructor auf. Dadurch werden die Eingabe- und Ausgabeparameterattribute in Ihrer Blockklasse zum Erstellen neuer Anschlüsse verwendet. **Dies ist nicht erwünscht**, da die geladenen Anschlüsse verwendet werden sollen, die an den Konstruktor übergeben werden.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

In diesem Beispiel wird der JSON-Konstruktor mit dem minimalsten Ladeaufwand hinzugefügt. Was passiert aber, wenn wir komplexere Konstruktionslogik ausführen müssen, wie z. B. die Einrichtung einiger Listener für die Ereignisbehandlung im Konstruktor? Das nächste aus dem \
[DynamoSamples-Repository](https://github.com/DynamoDS/DynamoSamples) entnommene Beispiel ist oben im `JsonConstructors Section` dieses Dokuments verknüpft.

Im Folgenden wird ein komplexerer Konstruktor für einen Benutzeroberflächen-Block dargestellt:

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

Wenn wir einen JSON-Konstruktor hinzufügen, um diesen Block aus einer Datei zu laden, müssen wir einen Teil dieser Logik neu erstellen. Beachten Sie jedoch, dass wir den Code, mit dem Anschlüsse erstellt, Vergitterungen festgelegt oder die Vorgabewerte für Eigenschaften angegeben werden, die aus der Datei geladen werden können, nicht berücksichtigen.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Beachten Sie, dass andere öffentliche Eigenschaften, die in JSON serialisiert wurden, wie `ButtonText` und `WindowText`, nicht als explizite Parameter zum Konstruktor hinzugefügt werden müssen. Sie werden automatisch von JSON.net mit den Settern für diese Eigenschaften festgelegt.
