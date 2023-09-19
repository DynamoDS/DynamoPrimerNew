# Обновление пакетов и библиотек Dynamo для Dynamo 2.x

### Введение <a href="#introduction" id="introduction"></a>

Dynamo 2.0 — это основной выпуск, в котором изменены или удалены некоторые API. Одно из самых значительных изменений, которое повлияет на разработчиков узлов и пакетов, — это переход на формат JSON.

В целом, от разработчиков узлов Zero-Touch не требуется никаких дополнительных действий для запуска пакетов версии 2.0.

Узлы пользовательского интерфейса и узлы, производные от NodeModel, требуют доработки для совместимости с версией 2.x.

Разработчикам расширений также потребуется внести некоторые изменения в зависимости от того, в каком объеме они используют API Dynamo Core в своих расширениях.



### Общие правила упаковки: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Не объединяйте DLL-файлы Dynamo или Dynamo Revit с пакетом. Эти DLL-файлы будут загружены Dynamo. Если ваша версия отличается от той, которую загрузил пользователь, _(то есть, вы распространяете Dynamo Core 1.3, а пользователь запускает пакет в Dynamo 2.0)_ во время выполнения возникнут непредвиденные ошибки. Это относится к таким библиотекам DLL, как `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll`.
* Не объединяйте и не распространяйте `newtonsoft.json.net` вместе с пакетом, если это возможно. Этот DLL-файл также будет загружен в Dynamo 2.x. Может возникнуть та же проблема, что и выше.
* Не объединяйте и не распространяйте `CEFSharp` вместе с пакетом, если этого можно избежать. Этот DLL-файл также будет загружен в Dynamo 2.x. Может возникнуть та же проблема, что и выше.
* В целом, не допускайте совместного использования зависимостей Dynamo или Revit, если требуется управлять версией такой зависимости.

### Распространенные проблемы: <a href="#common-issues" id="common-issues"></a>

1) При открытии графика у некоторых узлов несколько портов с одинаковым именем, но график выглядит нормально при сохранении. Эта проблема может происходить по нескольким причинам.

Обычно это связано с тем, что узел был создан с помощью конструктора, который воссоздал порты. Вместо него должен был использоваться конструктор, загружающий порты. Эти конструкторы обычно помечены как `[JsonConstructor]` _см. примеры ниже_.

![Поврежденный JSON](images/broken-json.jpg)

Это может произойти по следующим причинам.

* Соответствующий `[JsonConstructor]` просто отсутствует, или `Inports` и `Outports` не переданы из JSON DYN.
* В один и тот же процесс одновременно загружены две версии JSON.net, что привело к сбою в среде выполнения .NET, поэтому атрибут `[JsonConstructor]` невозможно корректно использовать для обозначения конструктора.
* Файл DynamoServices.dll, версия которого отличается от текущей версии Dynamo, включен в пакет и вызывает ошибку определения атрибута `[MultiReturn]` в среде выполнения .NET, поэтому невозможно применить необходимые атрибуты к узлам Zero-Touch. Возможно, узел возвращает один выходной словарь вместо нескольких портов.

2) При загрузке графика полностью отсутствуют узлы, при этом в консоли возникают ошибки.

* Это может произойти, если по какой-либо причине не удалось выполнить десериализацию. Рекомендуется сериализовать только необходимые свойства. Можно использовать `[JsonIgnore]` со сложными свойствами, которые не нужно загружать или сохранять, чтобы игнорировать их. Это могут быть такие свойства, как `function pointer, delegate, action,` или `event` и т. д. Их не следует сериализовать, так как обычно их невозможно десериализовать, и они могут вызвать ошибку во время выполнения.

### Подробное описание обновления: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Пользовательские узлы 1.3 - > 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Организация пользовательских узлов в файле librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Известные проблемы

* Совпадение имени пользовательского узла и имени категории на одном уровне в librarie.js приводит к непредвиденному поведению. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653): не используйте одинаковые имена для категорий и узлов.
* Комментарии будут преобразованы в комментарии к блокам, а не к строкам.
* Короткие имена типов будут заменены полными именами. Например, если при повторной загрузке пользовательского узла тип не был указан, то отобразится `var[]..[]`, так как это тип по умолчанию.

