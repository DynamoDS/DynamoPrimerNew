# Aktualizowanie pakietów i bibliotek dodatku Dynamo dla dodatku Dynamo 2.x

### Wprowadzenie: <a href="#introduction" id="introduction"></a>

Dodatek Dynamo 2.0 jest wersją główną i niektóre interfejsy API zostały w nim zmienione lub usunięte. Jedną z największych zmian istotnych dla twórców węzłów i pakietów jest przejście na format pliku JSON.

Ogólnie twórcy węzłów Zero Touch nie muszą robić wiele albo w ogóle nic, aby zadbać o działanie pakietów w wersji 2.0.

Zadbanie o działanie w wersji 2.x węzłów interfejsu użytkownika i węzłów pochodnych bezpośrednio od klasy NodeModel wymaga więcej pracy.

Twórcy rozszerzeń również mogą być zmuszeni do wprowadzenia pewnych zmian w zależności od tego, w jakim stopniu wykorzystują w rozszerzeniach podstawowe interfejsy API dodatku Dynamo.

***

### Ogólne zasady dotyczące pakowania: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Nie należy łączyć z pakietem plików .dll dodatku Dynamo ani dodatku Dynamo Revit. Te biblioteki dll zostaną już wczytane przez dodatek Dynamo. W przypadku utworzenia pakietu z wersją inną niż wersja wczytana przez użytkownika _(na przykład zostanie utworzona dystrybucja Dynamo Core 1.3, podczas gdy użytkownik uruchamia pakiet w dodatku Dynamo 2.0)_ wystąpią tajemnicze błędy w czasie wykonywania. Obejmuje to pliki dll takie jak `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll`
* Należy w miarę możliwości unikać dodawania do pakietu i dystrybuowania z pakietem pliku `newtonsoft.json.net`. Ten plik dll również zostanie wcześniej wczytany przez dodatek Dynamo 2.x. Może wystąpić ten sam problem co powyżej.
* Należy w miarę możliwości unikać dodawania do pakietu i dystrybuowania z pakietem pliku `CEFSharp`. Ten plik dll również zostanie wcześniej wczytany przez dodatek Dynamo 2.x. Może wystąpić ten sam problem co powyżej.
* Ogólnie należy unikać udostępniania zależności wraz z dodatkiem Dynamo lub programem Revit, jeśli zachodzi potrzeba kontrolowania wersji tej zależności.

### Typowe problemy: <a href="#common-issues" id="common-issues"></a>

1) Po otwarciu wykresu niektóre węzły mają wiele portów o tej samej nazwie, mimo że wykres wyglądał dobrze podczas zapisywania. Ten problem może mieć kilka przyczyn.

Typową przyczyną jest to, że węzeł utworzono za pomocą konstruktora ponownie tworzącego porty. Zamiast tego należało użyć konstruktora wczytującego porty. Te konstruktory mają zwykle oznaczenie `[JsonConstructor]` _zobacz przykłady poniżej_

![Uszkodzony kod JSON](images/broken-json.jpg)

Inna możliwa przyczyna:

* Nie było zgodnych elementów `[JsonConstructor]` lub nie przekazano elementów `Inports` i `Outports` z pliku JSON.dyn.
* W tym samym czasie do tego samego procesu wczytano dwie wersje JSON.net, co spowodowało błąd środowiska uruchomieniowego .NET, więc nie można było poprawnie użyć atrybutu `[JsonConstructor]` do oznaczenia konstruktora.
* Do pakietu dołączono plik DynamoServices.dll w wersji innej niż bieżąca wersja dodatku Dynamo i powoduje to, że środowisko uruchomieniowe .NET nie może zidentyfikować atrybutu `[MultiReturn]`, więc dla węzłów Zero-Touch oznaczonych różnymi atrybutami nie można zastosować tych atrybutów. Może się okazać, że węzeł zwraca jeden słownik wyjściowy zamiast wielu portów.

