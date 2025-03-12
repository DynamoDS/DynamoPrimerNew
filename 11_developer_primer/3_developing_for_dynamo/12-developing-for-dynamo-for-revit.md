# Sviluppo per Dynamo for Revit

## Utilizzo dei metodi in `GeometryPrimitiveConverter.cs`

La classe [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) nella libreria di codici di DynamoRevit fornisce vari metodi per convertire i tipi geometrici tra Revit e Dynamo. Questi metodi sono utili quando si utilizza la geometria negli script di Dynamo che interagiscono con i modelli di Revit.

### Categorie di metodi

I metodi in `GeometryPrimitiveConverter.cs` possono essere raggruppati in quattro categorie principali:

1. **Da prototipi a tipi di Revit**: metodi che convertono i tipi di Dynamo (prototipi) in tipi di Revit.
2. **Da tipi di Revit a prototipi**: metodi che convertono i tipi di Revit in tipi di Dynamo (prototipi).
3. **Gradi e radianti**: metodi che eseguono la conversione tra gradi e radianti.
4. **X e UZ**: metodi che consentono di ottenere vettori perpendicolari. 

### Da prototipi a tipi di Revit

#### ToRevitBoundingBox

Crea BoundingBoxXYZ di Revit da un sistema di coordinate di Dynamo e due punti di definizione (minimo e massimo).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Converte BoundingBox di Dynamo in BoundingBoxXYZ di Revit.

Il flag convertUnits (per default, impostato su true) determina se le coordinate devono essere convertite dal sistema di unità di misura di Dynamo nelle unità interne di Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Converte un punto di Dynamo in una coordinata XYZ di Revit.

Se necessario, il flag convertUnits (per default, impostato su true) converte le coordinate.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

Converte un vettore di Dynamo in una coordinata XYZ di Revit.

Notare che, per default, il flag convertUnits è impostato su false poiché i vettori rappresentano la direzione e l'intensità, che in genere non richiedono la conversione delle unità. La conversione potrebbe influenzare la direzione e la lunghezza del vettore. 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Converte un punto di Dynamo in una coordinata XYZ di Revit.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

Converte un vettore di Dynamo in una coordinata XYZ di Revit.

Notare che, per default, il flag convertUnits è impostato su false poiché i vettori rappresentano la direzione e l'intensità, che in genere non richiedono la conversione delle unità. La conversione potrebbe influenzare la direzione e la lunghezza del vettore. 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Converte CoordinateSystem di Dynamo in una trasformazione di Revit.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Converte un piano di Dynamo in un piano di Revit.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Converte le raccolte di oggetti punto di Dynamo in raccolte di coordinate XYZ di Revit.

Restituisce un elenco di coordinate XYZ. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Restituisce una matrice di coordinate XYZ. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

Converte una matrice di oggetti vettore di Dynamo in una matrice di vettori XYZ di Revit.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Converte una matrice di valori double in DoubleArray di Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Converte una matrice bidimensionale (double[][]) in cui ogni matrice interna rappresenta una coppia di valori (U e V) in una matrice di oggetti UV di Revit.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Converte una matrice bidimensionale (double[][]) in cui ogni matrice interna rappresenta una coppia di valori (U e V) in una matrice di oggetti UV di Dynamo.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Esempio di utilizzo di prototipi convertiti in tipi di Revit

In questo esempio viene illustrato un modo semplice e rapido per utilizzare il metodo ToXyz (Point) per convertire Point.ByCoordinates di Dynamo in una coordinata XYZ di Revit. 

![Conversione di Point.ByCoordinates di Dynamo in una coordinata XYZ di Revit](Images/dynamo-point-to-revit-xyz.png)

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



### Da tipi di Revit a prototipi

#### ToProtoType (BoundingBox)

Converte BoundingBoxXYZ di Revit in BoundingBox di Dynamo.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Converte una coordinata XYZ di Revit in un punto di Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Converte un punto di Revit in un punto di Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Converte una coordinata XYZ di Revit in un vettore di Dynamo.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Converte un valore UV di Revit in un valore UV di Dynamo.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Converte un piano di Revit in un piano di Dynamo.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Converte una trasformazione di Revit in CoordinateSystem di Dynamo.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Converte un elenco di punti XYZ di Revit in un elenco di punti di Dynamo.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Esempio di utilizzo di tipi di Revit convertiti in prototipi

In questo esempio viene illustrato un modo semplice e rapido per utilizzare il metodo ToPoint (XYZ) per convertire una coordinata XYZ di Revit in un punto di Dynamo. 

![Conversione di una coordinata XYZ di Revit in Point.ByCoordinates di Dynamo](Images/revit-xyz-to-dynamo-point.png)

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

### Gradi e radianti

#### ToRadians

Converte i gradi in radianti.

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

Converte i radianti in gradi.

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### Esempio di utilizzo di gradi e radianti

In questo esempio viene illustrato un modo semplice e rapido per utilizzare il metodo ToRadians per convertire i gradi in radianti. 

![Da gradi a radianti](Images/degrees-to-radians.png)

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
### X e UZ

#### GetPerpendicular (XYZ)

Questo metodo restituisce un vettore `XYZ` perpendicolare al vettore `XYZ` specificato.

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

Questo metodo restituisce `Vector` di Dynamo perpendicolare a `Vector` di Dynamo specificato.

` public static Vector GetPerpendicular(this Vector vector)`

#### Esempio di utilizzo di X e UZ

In questo esempio viene illustrato un modo semplice e rapido per utilizzare il metodo GetPerpendicular per ottenere il vettore perpendicolare ad un vettore di input. 

![Acquisizione del vettore perpendicolare](Images/get-perpendicular-vector.png)

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
