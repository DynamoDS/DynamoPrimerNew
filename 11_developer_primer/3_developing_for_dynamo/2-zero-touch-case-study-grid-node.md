# Пример использования узлов Zero-Touch — узел сетки

Мы подготовили проект Visual Studio, а теперь рассмотрим, как создать пользовательский узел, создающий прямоугольную сетку ячеек. Такую сетку можно создать с помощью нескольких стандартных узлов, однако мы также можем легко включить этот полезный инструмент в узел Zero-Touch. В отличие от линий сетки, ячейки можно масштабировать относительно центральных точек, запрашивать их угловые вершины или встраивать в грани.

В этом примере рассматриваются некоторые возможности и концепции, которые необходимо учитывать при создании узла Zero-Touch. После создания пользовательского узла и его добавления в Dynamo откройте страницу «Дальнейшая работа с Zero-Touch», чтобы просмотреть входные значения по умолчанию, возвращенные значения, документацию, объекты, типы геометрии Dynamo и миграцию.

![График прямоугольной сетки](images/cover-image.jpg)

### Пользовательский узел прямоугольной сетки <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

Чтобы начать построение узла сетки, создайте новый проект библиотеки классов Visual Studio. Подробные инструкции по настройке проекта см. на странице «Начало работы».

![Создание нового проекта в Visual Studio](images/vs-new-project-1.jpg)

![Настройка нового проекта в Visual Studio](images/vs-new-project-2.jpg)

> 1. Выберите `Class Library` (Библиотека классов) в качестве типа проекта.
> 2. Присвойте проекту имя `CustomNodes`.

Поскольку мы будем создавать геометрию, необходима ссылка на соответствующий пакет NuGet. Установите пакет ZeroTouchLibrary из диспетчера пакетов NuGet. Этот пакет необходим для оператора `using Autodesk.DesignScript.Geometry;`.

![Пакет ZeroTouchLibrary](images/vs-nugetpackage.jpg)

> 1. Найдите пакет ZeroTouchLibrary.
> 2. Этот узел будет использоваться в текущей сборке Dynamo Studio — 1.3. Выберите версию пакета, соответствующую этой сборке.
> 3. Обратите внимание, что мы переименовали файл класса в `Grids.cs`.

Далее необходимо установить пространство имен и класс, в котором будет использоваться метод RectangularGrid. В Dynamo узел будет назван в соответствии с методом и именами классов. Пока не нужно копировать его в Visual Studio.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
        //The method for creating a rectangular grid will live in here
        }
    }
}
```

> `Autodesk.DesignScript.Geometry;` ссылается на файл ProtoGeometry.dll в пакете ZeroTouchLibrary `System.Collections.Generic`, который необходим для создания списков.

Теперь добавим метод создания прямоугольников. Файл класса можно скопировать в Visual Studio. Он должен выглядеть следующим образом.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                }
            }
            return pList;
        }
    }
}
```

Если ваш проект выглядит аналогично, выполните сборку `.dll`.

![Сборка DLL](images/vs-grids.jpg)

> 1. Выберите Build > Build Solution (Сборка > Собрать решение).

Проверьте папку проекта `bin` на наличие файла `.dll`. Если сборка прошла успешно, можно добавить `.dll` в Dynamo.

![Пользовательские узлы в Dynamo](images/RectangularGrid-Dynamo.jpg)

> 1. Пользовательский узел RectangularGrids в библиотеке Dynamo.
> 2. Пользовательский узел в рабочей области.
> 3. Кнопка Add (Добавить) для добавления файл `.dll` в Dynamo.

### Пользовательские изменения узлов <a href="#custom-node-modifications" id="custom-node-modifications"></a>

В приведенном выше примере был создан относительно несложный узел, который лишь определяет метод `RectangularGrids`. Однако для более сложных узлов могут потребоваться подсказки для портов ввода или описание узла, подобно стандартным узлам Dynamo. Добавление этих возможностей в пользовательские узлы упрощает их использование и, в особенности, их поиск в библиотеке.

![Подсказка для ввода](images/nodemodification.png)

> 1. Входное значение по умолчанию.
> 2. Подсказка для ввода xCount.

Для узла RectangularGrid требуются некоторые из этих базовых функций. В коде ниже добавлены описания входных и выходных портов, описание и входные значения по умолчанию.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }
            return pList;
        }
    }
}
```

* Задайте значения по умолчанию для входных данных, назначив параметрам метода `RectangularGrid(int xCount = 10, int yCount = 10)`.
* Создавайте подсказки для входных и выходных данных, ключевые слова для поиска и описание с XML-документацией, используя `///` в качестве префикса.

Для добавления подсказок нам потребуется файл XML в папке проекта. Visual Studio может автоматически создать файл `.xml`, если этот параметр выбран.

![Прикрепление документации XML](images/vs-xml.jpg)

> 1. Прикрепите файл XML-документации и укажите путь к файлу. При этом создается файл XML.

Вот и все! Мы создали новый узел с несколькими стандартными элементами. В следующей главе, «Основы Zero-Touch», подробно рассматриваются вопросы разработки узлов Zero-Touch и возможные проблемы.