2) Całkowicie brakuje węzłów po wczytaniu wykresu z pewnymi błędami w konsoli.

* Może tak się zdarzyć, jeśli z jakiegoś powodu nie powiedzie się deserializacja. Zaleca się serializowanie tylko potrzebnych właściwości. Można używać atrybutu `[JsonIgnore]` w przypadku złożonych właściwości, których nie trzeba wczytywać ani zapisywać, aby je zignorować. Chodzi o właściwości takie jak `function pointer, delegate, action,` czy `event`. Nie należy ich serializować, ponieważ zazwyczaj nie można ich zdeserializować i powodują one błąd w trakcie wykonywania.

### Szczegółowe omówienie uaktualnienia: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Węzły niestandardowe z wersji 1.3 do wersji 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Organizowanie węzłów niestandardowych w pliku librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Znane problemy:

* Zbieżna nazwa węzła niestandardowego i nazwa kategorii na tym samym poziomie w pliku librarie.js skutkuje nieoczekiwanym zachowaniem. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) — unikaj używania tych samych nazw kategorii i węzłów.
* Komentarze zostaną zamienione na komentarze blokowe zamiast komentarzy jednowierszowych.
* Krótkie nazwy typów zostaną zastąpione pełnymi nazwami. Jeśli na przykład podczas ponownego wczytywania węzła niestandardowego nie został określony typ, pojawi się `var[]..[]` — ponieważ jest to typ domyślny.

### Węzły Zero-Touch z wersji 1.3 do wersji 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* W dodatku Dynamo 2.0 typy List (lista) i Dictionary (słownik) zostały rozdzielone, a składnia tworzenia list i słowników została zmieniona. Listy inicjuje się przy użyciu `[]`, a słowniki przy użyciu `{}`.\
 Jeśli wcześniej używano atrybutu `DefaultArgument` do oznaczania parametrów w węzłach Zero-Touch i używano składni listy w celu utworzenia konkretnej listy domyślnej, takiej jak `someFunc([DefaultArgument("{0,1,2}")])`, nie będzie to już poprawne. Należy zmodyfikować fragment kodu DesignScript, stosując nową składnię inicjowania list.
* Jak wspomniano powyżej, nie należy dystrybuować plików dll dodatku Dynamo wraz z pakietami. (`DynamoCore`, `DynamoServices` itp.)

### Węzły Node Model z wersji 1.3 do wersji 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Zaktualizowanie węzłów Node Model do wersji Dynamo 2.x wymaga najwięcej pracy. Ogólnie należy zaimplementować konstruktory, które będą używane tylko do wczytywania węzłów z pliku json obok zwykłych konstruktorów klasy nodeModel używanych do tworzenia nowych wystąpień typów węzłów. Aby odróżnić te elementy, należy oznaczyć konstruktory czasu ładowania atrybutem `[JsonConstructor]`, który jest atrybutem z biblioteki newtonsoft.Json.net.

