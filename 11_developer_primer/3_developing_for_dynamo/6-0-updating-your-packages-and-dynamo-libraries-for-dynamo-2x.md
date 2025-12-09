# Updating your Packages and Dynamo Libraries for Dynamo 2.x

### Introduction: <a href="#introduction" id="introduction"></a>

Dynamo 2.0 is a major release and some APIs have been changed or removed. One of the largest changes that will effect node and package authors is our move to a JSON file format.

In general Zero Touch node authors will have little to no work to do to get their packages running in 2.0.

UI nodes and nodes that derive directly from NodeModel will take more work to get running in 2.x.

Extension authors may also have some potential changes to make - depending on how much of the Dynamo core APIs they use in their extensions.

***

### General packaging rules: <a href="#general-packaging-rules" id="general-packaging-rules"></a>

* Do not bundle Dynamo or Dynamo Revit .dlls with your package. These dlls will already be loaded by dynamo. If you bundle a different version than the user has loaded _(ie you distribute dynamo core 1.3 but the user is running your package on dynamo 2.0)_ mysterious runtime bugs will occur. This includes dlls like `DynamoCore.dll`, `DynamoServices.dll`, `DSCodeNodes.dll`, `ProtoGeometry.dll`
* Do not bundle and distribute `newtonsoft.json.net` with your package if you can avoid it. This dll will also be loaded by dynamo 2.x already. The same issue as above can occur.
* Do not bundle and distribute `CEFSharp` with your package if you can avoid it. This dll will also be loaded by Dynamo 2.x already. The same issue as above can occur.
* In general avoid sharing dependencies with dynamo or revit if you need to control the version of that dependency.

### Common Issues: <a href="#common-issues" id="common-issues"></a>

1\) Upon opening a graph some nodes have multiple ports with the same name, but the graph looked fine when saving. This issue can have a few causes.

The common root cause is because the node was created using a constructor that recreated the ports. Instead a constructor which loaded the ports should have been used. These constructors are usually marked `[JsonConstructor]` _see below for examples_

![Broken JSON](images/broken-json.jpg)

This can occur because:

* There was simply no matching `[JsonConstructor]`, or it was not passed the `Inports` and `Outports` from the JSON .dyn.
* There were two versions of JSON.net loaded into the same process at the same time causing .net runtime failure so that the `[JsonConstructor]` attribute could not be used correctly to mark the constructor.
* DynamoServices.dll with a different version than the current dynamo version has been bundled with the package and is causing the .net runtime to fail to identify the `[MultiReturn]` attribute so zero touch nodes marked with various attributes will fail to have them applied. You might find that a node returns a single dictionary output instead of multiple ports.

2\) Nodes are completely missing upon loading the graph with some errors in the console.

* This might occur if your deserialization failed for some reason. It's good practice to serialize only properties you need. We can use `[JsonIgnore]` on complex properties you don't need to load or save to ignore them. Properties like a `function pointer, delegate, action,` or `event` etc. These should not be serialized as they will usually fail to deserialize and cause a runtime error.

### Upgrading In Depth: <a href="#upgrading-in-depth" id="upgrading-in-depth"></a>

### Custom Nodes 1.3 - > 2.0 <a href="#custom-nodes-13----20" id="custom-nodes-13----20"></a>

