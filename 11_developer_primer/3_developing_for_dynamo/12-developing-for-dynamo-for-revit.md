# Vývoj pro modul Dynamo pro aplikaci Revit

## Použití metod ve třídě `GeometryPrimitiveConverter.cs`

Třída [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) v knihovně kódu doplňku DynamoRevit poskytuje různé metody pro převod mezi typy geometrie aplikací Revit a Dynamo. Tyto metody jsou užitečné při práci s geometrií ve skriptech aplikace Dynamo, které interagují s modely aplikace Revit.

### Kategorie metod

Metody ve třídě `GeometryPrimitiveConverter.cs` lze seskupit do čtyř hlavních kategorií:

1. **Typy Proto na Revit**: Metody, které převádějí typy aplikace Dynamo (Proto) na typy aplikace Revit.
2. **Typy Revit na Proto**: Metody, které převádějí typy aplikace Revit na typy aplikace Dynamo (Proto).
3. **Stupně a radiány**: Metody, které převádějí stupně a radiány.
4. **X a UZ**: Metody, které se zabývají získáváním kolmých vektorů.

### Typy Proto na Revit

#### ToRevitBoundingBox

Vytvoří objekt BoundingBoxXYZ aplikace Revit ze souřadnicového systému aplikace Dynamo a dvou definičních bodů (minimum a maximum).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox( Autodesk.DesignScript.Geometry.CoordinateSystem cs, Autodesk.DesignScript.Geometry.Point minPoint, Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Převede objekt BoundingBox aplikace Dynamo na objekt BoundingBoxXYZ aplikace Revit.

Příznak convertUnits (výchozí hodnota true) určuje, zda mají být souřadnice převedeny ze systému jednotek aplikace Dynamo na vnitřní jednotky aplikace Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Převede objekt Point aplikace Dynamo na objekt XYZ aplikace Revit.

Příznak convertUnits (výchozí hodnota true) v případě potřeby převede souřadnice.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

Převede objekt Vector aplikace Dynamo na objekt XYZ aplikace Revit.

Všimněte si, že výchozí hodnotou příznaku convertUnits je false, protože vektory představují směr a velikost, které obvykle nevyžadují převod jednotek. Převod může ovlivnit směr a délku vektoru.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Převede objekt Point aplikace Dynamo na objekt XYZ aplikace Revit.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

Převede objekt Vector aplikace Dynamo na objekt XYZ aplikace Revit.

Všimněte si, že výchozí hodnotou příznaku convertUnits je false, protože vektory představují směr a velikost, které obvykle nevyžadují převod jednotek. Převod může ovlivnit směr a délku vektoru.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Převede objekt CoordinateSystem aplikace Dynamo na objekt Transform aplikace Revit.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Převede objekt Plane aplikace Dynamo na objekt Plane aplikace Revit.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Převede kolekce objektů Point aplikace Dynamo na kolekce objektů XYZ aplikace Revit.

Vrátí seznam objektů XYZ. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Vrátí pole hodnot XYZ. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

Převede pole objektů Vector aplikace Dynamo na pole vektorů XYZ aplikace Revit.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Převede pole hodnot double na objekt DoubleArray aplikace Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Převede dvojrozměrné pole (double[][]), kde každé vnitřní pole představuje dvojici hodnot (U a V), na pole objektů UV aplikace Revit.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Převede dvojrozměrné pole (double[][]), kde každé vnitřní pole představuje dvojici hodnot (U a V), na pole objektů UV aplikace Dynamo.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Příklad použití typů Proto To Revit

Tento příklad ukazuje rychlý a snadný způsob použití metody .ToXyz (Point) k převodu objektu Point.ByCoordinates aplikace Dynamo na objekt XYZ aplikace Revit.

![Převod objektu Point.ByCoordinates aplikace Dynamo na objekt XYZ aplikace Revit](Images/dynamo-point-to-revit-xyz.png)

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

### Typy Revit na Proto

#### ToProtoType (BoundingBox)

Převede objekt BoundingBoxXYZ aplikace Revit na objekt BoundingBox aplikace Dynamo.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Převede objekt XYZ aplikace Revit na objekt Point aplikace Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Převede objekt Point aplikace Revit na objekt Point aplikace Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Převede objekt XYZ aplikace Revit na objekt Vector aplikace Dynamo.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Převede objekt UV aplikace Revit na objekt UV aplikace Dynamo.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Převede objekt Plane aplikace Revit na objekt Plane aplikace Dynamo.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Převede objekt Transform aplikace Revit na objekt CoordinateSystem aplikace Dynamo.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Převede seznam bodů XYZ aplikace Revit na seznam bodů objektů Point aplikace Dynamo.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Příklad použití typů Revit na Proto

Tento příklad ukazuje rychlý a snadný způsob použití metody .ToPoint (XYZ) k převodu objektu XYZ aplikace Revit na objekt Point aplikace Dynamo.

![Převod objektu XYZ aplikace Revit na objekt Point.ByCoordinates aplikace Dynamo](Images/revit-xyz-to-dynamo-point.png)

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

### Stupně a radiány

#### ToRadians

Převede stupně na radiány.

`public static double ToRadians(this double degrees) { return degrees * Math.PI / 180.0; }`

#### ToDegrees

Převede radiány na stupně.

`public static double ToDegrees(this double degrees) { return degrees * 180.0 / Math.PI; }`

#### Příklad použití stupňů a radiánů

Tento příklad ukazuje rychlý a snadný způsob použití metody .ToRadians pro převod ze stupňů na radiány.

![Stupně na radiány](Images/degrees-to-radians.png)

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

### X a UZ

#### GetPerpendicular (XYZ)

Tato metoda vrací vektor `XYZ` kolmý na zadaný vektor `XYZ`.

`public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

Tato metoda vrací objekt `Vector` aplikace Dynamo kolmý na zadaný objekt `Vector` aplikace Dynamo.

`public static Vector GetPerpendicular(this Vector vector)`

#### Příklad použití metody X a UZ

Tento příklad ukazuje rychlý a snadný způsob použití metody .GetPerpendicular k získání vektoru kolmého na vstupní vektor.

![Získání kolmého vektoru](Images/get-perpendicular-vector.png)

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
