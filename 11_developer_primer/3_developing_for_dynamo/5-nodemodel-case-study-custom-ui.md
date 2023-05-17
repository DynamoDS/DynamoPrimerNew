# Case study NodeModel - Interfaccia utente personalizzata

I nodi basati su NodeModel offrono una flessibilità e una potenza notevolmente superiori rispetto ai nodi zero-touch. In questo esempio, si porta il nodo griglia zero-touch al livello successivo aggiungendo un dispositivo di scorrimento integrato che imposta dimensioni casuali del rettangolo.

![Grafico a griglia rettangolare](images/cover-image-2.jpg)

> Il dispositivo di scorrimento consente di mettere in scala le celle rispetto alle relative dimensioni, in modo che l'utente non debba fornire un dispositivo di scorrimento con l'intervallo corretto.

#### Modello Model-View-Viewmodel <a href="#the-model-view-viewmodel-pattern" id="the-model-view-viewmodel-pattern"></a>

Dynamo è basato sul modello di architettura del software [Model-View-Viewmodel](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) (MVVM) per mantenere l'interfaccia utente separata dal back-end. Quando si creano nodi zero-touch, Dynamo esegue l'associazione tra i dati di un nodo e la relativa interfaccia utente. Per creare un'interfaccia utente personalizzata, è necessario aggiungere la logica di associazione dei dati.

Ad un livello generale sono disponibili due parti per stabilire una relazione modello-vista in Dynamo:

* Una classe `NodeModel` per stabilire la logica di base del nodo (il "modello")
* Una classe `INodeViewCustomization` per personalizzare la modalità di visualizzazione di `NodeModel` (la "vista").

> Gli oggetti NodeModel dispongono già di una relazione vista-modello associata (NodeViewModel), pertanto possiamo concentrarci solo sul modello e sulla vista per l'interfaccia utente personalizzata.

#### Come implementare NodeModel <a href="#how-to-implement-nodemodel" id="how-to-implement-nodemodel"></a>

I nodi NodeModel presentano diverse differenze significative rispetto ai nodi zero-touch, che verranno illustrate in questo esempio. Prima di passare alla personalizzazione dell'interfaccia utente, iniziamo costruendo la logica NodeModel.

**1\. Creare la struttura di progetto:**

Un nodo NodeModel può chiamare solo funzioni, pertanto è necessario separare NodeModel e le funzioni in librerie diverse. Il modo standard per eseguire questa operazione per i pacchetti di Dynamo consiste nella creazione di progetti separati per ciascuno di essi. Iniziare creando una nuova soluzione per includere i progetti.

> 1. Selezionare `File > New > Project`.
> 2. Selezionare `Other Project Types` per visualizzare l'opzione Solution.
> 3. Selezionare `Blank Solution`.
> 4. Assegnare un nome alla soluzione `CustomNodeModel`.
> 5. Selezionare `Ok`.

Creare due progetti di libreria di classi C# nella soluzione: uno per le funzioni e uno per implementare l'interfaccia NodeModel.

![Aggiunta di una nuova libreria di classi](images/vs-new-class-projects.jpg)

> 1. Fare clic con il pulsante destro del mouse sulla soluzione e selezionare `Add > New Project`.
> 2. Scegliere Class Library.
> 3. Assegnare il nome `CustomNodeModel`.
> 4. Fare clic su `Ok`.
> 5. Ripetere la procedura per aggiungere un altro progetto denominato `CustomNodeModelFunctions`.

Successivamente, è necessario rinominare le librerie di classi create automaticamente e aggiungerne una al progetto `CustomNodeModel`. La classe `GridNodeModel` implementa la classe astratta NodeModel, la classe `GridNodeView` viene utilizzata per personalizzare la vista e `GridFunction` contiene eventuali funzioni che è necessario chiamare.

![Solution Explorer](images/vs-new-class.jpg)

> 1. Aggiungere un'altra classe facendo clic con il pulsante destro del mouse sul progetto `CustomNodeModel`, selezionando `Add > New Item...` e scegliendo `Class`.
> 2. Nel progetto `CustomNodeModel`, sono necessarie le classi `GridNodeModel.cs` e `GridNodeView.cs`.
> 3. Nel progetto `CustomNodeModelFunction`, è necessaria una classe `GridFunctions.cs`.

Prima di aggiungere qualsiasi codice alle classi, aggiungere i pacchetti necessari per questo progetto. `CustomNodeModel` richiede ZeroTouchLibrary e WpfUILibrary e `CustomNodeModelFunction` richiede solo ZeroTouchLibrary. Si utilizzerà WpfUILibrary nella personalizzazione dell'interfaccia utente che verrà eseguita in seguito e si userà ZeroTouchLibrary per la creazione della geometria. I pacchetti possono essere aggiunti singolarmente per i progetti. Poiché questi pacchetti presentano dipendenze, Core e DynamoServices verranno installati automaticamente.