Nazwy parametrów w konstruktorze powinny zasadniczo odpowiadać nazwom właściwości JSON — jednak to odwzorowanie jest bardziej skomplikowane w przypadku nadpisywania nazw serializowanych przy użyciu atrybutów [JsonProperty].\
 [Więcej informacji można znaleźć w dokumentacji Json.net.](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### Konstruktory JSON <a href="#json-constructors" id="json-constructors"></a>

Najczęstszą zmianą, jaką należy wprowadzić w celu zaktualizowania węzłów pochodnych od klasy bazowej `NodeModel` (lub innych klas bazowych dodatku Dynamo, na przykład `DSDropDownBase`), jest konieczność dodania do klasy konstruktora JSON.

Oryginalny konstruktor bez parametrów nadal będzie obsługiwał inicjowanie nowego węzła tworzonego w dodatku Dynamo (na przykład za pomocą biblioteki). Konstruktor JSON jest wymagany do zainicjowania węzła, który został zdeserializowany _(wczytany)_ z zapisanego pliku .dyn lub .dyf.

Konstruktor JSON różni się od konstruktora bazowego tym, że ma parametry `PortModel` dla portów `inPorts` i `outPorts`, które są dostarczane przez logikę ładowania JSON. Wywołanie w celu zarejestrowania portów dla węzła nie jest tutaj wymagane, ponieważ dane istnieją w pliku .dyn. Przykład konstruktora JSON wygląda tak:

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Ta składnia `:base(Inports,outPorts){}` wywołuje konstruktor bazowy `nodeModel` i przekazuje do niego zdeserializowane porty.

Nie jest wymagane powtarzanie w tym konstruktorze żadnej specjalnej logiki istniejącej w konstruktorze klasy, która obejmuje zainicjowanie określonych danych zserializowanych do pliku .dyn _(na przykład ustawiania rejestracji portu, strategii skratowania itp.)_, ponieważ te wartości można odczytać z pliku JSON.

Jest to główna różnica między konstruktorami JSON i innymi konstruktorami NC w przypadku klas nodeModel. Konstruktory JSON są wywoływane podczas wczytywania z pliku i są do nich przekazywane wczytane dane. W konstruktorze JSON należy jednak powielić inną logikę użytkownika _(na przykład inicjowanie obsługi zdarzeń dla węzła lub dołączanie)_.

Przykłady można znaleźć tutaj w repozytorium DynamoSamples -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) lub [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123)

#### Właściwości publiczne i serializowanie <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Wcześniej programista mógł serializować i deserializować określone dane modelu do dokumentu xml za pomocą metod `SerializeCore` i `DeserializeCore`. Te metody nadal istnieją w interfejsie API, ale zostaną wycofane w przyszłej wersji dodatku Dynamo (przykład można znaleźć [tutaj](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Dzięki implementacji JSON.NET właściwości `public` klasy pochodnej od klasy NodeModel można teraz serializować bezpośrednio do pliku .dyn. W środowisku JSON.Net dostępnych jest wiele atrybutów umożliwiających sterowanie sposobem serializowania właściwości.

W repozytorium dodatku Dynamo, [tutaj](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38), można znaleźć przykład określający atrybut `PropertyName`.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### Konwertery: <a href="#converters" id="converters"></a>

**Uwaga**\
 Jeśli tworzysz własną klasę konwertera JSON.net: dodatek Dynamo nie ma obecnie mechanizmu umożliwiającego wstrzyknięcie jej do metod wczytywania i zapisywania, więc nawet jeśli oznaczysz tę klasę atrybutem `[JsonConverter]`, może ona nie zostać użyta. Zamiast tego możesz wywołać ten konwerter bezpośrednio w mechanizmie ustawiania (setter) lub pobierania (getter). _//DO OPRACOWANIA Wymagane jest potwierdzenie tego ograniczenia. Wszelkie dowody są mile widziane._

W repozytorium dodatku Dynamo, [tutaj](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66), można znaleźć przykład określający metodę serializacji do konwertowania właściwości na ciąg.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### Ignorowanie właściwości <a href="#ignoring-properties" id="ignoring-properties"></a>

Właściwości `public`, które nie są przeznaczone do serializacji, muszą mieć dodany atrybut `[JsonIgnore]`. Po zapisaniu węzłów w pliku .dyn zapewnia to ignorowanie tych danych przez mechanizm serializowania, więc nie będą one powodować nieoczekiwanych konsekwencji po ponownym otwarciu wykresu. Przykład tego można znaleźć [tutaj](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) w repozytorium dodatku Dynamo.

***

#### Cofanie/ponawianie <a href="#undoredo" id="undoredo"></a>

Jak wspomniano powyżej, w przeszłości używano metod `SerializeCore` i `DeserializeCore` do zapisywania i wczytywania węzłów do pliku xml .dyn. Dodatkowo były też używane do zapisywania i wczytywania stanu węzła na potrzeby operacji cofania/ponawiania — i **nadal są**. Aby zaimplementować złożone funkcje cofania/ponawiania dla węzła interfejsu użytkownika nodeModel, należy zaimplementować te metody i zserializować je w obiekcie dokumentu XML dostarczanym jako parametr tych metod. Powinno to być stosowane rzadko, w przypadku złożonych węzłów interfejsu użytkownika.

