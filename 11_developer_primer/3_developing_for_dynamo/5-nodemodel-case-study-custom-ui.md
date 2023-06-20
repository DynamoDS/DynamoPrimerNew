# Analiza przypadku NodeModel — niestandardowy interfejs użytkownika 

Węzły oparte na klasie NodeModel zapewniają znacznie większą elastyczność i możliwości niż węzły Zero-Touch. W tym przykładzie przeniesiemy węzeł siatki Zero-Touch na następny poziom, dodając zintegrowany suwak losowo ustawiający rozmiar prostokąta.

![Wykres siatki prostokątnej](images/cover-image-2.jpg)

> Ten suwak umożliwia skalowanie komórek względem ich rozmiaru, dzięki czemu użytkownik nie musi udostępniać suwaka z odpowiednim zakresem.

#### Wzorzec Model-View-Viewmodel <a href="#the-model-view-viewmodel-pattern" id="the-model-view-viewmodel-pattern"></a>

Dodatek Dynamo jest oparty na wzorcu architektury oprogramowania [model-view-viewmodel](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) (MVVM), który umożliwia oddzielenie interfejsu użytkownika od zaplecza. W przypadku tworzenia węzłów ZeroTouch dodatek Dynamo tworzy powiązanie danych między danymi węzła a jego interfejsem użytkownika. Aby utworzyć niestandardowy interfejs użytkownika, należy dodać logikę powiązania danych.

Ogólnie istnieją dwie części ustanawiania relacji model-widok (model-view) w dodatku Dynamo:

* Klasa `NodeModel` ustanawiająca podstawową logikę węzła („model”)
* Klasa `INodeViewCustomization` umożliwiająca dostosowanie sposobu wyświetlania modelu `NodeModel` („widok”, ang. „view”)

> Obiekty NodeModel mają już skojarzoną relację widok-model (NodeViewModel), więc można skupić się na modelu i widoku dla niestandardowego interfejsu użytkownika.

#### Jak zaimplementować klasę NodeModel <a href="#how-to-implement-nodemodel" id="how-to-implement-nodemodel"></a>

W tym przykładzie omówimy kilka istotnych różnic między węzłami NodeModel a węzłami Zero-Touch. Zanim przejdziemy do dostosowywania interfejsu użytkownika, rozwińmy logikę klasy NodeModel.

**1\. Tworzenie struktury projektu:**

Węzeł NodeModel może tylko wywoływać funkcje, dlatego należy rozdzielić węzeł NodeModel i funkcje między różnymi bibliotekami. Standardowym sposobem realizacji tego w przypadku pakietów dodatku Dynamo jest utworzenie oddzielnych projektów dla obu tych typów zawartości. Zacznij od utworzenia nowego rozwiązania obejmującego te projekty.

> 1. Wybierz opcję `File > New > Project`
> 2. Wybierz opcję `Other Project Types`, aby wywołać opcję rozwiązania
> 3. Wybierz opcję `Blank Solution`
> 4. Nadaj rozwiązaniu nazwę `CustomNodeModel`
> 5. Wybierz przycisk `Ok`

Utwórz w rozwiązaniu dwa projekty biblioteki klas C#: jeden dla funkcji, a drugi dla interfejsu NodeModel.

![Dodawanie nowej biblioteki klas](images/vs-new-class-projects.jpg)

> 1. Kliknij prawym przyciskiem myszy rozwiązanie i wybierz pozycję `Add > New Project`
> 2. Wybierz bibliotekę klas
> 3. Nadaj jej nazwę `CustomNodeModel`
> 4. Kliknij przycisk `Ok`
> 5. Powtórz ten proces, aby dodać kolejny projekt o nazwie `CustomNodeModelFunctions`

Następnie należy zmienić nazwy automatycznie utworzonych bibliotek klas i dodać je do projektu `CustomNodeModel`. Klasa `GridNodeModel` służy do zaimplementowania klasy abstrakcyjnej NodeModel, a `GridNodeView` do dostosowania widoku. Natomiast klasa `GridFunction` ma zawierać wszystkie funkcje, które będą wywoływane.

![Eksplorator rozwiązań](images/vs-new-class.jpg)

