# Actualización de paquetes y bibliotecas de Dynamo para Dynamo 2.x

### Introducción <a href="#introduction" id="introduction"></a>

Dynamo 2.0 es una versión principal y se han modificado o eliminado algunas API. Uno de los mayores cambios que afectará a los autores de nodos y paquetes es el paso a un formato de archivo JSON.

En general, los autores de nodos Zero-Touch tendrán poco o ningún trabajo que hacer para que sus paquetes funcionen en 2.0.

Los nodos de interfaz de usuario y aquellos que proceden directamente de NodeModel requerirán más trabajo para funcionar en 2.x.

Es posible que los autores de extensiones también deban realizar algunos cambios en función de la cantidad de API de Dynamo Core que utilicen en sus extensiones.



### Reglas generales de empaquetado <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* No incluya archivos .dll de Dynamo o de DynamoRevit en el paquete. Dynamo ya cargará estos archivos. Si compila una versión diferente a la que ha cargado el usuario _(es decir, si distribuye Dynamo Core 1.3, pero el usuario ejecuta el paquete en Dynamo 2.0)_, se producirán extraños errores de ejecución. Esto incluye archivos .dll como `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll` y `ProtoGeometry.dll`.
* No agrupe y distribuya `newtonsoft.json.net` con el paquete si puede evitarlo. Dynamo 2.x también cargará este archivo dll. Puede producirse el mismo problema indicado anteriormente.
* No agrupe y distribuya `CEFSharp` con el paquete si puede evitarlo. Dynamo 2.x también cargará este archivo dll. Puede producirse el mismo problema indicado anteriormente.
* En general, evite compartir dependencias con Dynamo o Revit si necesita controlar la versión de esa dependencia.

### Problemas frecuentes <a href="#common-issues" id="common-issues"></a>

1) Al abrir un gráfico, algunos nodos tienen varios puertos con el mismo nombre, pero el gráfico presenta un aspecto correcto al guardar. Este problema puede tener varias causas.

La principal causa habitual es que el nodo se ha creado con un constructor que ha vuelto a crear los puertos. En su lugar, debería haberse utilizado un constructor que cargara los puertos. Estos constructores suelen estar marcados con `[JsonConstructor]`. _Consulte los ejemplos siguientes:_

![JSON interrumpido](images/broken-json.jpg)

Esto puede deberse a lo siguiente:

* No se ha encontrado ningún `[JsonConstructor]` coincidente o no se han transferido los archivos `Inports` y `Outports` del archivo .dyn de JSON.
* Había dos versiones de JSON.net cargadas en el mismo proceso al mismo tiempo, lo que provocaba fallos en el tiempo de ejecución de .net, por lo que el atributo `[JsonConstructor]` no podía utilizarse correctamente para marcar el constructor.
* DynamoServices.dll con una versión diferente a la versión actual de Dynamo se ha incluido con el paquete, lo que provoca que el tiempo de ejecución de .net no identifique el atributo `[MultiReturn]`, por lo que los nodos Zero-Touch marcados con varios atributos no podrán aplicarlos. Es posible que un nodo devuelva una única salida de diccionario en lugar de varios puertos.

2) Faltan completamente nodos al cargar el gráfico con algunos errores en la consola.

* Esto puede producirse si la deserialización falla por algún motivo. Es recomendable serializar solo las propiedades necesarias. Podemos utilizar `[JsonIgnore]` en propiedades complejas que no sea necesario cargar o guardar para omitirlas como, por ejemplo, `function pointer, delegate, action,` o `event`, etc. No deben serializarse, ya que normalmente fallarán durante la deserialización y provocarán un error de ejecución.

### Descripción detallada de la actualización: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Nodos personalizados 1.3 - > 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Organización de nodos personalizados en librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Problemas conocidos

* La coincidencia de un nombre de nodo y un nombre de categoría personalizados en el mismo nivel en librarie.js provoca un comportamiento inesperado. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653): evite utilizar los mismos nombres para la categoría y los nodos.
* Los comentarios se convertirán en comentarios de bloque en lugar de comentarios de línea.
* Los nombres de tipo cortos se sustituirán por nombres completos. Por ejemplo, si no ha especificado un tipo al cargar de nuevo el nodo personalizado, aparecerá `var[]..[]`, ya que este es el tipo por defecto.

