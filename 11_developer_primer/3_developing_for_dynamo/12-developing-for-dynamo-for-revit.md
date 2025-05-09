# Desenvolvimento do Dynamo para Revit

## Usar métodos em `GeometryPrimitiveConverter.cs`

A classe [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) na biblioteca de códigos do DynamoRevit fornece vários métodos para converter entre tipos geométricos do Revit e do Dynamo. Esses métodos são úteis ao trabalhar com geometria em scripts do Dynamo que interagem com modelos do Revit.

### Categorias de métodos

É possível agrupar os métodos em `GeometryPrimitiveConverter.cs` em quatro categorias principais:

1. **Tipos do Proto para Revit**: métodos que convertem tipos do Dynamo (Proto) em tipos do Revit.
2. **Tipos do Revit para Proto**: métodos que convertem tipos do Revit em tipos do Dynamo (Proto).
3. **Graus e radianos**: métodos que convertem entre graus e radianos.
4. **X e UZ**: métodos que lidam com a obtenção de vetores perpendiculares.

### Tipos do Proto para Revit

#### ToRevitBoundingBox

Cria uma BoundingBoxXYZ do Revit com base em um sistema de coordenadas do Dynamo e em dois pontos de definição (mínimo e máximo).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox( Autodesk.DesignScript.Geometry.CoordinateSystem cs, Autodesk.DesignScript.Geometry.Point minPoint, Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Converte uma BoundingBox do Dynamo em uma BoundingBoxXYZ do Revit.

O indicador convertUnits (true definido como padrão) determina se as coordenadas devem ser convertidas do sistema de unidades do Dynamo em unidades internas do Revit.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (ponto)

Converte um ponto do Dynamo em um XYZ do Revit.

O indicador convertUnits (true definido como padrão) converte as coordenadas, se necessário.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (vetor)

Converte um vetor do Dynamo em um XYZ do Revit.

Observe que o indicador convertUnits tem como padrão false porque os vetores representam a direção e a magnitude, que normalmente não requerem conversão de unidade. A conversão pode afetar a direção e o comprimento do vetor.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (ponto)

Converte um ponto do Dynamo em um XYZ do Revit.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (vetor)

Converte um vetor do Dynamo em um XYZ do Revit.

Observe que o indicador convertUnits tem como padrão false porque os vetores representam a direção e a magnitude, que normalmente não requerem conversão de unidade. A conversão pode afetar a direção e o comprimento do vetor.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Converte um CoordinateSystem do Dynamo em uma transformação do Revit.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Converte um plano do Dynamo em um plano do Revit.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (pontos)

Converte coleções de objetos de ponto do Dynamo em coleções XYZ do Revit.

Retornar uma lista de XYZs. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Retornar uma matriz de XYZs. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (vetores)

Converte uma matriz de objetos de vetor do Dynamo em uma matriz de vetores XYZ do Revit.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Converte uma matriz de valores duplos em DoubleArray do Revit.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Converte uma matriz bidimensional (double[][]), em que cada matriz interna representa um par de valores (U e V) em uma matriz de objetos UV do Revit.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Converte uma matriz bidimensional (double[][]), em que cada matriz interna representa um par de valores (U e V) em uma matriz de objetos UV do Dynamo.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Exemplo de uso de tipos do Proto para Revit

Este exemplo mostra um método rápido e fácil de usar o método .ToXyz (ponto) para converter um Point.ByCoordinates do Dynamo em um XYZ do Revit.

![Converter um Point.ByCoordinates do Dynamo em um XYZ do Revit](Images/dynamo-point-to-revit-xyz.png)

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

### Tipos do Revit para Proto

#### ToProtoType (BoundingBox)

Converte uma BoundingBoxXYZ do Revit em uma BoundingBox do Dynamo.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Converte um XYZ do Revit em um ponto do Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (ponto)

Converte um ponto do Revit em um ponto do Dynamo.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Converte um XYZ do Revit em um vetor do Dynamo.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Converte um UV do Revit em um UV do Dynamo.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Plano do Revit)

Converte um plano do Revit em um plano do Dynamo.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Converte uma transformação do Revit em um CoordinateSystem do Dynamo.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Converte uma lista de pontos XYZ do Revit em uma lista de pontos do Dynamo.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Exemplo de uso dos tipos do Revit para Proto

Este exemplo mostra um método rápido e fácil de usar o método .ToPoint (XYZ) para converter um XYZ do Revit em um ponto do Dynamo.

![Converter um XYZ do Revit em um Point.ByCoordinates do Dynamo](Images/revit-xyz-to-dynamo-point.png)

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

### Graus e radianos

#### ToRadians

Converte graus em radianos.

`public static double ToRadians(this double degrees) { return degrees * Math.PI / 180.0; }`

#### ToDegrees

Converte radianos em graus.

`public static double ToDegrees(this double degrees) { return degrees * 180.0 / Math.PI; }`

#### Exemplo de uso de graus e radianos

Este exemplo mostra um método rápido e fácil de usar o método .ToRadianos para converter de graus em radianos.

![Graus em radianos](Images/degrees-to-radians.png)

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

Esse método retorna um vetor `XYZ` perpendicular ao vetor `XYZ` fornecido.

`public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (vetor)

Esse método retorna um `Vector` do Dynamo perpendicular ao `Vector` do Dynamo fornecido.

`public static Vector GetPerpendicular(this Vector vector)`

#### Exemplo de uso de X e UZ

Este exemplo mostra um método rápido e fácil de usar o método .GetPerpendicular para obter o vetor perpendicular a um vetor de entrada.

![Obter vetor perpendicular](Images/get-perpendicular-vector.png)

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
