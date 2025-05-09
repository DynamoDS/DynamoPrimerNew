# Разработка для Dynamo for Revit

## Использование методов в `GeometryPrimitiveConverter.cs`

Класс [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) в библиотеке кода DynamoRevit предоставляет различные способы преобразования типов геометрии в Revit и Dynamo. Эти методы удобно использовать при работе с геометрией в сценариях Dynamo, которые взаимодействуют с моделями Revit.

### Категории методов

Методы, описанные в `GeometryPrimitiveConverter.cs`, можно разделить на четыре основные категории.

1. **Типы Proto в типы Revit**: методы преобразования типов Dynamo (Proto) в типы Revit.
2. **Типы Revit в типы Proto**: методы преобразования типов Revit в типы Dynamo (Proto).
3. **Градусы и радианы**: методы преобразования градусов в радианы.
4. **X и UZ**: методы получения перпендикулярных векторов.

### Типы Proto в типы Revit

#### ToRevitBoundingBox

Создание объекта BoundingBoxXYZ Revit на основе системы координат Dynamo и двух определяющих точек (максимальной и минимальной).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox( Autodesk.DesignScript.Geometry.CoordinateSystem cs, Autodesk.DesignScript.Geometry.Point minPoint, Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Преобразование объекта BoundingBox Dynamo в объект BoundingBoxXYZ Revit.

Флаг convertUnits (по умолчанию имеет значение true) определяет, следует ли преобразовывать координаты из системы единиц Dynamo во внутренние единицы Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Преобразование объекта Point Dynamo в координаты XYZ Revit.

Флаг convertUnits (по умолчанию имеет значение true) при необходимости преобразует координаты.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

Преобразование объекта Vector Dynamo в координаты XYZ Revit.

Обратите внимание, что флаг convertUnits по умолчанию имеет значение false, поскольку векторы представляют направление и величину, которые обычно не требуют преобразования единиц измерения. Преобразование может повлиять на направление и длину вектора.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Преобразование объекта Point Dynamo в координаты XYZ Revit.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

Преобразование объекта Vector Dynamo в координаты XYZ Revit.

Обратите внимание, что флаг convertUnits по умолчанию имеет значение false, поскольку векторы представляют направление и величину, которые обычно не требуют преобразования единиц измерения. Преобразование может повлиять на направление и длину вектора.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Преобразование объекта CoordinateSystem Dynamo в объект Transform Revit.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Преобразование плоскости Dynamo в плоскость Revit.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Преобразование наборов объектов Point Dynamo в наборы XYZ Revit.

Возвращает список координат XYZ. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Возвращает массив элементов XYZ. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

Преобразование массива объектов Vector Dynamo в массив векторов XYZ Revit.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Преобразование массива двойных значений в массив DoubleArray Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Преобразование двумерного массива (double[][]), где каждый внутренний массив представляет пару значений (U и V), в массив объектов UV Revit.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Преобразование двумерного массива (double[][]), где каждый внутренний массив представляет пару значений (U и V), в массив объектов UV Dynamo.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Пример использования преобразования типов Proto в типы Revit

В этом примере показан простой и быстрый способ использования метода .ToXyz (Point) для преобразования объекта Point.ByCoordinates Dynamo в координаты XYZ Revit.

![Преобразование объекта Point.ByCoordinates Dynamo в координаты XYZ Revit](Images/dynamo-point-to-revit-xyz.png)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# Revit API
from Autodesk.Revit.DB import *

# Revit Geometry Conversion Library
import Revit
clr.ImportExtensions(Revit.GeometryConversion)

# Input: Dynamo Point
dyn_point = IN[0]

# Convert to Revit XYZ
revit_point = dyn_point.ToXyz()

# Output
OUT = revit_point

```

### Типы Revit в типы Proto

#### ToProtoType (BoundingBox)

Преобразование объекта BoundingBoxXYZ Revit в объект BoundingBox Dynamo.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Преобразование координат XYZ Revit в объект Point Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Преобразование объекта Point Revit в объект Point Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Преобразование координат XYZ Revit в объект Vector Dynamo.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Преобразование объекта UV Revit в объект UV Dynamo.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Преобразование плоскости Revit в плоскость Dynamo.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Преобразование объекта Transform Revit в объект CoordinateSystem Dynamo.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Преобразование списка точек XYZ Revit в список точек Dynamo.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Пример использования преобразования типов Revit в типы Proto

В этом примере показан простой и быстрый способ использования метода .ToPoint (XYZ) для преобразования координат XYZ Revit в объект Point Dynamo.

![Преобразование координат XYZ Revit в объект Point.ByCoordinates Dynamo](Images/revit-xyz-to-dynamo-point.png)

```
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# Revit API
from Autodesk.Revit.DB import *

# Revit Geometry Conversion Library
import Revit
clr.ImportExtensions(Revit.GeometryConversion)

# Input: Dynamo Point
dyn_point = IN[0]

# Convert to Revit XYZ
revit_point = dyn_point.ToPoint()

# Output
OUT = revit_point
```

### Градусы и радианы

#### ToRadians

Преобразование градусов в радианы.

`public static double ToRadians(this double degrees) { return degrees * Math.PI / 180.0; }`

#### ToDegrees

Преобразование радианов в градусы.

`public static double ToDegrees(this double degrees) { return degrees * 180.0 / Math.PI; }`

#### Пример использования преобразования градусов и радианов

В этом примере показан простой и быстрый способ использования метода .ToRadians для преобразования градусов в радианы.

![Преобразование градусов в радианы](Images/degrees-to-radians.png)

```
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# Revit API
from Autodesk.Revit.DB import *

# Revit Geometry Conversion Library
import Revit
clr.ImportExtensions(Revit.GeometryConversion)

# Input: Measure Angle
degree_angle = IN[0]

# Convert to Radians
radian_angle = Revit.GeometryConversion.GeometryPrimitiveConverter.ToRadians(degree_angle)

# Output
OUT = radian_angle
```

### X и UZ

#### GetPerpendicular (XYZ)

Этот метод возвращает вектор `XYZ`, перпендикулярный заданному вектору `XYZ`.

`public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

Этот метод возвращает объект `Vector` Dynamo, перпендикулярный заданному объекту `Vector` Dynamo.

`public static Vector GetPerpendicular(this Vector vector)`

#### Пример использования преобразования X и UZ

В этом примере показан простой и быстрый способ использования метода .GetPerpendicular для получения вектора, перпендикулярного заданному вектору.

![Получение перпендикулярного вектора](Images/get-perpendicular-vector.png)

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# Revit API
from Autodesk.Revit.DB import *

# Revit Geometry Conversion Library
import Revit
clr.ImportExtensions(Revit.GeometryConversion)

# Input Dynamo Vector
input_vector = IN[0]

# Get perpendicular vector using GetPerpendicular
perpendicular_vector = Revit.GeometryConversion.GeometryPrimitiveConverter.GetPerpendicular(input_vector)

# Output the perpendicular vector
OUT = perpendicular_vector

```
