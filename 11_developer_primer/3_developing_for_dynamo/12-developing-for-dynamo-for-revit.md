# 为适用于 Revit 的 Dynamo 开发

## 在 `GeometryPrimitiveConverter.cs` 中使用方法

DynamoRevit 代码库中的 [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) 类提供了各种方法用于在 Revit 几何类型和 Dynamo 几何类型之间进行转换。在与 Revit 模型交互的 Dynamo 脚本中处理几何图形时，这些方法非常有用。

### 方法类别

`GeometryPrimitiveConverter.cs` 中的方法可以分为四大类：

1. **Proto 到 Revit 类型**：将 Dynamo (Proto) 类型转换为 Revit 类型的方法。
2. **Revit 到 Proto 类型**：将 Revit 类型转换为 Dynamo (Proto) 类型的方法。
3. **度数和弧度**：在度数和弧度之间转换的方法。
4. **X 和 UZ**：用于获取垂直向量的方法。 

### 原型到 Revit 类型

#### ToRevitBoundingBox

从 Dynamo 坐标系和两个定义点（最小值和最大值）创建 Revit BoundingBoxXYZ。

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

将 Dynamo BoundingBox 转换为 Revit BoundingBoxXYZ。

convertUnits 标志（默认为 true）确定坐标是否应从 Dynamo 的单位系统转换为 Revit 的内部单位。

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType （点）

将 Dynamo 点转换为 Revit XYZ。

convertUnits 标志（默认为 true）在必要时转换坐标。

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType （向量）

将 Dynamo 向量转换为 Revit XYZ。

请注意，convertUnits 标志默认为 false，因为向量表示方向和大小，而这通常不需要单位转换。转换可能会影响向量的方向和长度。 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz（点）

将 Dynamo 点转换为 Revit XYZ。

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz（向量）

将 Dynamo 向量转换为 Revit XYZ。

请注意，convertUnits 标志默认为 false，因为向量表示方向和大小，而这通常不需要单位转换。转换可能会影响向量的方向和长度。 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

将 Dynamo CoordinateSystem 转换为 Revit 变换。

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

将 Dynamo 平面转换为 Revit 平面。

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs（点）

将 Dynamo 点对象集合转换为 Revit XYZ 集合。

返回 XYZ 列表。`public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

返回 XYZ 阵列。`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs（向量）

将 Dynamo 矢量对象的阵列转换为 Revit XYZ 矢量阵列。

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

将双精度值的阵列转换为 Revit 的 DoubleArray。

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUv

将二维阵列 (double[][])（其中每个内部阵列表示一对值（U 和 V））转换为 Revit UV 对象阵列。

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

将二维阵列 (double[][])（其中每个内部阵列表示一对值（U 和 V））转换为 Dynamo UV 对象阵列。

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### 使用原型到 Revit 类型的示例

此示例显示了一种快捷方法，用于使用 .ToXyz（点）方法将 Dynamo Point.ByCoordinates 转换为 Revit XYZ。 

![将 Dynamo Point.ByCoordinates 转换为 Revit XYZ](Images/dynamo-point-to-revit-xyz.png)

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



### Revit 到原型类型

#### ToProtoType (BoundingBox)

将 Revit BoundingBoxXYZ 转换为 Dynamo BoundingBox。

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

将 Revit XYZ 转换为 Dynamo 点。

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType（点）

将 Revit 点转换为 Dynamo 点。

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

将 Revit XYZ 转换为 Dynamo 向量。

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

将 Revit UV 转换为 Dynamo UV。

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

将 Revit 平面转换为 Dynamo 平面。

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

将 Revit 变换转换为 Dynamo CoordinateSystem。

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

将 Revit XYZ 点列表转换为 Dynamo 点列表。

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### 使用 Revit 到原型类型的示例

此示例显示了一种快捷方法，用于使用 .ToPoint (XYZ) 方法将 Revit XYZ 转换为 Dynamo 点。 

![将 Revit XYZ 转换为 Dynamo Point.ByCoordinates](Images/revit-xyz-to-dynamo-point.png)

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

### 度和弧度

#### ToRadians

将度转换为弧度。

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

将弧度转换为度。

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### 度和弧度的使用示例

此示例显示了一种快捷方法，用于使用 .ToRadians 方法从度转换为弧度。 

![将度转换为弧度](Images/degrees-to-radians.png)

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
### X 和 UZ

#### GetPerpendicular (XYZ)

此方法返回给定 `XYZ` 向量的垂直 `XYZ` 向量。

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

此方法返回给定 Dynamo `Vector` 的垂直 Dynamo `Vector`。

` public static Vector GetPerpendicular(this Vector vector)`

#### X 和 UZ 的使用示例

此示例显示了一种快捷方法，用于使用 .GetPerpendicular 方法获取输入向量的垂直向量。 

![获取垂直向量](Images/get-perpendicular-vector.png)

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