> 1. Dodaj kolejną klasę, klikając prawym przyciskiem myszy projekt `CustomNodeModel`, wybierając polecenie `Add > New Item...` i wybierając opcję `Class`.
> 2. W projekcie `CustomNodeModel` potrzebne są klasy `GridNodeModel.cs` i `GridNodeView.cs`
> 3. W projekcie `CustomNodeModelFunction` potrzebna jest klasa `GridFunctions.cs`

Przed dodaniem kodu do klas dodaj wymagane dla tego projektu pakiety. Projekt `CustomNodeModel` będzie wymagać bibliotek ZeroTouchLibrary i WpfUILibrary. Natomiast projekt `CustomNodeModelFunction` będzie wymagać tylko biblioteki ZeroTouchLibrary. Biblioteka WpfUILibrary zostanie użyta podczas dostosowywania interfejsu użytkownika, które wykonamy później, a biblioteka ZeroTouchLibrary — do tworzenia geometrii. Pakiety można dodawać dla projektów pojedynczo. Ponieważ te pakiety mają zależności, składniki Core i DynamoServices zostaną zainstalowane automatycznie.

![Instalowanie pakietów](images/vs-add-packages.jpg)

> 1. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję `Manage NuGet Packages`
> 2. Zainstaluj tylko wymagane pakiety dla tego projektu

Program Visual Studio skopiuje pakiety NuGet, do których dodaliśmy odwołania w katalogu kompilacji. Dla tej pozycji można ustawić wartość false (fałsz), aby w pakiecie nie było żadnych niepotrzebnych plików.

![Wyłączanie kopii lokalnej pakietu](images/vs-disable-package-copying.jpg)

> 1. Wybierz pakiety NuGet dodatku Dynamo
> 2. Ustaw wartość false (fałsz) dla pozycji `Copy Local`

**2\. Dziedziczenie klasy NodeModel**

Jak wspomniano wcześniej, główna różnica między węzłem NodeModel a węzłem ZeroTouch polega na tym, że ten pierwszy zawiera implementację klasy `NodeModel`. Węzeł NodeModel wymaga kilku funkcji z tej klasy, które można uzyskać, dodając po nazwie klasy `:NodeModel`.

Skopiuj następujący kod do pliku `GridNodeModel.cs`.

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

To różnica w stosunku do węzłów Zero-Touch. Przeanalizujmy funkcje poszczególnych części.

* Określ atrybuty węzła, takie jak nazwa, kategoria, nazwy portów wejściowych/wyjściowych, typy portów wejściowych/wyjściowych oraz opisy.
* `public class GridNodeModel : NodeModel` to klasa dziedzicząca klasę `NodeModel` z `Dynamo.Graph.Nodes`.
* `public GridNodeModel() { RegisterAllPorts(); }` to konstruktor rejestrujący dane wejściowe i wyjściowe węzła.
* `BuildOutputAst()` zwraca drzewo AST (Abstract Syntax Tree), strukturę wymaganą do zwracania danych z węzła NodeModel.
* metoda `AstFactory.BuildFunctionCall()` wywołuje funkcję RectangularGrid z pliku `GridFunctions.cs`.
* Instrukcja `new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid)` określa tę funkcję i jej parametry.
* Instrukcja `new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });` odwzorowuje dane wejściowe węzła na parametry funkcji
* Metoda `AstFactory.BuildNullNode()` tworzy węzeł o wartości null, jeśli porty wejściowe nie są połączone. Zapobiega to wyświetleniu ostrzeżenia dotyczącego tego węzła.
* Instrukcja `RaisePropertyChanged("SliderValue")` powiadamia interfejs użytkownika o zmianie wartości suwaka
* Instrukcja `var sliderValue = AstFactory.BuildDoubleNode(SliderValue)` tworzy węzeł AST reprezentujący wartość suwaka
* Zmień dane wejściowe na zmienną `sliderValue` w zmiennej functionCall `new List<AssociativeNode> { inputAstNodes[0], sliderValue });`

**3\. Wywoływanie funkcji**

Projekt `CustomNodeModelFunction` zostanie skompilowany jako zespół oddzielny od projektu `CustomNodeModel`, dzięki czemu będzie można go wywołać.

Skopiuj następujący kod do pliku `GridFunction.cs`.

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

