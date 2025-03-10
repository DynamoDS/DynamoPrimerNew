# Opracowywanie rozwiązań dla dodatku Dynamo dla programu Revit

## Korzystanie z metod w pliku `GeometryPrimitiveConverter.cs`

Klasa [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) w bibliotece kodu dodatku DynamoRevit udostępnia różne metody konwersji typów geometrii między programem Revit i dodatkiem Dynamo. Te metody są przydatne podczas pracy z geometrią w skryptach dodatku Dynamo, które wchodzą w interakcje z modelami programu Revit.

### Kategorie metod

Metody w pliku `GeometryPrimitiveConverter.cs` można podzielić na cztery główne kategorie:

1. **Z typów Proto na typy programu Revit**: metody, które konwertują typy dodatku Dynamo (Proto) na typy programu Revit.
2. **Z typów programu Revit na typy Proto**: metody, które konwertują typy programu Revit na typy dodatku Dynamo (Proto).
3. **Stopnie i radiany**: metody konwersji między stopniami i radianami.
4. **X i UZ**: metody do pobierania wektorów prostopadłych. 

### Z typów Proto na typy programu Revit

#### ToRevitBoundingBox

Tworzy obiekt BoundingBoxXYZ programu Revit na podstawie układu współrzędnych dodatku Dynamo i dwóch punktów definiujących (minimalnego i maksymalnego).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Konwertuje obiekt BoundingBox dodatku Dynamo na obiekt BoundingBoxXYZ programu Revit.

Flaga convertUnits, z wartością domyślną true (prawda), określa, czy współrzędne powinny zostać przekonwertowane z układu jednostek dodatku Dynamo na jednostki wewnętrzne programu Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Konwertuje punkt dodatku Dynamo na zestaw współrzędnych XYZ programu Revit.

Flaga convertUnits, z wartością domyślną true (prawda), konwertuje współrzędne, jeśli jest to konieczne.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

Konwertuje wektor dodatku Dynamo na wektor XYZ programu Revit.

Zwróć uwagę, że flaga convertUnits ma domyślnie ustawioną wartość false (fałsz), ponieważ wektory reprezentują kierunek i moduł, które zazwyczaj nie wymagają konwersji jednostek. Konwersja może wpłynąć na kierunek i długość wektora. 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Konwertuje punkt dodatku Dynamo na zestaw współrzędnych XYZ programu Revit.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

Konwertuje wektor dodatku Dynamo na wektor XYZ programu Revit.

Zwróć uwagę, że flaga convertUnits ma domyślnie ustawioną wartość false (fałsz), ponieważ wektory reprezentują kierunek i moduł, które zazwyczaj nie wymagają konwersji jednostek. Konwersja może wpłynąć na kierunek i długość wektora. 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Konwertuje układ CoordinateSystem dodatku Dynamo na przekształcenie programu Revit.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Konwertuje płaszczyznę dodatku Dynamo na płaszczyznę programu Revit.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Konwertuje kolekcje obiektów punktów Dynamo na kolekcje zestawów współrzędnych XYZ programu Revit.

Zwraca listę zestawów współrzędnych XYZ. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Zwraca szyk zestawów współrzędnych XYZ. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

Konwertuje szyk obiektów wektorów dodatku Dynamo na szyk wektorów XYZ programu Revit.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Konwertuje szyk wartości double na szyk DoubleArray programu Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Konwertuje szyk dwuwymiarowy (double[][]), w którym każdy szyk wewnętrzny reprezentuje parę wartości (U i V), na szyk obiektów UV programu Revit.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Konwertuje szyk dwuwymiarowy (double[][]), w którym każdy szyk wewnętrzny reprezentuje parę wartości (U i V), na szyk obiektów UV dodatku Dynamo.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Przykład konwersji typów Proto na typy programu Revit

W tym przykładzie pokazano szybkie i łatwe użycie metody .ToXyz (Point) do przekonwertowania punktu Point.ByCoordinates dodatku Dynamo na zestaw współrzędnych XYZ programu Revit. 

![Konwertowanie punktu Point.ByCoordinates dodatku Dynamo na zestaw współrzędnych XYZ programu Revit](Images/dynamo-point-to-revit-xyz.png)

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



### Z typów programu Revit na typy Proto

#### ToProtoType (BoundingBox)

Konwertuje obiekt BoundingBoxXYZ programu Revit na obiekt BoundingBox dodatku Dynamo.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Konwertuje zestaw współrzędnych XYZ programu Revit na punkt dodatku Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Konwertuje punkt programu Revit na punkt dodatku Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Konwertuje zestaw współrzędnych XYZ programu Revit na wektor dodatku Dynamo.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Konwertuje zestaw współrzędnych UV programu Revit na zestaw współrzędnych UV dodatku Dynamo.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Konwertuje płaszczyznę programu Revit na płaszczyznę dodatku Dynamo.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Konwertuje przekształcenie programu Revit na układ CoordinateSystem dodatku Dynamo.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Konwertuje listę punktów XYZ programu Revit na listę punktów dodatku Dynamo.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Przykład konwersji typów programu Revit na typy Proto

W tym przykładzie pokazano szybkie i łatwe użycie metody .ToPoint (XYZ) do przekonwertowania zestawu współrzędnych XYZ programu Revit na punkt dodatku Dynamo. 

![Konwertowanie zestawu współrzędnych XYZ programu Revit na punkt Point.ByCoordinates dodatku Dynamo](Images/revit-xyz-to-dynamo-point.png)

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

### Stopnie i radiany

#### ToRadians

Konwertuje stopnie na radiany.

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

Konwertuje radiany na stopnie.

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### Przykładowe użycie stopni i radianów

W tym przykładzie pokazano szybkie i łatwe użycie metody .ToRadians do przekonwertowania stopni na radiany. 

![Ze stopni na radiany](Images/degrees-to-radians.png)

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
### X i UZ

#### GetPerpendicular (XYZ)

Ta metoda zwraca wektor `XYZ` prostopadły do danego wektora `XYZ`.

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

Ta metoda zwraca wektor `Vector` dodatku Dynamo prostopadły do danego wektora `Vector` dodatku Dynamo.

` public static Vector GetPerpendicular(this Vector vector)`

#### Przykładowe użycie metod X i UZ

W tym przykładzie pokazano szybkie i łatwe użycie metody .GetPerpendicular do wygenerowania wektora prostopadłego do wektora wejściowego. 

![Pobieranie wektora prostopadłego](Images/get-perpendicular-vector.png)

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
