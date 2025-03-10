# Dynamo for Revit을 위한 개발

## `GeometryPrimitiveConverter.cs`에서 메서드 사용

DynamoRevit 코드 라이브러리의 [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) 클래스는 Revit 형상 유형과 Dynamo 형상 유형 간에 변환하는 다양한 메서드를 제공합니다. 이러한 메서드는 Revit 모델과 상호 작용하는 Dynamo 스크립트에서 형상 작업을 수행할 때 유용합니다.

### 메서드 카테고리

`GeometryPrimitiveConverter.cs`의 메서드는 다음 네 가지 주요 카테고리로 그룹화할 수 있습니다.

1. **Proto에서 Revit으로 변환 유형**: Dynamo(Proto) 유형을 Revit 유형으로 변환하는 메서드입니다.
2. **Revit에서 Proto로 변환 유형**: Revit 유형을 Dynamo(Proto) 유형으로 변환하는 메서드입니다.
3. **각도 및 라디안**: 각도와 라디안 간에 변환하는 메서드입니다.
4. **X 및 UZ**: 수직 벡터를 구하는 메서드입니다. 

### Proto에서 Revit으로 변환 유형

#### ToRevitBoundingBox

Dynamo 좌표계와 정의점 두 개(최솟값 및 최댓값)에서 Revit BoundingBoxXYZ를 작성합니다.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType(BoundingBox)

Dynamo BoundingBox를 Revit BoundingBoxXYZ로 변환합니다.

convertUnits 플래그(기본값: true)는 좌표를 Dynamo의 단위 시스템에서 Revit의 내부 단위로 변환해야 하는지를 결정합니다.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType(점)

Dynamo 점을 Revit XYZ로 변환합니다.

convertUnits 플래그(기본값: true)는 필요한 경우 좌표를 변환합니다.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType(벡터)

Dynamo 벡터를 Revit XYZ로 변환합니다.

벡터는 일반적으로 단위 변환이 필요하지 않은 방향과 크기를 나타내므로 convertUnits 플래그의 기본값은 false입니다. 변환은 벡터의 방향 및 길이에 영향을 줄 수 있습니다. 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz(포인트)

Dynamo 점을 Revit XYZ로 변환합니다.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz(벡터)

Dynamo 벡터를 Revit XYZ로 변환합니다.

벡터는 일반적으로 단위 변환이 필요하지 않은 방향과 크기를 나타내므로 convertUnits 플래그의 기본값은 false입니다. 변환은 벡터의 방향 및 길이에 영향을 줄 수 있습니다. 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Dynamo CoordinateSystem을 Revit 변환으로 변환합니다.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Dynamo 평면을 Revit 평면으로 변환합니다.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs(점)

Dynamo 점 객체의 모음을 Revit XYZ 모음으로 변환합니다.

XYZ의 리스트를 반환합니다. `public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

XYZ의 배열을 반환합니다. `public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs(벡터)

Dynamo 벡터 객체의 배열을 Revit XYZ 벡터 배열로 변환합니다.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

double 값 배열을 Revit의 DoubleArray로 변환합니다.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

각각의 내부 배열이 값 쌍(U와 V)을 나타내는 2차원 배열(double[][])을 Revit UV 객체의 배열로 변환합니다.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

각각의 내부 배열이 값 쌍(U와 V)을 나타내는 2차원 배열(double[][])을 Dynamo UV 객체의 배열로 변환합니다.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Proto에서 Revit으로 변환 유형 사용 예시

이 예에서는 .ToXyz(점) 메서드를 사용하여 Dynamo Point.ByCoordinates를 Revit XYZ로 빠르고 쉽게 변환하는 방법을 보여줍니다. 

![Dynamo Point.ByCoordinates를 Revit XYZ로 변환](Images/dynamo-point-to-revit-xyz.png)

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



### Revit에서 Proto로 변환 유형

#### ToProtoType(BoundingBox)

Revit BoundingBoxXYZ를 Dynamo BoundingBox로 변환합니다.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint(XYZ)

Revit XYZ를 Dynamo 점으로 변환합니다.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType(점)

Revit 점을 Dynamo 점으로 변환합니다.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector(XYZ)

Revit XYZ를 Dynamo 벡터로 변환합니다.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType(UV)

Revit UV를 Dynamo UV로 변환합니다.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane(Revit 평면)

Revit 평면을 Dynamo 평면으로 변환합니다.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Revit 변환을 Dynamo CoordinateSystem으로 변환합니다.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Revit XYZ 점 리스트를 Dynamo 점 리스트로 변환합니다.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Revit에서 Proto로 변환 유형 사용 예시

이 예에서는 .ToPoint(XYZ) 메서드를 사용하여 Revit XYZ를 Dynamo 점으로 빠르고 쉽게 변환하는 방법을 보여줍니다. 

![Revit XYZ를 Dynamo Point.ByCoordinates로 변환](Images/revit-xyz-to-dynamo-point.png)

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

### 각도 및 라디안

#### ToRadians

도 단위를 라디안으로 변환합니다.

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

라디안을 도 단위로 변환합니다.

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### 각도 및 라디안 사용 예시

이 예에서는 .ToRadians 메서드를 사용하여 각도를 라디안으로 빠르고 쉽게 변환하는 방법을 보여줍니다. 

![각도 라디안 변환](Images/degrees-to-radians.png)

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

#### GetPerpendicular(XYZ)

이 메서드는 지정된 `XYZ` 벡터에 대한 수직 `XYZ` 벡터를 반환합니다.

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular(벡터)

이 메서드는 지정된 Dynamo `Vector`에 대한 수직 Dynamo `Vector`를 반환합니다.

` public static Vector GetPerpendicular(this Vector vector)`

#### X 및 UZ 사용 예시

이 예에서는 .GetPerpendicular 메서드를 사용하여 입력 벡터에 대한 수직 벡터를 빠르고 쉽게 가져오는 방법을 보여줍니다. 

![수직 벡터 가져오기](Images/get-perpendicular-vector.png)

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