### Nodos Zero-Touch 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* En Dynamo 2.0, los tipos de lista y diccionario se han dividido y se ha cambiado la sintaxis para crear listas y diccionarios. Las listas se inicializan mediante `[]` mientras que los diccionarios utilizan `{}`.\
 Si anteriormente utilizaba el atributo `DefaultArgument` para marcar parámetros en los nodos Zero-Touch y utilizaba la sintaxis de lista para establecer por defecto una lista específica como `someFunc([DefaultArgument("{0,1,2}")])`, esto ya no será válido y deberá modificar el fragmento de código de DesignScript para utilizar la nueva sintaxis de inicialización para las listas.
* Como se ha indicado anteriormente, no distribuya archivos dll de Dynamo con los paquetes (`DynamoCore`, `DynamoServices`, etc.).

### Nodos Node Model 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Los nodos Node Model son los que más trabajo requieren para actualizarse a Dynamo 2.x. En un nivel alto, deberá implementar constructores que solo se usen para cargar los nodos desde json, además de los constructores nodeModel habituales que se usan para crear nuevas instancias de los tipos de nodos. Para diferenciar entre estos, se marcan los constructores de tiempo de carga con `[JsonConstructor]`, que es un atributo de newtonsoft.Json.net.

Por lo general, los nombres de los parámetros del constructor deben coincidir con los nombres de las propiedades JSON, aunque esta asignación se complica más si se modifican los nombres serializados mediante atributos [JsonProperty].\
 [Consulte la documentación de Json.net para obtener más información.](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### Constructores JSON <a href="#json-constructors" id="json-constructors"></a>

El cambio más habitual requerido para actualizar nodos derivados de la clase base `NodeModel` (u otras clases base de Dynamo Core, por ejemplo, `DSDropDownBase`) es la necesidad de añadir un constructor JSON a la clase.

El constructor original sin parámetros seguirá gestionando la inicialización de un nuevo nodo creado en Dynamo (por ejemplo, mediante la biblioteca). El constructor JSON es necesario para inicializar un nodo deserializado _(cargado)_ desde un archivo .dyn o .dyf guardado.

El constructor JSON se diferencia del constructor base en que tiene parámetros `PortModel` para `inPorts` y `outPorts`, que proporciona la lógica de carga JSON. La llamada para registrar los puertos del nodo no es necesaria aquí, ya que los datos existen en el archivo .dyn. A continuación, se muestra un constructor JSON de ejemplo:

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

Esta sintaxis `:base(Inports,outPorts){}` llama al constructor base `nodeModel` y le transfiere los puertos deserializados.

No es necesario repetir en este constructor la lógica especial presente en el constructor de clase que implicará la inicialización de datos específicos serializados en el archivo .dyn _(por ejemplo, la configuración del registro del puerto, la estrategia de encaje, etc.)_, ya que estos valores se pueden leer desde JSON.

Esta es la diferencia principal entre el constructor JSON y los constructores que no son JSON para nodeModels. Se llama a los constructores JSON al cargar desde un archivo y se transfieren los datos cargados a estos. Sin embargo, debe duplicarse otra lógica de usuario en el constructor JSON _(por ejemplo, mediante la inicialización de controladores de eventos para el nodo o el enlace)_.

Aquí se pueden encontrar ejemplos en el repositorio de DynamoSamples -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23) o [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123).

#### Propiedades públicas y serialización <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Anteriormente, un desarrollador podía serializar y deserializar datos específicos del modelo en el documento xml a través de los métodos `SerializeCore` y `DeserializeCore`. Estos métodos siguen existiendo en la API, pero se dejarán de utilizar en una versión futura de Dynamo (puede encontrar un ejemplo [aquí](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). Con la implementación de JSON.NET actual, las propiedades `public` de la clase derivada NodeModel se pueden serializar directamente en el archivo .dyn. JSON.Net proporciona varios atributos para controlar cómo se serializa la propiedad.

Este ejemplo que especifica un `PropertyName` se encuentra [aquí](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38), en el repositorio de Dynamo.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### Conversores <a href="#converters" id="converters"></a>

**Nota**\
 Si crea su propia clase de conversor de JSON.net, Dynamo no dispone actualmente de un mecanismo para permitir su inserción en los métodos de carga y almacenamiento, por lo que, aunque marque la clase con el atributo `[JsonConverter]`, es posible que no se utilice; en su lugar, puede llamar al conversor directamente en el establecedor o el captador. _//TODO necesita confirmación de esta limitación. Es recomendable proporciona todas las pruebas posibles._