[Organizing Custom Nodes in librarie.js](https://github.com/DynamoDS/Dynamo/wiki/Library-2.0-Add-Ons-Organization#customnodes)

Known Issues:

* A coinciding custom node name and category name at the same level in librarie.js causes unexpected behavior. [QNTM-3653](https://jira.autodesk.com/browse/QNTM-3653) - avoid using the same names for category and nodes.
* Comments will be turned into block comments instead of line comments.
* Short type names will be replaced with full names. For example if you did not specify a type when you load the custom node again you will see `var[]..[]` - as this is the default type.

### Zero Touch Nodes 1.3 -> 2.0 <a href="#zero-touch-nodes-13---20" id="zero-touch-nodes-13---20"></a>

* In Dynamo 2.0 List and Dictionary types have been split and the syntax for creating Lists and Dictionaries has been changed. Lists are initialized using `[]` while dictionaries use `{}`.\
  If you were previously using the `DefaultArgument` attribute to mark parameters on your zero touch nodes and used list syntax to default to a specific list like `someFunc([DefaultArgument("{0,1,2}")])` - this will no longer be valid, and you will need to modify the DesignScript snippet to use the new initialization syntax for lists.
* As noted above do not distribute Dynamo dlls with your packages. (`DynamoCore`, `DynamoServices` etc).

### Node Model Nodes 1.3 -> 2.0 <a href="#node-model-nodes-13---20" id="node-model-nodes-13---20"></a>

Node Model nodes require the most work to update to Dynamo 2.x. At a high level you will need to implement constructors that will only be used for loading your nodes from json in addition to the regular nodeModel constructors that are used for instantiating new instances of your node types. To differentiate between these you mark the load time constructors with `[JsonConstructor]` which is an attribute from newtonsoft.Json.net.

The names of the parameters in the constructor should generally match the names of the JSON properties - though this mapping gets more complicated if you override the names that are serialized using \[JsonProperty] attributes.\
[See the Json.net documentation for more information.](https://www.newtonsoft.com/json/help/html/Introduction.htm)

#### JSON Constructors <a href="#json-constructors" id="json-constructors"></a>

The most common change required for updating nodes derived from the `NodeModel` base class (or other dynamo core base classes, ie `DSDropDownBase`) is the need to add a JSON constructor to your class.

Your original parameter-less constructor will still handle initializing a new node that is created within Dynamo (via the library for example). The JSON constructor is required to initialize a node that is deserialized _(loaded)_ from a saved .dyn or .dyf file.

The JSON constructor differs from the base constructor in that it has `PortModel` parameters for the `inPorts` and `outPorts`, which are provided by the JSON loading logic. The call to register the ports for the node are not required here, as the data exists in the .dyn file. An example of a JSON constructor looks like this:

`using Newtonsoft.Json; //New dependency for Json`

………

`[JsonConstructor] //Attribute required to identity the Json constructor`

`//Minimum constructor implementation. Note that the base method invocation must also be present.`

`FooNode(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts) { }`

This syntax `:base(Inports,outPorts){}` calls the base `nodeModel` constructor and passes the deserialized ports to it.

Any special logic that existed in the class constructor that involves initialization of specific data that is serialized into the .dyn file _(for example setting Port registration, lacing strategy, etc)_ are not required to be repeated in this constructor as these values can be read from the JSON.

This is the major difference between the JSON constructor and non-JSON constructors for your nodeModels. JSON constructors are invoked when loading from a file and are passed loaded data. Other user logic however must be duplicated in the JSON constructor _(for example, initializing event handlers for the node or attaching)_.

Examples can be found here in the DynamoSamples repo -> [ButtonCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/ButtonCustomNodeModel.cs#L156), [DropDown](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs#L23), or [SliderCustomNodeModel](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/SliderCustomNodeModel.cs#L123)

#### Public Properties and Serialization <a href="#public-properties-and-serialization" id="public-properties-and-serialization"></a>

Previously a developer could serialize and deserialize specific model data to the xml document via the `SerializeCore` and `DeserializeCore` methods. These methods still exist in the API but will be deprecated in a future release of Dynamo (an example can be found [here](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/DoubleSlider.cs#L140)). With the JSON.NET implementation now `public` properties on the NodeModel derived class can be serialized to the .dyn file directly. JSON.Net provides multiple attributes to control how the property is serialized.

This example which specifies a `PropertyName` is found [here](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/Input/ColorPalette.cs#L38) in the Dynamo repo.

`[JsonProperty(PropertyName = "InputValue")]`

`public DSColor DsColor {...`

#### Converters: <a href="#converters" id="converters"></a>

**Note**\
If you make your own JSON.net converter class, Dynamo does not currently have a mechanism to let you inject it into the load and save methods and so even if you mark your class with the `[JsonConverter]` attribute it may not be used - instead you can call your converter directly in your setter or getter. _//TODO need confirmation of this limitation. Any evidence is welcome._

An example that specifies a serialization method to convert the property to a string is found [here](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L66) in the Dynamo repo.

`[JsonProperty("MeasurementType"), JsonConverter(typeof(StringEnumConverter))]`

`public ConversionMetricUnit SelectedMetricConversion{...`

#### Ignoring Properties <a href="#ignoring-properties" id="ignoring-properties"></a>

`public` properties that are not meant for serialization need to have the `[JsonIgnore]` attribute added. When the nodes is saved to the .dyn file this insures this data is ignored by the serialization mechanism and will not cause unexpected consequences when the graph is opened again. An example of this can be [here](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L45) in the Dynamo repo.

***

#### Undo/Redo <a href="#undoredo" id="undoredo"></a>

As mentioned above `SerializeCore` and `DeserializeCore` methods were used in the past to save and load nodes into the xml .dyn file. In addition they were also used to save and load the node state for undo/redo and **still are!** If you wish to implement complex undo/redo functionality for your nodeModel UI node then you will need to implement these methods and serialize into the XML document object provided as a parameter to these methods. This should be a rare use case except for complex UI nodes.

#### Input and Output Port APIs <a href="#input-and-output-port-apis" id="input-and-output-port-apis"></a>

One common occurrence in nodeModel nodes affected by 2.0 API changes is port registration in the node constructor. Looking at examples in the Dynamo or DynamoSamples repo you previously will have found use of the `InPortData.Add()` or `OutPortData.Add()` methods. Previously in the Dynamo API the `InPortData` and `OutPortData` public properties were marked as deprecated. In 2.0 these properties have been removed. Developers should now use `InPorts.Add()` and `OutPorts.Add()` methods. Additionally these two `Add()` methods have slightly different signatures:

`InPortData.Add(new PortData("Port Name", "Port Description")); //Old version valid in 1.3 but now deprecated`

vs

`InPorts.Add(new PortModel(PortType.Input, this, new PortData("Port Name", "Port Description"))); //Recommended 2.0`

Examples of converted code can be found here in the Dynamo Repo -> [DynamoConvert.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/DynamoConvert.cs#L142), or [FileSystem.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/Input/FileSystem.cs#L281)

The other common use case that is affected by the 2.0 API changes relates to the methods commonly used in the `BuildAst()` method to determine node behavior based on the presence or absence of port connectors. Previously `HasConnectedInput(index)` was used to validate a connected port state. Developers should now use the `InPorts[0].IsConnected` property to check the port connection state. An example of the this con be found in [ColorRange.cs](https://github.com/DynamoDS/Dynamo/blob/RC2.0.0\_master/src/Libraries/CoreNodeModels/ColorRange.cs#L83) in the Dynamo Repo.

### Examples: <a href="#examples" id="examples"></a>

Let's walk through upgrading a 1.3 UI node to Dynamo 2.x.

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

All we need to do to this `nodeModel` class to get it loading and saving correctly in 2.0 is add a jsonConstructor to handle loading of the ports. We simply pass the ports on the base constructor and this implementation is empty.

```
[JsonConstructor]
protected GridNodeModel(IEnumerable<PortModel> Inports, IEnumerable<PortModel> Outports ) :
base(Inports,Outports)
{

}
```

Note: Do not call `RegisterPorts()` or some variation of that in your JsonConstructor - this will use the input and output parameter attributes on your node class to construct new ports! **We don't want this**, since we want to use the loaded ports which are passed to your constructor.

```
[InPortNames("xCount", "yCount")]
[InPortTypes("double", "double")]
```

This example adds the minimal loading JSON constructor possible. But what if we need to do some more complex construction logic, like setup some listeners for event handling inside the constructor. The next sample taken from the\
[DynamoSamples Repo](https://github.com/DynamoDS/DynamoSamples) is linked above in the `JsonConstructors Section` of this document.

Here is a more complex constructor for a UI node:

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

When we add a JSON constructor for loading this node from a file we have to recreate some of this logic, but note that we do not include the code that creates ports, sets lacing, or sets the default values for properties which we can load from the file.

```
        // This constructor is called when opening a Json graph.

        [JsonConstructor]
        ButtonCustomNodeModel(IEnumerable<PortModel> inPorts, IEnumerable<PortModel> outPorts) : base(inPorts, outPorts)
        {
            this.PortDisconnected += ButtonCustomNodeModel_PortDisconnected;
            ButtonCommand = new DelegateCommand(ShowMessage, CanShowMessage);
        }
```

Note that other public properties that were serialized into the JSON like `ButtonText` and `WindowText` will not to be added as explicit parameters to the constructor - they are set automatically by JSON.net using the setters for those properties.