### Узлы Zero Touch 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* Типы списка и словаря в Dynamo 2.0 разделены, а синтаксис создания списков и словарей изменен. Списки инициализируются с помощью `[]`, а словари — `{}`.\
 Если ранее для маркировки параметров на узлах Zero-Touch использовался атрибут `DefaultArgument`, а синтаксис списка использовался по умолчанию для определенного списка, например `someFunc([DefaultArgument("{0,1,2}")])`, то этот атрибут больше не будет действительным, и для использования нового синтаксиса инициализации списков потребуется изменить фрагмент кода DesignScript.
* Как указано выше, DLL-файлы Dynamo не распространяются вместе с пакетами (`DynamoCore`, `DynamoServices` и т. д.).

### Узлы NodeModel 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Больше всего работы потребуется для обновления узлов NodeModel до версии Dynamo 2.x. В двух словах, необходимо будет реализовать конструкторы, которые будут использоваться только для загрузки узлов из JSON, в дополнение к обычным конструкторам nodeModel, используемым для создания новых экземпляров типов узлов. Чтобы различать их, нужно пометить конструкторы времени загрузки атрибутом `[JsonConstructor]` из newtonsoft.Json.net.

Имена параметров в конструкторе должны, как правило, совпадать с именами свойств JSON, хотя это сопоставление усложняется, если вы переопределяете имена, сериализованные с помощью атрибутов [JsonProperty].\
 [Дополнительные сведения см. в документации по Json.net.](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### Конструкторы JSON <a href="#json-constructors" id="json-constructors"></a>

Чаще всего для обновления узлов, производных от базового класса `NodeModel` (или других базовых классов Dynamo Core, т. е. `DSDropDownBase`), требуется добавить конструктор JSON в класс.

Исходный конструктор без параметров будет по-прежнему обрабатывать инициализацию нового узла, созданного в Dynamo (например, с помощью библиотеки). Конструктор JSON необходим для инициализации десериализованного узла _(загруженного)_ из сохраненного файла DYN или DYF.

Конструктор JSON отличается от базового конструктора тем, что имеет параметры `PortModel` для `inPorts` и `outPorts`, которые предоставляются логикой загрузки JSON. Вызов для регистрации портов для узла не требуется, поскольку данные существуют в файле DYN. Пример конструктора JSON:

`using Newtonsoft.Json; //New dependency for Json` (Новая зависимость для JSON)

………

`[JsonConstructor] //Attribute required to identity the Json constructor` (Атрибут, необходимый для идентификации конструктора JSON)

`//Minimum constructor implementation. Note that the base method invocation must also be present.` (Минимальная реализация конструктора. Обратите внимание, что также должен присутствовать вызов базового метода)

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Этот синтаксис `:base(Inports,outPorts){}` вызывает базовый конструктор `nodeModel` и передает ему десериализованные порты.

Любая специальная логика, существующая в конструкторе классов и связанная с инициализацией определенных данных, сериализуемых в файл DYN _(например, настройка регистрации порта, стратегия переплетения и т. д.)_, не обязательно должна повторяться в этом конструкторе, так как эти значения можно прочитать из JSON.

Это основное различие между конструкторами JSON и конструкторами, не использующими JSON, для NodeModel. Конструкторы JSON вызываются при загрузке из файла и передают загруженные данные. Однако в конструкторе JSON должна быть продублирована другая логика пользователя _(например, инициализация обработчиков событий для узла или подключение)_.

Примеры можно найти в репозитории DynamoSamples -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) или [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123)

#### Общие свойства и сериализация <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Ранее разработчик мог сериализовать и десериализовать определенные данные модели в XML-документ с помощью методов `SerializeCore` и `DeserializeCore`. Эти методы по-прежнему существуют в API, но будут исключены из следующей версии Dynamo (пример можно найти [здесь](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Теперь при реализации JSON.NET свойства `public` производного класса NodeModel можно сериализовать непосредственно в файл DYN. JSON.Net предоставляет несколько атрибутов для управления сериализацией свойства.

Этот пример, в котором указано, что `PropertyName` находится [здесь](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38) в хранилище Dynamo.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### Конвертеры: <a href="#converters" id="converters"></a>

**Примечание.**\
 Если вы создаете собственный класс конвертера JSON.NET, в Dynamo в настоящее время нет механизма для его вставки в методы загрузки и сохранения. Поэтому даже если класс помечен атрибутом `[JsonConverter]`, он может не использоваться. Вместо этого можно вызвать конвертер непосредственно в установщике или получателе. _//TODO: требуется подтверждение этого ограничения. Приветствуются любые подтверждения._