Ta klasa funkcji jest bardzo podobna do tej z analizy przypadku siatki Zero-Touch z jedną różnicą:

* Instrukcja `[IsVisibleInDynamoLibrary(false)]` „ukrywa” przed dodatkiem Dynamo następującą metodę i klasę, ponieważ ta funkcja jest już wywoływana z projektu `CustomNodeModel`.

Tak jak dodaliśmy odwołania do pakietów NuGet, projekt `CustomNodeModel` musi odwoływać się do projektu `CustomNodeModelFunction`, aby wywołać funkcję.

![Dodawanie odwołania](images/vs-add-project-reference.jpg)

> Instrukcja using dla projektu CustomNodeModel będzie nieaktywna, dopóki nie będzie odwołania do tej funkcji
>
> 1. Kliknij prawym przyciskiem myszy pozycję `CustomNodeModel` i wybierz polecenie `Add > Reference`
> 2. Wybierz opcję `Projects > Solution`
> 3. Zaznacz pozycję `CustomNodeModelFunction`
> 4. Kliknij przycisk `Ok`

**4\. Dostosowywanie widoku**

Aby utworzyć suwak, należy dostosować interfejs użytkownika przez zaimplementowanie interfejsu `INodeViewCustomization`.

Skopiuj następujący kod do pliku `GridNodeView.cs`

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

* Instrukcja `public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>` definiuje funkcje niezbędne do dostosowania interfejsu użytkownika.

Po skonfigurowaniu struktury projektu należy za pomocą środowiska projektowego programu Visual Studio utworzyć element sterujący użytkownika i zdefiniować jego parametry w pliku `.xaml`. Z pola przybornika dodaj suwak do części `<Grid>...</Grid>`.

![Dodawanie nowego suwaka](images/vs-usercontrol.jpg)

> 1. Kliknij prawym przyciskiem myszy pozycję `CustomNodeModel` i wybierz polecenie `Add > New Item`
> 2. Wybierz opcję `WPF`
> 3. Nadaj elementowi sterującemu użytkownika nazwę `Slider`
> 4. Kliknij opcję `Add`

Skopiuj następujący kod do pliku `Slider.xaml`

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

* Parametry suwaka są zdefiniowane w pliku `.xaml`. Atrybuty _Minimum i Maximum_ definiują zakres liczbowy tego suwaka.
* Wewnątrz tagów `<Grid>...</Grid>` można umieszczać różne elementy sterujące użytkownika z przybornika programu Visual Studio

Po utworzeniu pliku `Slider.xaml` program Visual Studio automatycznie utworzył plik z kodem C# o nazwie `Slider.xaml.cs`, który inicjuje suwak. Zmień przestrzeń nazw w tym pliku.

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

* Przestrzenią nazw powinna być `CustomNodeModel.CustomNodeModel`

Plik `GridNodeModel.cs` definiuje logikę obliczeń suwaka.

**5\. Konfigurowanie jako pakiet**

Ostatnią czynnością przed rozpoczęciem kompilowania projektu jest dodanie pliku `pkg.json`, aby umożliwić dodatkowi Dynamo odczytanie pakietu.

![Dodawanie pliku JSON](images/vs-pkg-json.jpg)

> 1. Kliknij prawym przyciskiem myszy pozycję `CustomNodeModel` i wybierz polecenie `Add > New Item`
> 2. Wybierz opcję `Web`
> 3. Wybierz opcję `JSON File`
> 4. Nadaj plikowi nazwę `pkg.json`
> 5. Kliknij opcję `Add`

* Skopiuj następujący kod do pliku `pkg.json`

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

* `"name":` określa nazwę pakietu i jego grupę w bibliotece dodatku Dynamo
* `"keywords":` określa terminy wyszukiwania na potrzeby wyszukiwania w bibliotece dodatku Dynamo
*   `"node_libraries": []` biblioteki skojarzone z pakietem

    Ostatnią czynnością jest skompilowanie rozwiązania i opublikowanie go jako pakietu dodatku Dynamo. Zapoznaj się z rozdziałem dotyczącym wdrażania pakietów, aby dowiedzieć się, jak utworzyć pakiet lokalny przed opublikowaniem go online i jak skompilować pakiet bezpośrednio z programu Visual Studio.