![Installazione dei pacchetti](images/vs-add-packages.jpg)

> 1. Fare clic con il pulsante destro del mouse su un progetto e selezionare `Manage NuGet Packages`.
> 2. Installare solo i pacchetti necessari per il progetto.

Visual Studio copierà i pacchetti NuGet a cui si fa riferimento nella directory della build. Questa opzione può essere impostata su False, in modo da non includere eventuali file non necessari nel pacchetto.

![Disattivazione della copia locale del pacchetto](images/vs-disable-package-copying.jpg)

> 1. Seleziona i pacchetti NuGet di Dynamo.
> 2. Impostare `Copy Local` su False.

**2\. Ereditare la classe NodeModel**

Come accennato in precedenza, l'aspetto principale che rende un nodo NodeModel diverso da un nodo zero-touch è la sua implementazione della classe `NodeModel`. Un nodo NodeModel richiede diverse funzioni di questa classe ed è possibile ottenerle aggiungendo `:NodeModel` dopo il nome della classe.

Copiare il seguente codice in `GridNodeModel.cs`.

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

È differente dai nodi zero-touch. Cerchiamo di capire cosa fa ogni parte.

* Specificare gli attributi del nodo, ad esempio NodeName, NodeCategory, InPortNames/OutPortNames, InPortTypes/OutPortTypes, InPortDescriptions/OutPortDescriptions.
* `public class GridNodeModel : NodeModel` è una classe che eredita la classe `NodeModel` da `Dynamo.Graph.Nodes`.
* `public GridNodeModel() { RegisterAllPorts(); }` è un costruttore che registra gli input e gli output del nodo.
* `BuildOutputAst()` restituisce un AST (albero della sintassi astratta), la struttura necessaria per la restituzione dei dati da un nodo NodeModel.
* `AstFactory.BuildFunctionCall()` chiama la funzione RectangularGrid da `GridFunctions.cs`.
* `new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid)` specifica la funzione e i relativi parametri.
* `new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });` mappa gli input del nodo ai parametri di funzione.
* `AstFactory.BuildNullNode()` crea un nodo nullo se le porte di input non sono connesse. Ciò consente di evitare la visualizzazione di un avviso sul nodo.
* `RaisePropertyChanged("SliderValue")` avvisa l'interfaccia utente quando il valore del dispositivo di scorrimento cambia.
* `var sliderValue = AstFactory.BuildDoubleNode(SliderValue)` crea un nodo nell'albero AST che rappresenta il valore del dispositivo di scorrimento.
* Modificare un input nella variabile `sliderValue` nella variabile functionCall `new List<AssociativeNode> { inputAstNodes[0], sliderValue });`.

**3\. Chiamare una funzione**

Il progetto `CustomNodeModelFunction` verrà integrato in un assieme separato da `CustomNodeModel` in modo che possa essere chiamato.

Copiare il seguente codice in `GridFunction.cs`.

```
using Autodesk.DesignScript.Geometry;
using Autodesk.DesignScript.Runtime;
using System;
using System.Collections.Generic;

namespace CustomNodeModel.CustomNodeModelFunction
{
    [IsVisibleInDynamoLibrary(false)]
    public class GridFunction
    {
        [IsVisibleInDynamoLibrary(false)]
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10, double rand = 1)
        {
            double x = 0;
            double y = 0;

            Point pt = null;
            Vector vec = null;
            Plane bP = null;

            Random rnd = new Random(2);

            var pList = new List<Rectangle>();
            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    double rNum = rnd.NextDouble();
                    double scale = rNum * (1 - rand) + rand;
                    x++;
                    pt = Point.ByCoordinates(x, y);
                    vec = Vector.ZAxis();
                    bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, scale, scale);
                    pList.Add(rect);
                }
            }
            pt.Dispose();
            vec.Dispose();
            bP.Dispose();
            return pList;
        }
    }
}
```

Questa classe di funzioni è molto simile al case study del nodo griglia zero-touch con una differenza:

* `[IsVisibleInDynamoLibrary(false)]` impedisce a Dynamo di "vedere" il seguente metodo e la seguente classe, poiché la funzione è già stata chiamata da `CustomNodeModel`.

Così come sono stati aggiunti i riferimenti per i pacchetti NuGet, `CustomNodeModel` dovrà fare riferimento a `CustomNodeModelFunction` per chiamare la funzione.

![Aggiunta di un riferimento](images/vs-add-project-reference.jpg)

> L'istruzione using per CustomNodeModel sarà inattiva fino a quando non si fa riferimento alla funzione.
>
> 1. Fare clic con il pulsante destro del mouse su `CustomNodeModel` e selezionare `Add > Reference`.
> 2. Scegliere `Projects > Solution`.
> 3. Selezionare `CustomNodeModelFunction`.
> 4. Fare clic su `Ok`.

**4\. Personalizzare la vista**

