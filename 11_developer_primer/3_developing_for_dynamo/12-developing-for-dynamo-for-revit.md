# Développement pour Dynamo for Revit

## Utilisation de méthodes dans `GeometryPrimitiveConverter.cs`

La classe [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) de la bibliothèque de codes de DynamoRevit propose différentes méthodes de conversion entre les types géométriques Revit et Dynamo. Ces méthodes sont utiles pour travailler avec la géométrie dans des scripts Dynamo qui interagissent avec des modèles Revit.

### Catégories de méthodes

Les méthodes de `GeometryPrimitiveConverter.cs` peuvent être regroupées en quatre catégories principales :

1. **Types Proto à Revit** : méthodes qui convertissent les types Dynamo (Proto) en types Revit.
2. **Types Revit à Proto**  : méthodes qui convertissent les types Revit en types Dynamo (Proto).
3. **Degrés et radians** : méthodes qui convertissent entre degrés et radians.
4. **X et UZ** : méthodes qui traitent de l’obtention de vecteurs perpendiculaires. 

### Types Proto à Revit

#### ToRevitBoundingBox

Crée une zone Revit BoundingBoxXYZ à partir d’un système de coordonnées Dynamo et de deux points de définition (minimum et maximum).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Convertit une zone Dynamo BoundingBox en zone Revit BoundingBoxXYZ.

L’indicateur convertUnits (true par défaut) détermine si les coordonnées doivent être converties du système d’unités de Dynamo en unités internes de Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Convertit un Dynamo Point en Revit XYZ.

L’indicateur convertUnits (true par défaut) convertit les coordonnées si nécessaire.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vecteur)

Convertit un vecteur Dynamo Vector en Revit XYZ.

Notez que l’indicateur convertUnits est défini par défaut sur false, car les vecteurs représentent la direction et la magnitude, qui ne nécessitent généralement pas de conversion d’unités. La conversion peut avoir une incidence sur la direction et la longueur du vecteur. 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Convertit un Dynamo Point en Revit XYZ.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vecteur)

Convertit un vecteur Dynamo Vector en Revit XYZ.

Notez que l’indicateur convertUnits est défini par défaut sur false, car les vecteurs représentent la direction et la magnitude, qui ne nécessitent généralement pas de conversion d’unités. La conversion peut avoir une incidence sur la direction et la longueur du vecteur. 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Convertit un système Dynamo CoordinateSystem en transformation Revit Transform.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Convertit un plan Dynamo Plane en Revit Plane.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Convertit les collections d’objets Dynamo Point en collections Revit XYZ.

Renvoie une liste de XYZ. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Renvoie un tableau de XYZ. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vecteurs)

Convertit un tableau d’objets Dynamo Vector en tableau de vecteurs Revit XYZ.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Convertit un tableau de valeurs doubles en DoubleArray de Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Convertit un tableau à deux dimensions (double[][]), où chaque tableau interne représente une paire de valeurs (U et V) en un tableau d’objets Revit UV.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Convertit un tableau à deux dimensions (double[][]), où chaque tableau interne représente une paire de valeurs (U et V) en un tableau d’objets Dynamo UV.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Exemple d’utilisation de types Proto à Revit

Cet exemple illustre une façon simple et rapide d’utiliser la méthode .ToXyz (Point) pour convertir un système Dynamo Point.ByCoordinates en Revit XYZ. 

![Conversion de Dynamo Point.ByCoordinates en Revit XYZ](Images/dynamo-point-to-revit-xyz.png)

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



### Types Revit à Proto

#### ToProtoType (BoundingBox)

Convertit une zone Revit BoundingBoxXYZ en zone Dynamo BoundingBox.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Convertit un Revit XYZ en Dynamo Point.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Convertit un Revit Point en Dynamo Point.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Convertit un Revit XYZ en Dynamo Vector.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Convertit un Revit UV en Dynamo UV.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Convertit un plan Revit Plane en Dynamo Plane.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Convertit une transformation Revit Transform en Dynamo CoordinateSystem.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Convertit une liste de points Revit XYZ en liste de points Dynamo Point.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Exemple d’utilisation de types Revit à Proto

Cet exemple illustre une façon simple et rapide d’utiliser la méthode .ToPoint (XYZ) pour convertir un Revit XYZ en point Dynamo Point. 

![Conversion de Revit XYZ en Dynamo Point.ByCoordinates](Images/revit-xyz-to-dynamo-point.png)

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

### Degrés et radians

#### ToRadians

Convertit les degrés en radians.

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

Convertit les radians en degrés.

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### Exemple d’utilisation de Degrés et radians

Cet exemple illustre une façon simple et rapide d’utiliser la méthode .ToRadians pour effectuer une conversion de degrés en radians. 

![Degrés en radians](Images/degrees-to-radians.png)

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
### X et UZ

#### GetPerpendicular (XYZ)

Cette méthode renvoie un vecteur `XYZ` perpendiculaire au vecteur `XYZ` donné.

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vecteur)

Cette méthode renvoie un vecteur Dynamo `Vector` perpendiculaire au vecteur Dynamo `Vector` donné.

` public static Vector GetPerpendicular(this Vector vector)`

#### Exemple d’utilisation de X et UZ

Cet exemple illustre une façon simple et rapide d’utiliser la méthode .GetPerpendicular pour obtenir le vecteur perpendiculaire à un vecteur d’entrée. 

![Obtention de vecteur perpendiculaire](Images/get-perpendicular-vector.png)

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
