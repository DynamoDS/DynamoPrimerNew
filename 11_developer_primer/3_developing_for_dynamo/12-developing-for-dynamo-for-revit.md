# Desarrollo para Dynamo for Revit

## Uso de métodos en `GeometryPrimitiveConverter.cs`

La clase [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) de la biblioteca de código de DynamoRevit proporciona varios métodos para la conversión entre tipos geométricos de Revit y Dynamo. Estos métodos son útiles cuando se trabaja con geometría en secuencias de comandos de Dynamo que interactúan con modelos de Revit.

### Categorías de métodos

Los métodos de `GeometryPrimitiveConverter.cs` se pueden agrupar en cuatro categorías principales:

1. **De Proto a tipos de Revit**: métodos que convierten tipos de Dynamo (Proto) en tipos de Revit.
2. **De Revit a tipos de Proto**: métodos que convierten tipos de Revit en tipos de Dynamo (Proto).
3. **Grados y radianes**: métodos que convierten entre grados y radianes.
4. **X y UZ**: métodos que se encargan de obtener vectores perpendiculares.

### De Proto a tipos de Revit

#### ToRevitBoundingBox

Crea un BoundingBoxXYZ de Revit a partir de un sistema de coordenadas de Dynamo y dos puntos de definición (mínimo y máximo).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox( Autodesk.DesignScript.Geometry.CoordinateSystem cs, Autodesk.DesignScript.Geometry.Point minPoint, Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Convierte un BoundingBox de Dynamo en un BoundingBoxXYZ de Revit.

El indicador convertUnits (establecido por defecto en "true") determina si las coordenadas deben convertirse del sistema de unidades de Dynamo a las unidades internas de Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (punto)

Convierte un punto de Dynamo en un XYZ de Revit.

El indicador convertUnits (establecido por defecto en "true") convierte las coordenadas si es necesario.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (vector)

Convierte un vector de Dynamo en un XYZ de Revit.

Observe que el indicador convertUnits se establece por defecto en "false" (falso) porque los vectores representan la dirección y la magnitud, que normalmente no requieren conversión de unidades. La conversión puede afectar a la dirección y la longitud del vector.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (punto)

Convierte un punto de Dynamo en un XYZ de Revit.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (vector)

Convierte un vector de Dynamo en un XYZ de Revit.

Observe que el indicador convertUnits se establece por defecto en "false" (falso) porque los vectores representan la dirección y la magnitud, que normalmente no requieren conversión de unidades. La conversión puede afectar a la dirección y la longitud del vector.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Convierte un CoordinateSystem de Dynamo en una transformación de Revit.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Convierte un plano de Dynamo en un plano de Revit.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (punto)

Convierte colecciones de objetos de puntos de Dynamo en colecciones XYZ de Revit.

Devuelve una lista de XYZ. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Devuelve una matriz de XYZ. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (vectores)

Convierte una matriz de objetos de vectores de Dynamo en una matriz de vectores XYZ de Revit.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Convierte una matriz de valores dobles en DoubleArray de Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Convierte una matriz bidimensional (double[][]) en la que cada matriz interna representa un par de valores (U y V) en una matriz de objetos UV de Revit.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Convierte una matriz bidimensional (double[][]) en la que cada matriz interior representa un par de valores (U y V) en una matriz de objetos UV de Dynamo.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Ejemplo de uso de Proto a tipos de Revit

En este ejemplo, se muestra un método rápido y sencillo de utilizar el método .ToXyz (punto) para convertir un Point.ByCoordinates de Dynamo en un XYZ de Revit.

![Conversión de un Point.ByCoordinates de Dynamo en un XYZ de Revit](Images/dynamo-point-to-revit-xyz.png)

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

### De Revit a tipos de Proto

#### ToProtoType (BoundingBox)

Convierte un BoundingBoxXYZ de Revit en un BoundingBox de Dynamo.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Convierte un XYZ de Revit en un punto de Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (punto)

Convierte un punto de Revit en un punto de Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Convierte un XYZ de Revit en un vector de Dynamo.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Convierte un UV de Revit en un UV de Dynamo.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (plano de Revit)

Convierte un plano de Revit en un plano de Dynamo.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Convierte una transformación de Revit en un CoordinateSystem de Dynamo.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Convierte una lista de puntos XYZ de Revit en una lista de puntos de Dynamo.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Ejemplo de uso de Revit a tipos de Proto

En este ejemplo, se muestra una forma rápida y sencilla de utilizar el método .ToPoint (XYZ) para convertir un XYZ de Revit en un punto de Dynamo.

![Conversión de un XYZ de Revit en un Point.ByCoordinates de Dynamo](Images/revit-xyz-to-dynamo-point.png)

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

### Grados y radianes

#### ToRadians

Convierte grados en radianes.

`public static double ToRadians(this double degrees) { return degrees * Math.PI / 180.0; }`

#### ToDegrees

Convierte radianes en grados.

`public static double ToDegrees(this double degrees) { return degrees * 180.0 / Math.PI; }`

#### Ejemplo de uso de grados y radianes

En este ejemplo, se muestra una forma rápida y sencilla de utilizar el método .ToRadians para convertir de grados a radianes.

![Grados a radianes](Images/degrees-to-radians.png)

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

### X & UZ

#### GetPerpendicular (XYZ)

Este método devuelve un vector `XYZ` perpendicular al vector `XYZ` especificado.

`public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (vector)

Este método devuelve un `Vector` de Dynamo perpendicular al `Vector` especificado.

`public static Vector GetPerpendicular(this Vector vector)`

#### Ejemplo de uso de X y UZ

En este ejemplo, se muestra una forma rápida y sencilla de utilizar el método .GetPerpendicular para obtener el vector perpendicular a un vector de entrada.

![Obtener vector perpendicular](Images/get-perpendicular-vector.png)

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