#### Interfejsy API portów wejściowych i wyjściowych <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

Jedną z typowych sytuacji w przypadku węzłów nodeModel, na którą wpływają zmiany interfejsu API 2.0, jest rejestracja portów w konstruktorze węzła. Wcześniej podczas przyglądania się przykładom w repozytorium Dynamo lub DynamoSamples można było znaleźć przypadki użycia metody `InPortData.Add()` lub `OutPortData.Add()`. Wcześniej w interfejsie API dodatku Dynamo właściwości publiczne `InPortData` i `OutPortData` były oznaczone jako wycofane. W wersji 2.0 właściwości te zostały usunięte. Programiści powinni teraz korzystać z metod `InPorts.Add()` i `OutPorts.Add()`. Ponadto te dwie metody `Add()` mają nieco inne sygnatury:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

w porównaniu z nową

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Przykłady przekonwertowanego kodu można znaleźć tutaj w repozytorium dodatku Dynamo -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) lub [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281)

Inny typowy przypadek użycia, na który wpływają zmiany interfejsu API 2.0, dotyczy metod powszechnie używanych w metodzie `BuildAst()` w celu określania zachowania węzłów na podstawie występowania lub braku złączy portów. Wcześniej do sprawdzania stanu połączenia portu używano metody `HasConnectedInput(index)`. Programiści powinni teraz sprawdzać stan połączenia portu za pomocą właściwości `InPorts[0].IsConnected`. Przykład tego można znaleźć w pliku [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) w repozytorium dodatku Dynamo.

### Przykłady: <a href="#examples" id="examples"></a>

Przeanalizujmy proces uaktualniania węzła interfejsu użytkownika w wersji 1.3 do wersji Dynamo 2.x.

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

W przypadku klasy `nodeModel`, aby zapewnić poprawne wczytywanie i zapisywanie portów w wersji 2.0, wystarczy tylko dodać konstruktor jsonConstructor do obsługi wczytywania portów. Po prostu przekazujemy porty do konstruktora bazowego, a ta implementacja jest pusta.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Uwaga: nie należy wywoływać operacji `RegisterPorts()` ani jej odmian w konstruktorze JsonConstructor — użyje ona atrybutów parametrów wejściowych i wyjściowych w klasie węzła w celu utworzenia nowych portów. **Nie chcemy, aby tak się stało**, ponieważ chcemy używać wczytanych portów, które są przekazywane do konstruktora.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

W tym przykładzie dodano minimalny konstruktor JSON wczytywania. Co jednak zrobić, jeśli trzeba utworzyć bardziej złożoną logikę konstrukcji, na przykład skonfigurować pewne detektory do obsługi zdarzeń wewnątrz konstruktora. Następny przykład pochodzący z\
 [repozytorium DynamoSamples](https://github.com/DynamoDS/DynamoSamples) połączono powyżej części `JsonConstructors Section` w tym dokumencie.

Poniżej przedstawiono bardziej złożony konstruktor węzła interfejsu użytkownika:

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

Podczas dodawania konstruktora JSON na potrzeby wczytywania tego węzła z pliku należy ponownie utworzyć niektóre elementy tej logiki, ale nie należy dodawać kodu tworzącego porty, ustawiającego skratowanie ani ustawiającego wartości domyślne właściwości, który można wczytać z pliku.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Należy pamiętać, że inne właściwości publiczne zserializowane do pliku JSON, takie jak `ButtonText` i `WindowText`, nie powinny być dodawane do konstruktora jako parametry jawne — są one ustawiane automatycznie przez środowisko JSON.net za pomocą mechanizmów ustawiania (setter) tych właściwości.
