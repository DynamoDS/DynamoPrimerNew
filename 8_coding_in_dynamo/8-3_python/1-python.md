# Узлы Python

Зачем использовать текстовое программирование в среде визуального программирования Dynamo? [Визуальное программирование](../../a\_appendix/visual-programming-and-dynamo.md) имеет много преимуществ. Оно позволяет создавать программы в интуитивно-понятном визуальном интерфейсе, не обладая навыками работы со специальным синтаксисом. Однако визуальная программа может оказаться перегруженной, а порой и недостаточно функциональной. Для сравнения, в Python реализованы гораздо более доступные способы записи условных выражений (если/то) и создания циклов. Python — это мощный инструмент, который позволяет расширить возможности Dynamo и заменить большое количество узлов компактными строками кода.

**Визуальная программа**

![](<../images/8-3/1/python node - visual vs textual programming.jpg>)

**Текстовая программа**

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

solid = IN[0]
seed = IN[1]
xCount = IN[2]
yCount = IN[3]

solids = []

yDist = solid.BoundingBox.MaxPoint.Y-solid.BoundingBox.MinPoint.Y
xDist = solid.BoundingBox.MaxPoint.X-solid.BoundingBox.MinPoint.X

for i in xRange:
	for j in yRange:
		fromCoord = solid.ContextCoordinateSystem
		toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin,Vector.ByCoordinates(0,0,1),(90*(i+j%val)))
		vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
		toCoord = toCoord.Translate(vec)
		solids.append(solid.Transform(fromCoord,toCoord))

OUT = solids
```

### Узел Python

Подобно блокам кода узлы Python представляют собой интерфейс сценариев в среде визуального программирования. Узел Python находится в библиотеке в разделе Script > Editor > Python Script.

![](<../images/8-3/1/python node - the python node 01.jpg>)

Двойной щелчок на узле приводит к открытию редактора сценариев Python (можно также щелкнуть узел правой кнопкой мыши и выбрать команду _Редактировать..._). Сверху находится подсказка, которая поможет обратиться к нужным библиотекам. Входные данные хранятся в массиве IN. Значения возвращаются в Dynamo путем назначения переменной OUT.

![](<../images/8-3/1/python node - the python node 02.jpg>)

Библиотека Autodesk.DesignScript.Geometry позволяет использовать точечные обозначения, аналогичные блокам кода. Дополнительные сведения о синтаксисе Dynamo см. в файле [7-2\_design-script-syntax.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "mention"), а также в [Руководстве по DesignScript](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf) (чтобы скачать этот файл PDF, щелкните правой кнопкой мыши ссылку и выберите «Сохранить как»). При вводе определенного типа геометрии (например, Point.) отображается список методов, доступных для создания и запроса точек.

![](<../images/8-3/1/python node - the python node 03.jpg>)

> Методы включают в себя конструкторы (например, _ByCoordinates_), действия (например, _Add_) и запросы (например, координаты _X_, _Y_ и _Z_).

## Упражнение «Пользовательский узел со сценарием Python для создания массивов из твердотельного модуля»

### Часть I. Настройка сценария Python

> Скачайте файл примера, щелкнув указанную ниже ссылку.
>
> Полный список файлов примеров можно найти в приложении.

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

В этом примере мы напишем сценарий Python для создания образцов на основе твердотельного модуля и преобразуем этот сценарий в пользовательский узел. Сначала создадим твердотельный модуль с помощью узлов Dynamo.

![](<../images/8-3/1/python node - exercise pt I-01.jpg>)

> 1. **Rectangle.ByWidthLength.** Создайте прямоугольник, который будет служить основой твердого тела.
> 2. **Surface.ByPatch.** Соедините прямоугольник с входным параметром _closedCurve_ для создания нижней поверхности.

![](<../images/8-3/1/python node - exercise pt I-02.jpg>)

> 1. **Geometry.Translate.** Соедините прямоугольник с входным параметром _geometry_ для его перемещения вверх, используя блок кода для указания толщины основания тела.
> 2. **Polygon.Points.** Запросите извлечение угловых точек из преобразованного прямоугольника.
> 3. **Geometry.Translate.** Используйте блок кода для создания списка из четырех значений, соответствующих четырем точкам, перемещающим один угол тела вверх.
> 4. **Polygon.ByPoints.** С помощью преобразованных точек воссоздайте верхний полигон.
> 5. **Surface.ByPatch.** Присоедините полигон для создания верхней поверхности.

Теперь, имея в распоряжении верхнюю и нижнюю поверхности, выполним лофтинг между двумя профилями, чтобы создать стороны тела.

![](<../images/8-3/1/python node - exercise pt I-03.jpg>)

> 1. **List.Create.** Соедините нижний прямоугольник и верхний полигон с входными параметрами индекса.
> 2. **Surface.ByLoft.** Выполните лофтинг двух профилей для создания сторон тела.
> 3. **List.Create.** Соедините верхнюю, боковую и нижнюю поверхности с входными параметрами индекса для создания списка поверхностей.
> 4. **Solid.ByJoinedSurfaces.** Соедините поверхности для создания твердотельного модуля.

Теперь, получив твердое тело, перетащите в рабочее пространство узел сценария Python.

![](<../images/8-3/1/python node - exercise pt I-04.jpg>)

> 1. Чтобы добавить дополнительные входные параметры к узлу, закройте редактор и щелкните значок «+» на узле. Входным параметрам присваиваются имена IN\[0], IN\[1] и т. д. Это говорит о том, что они представляют элементы в списке.

Начнем с определения входных и выходных параметров. Дважды щелкните узел, чтобы открыть редактор Python. Используйте приведенный ниже код, чтобы изменить код в редакторе.

![](<../images/8-3/1/python node - exercise pt I-05.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []

# Place your code below this line


# Assign your output to the OUT variable.
OUT = solids
```

