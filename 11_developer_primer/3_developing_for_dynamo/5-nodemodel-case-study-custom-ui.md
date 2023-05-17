# Пример использования NodeModel — настраиваемый пользовательский интерфейс

Узлы на основе NodeModel обеспечивают гораздо больше гибкости и мощности, чем узлы Zero-Touch. В этом примере мы улучшим узел сетки Zero-Touch, добавив встроенный ползунок, который случайным образом изменяет размер прямоугольника.

![График прямоугольной сетки](images/cover-image-2.jpg)

> Регулятор масштабирует ячейки относительно их размера, чтобы пользователю не приходилось указывать в ползунке нужный диапазон.

#### Шаблон Model-View-Viewmodel <a href="#the-model-view-viewmodel-pattern" id="the-model-view-viewmodel-pattern"></a>

В Dynamo используется архитектурный шаблон [Model-View-Viewmodel](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) (MVVM), благодаря чему пользовательский интерфейс не зависит от серверной части. При создании узлов ZeroTouch в Dynamo выполняется привязка данных между данными узла и его пользовательским интерфейсом. Чтобы создать пользовательский интерфейс, необходимо добавить логику привязки данных.

В общих чертах, для установления взаимосвязи между моделью и видом в Dynamo используется два класса:

* Класс `NodeModel` для определения основной логики узла (модель).
* Класс `INodeViewCustomization` для настройки способа просмотра `NodeModel` (вид).

> У объектов NodeModel уже есть связанная пара модели и вида (NodeViewModel), поэтому мы можем сосредоточиться только на модели и виде для пользовательского интерфейса.

#### Реализация NodeModel <a href="#how-to-implement-nodemodel" id="how-to-implement-nodemodel"></a>

Узлы NodeModel имеют несколько существенных отличий от узлов Zero-Touch. Мы рассмотрим их на следующем примере. Прежде чем мы перейдем к настройке пользовательского интерфейса, создадим логику NodeModel.

**1\. Создадим структуру проекта. **

Узел NodeModel может вызывать только функции, поэтому необходимо поместить NodeModel и функции в разные библиотеки. Стандартный способ сделать это в пакетах Dynamo — создать отдельные проекты для каждой библиотеки. Начнем с создания нового решения, куда будут входить эти проекты.

> 1. Выберите `File > New > Project` (Файл > Создать > Проект).
> 2. Выберите `Other Project Types` (Другие типы проектов), чтобы открыть параметр решения.
> 3. Выберите `Blank Solution` (Пустое решение).
> 4. Присвойте решению имя `CustomNodeModel`.
> 5. Нажмите `Ok`.

Создайте два проекта библиотеки классов C# в решении: один для функций, а другой для реализации интерфейса NodeModel.

![Добавление новой библиотеки классов](images/vs-new-class-projects.jpg)

> 1. Щелкните правой кнопкой мыши на решении и выберите `Add > New Project` (Добавить > Новый проект).
> 2. Выберите библиотеку классов.
> 3. Присвойте ей имя `CustomNodeModel`.
> 4. Нажмите `Ok`
> 5. Повторите процедуру, чтобы добавить другой проект, с именем `CustomNodeModelFunctions`.

Теперь необходимо переименовать библиотеки классов, которые были созданы автоматически, и добавить их в проект `CustomNodeModel`. Класс `GridNodeModel` реализует абстрактный класс NodeModel, `GridNodeView` используется для настройки вида, а `GridFunction` содержит все функции, которые необходимо вызвать.

![Обозреватель решений](images/vs-new-class.jpg)

> 1. Добавьте другой класс: щелкните правой кнопкой мыши проект `CustomNodeModel`, нажмите `Add > New Item...` (Добавить > Новый элемент) и выберите `Class` (Класс).
> 2. В проекте `CustomNodeModel` нам необходимы классы `GridNodeModel.cs` и `GridNodeView.cs`.
> 3. В проекте `CustomNodeModelFunction` требуется класс `GridFunctions.cs`.

Прежде чем добавить к классам код, добавьте необходимые пакеты для этого проекта. Для `CustomNodeModel` потребуются ZeroTouchLibrary и WpfUILibrary, а для `CustomNodeModelFunction` — только ZeroTouchLibrary. WpfUILibrary будет использоваться в настройке адаптации пользовательского интерфейса, а ZeroTouchLibrary будет использоваться для создания геометрии. Пакеты можно добавлять для проектов по отдельности. Так как эти пакеты имеют зависимости, будут автоматически установлены Core и DynamoServices.

![Установка пакетов](images/vs-add-packages.jpg)

> 1. Щелкните проект правой кнопкой мыши и выберите `Manage NuGet Packages` (Управлять пакетами NuGet).
> 2. Установите только необходимые пакеты для данного проекта.

Visual Studio скопирует пакеты NuGet, на которые мы ссылались в каталоге сборки. Для этого параметра можно установить значение false, чтобы в пакете не было ненужных файлов.

![Отключение локальной копии пакета](images/vs-disable-package-copying.jpg)

> 1. Выберите пакет Dynamo NuGet.
> 2. Задайте для параметра `Copy Local` (Скопировать локально) значение false.

**2\. Наследование от класса NodeModel.**

Как упоминалось ранее, основное отличие узла NodeModel от узла ZeroTouch заключается в реализации класса `NodeModel`. Узлу NodeModel требуется несколько функций из этого класса, и их можно получить, добавив `:NodeModel` после имени класса.