Per creare un dispositivo di scorrimento, è necessario personalizzare l'interfaccia utente implementando l'interfaccia `INodeViewCustomization`.

Copiare il seguente codice in `GridNodeView.cs`.

```
using Dynamo.Controls;
using Dynamo.Wpf;

namespace CustomNodeModel.CustomNodeModel
{
    public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>
    {
        public void CustomizeView(GridNodeModel model, NodeView nodeView)
        {
            var slider = new Slider();
            nodeView.inputGrid.Children.Add(slider);
            slider.DataContext = model;
        }

        public void Dispose()
        {
        }
    }
}
```

* `public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>` definisce le funzioni necessarie per personalizzare l'interfaccia utente.

Dopo aver impostato la struttura del progetto, utilizzare l'ambiente di progettazione di Visual Studio per creare un controllo utente e definirne i parametri in un file `.xaml`. Dalla casella degli strumenti, aggiungere un dispositivo di scorrimento a `<Grid>...</Grid>`.

![Aggiunta di un nuovo dispositivo di scorrimento](images/vs-usercontrol.jpg)

> 1. Fare clic con il pulsante destro del mouse su `CustomNodeModel` e selezionare `Add > New Item`.
> 2. Selezionare `WPF`.
> 3. Assegnare un nome al controllo utente `Slider`.
> 4. Fare clic su `Add`.

Copiare il seguente codice in `Slider.xaml`.

```
<UserControl x:Class="CustomNodeModel.CustomNodeModel.Slider"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:CustomNodeModel.CustomNodeModel"
             mc:Ignorable="d" 
             d:DesignHeight="75" d:DesignWidth="100">
    <Grid Margin="10">
        <Slider Grid.Row="0" Width="80" Minimum="0" Maximum="1" IsSnapToTickEnabled="True" TickFrequency="0.01" Value="{Binding SliderValue}"/>
    </Grid>
</UserControl>
```

* I parametri del controllo del dispositivo di scorrimento sono definiti nel file `.xaml`. Gli attributi _Minimum e Maximum_ definiscono l'intervallo numerico di questo dispositivo di scorrimento.
* All'interno di `<Grid>...</Grid>` è possibile posizionare diversi controlli utente dalla casella degli strumenti di Visual Studio.

Quando è stato creato il file `Slider.xaml`, Visual Studio ha creato automaticamente un file C# denominato `Slider.xaml.cs` che inizializza il dispositivo di scorrimento. Modificare lo spazio dei nomi in questo file.

```
using System.Windows.Controls;

namespace CustomNodeModel.CustomNodeModel
{
    /// <summary>
    /// Interaction logic for Slider.xaml
    /// </summary>
    public partial class Slider : UserControl
    {
        public Slider()
        {
            InitializeComponent();
        }
    }
}
```

* Lo spazio dei nomi deve essere `CustomNodeModel.CustomNodeModel`.

`GridNodeModel.cs` definisce la logica di calcolo del dispositivo di scorrimento.

**5\. Configurazione come pacchetto**

Prima di creare il progetto, il passaggio finale consiste nell'aggiungere un file `pkg.json` in modo che Dynamo possa leggere il pacchetto.

![Aggiunta di un file JSON](images/vs-pkg-json.jpg)

> 1. Fare clic con il pulsante destro del mouse su `CustomNodeModel` e selezionare `Add > New Item`.
> 2. Selezionare `Web`.
> 3. Selezionare `JSON File`.
> 4. Assegnare un nome al file `pkg.json`.
> 5. Fare clic su `Add`.

* Copiare il seguente codice in `pkg.json`.

```
{
  "license": "MIT",
  "file_hash": null,
  "name": "CustomNodeModel",
  "version": "1.0.0",
  "description": "Sample node",
  "group": "CustomNodes",
  "keywords": [ "grid", "random" ],
  "dependencies": [],
  "contents": "Sample node",
  "engine_version": "1.3.0",
  "engine": "dynamo",
  "engine_metadata": "",
  "site_url": "",
  "repository_url": "",
  "contains_binaries": true,
  "node_libraries": [
    "CustomNodeModel, Version=1.0.0, Culture=neutral, PublicKeyToken=null",
    "CustomNodeModelFunction, Version=1.0.0, Culture=neutral, PublicKeyToken=null"
  ]
}
```

* `"name":` determina il nome del pacchetto e il relativo gruppo nella libreria di Dynamo.
* `"keywords":` fornisce i termini di ricerca per la ricerca nella libreria di Dynamo.
*   `"node_libraries": []` indica le librerie associate al pacchetto.

    L'ultimo passaggio consiste nel creare la soluzione e pubblicarla come pacchetto di Dynamo. Per informazioni su come creare un pacchetto locale prima di pubblicarlo in linea e su come creare un pacchetto direttamente da Visual Studio, vedere il capitolo sull'installazione client dei pacchetti.