Смысл этого кода будет понятен позже по мере выполнения упражнения. Далее необходимо подумать о том, какая информация необходима для создания массива на основе имеющегося твердотельного модуля. Во-первых, необходимо знать размеры тела, чтобы определить расстояние переноса. Из-за ошибки, связанной с ограничивающей рамкой, для ее создания необходимо использовать геометрию кривой кромки.

![](../images/8-3/1/python07.png)

> Посмотрите пример узла Python в Dynamo. Обратите внимание, что используется тот же синтаксис, что и в заголовках узлов Dynamo. Ознакомьтесь с кодом, приведенным ниже.

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

# Assign your output to the OUT variable.
OUT = solids
```

Поскольку твердотельные модули будут не только преобразовываться, но и поворачиваться, воспользуемся операцией Geometry.Transform. Если посмотреть на узел Geometry.Transform, становится понятно, что для преобразования тела потребуется исходная система координат и целевая система координат. В качестве первой выступает контекстная система координат тела, а в качестве второй — система координат, которая различна для каждого модуля массива. Таким образом, чтобы система координат каждый раз преобразовывалась по-разному, необходимо перебирать значения координат по осям X и Y.

![](<../images/8-3/1/python node - exercise pt I-06.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

#Get the source coordinate system
fromCoord = solid.ContextCoordinateSystem

#Loop through x and y
for i in range(xCount):
    for j in range(yCount):
        #Rotate and translate the coordinate system
        toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin, Vector.ByCoordinates(0,0,1), (90*(i+j%seed)))
        vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
        toCoord = toCoord.Translate(vec)
        #Transform the solid from the source coord syste, to the target coord system and append to the list
        solids.append(solid.Transform(fromCoord,toCoord))

# Assign your output to the OUT variable.
OUT = solids
```

Нажмите кнопку «Выполнить», а затем сохраните код. Соедините узел Python с существующим сценарием следующим образом.

![](<../images/8-3/1/python node - exercise pt I-07.jpg>)

> 1. Соедините выходные данные узла Python с узлом **Solid.ByJoinedSurfaces** в качестве первого порта ввода и используйте узел Code Block для определения других входных данных.
> 2. Создайте узел **Topology.Edges** и используйте в качестве входных данных выходные данные узла Python.
> 3. Наконец, создайте узел **Edge.CurveGeometry** и в качестве входных данных используйте выходные данные Topology.Edges.

Попробуйте изменить начальное значение для создания различных образцов. Кроме того, можно изменять параметры самого твердотельного модуля для получения различных эффектов.

![](../images/8-3/1/python10.png)

### Часть II. Преобразование узла сценария Python в пользовательский узел

Создав нужный сценарий Python, сохраним его как пользовательский узел. Выберите узел сценария Python, щелкните правой кнопкой мыши рабочее пространство и выберите «Создание пользовательского узла».

![](<../images/8-3/1/python node - exercise pt II-01.jpg>)

Присвойте имя, добавьте описание и категорию.

![](<../images/8-3/1/python node - exercise pt II-02.jpg>)

При этом откроется новое рабочее пространство для редактирования пользовательского узла.

![](<../images/8-3/1/python node - exercise pt II-03.jpg>)

> 1. **Входные параметры.** Измените имена входных параметров, сделав их более описательными, и добавьте типы данных и значения по умолчанию.
> 2. **Output**: измените имя узла Output.

Сохраните узел в файле DYF. В пользовательском узле отобразятся внесенные изменения.

![](<../images/8-3/1/python node - exercise pt II-04.jpg>)
