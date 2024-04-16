# Atualizar os pacotes e as bibliotecas do Dynamo para Dynamo 2.x

### Introdução: <a href="#introduction" id="introduction"></a>

O Dynamo 2.0 é uma versão principal e algumas APIs foram alteradas ou removidas. Uma das maiores alterações que afetarão os autores de nó e pacote é a nossa mudança para um formato de arquivo JSON.

Em geral, os autores do nó Sem toque não terão muito trabalho a fazer para que os pacotes sejam executados na versão 2.0.

Os nós da interface do usuário e os nós que derivam diretamente do NodeModel exigirão mais trabalho para serem executados na versão 2.x.

Os autores da extensão também podem ter algumas alterações potenciais a serem feitas, dependendo de quanto das APIs principais do Dynamo eles usam em suas extensões.

***

### Regras gerais de empacotamento: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Não agrupe o Dynamo ou o Dynamo Revit .dlls com o pacote. Esses dlls já serão carregados pelo Dynamo. Se você agrupar uma versão diferente da que o usuário carregou _(ou seja, você distribui o dynamo core 1.3, mas o usuário está executando o pacote no dynamo 2.0) _, ocorrerão erros misteriosos de tempo de execução. Isso inclui dlls como `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll`
* Não agrupe nem distribua o `newtonsoft.json.net` com o pacote se você puder evitá-lo. Esse dll também será carregado pelo Dynamo 2.x. O mesmo problema que acima pode ocorrer.
* Não agrupe nem distribua o `CEFSharp` com o pacote se você puder evitá-lo. Esse dll também será carregado pelo Dynamo 2.x. O mesmo problema que acima pode ocorrer.
* Em geral, evite compartilhar dependências com o Dynamo ou o Revit se precisar controlar a versão dessa dependência.

### Problemas comuns: <a href="#common-issues" id="common-issues"></a>

1\) Durante a abertura de um gráfico, alguns nós têm várias portas com o mesmo nome, mas o gráfico parecia correto ao salvar. Esse problema pode ter algumas causas.

A causa raiz comum é porque o nó foi criado usando um construtor que recriou as portas. Em vez disso, o construtor que carregou as portas deveria ter sido usado. Esses construtores são geralmente marcados `[JsonConstructor]`_veja exemplos abaixo_

![JSON quebrado](images/broken-json.jpg)

Isso pode ocorrer porque:

* Não havia nenhum `[JsonConstructor]` correspondente ou ele não foi passado de `Inports` e `Outports` do .dyn JSON.
* Havia duas versões do JSON.net carregadas no mesmo processo ao mesmo tempo, causando falha de tempo de execução .net, de modo que o atributo `[JsonConstructor]` não podia ser usado corretamente para marcar o construtor.
* O DynamoServices.dll com uma versão diferente da versão atual do Dynamo foi fornecido com o pacote e está causando a falha do tempo de execução .net ao identificar o atributo `[MultiReturn]`; portanto, os nós Sem toque marcados com vários atributos não terão sido aplicados. Você pode descobrir que um nó retorna uma única saída de dicionário em vez de várias portas.

2\) Os nós estão completamente ausentes ao carregar o gráfico com alguns erros no console.

* Isso pode ocorrer se a desserialização falhar por algum motivo. É uma boa prática serializar somente as propriedades que você precisa. Podemos usar `[JsonIgnore]` em propriedades complexas que não precisam ser carregadas ou salvas para serem ignoradas. Propriedades como `function pointer, delegate, action,` ou `event` etc. Elas não devem ser serializadas, pois normalmente falharão ao desserializar e causarão um erro de tempo de execução.

### Atualização em profundidade: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Nós personalizados 1.3 -> 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Organizar nós personalizados no arquivo librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Problemas conhecidos:

* Um nome de nó personalizado coincidente e um nome de categoria no mesmo nível no librarie.js causa um comportamento inesperado. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) – evite usar os mesmos nomes para a categoria e os nós.
* Os comentários serão transformados em comentários de bloco ao invés de comentários de linha.
* Nomes de tipo curto serão substituídos por nomes completos. Por exemplo, se você não especificou um tipo ao carregar o nó personalizado novamente, verá `var[]..[]` – pois esse é o tipo padrão.

