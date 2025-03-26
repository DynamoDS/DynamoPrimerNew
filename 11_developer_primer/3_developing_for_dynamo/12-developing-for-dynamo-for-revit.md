# Entwickeln für Dynamo für Revit

## Verwenden von Methoden in `GeometryPrimitiveConverter.cs`

Die Klasse [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) in der Codebibliothek von DynamoRevit bietet verschiedene Methoden zum Konvertieren zwischen geometrischen Typen in Revit und Dynamo. Diese Methoden sind nützlich beim Arbeiten mit Geometrie in Dynamo-Skripten, die mit Revit-Modellen interagieren.

### Methodenkategorien

Die Methoden in `GeometryPrimitiveConverter.cs` können in vier Hauptkategorien eingeteilt werden:

1. **Proto-in-Revit-Typen**: Methoden, mit denen Dynamo-(Proto-)Typen in Revit-Typen konvertiert werden.
2. **Revit-in-Proto-Typen**: Methoden, mit denen Revit-Typen in Dynamo-Typen (Proto-Typen) konvertiert werden.
3. **Grad und Bogenmaß**: Methoden, mit denen zwischen Grad und Bogenmaß gewechselt werden kann.
4. **X und UZ**: Methoden, die sich mit dem Abrufen lotrechter Vektoren befassen. 

### Proto-in-Revit-Typen

#### ToRevitBoundingBox

Erstellt ein Revit-BoundingBoxXYZ-Objekt aus einem Dynamo-Koordinatensystem und zwei definierenden Punkten (Minimum und Maximum).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Konvertiert ein Dynamo-BoundingBox-Objekt in ein Revit-BoundingBoxXYZ-Objekt.

Das Flag convertUnits (Vorgabe ist True) bestimmt, ob die Koordinaten vom Dynamo-Einheitensystem in die internen Revit-Einheiten konvertiert werden sollen.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Konvertiert einen Dynamo-Punkt in einen Revit-XYZ-Wert.

Das Flag convertUnits (Vorgabe ist True) konvertiert die Koordinaten, falls erforderlich.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

Konvertiert einen Dynamo-Vektor in einen Revit-XYZ-Wert.

Beachten Sie, dass das Flag convertUnits vorgabemäßig auf False gesetzt ist, da Vektoren Richtung und Größe darstellen, für die normalerweise keine Einheitenkonvertierung erforderlich ist. Die Konvertierung kann sich auf Richtung und Länge des Vektors auswirken. 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Konvertiert einen Dynamo-Punkt in einen Revit-XYZ-Wert.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

Konvertiert einen Dynamo-Vektor in einen Revit-XYZ-Wert.

Beachten Sie, dass das Flag convertUnits vorgabemäßig auf False gesetzt ist, da Vektoren Richtung und Größe darstellen, für die normalerweise keine Einheitenkonvertierung erforderlich ist. Die Konvertierung kann sich auf Richtung und Länge des Vektors auswirken. 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Konvertiert ein Dynamo-CoordinateSystem-Objekt in eine Revit-Transformation.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Konvertiert eine Dynamo-Ebene in eine Revit-Ebene.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Konvertiert Sammlungen von Dynamo-Punktobjekten in Revit-XYZ-Sammlungen.

Gibt eine Liste mit XYZ-Werten zurück. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Gibt ein Array von XYZ-Werten zurück. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

Konvertiert ein Array von Dynamo-Vektorobjekten in ein Array von Revit-XYZ-Vektoren.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Konvertiert ein Array von double-Werten in ein Revit-DoubleArray-Objekt.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Konvertiert ein zweidimensionales Array (double[][]), in dem jedes innere Array ein Wertepaar (U und V) darstellt, in ein Array von Revit-UV-Objekten.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Konvertiert ein zweidimensionales Array (double[][]), in dem jedes innere Array ein Wertepaar (U und V) darstellt, in ein Array von Dynamo-UV-Objekten.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Beispiel für die Verwendung von Proto-in-Revit-Typen

Dieses Beispiel zeigt eine schnelle und einfache Methode zur Verwendung der Methode .ToXyz (Point) zum Konvertieren eines Dynamo-Point.ByCoordinates-Objekts in einen Revit-XYZ-Wert. 

![Konvertieren eines Dynamo-Point.ByCoordinates-Objekts in einen Revit-XYZ-Wert](Images/dynamo-point-to-revit-xyz.png)

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



### Revit-in-Proto-Typen

#### ToProtoType (BoundingBox)

Konvertiert ein Revit-BoundingBoxXYZ-Objekt in ein Dynamo-BoundingBox-Objekt.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Konvertiert einen Revit-XYZ-Wert in einen Dynamo-Punkt.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Konvertiert einen Revit-Punkt in einen Dynamo-Punkt.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Konvertiert einen Revit-XYZ-Wert in einen Dynamo-Vektor.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Konvertiert einen Revit-UV-Wert in einen Dynamo-UV-Wert.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Konvertiert eine Revit-Ebene in eine Dynamo-Ebene.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Konvertiert eine Revit-Transformation in ein Dynamo-CoordinateSystem-Objekt.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Konvertiert eine Liste mit Revit-XYZ-Punkten in eine Liste mit Dynamo-Punkten.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Beispiel für die Verwendung von Revit-in-Proto-Typen

Dieses Beispiel zeigt eine schnelle und einfache Methode zur Verwendung der Methode .ToPoint (XYZ) zum Konvertieren eines Revit-XYZ-Werts in einen Dynamo-Punkt. 

![Konvertieren eines Revit-XYZ-Werts in ein Dynamo-Point.ByCoordinates-Objekt](Images/revit-xyz-to-dynamo-point.png)

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

### Grad und Bogenmaß

#### ToRadians

Wandelt Grad in Bogenmaß um.

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

Wandelt Bogenmaß in Grad um.

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### Beispiel für die Verwendung von Grad und Bogenmaß

Dieses Beispiel zeigt eine schnelle und einfache Methode zur Verwendung der Methode .ToRadians zum Umwandeln von Grad in Bogenmaß. 

![Grad in Bogenmaß](Images/degrees-to-radians.png)

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
### X und UZ

#### GetPerpendicular (XYZ)

Diese Methode gibt einen lotrechten `XYZ`-Vektor für den angegebenen `XYZ`-Vektor zurück.

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

Diese Methode gibt einen lotrechten Dynamo-`Vector`-Wert für den angegebenen Dynamo-`Vector`-Wert zurück.

` public static Vector GetPerpendicular(this Vector vector)`

#### Beispiel für die Verwendung von X und UZ

Dieses Beispiel zeigt eine schnelle und einfache Methode zur Verwendung der Methode .GetPerpendicular zum Abrufen des lotrechten Vektors für einen Eingabevektor. 

![Lotrechten Vektor abrufen](Images/get-perpendicular-vector.png)

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