[Aquí](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) se proporciona un ejemplo que especifica un método de serialización para convertir la propiedad en una cadena en el repositorio de Dynamo.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### Omisión de propiedades <a href="#ignoring-properties" id="ignoring-properties"></a>

Para las propiedades `public` que no se han diseñado para la serialización, es necesario añadir el atributo `[JsonIgnore]`. Cuando se guardan los nodos en el archivo .dyn, esto garantiza que el mecanismo de serialización no omitirá estos datos y estos no provocarán consecuencias inesperadas cuando se abra de nuevo el gráfico. [Aquí](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) puede encontrar un ejemplo es esto en el repositorio de Dynamo.



#### Deshacer/rehacer <a href="#undoredo" id="undoredo"></a>

Como se ha mencionado anteriormente, los métodos `SerializeCore` y `DeserializeCore` se utilizaban anteriormente para guardar y cargar nodos en el archivo .dyn xml. Además, también se utilizaban para guardar y cargar el estado del nodo para deshacer/rehacer **y aún se utilizan**. Si desea implementar funciones complejas de deshacer/rehacer para el nodo de interfaz de usuario nodeModel, deberá implementar estos métodos y serializarlos en el objeto de documento XML proporcionado como parámetro para estos métodos. Por lo general, este caso de uso es bastante infrecuente, excepto en los nodos de interfaz de usuario complejos.

#### API de puertos de entrada y salida <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

Una incidencia habitual en los nodos nodeModel afectados por los cambios en las API de la versión 2.0 es el registro de puertos en el constructor de nodos. Si observa los ejemplos del repositorio de Dynamo o DynamoSamples, habrá detectado el uso de los métodos `InPortData.Add()` o `OutPortData.Add()`. Anteriormente, en la API de Dynamo, las propiedades públicas `InPortData` y `OutPortData` se marcaban como obsoletas. En la versión 2.0, estas propiedades se han eliminado. Los desarrolladores deberían utilizar ahora los métodos `InPorts.Add()` y `OutPorts.Add()`. Además, estos dos métodos `Add()` presentan firmas ligeramente diferentes:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

frente a

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Puede encontrar ejemplos de código convertido aquí, en el repositorio de Dynamo: > [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142) o [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281).

El otro caso de uso común que se ve afectado por los cambios en las API de la versión 2.0 está relacionado con los métodos utilizados habitualmente en el método `BuildAst()` para determinar el comportamiento de los nodos en función de la presencia o la ausencia de conectores de puerto. Anteriormente, `HasConnectedInput(index)` se utilizaba para validar un estado de puerto conectado. Los desarrolladores deben utilizar ahora la propiedad `InPorts[0].IsConnected` para comprobar el estado de conexión del puerto. Un ejemplo de esto se puede encontrar en [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83), en el repositorio de Dynamo.

### Ejemplos <a href="#examples" id="examples"></a>

Veamos cómo actualizar un nodo de interfaz de usuario 1.3 a Dynamo 2.x.

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

Todo lo que necesitamos hacer con esta clase `nodeModel` para que se cargue y se guarde correctamente en la versión 2.0 es añadir un jsonConstructor a fin de gestionar la carga de los puertos. Simplemente transferimos los puertos en el constructor base; esta implementación está vacía.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Nota: No llame a `RegisterPorts()` ni a ninguna variación de este parámetro en el JsonConstructor, ya que se utilizarán los atributos de parámetros de entrada y salida de la clase de nodo para crear nuevos puertos. **Esto no es lo que queremos**, ya que deseamos utilizar los puertos cargados que se han transferido al constructor.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

En este ejemplo, se añade el constructor JSON de carga mínima posible. ¿Pero qué pasa si necesitamos usar una lógica de construcción más compleja, como configurar algunas escuchas para la gestión de eventos dentro del constructor? El vínculo al siguiente ejemplo obtenido de\
 [DynamoSamples Repo](https://github.com/DynamoDS/DynamoSamples) aparece arriba en la `JsonConstructors Section` de este documento.

A continuación, se muestra un constructor más complejo para un nodo de interfaz de usuario:

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

Cuando añadimos un constructor JSON para cargar este nodo desde un archivo, debemos volver a crear parte de esta lógica, pero tenga en cuenta que no incluimos el código que crea los puertos, establece el encaje o define los valores por defecto para las propiedades que podemos cargar desde el archivo.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Tenga en cuenta que otras propiedades públicas serializadas en JSON como `ButtonText` y `WindowText` no se añadirán como parámetros explícitos para el constructor; JSON.net las define automáticamente mediante los establecedores de esas propiedades.