### Nós Sem toque 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* No Dynamo 2.0, os tipos Lista e Dicionário foram divididos e a sintaxe para a criação de listas e dicionários foi alterada. As listas são inicializadas usando `[]`, enquanto os dicionários usam `{}`.\
 Se você estava usando o atributo `DefaultArgument` anteriormente para marcar parâmetros nos nós sem toque e usou a sintaxe da lista para padronizar para uma lista específica como `someFunc([DefaultArgument("{0,1,2}")])`, isso não será mais válido e será necessário modificar o fragmento DesignScript para usar a nova sintaxe de inicialização das listas.
* Como observado acima, não distribua os dlls do Dynamo com os pacotes. (`DynamoCore`, `DynamoServices` etc.)

### Nós do modelo de nós 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Os nós do modelo de nós são os que requerem mais trabalho para serem atualizados para o Dynamo 2.x. Em um nível alto, será necessário implementar construtores que serão usados somente para carregar os nós do json, além dos construtores nodeModel normais que são usados para instanciar novas instâncias dos tipos de nó. Para diferenciar entre esses, marque os construtores de tempo de carga com `[JsonConstructor]`, que é um atributo de newtonsoft.Json.net.

Os nomes dos parâmetros no construtor devem normalmente coincidir com os nomes das propriedades JSON – embora esse mapeamento se torne mais complicado se você substituir os nomes que são serializados usando os atributos [JsonProperty].\
 [Consulte a documentação do Json.net para obter mais informações.](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### Construtores JSON <a href="#json-constructors" id="json-constructors"></a>

A alteração mais comum necessária para atualizar nós derivados da classe base `NodeModel` (ou outras classes base do núcleo do Dynamo, ou seja, `DSDropDownBase`) é a necessidade de adicionar um construtor JSON à classe.

O construtor original sem parâmetros ainda lidará com a inicialização de um novo nó que é criado no Dynamo (por exemplo, através da biblioteca). O construtor JSON é necessário para inicializar um nó que é desserializado _(carregado)_ de um arquivo .dyn ou .dyf salvo.

O construtor JSON difere do construtor base no fato de ter parâmetros `PortModel` para `inPorts` e `outPorts`, que são fornecidos pela lógica de carregamento JSON. A chamada para registrar as portas para o nó não é necessária aqui, pois os dados existem no arquivo .dyn. Um exemplo de construtor JSON se parece com isto:

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Esta sintaxe `:base(Inports,outPorts){}` chama o construtor base `nodeModel` e passa as portas desserializadas para ele.

Qualquer lógica especial que existisse no construtor de classe que envolva a inicialização de dados específicos que são serializados no arquivo .dyn _(por exemplo, definir o registro de porta, estratégia de amarra, etc)_ não precisa ser repetida nesse construtor, pois esses valores podem ser lidos do JSON.

Essa é a principal diferença entre o construtor JSON e os construtores não JSON para os nodeModels. Os construtores JSON são chamados ao carregar de um arquivo e são passados os dados carregados. No entanto, outra lógica do usuário deve ser duplicada no construtor JSON _(por exemplo, inicializando manipuladores de eventos para o nó ou anexando)_.

Exemplos podem ser encontrados aqui no repositório do DynamoSamples -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) ou [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123)

#### Propriedades públicas e serialização <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Anteriormente, um desenvolvedor podia serializar e desserializar dados de modelo específicos para o documento xml usando os métodos `SerializeCore` e `DeserializeCore`. Esses métodos ainda existem na API, mas serão obsoletos em uma versão futura do Dynamo (um exemplo pode ser encontrado [aqui](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Com a implementação do JSON.NET, agora as propriedades `public` na classe derivada NodeModel podem ser serializadas diretamente para o arquivo .dyn. O JSON.Net fornece vários atributos para controlar como a propriedade é serializada.

Esse exemplo que especifica um `PropertyName` é encontrado [aqui](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38) no repositório do Dynamo.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### Conversores: <a href="#converters" id="converters"></a>

**Observação**\
 Se você criar sua própria classe de conversor JSON.net, o Dynamo não terá um mecanismo que permita injetá-la nos métodos de carregamento e salvamento e, portanto, mesmo que você marque a classe com o atributo `[JsonConverter]`, ela poderá não ser usada. Em vez disso, você poderá chamar o conversor diretamente no configurador ou getter. _//TODO precisa de confirmação dessa limitação. Qualquer evidência é bem-vinda._