Пример, указывающий метод сериализации для преобразования свойства в строку, находится [здесь](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) в репозитории Dynamo.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### Игнорирование свойств <a href="#ignoring-properties" id="ignoring-properties"></a>

К свойствам `public`, которые не предназначены для сериализации, необходимо добавить атрибут `[JsonIgnore]`. Если узлы сохраняются в файле DYN, эти данные игнорируются механизмом сериализации и не приводят к непредвиденным последствиям при повторном открытии графика. Пример можно найти [здесь](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) в репозитории Dynamo.



#### Отмена и повтор <a href="#undoredo" id="undoredo"></a>

Как уже упоминалось выше, методы `SerializeCore` и `DeserializeCore` ранее использовались для сохранения и загрузки узлов в файл XML DYN. Кроме того, они также использовались для сохранения и загрузки состояния узла для команд отмены и повтора, и **по-прежнему применяются в этих целях.** Если требуется реализовать сложную функцию отмены и повтора для узла пользовательского интерфейса nodeModel, необходимо реализовать эти методы и сериализовать их в объект XML-документа, предоставляемый в качестве параметра для этих методов. Применяйте этот способ, только если используете сложные узлы пользовательского интерфейса.

#### API портов ввода и вывода <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

Один из аспектов в узлах nodeModel, на которые повлияют изменения в API версии 2.0, — это регистрация портов в конструкторе узлов. Примеры в Dynamo или DynamoSamples можно найти в предыдущих версиях, где использовались методы `InPortData.Add()` и `OutPortData.Add()`. Ранее в API Dynamo открытые свойства `InPortData` и `OutPortData` были помечены как устаревшие. В версии 2.0 эти свойства удалены. Разработчикам теперь следует использовать методы `InPorts.Add()` и `OutPorts.Add()`. Кроме того, у этих двух методов `Add()` немного отличаются подписи:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated` (Старая версия 1.3 действительна, но устарела)

vs

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0` (Рекомендуется 2.0)

Примеры преобразованного кода можно найти в файле Dynamo Repo -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) или [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281)

Еще один распространенный вариант использования, на который влияют изменения API версии 2.0, относится к методам, обычно используемым в методе `BuildAst()` для определения поведения узла в зависимости от наличия или отсутствия соединителей портов. Ранее `HasConnectedInput(index)` использовался для проверки состояния подключенного порта. Теперь разработчикам следует использовать свойство `InPorts[0].IsConnected` для проверки состояния подключения порта. Пример такого соединения можно найти в файле [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) в репозитории Dynamo.

### Примеры: <a href="#examples" id="examples"></a>

Рассмотрим обновление узла пользовательского интерфейса версии 1.3 до версии Dynamo 2.x.

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

Все, что нам нужно сделать с этим классом `nodeModel`, чтобы он корректно загружался и сохранялся в версии 2.0, — это добавить jsonConstructor для обработки загрузки портов. Мы просто передаем порты в базовом конструкторе, и это пустая реализация.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Примечание. Не вызывайте `RegisterPorts()` и какие-либо его варианты в JsonConstructor, потому что при этом для создания новых портов будут использоваться атрибуты входных и выходных параметров класса узла. **Нам это не нужно**, так как требуется использовать загруженные порты, которые передаются в конструктор.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

В этом примере конструктор JSON добавляется с минимальной загрузкой. Но что если нам потребуется более сложная логика построения, например, настроить слушатели для обработки событий в конструкторе? Следующий пример, взятый из \
[репозитория DynamoSamples](https://github.com/DynamoDS/DynamoSamples), приведен выше в разделе `JsonConstructors Section` в этом документе.

Ниже приведен более сложный конструктор для узла пользовательского интерфейса:

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

При добавлении конструктора JSON для загрузки этого узла из файла необходимо воссоздать часть этой логики, но при этом следует помнить, что мы не включаем код, который создает порты, устанавливает переплетение или задает значения по умолчанию для свойств, которые можно загрузить из файла.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Обратите внимание, что другие общие свойства, сериализованные в JSON, например `ButtonText` и `WindowText`, не будут добавлены в конструктор как явные параметры. Они устанавливаются автоматически в JSON.NET с помощью методов задания этих свойств.