Скопируйте следующий код в `GridNodeModel.cs`.

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

В этом отличие от Zero-Touch. Давайте попробуем разобраться, что делает каждый элемент.

* Задайте атрибуты узла, такие как имя, категория, имена и типы InPort/OutPort (входящего и выходящего портов), описания.
* `public class GridNodeModel : NodeModel` — это класс, наследующий класс `NodeModel` от класса `Dynamo.Graph.Nodes`.
* `public GridNodeModel() { RegisterAllPorts(); }` является конструктором, регистрирующим входные и выходные данные узла.
* `BuildOutputAst()` возвращает AST (абстрактное синтаксическое дерево) — необходимую структуру для возврата данных из узла NodeModel.
* `AstFactory.BuildFunctionCall()` вызывает функцию RectangularGrid из `GridFunctions.cs`.
* `new Func<int, int, double, List<Rectangle>>(GridFunction.RectangularGrid)` задает функцию и ее параметры.
* `new List<AssociativeNode> { inputAstNodes[0], inputAstNodes[1], sliderValue });` сопоставляет входные данные узла с параметрами функции.
* `AstFactory.BuildNullNode()` создает пустой узел, если входные порты не подключены. Это позволяет избежать отображения предупреждения на узле.
* `RaisePropertyChanged("SliderValue")` уведомляет пользовательский интерфейс об изменении значения ползунка.
* `var sliderValue = AstFactory.BuildDoubleNode(SliderValue)` создает узел AST, представляющий значение ползунка.
* Измените входные данные на переменную `sliderValue` в переменной functionCall `new List<AssociativeNode> { inputAstNodes[0], sliderValue });`.

**3\. Вызов функции.**

Проект `CustomNodeModelFunction` будет создан в отдельной сборке из `CustomNodeModel`, чтобы его можно было вызвать.

Скопируйте следующий код в `GridFunction.cs`.

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

Этот класс функций очень похож на пример использования Zero-Touch Grid, но есть одно отличие:

* При указании `[IsVisibleInDynamoLibrary(false)]` Dynamo не видит следующий метод и класс, поскольку функция уже вызывается из `CustomNodeModel`.

Так же как мы добавили ссылки на пакеты NuGet, `CustomNodeModel` должен будет ссылаться на `CustomNodeModelFunction` для вызова функции.

![Добавление ссылки](images/vs-add-project-reference.jpg)

> Оператор using для CustomNodeModel будет неактивен до тех пор, пока мы не сошлемся на функцию.
>
> 1. Щелкните правой кнопкой мыши `CustomNodeModel` и выберите `Add > Reference` (Добавить > Ссылка).
> 2. Выберите `Projects > Solution` (Проекты > Решение).
> 3. Выберите `CustomNodeModelFunction`.
> 4. Нажмите `Ok`

**4\. Настройка вида.**

Чтобы создать ползунок, необходимо настроить пользовательский интерфейс, реализовав интерфейс `INodeViewCustomization`.

Скопируйте следующий код в `GridNodeView.cs`.

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

* `public class CustomNodeModelView : INodeViewCustomization<GridNodeModel>` определяет функции, необходимые для настройки пользовательского интерфейса.

После настройки структуры проекта используйте среду разработки Visual Studio, чтобы собрать пользовательский элемент управления и определить его параметры в файле `.xaml`. В окне инструментов добавьте ползунок в элемент `<Grid>...</Grid>`.

![Добавление нового ползунка](images/vs-usercontrol.jpg)

> 1. Щелкните правой кнопкой мыши `CustomNodeModel` и выберите `Add > New Item` (Добавить > Новый элемент).
> 2. Выберите `WPF`.
> 3. Присвойте пользовательскому элементу управления имя `Slider` (Ползунок).
> 4. Нажмите `Add` (Добавить).

Скопируйте следующий код в `Slider.xaml`.

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

* Параметры ползунка определяются в файле `.xaml`. Атрибуты _минимума и максимума_ определяют числовой диапазон этого ползунка.
* Внутри `<Grid>...</Grid>` можно разместить различные пользовательские элементы управления с панели инструментов Visual Studio.

При создании файла `Slider.xaml` Visual Studio был автоматически создан файл C# с именем `Slider.xaml.cs`, который инициализирует ползунок. Измените пространство имен в этом файле.

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

* Нам требуется пространство имен `CustomNodeModel.CustomNodeModel`.

`GridNodeModel.cs` определяет логику вычисления ползунка.

**5\. Настройка пакета.**

Перед сборкой проекта добавьте файл `pkg.json`, чтобы Dynamo мог прочитать пакет.

![Добавление файла JSON](images/vs-pkg-json.jpg)

> 1. Щелкните правой кнопкой мыши `CustomNodeModel` и выберите `Add > New Item` (Добавить > Новый элемент).
> 2. Выберите `Web` (Веб).
> 3. Выберите `JSON File` (Файл JSON).
> 4. Присвойте файлу имя `pkg.json`.
> 5. Нажмите `Add` (Добавить).

* Скопируйте следующий код в `pkg.json`.

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

* `"name":` определяет имя пакета и его группу в библиотеке Dynamo.
* `"keywords":` предоставляет условия поиска по библиотеке Dynamo.
*   `"node_libraries": []` — библиотеки, связанные с пакетом

    Последний шаг — собрать решение и опубликовать его в виде пакета Dynamo. Сведения о создании локального пакета перед публикацией в Интернете и о сборке пакета непосредственно в Visual Studio см. в разделе «Развертывание пакета».