Um exemplo que especifica um método de serialização para converter a propriedade em uma sequência de caracteres é encontrado [aqui](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) no repositório do Dynamo.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### Ignorar as propriedades <a href="#ignoring-properties" id="ignoring-properties"></a>

As propriedades `public` que não são destinadas à serialização precisam ter o atributo `[JsonIgnore]` adicionado. Quando os nós são salvos no arquivo .dyn, isso garante que esses dados sejam ignorados pelo mecanismo de serialização e não causará consequências inesperadas quando o gráfico for aberto novamente. Um exemplo disso pode ser encontrado [aqui](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) no repositório do Dynamo.

***

#### Desfazer/Refazer <a href="#undoredo" id="undoredo"></a>

Como mencionado acima, os métodos `SerializeCore` e `DeserializeCore` foram usados no passado para salvar e carregar nós no arquivo xml .dyn. Além disso, eles também foram usados para salvar e carregar o estado do nó para desfazer/refazer e **ainda são**. Se você desejar implementar a funcionalidade desfazer/refazer complexa para o nó da interface do usuário nodeModel, será necessário implementar esses métodos e serializar no objeto de documento XML fornecido como um parâmetro para esses métodos. Esse deve ser um caso de uso raro, exceto para nós de interface do usuário complexos.

#### APIs de porta de entrada e saída <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

Uma ocorrência comum nos nós nodeModel afetados pelas alterações da API 2.0 é o registro de porta no construtor do nó. Observando exemplos no repositório do Dynamo ou DynamoSamples, você terá encontrado o uso dos métodos `InPortData.Add()` ou `OutPortData.Add()`. Anteriormente, na API do Dynamo, as propriedades `InPortData` e `OutPortData` públicas eram marcadas como obsoletas. Na versão 2.0, essas propriedades foram removidas. Os desenvolvedores agora devem usar os métodos `InPorts.Add()` e `OutPorts.Add()`. Além disso, esses dois métodos `Add()` têm assinaturas ligeiramente diferentes:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

x

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Exemplos de código convertido podem ser encontrados aqui no repositório do Dynamo -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) ou [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281)

O outro caso de uso comum afetado pelas alterações da API 2.0 se relaciona aos métodos comumente usados no método `BuildAst()` para determinar o comportamento do nó com base na presença ou ausência de conectores de porta. Anteriormente, `HasConnectedInput(index)` era usado para validar um estado de porta conectada. Os desenvolvedores agora devem usar a propriedade `InPorts[0].IsConnected` para verificar o estado de conexão da porta. Um exemplo desse ícone pode ser encontrado em [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) no repositório do Dynamo.

### Exemplos: <a href="#examples" id="examples"></a>

Vamos passar pela atualização de um nó de interface do usuário 1.3 para o Dynamo 2.x.

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

Tudo o que precisamos fazer para esta classe `nodeModel` para que ela seja carregada e salva corretamente na versão 2.0 é adicionar um jsonConstructor para lidar com o carregamento das portas. Simplesmente passamos as portas no construtor base e essa implementação fica vazia.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Observação: Não chame `RegisterPorts()` ou alguma variação disso no JsonConstructor. Isso usará os atributos de parâmetro de entrada e saída na classe de nó para construir novas portas. **Não queremos isso**, pois queremos usar as portas carregadas que são passadas para o construtor.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

Este exemplo adiciona o construtor JSON de carga mínima possível. Mas e se precisarmos fazer uma lógica de construção mais complexa, como configurar alguns ouvintes para manipulação de eventos dentro do construtor? A próxima amostra obtida do\
 [Repositório do DynamoSamples](https://github.com/DynamoDS/DynamoSamples) está vinculada acima na `JsonConstructors Section` deste documento.

Aqui está um construtor mais complexo para um nó de interface do usuário:

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

Quando adicionamos um construtor JSON para carregar esse nó de um arquivo, precisamos recriar parte dessa lógica, mas observe que não incluímos o código que cria portas, que define amarra ou que define os valores padrão para propriedades que podemos carregar do arquivo.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Observe que outras propriedades públicas que foram serializadas no JSON como `ButtonText` e `WindowText` não serão adicionadas como parâmetros explícitos ao construtor – elas são definidas automaticamente pelo JSON.net usando os definidores para essas propriedades.
