# 為 Dynamo for Revit 開發

## 使用 `GeometryPrimitiveConverter.cs` 中的方法

DynamoRevit 程式碼資源庫中的 [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) 類別提供各種方法，在 Revit 和 Dynamo 幾何類型之間進行轉換。在與 Revit 模型互動的 Dynamo 腳本中處理幾何圖形時，這些方法非常有用。

### 方法品類

`GeometryPrimitiveConverter.cs` 中的方法可分為四個主要品類：

1. **Proto 轉換為 Revit 類型**：將 Dynamo (Proto) 類型轉換為 Revit 類型的方法。
2. **Revit 轉換為 Proto 類型**：將 Revit 類型轉換為 Dynamo (Proto) 類型的方法。
3. **度與弳度**：在度與弳度之間轉換的方法。
4. **X 與 UZ**：處理取得互垂向量的方法。

### Proto 轉換為 Revit 類型

#### ToRevitBoundingBox

從 Dynamo 座標系統和兩個定義點 (最小值和最大值) 建立 Revit BoundingBoxXYZ。

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox( Autodesk.DesignScript.Geometry.CoordinateSystem cs, Autodesk.DesignScript.Geometry.Point minPoint, Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

將 Dynamo BoundingBox 轉換為 Revit BoundingBoxXYZ。

convertUnits 旗標 (預設為 True) 決定座標是否應從 Dynamo 的單位系統轉換為 Revit 的內部單位。

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

將 Dynamo Point 轉換為 Revit XYZ。

convertUnits 旗標 (預設為 True) 會視需要轉換座標。

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

將 Dynamo Vector 轉換為 Revit XYZ。

請注意，convertUnits 旗標預設為 False，因為向量表示方向和大小，這通常不需要單位轉換。轉換可能會影響向量的方向和長度。

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

將 Dynamo Point 轉換為 Revit XYZ。

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

將 Dynamo Vector 轉換為 Revit XYZ。

請注意，convertUnits 旗標預設為 False，因為向量表示方向和大小，這通常不需要單位轉換。轉換可能會影響向量的方向和長度。

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

將 Dynamo CoordinateSystem 轉換為 Revit Transform。

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

將 Dynamo Plane 轉換為 Revit Plane。

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

將 Dynamo Point 物件的集合轉換為 Revit XYZ 集合。

傳回 XYZ 清單。`public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

傳回 XYZ 的陣列。`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

將 Dynamo Vector 物件的陣列轉換為 Revit XYZ 向量的陣列。

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

將倍精度值的陣列轉換為 Revit 的 DoubleArray。

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

將每個內部陣列都表示一對值 (U 和 V) 的二維陣列 (double[][]) 轉換為 Revit UV 物件的陣列。

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

將每個內部陣列都表示一對值 (U 和 V) 的二維陣列 (double[][]) 轉換為 Dynamo UV 物件的陣列。

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### 使用 Proto 轉換為 Revit 類型的範例

此範例示範使用 .ToXyz (Point) 方法將 Dynamo Point.ByCoordinates 快速簡單轉換為 Revit XYZ 的方法。

![將 Dynamo Point.ByCoordinates 轉換為 Revit XYZ](Images/dynamo-point-to-revit-xyz.png)

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

### Revit 轉換為 Proto 類型

#### ToProtoType (BoundingBox)

將 Revit BoundingBoxXYZ 轉換為 Dynamo BoundingBox。

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

將 Revit XYZ 轉換為 Dynamo Point。

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

將 Revit Point 轉換為 Dynamo Point。

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

將 Revit XYZ 轉換為 Dynamo Vector。

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

將 Revit UV 轉換為 Dynamo UV。

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

將 Revit Plane 轉換為 Dynamo Plane。

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

將 Revit Transform 轉換為 Dynamo CoordinateSystem。

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

將 Revit XYZ 點清單轉換為 Dynamo Point 清單。

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### 使用 Revit 轉換為 Proto 類型範例

此範例示範使用 .ToPoint (XYZ) 方法將 Revit XYZ 快速簡單轉換為 Dynamo Point 的方法。

![將 Revit XYZ 轉換為 Dynamo Point.ByCoordinates](Images/revit-xyz-to-dynamo-point.png)

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

### 度與弳度

#### ToRadians

將度轉換為弳度。

`public static double ToRadians(this double degrees) { return degrees * Math.PI / 180.0; }`

#### ToDegrees

將弳度轉換為度。

`public static double ToDegrees(this double degrees) { return degrees * 180.0 / Math.PI; }`

#### 度與弳度的範例用法

此範例示範使用 .ToRadians 方法將度快速簡單轉換為弳度的方法。

![度轉換為弳度](Images/degrees-to-radians.png)

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

### X 與 UZ

#### GetPerpendicular (XYZ)

此方法會傳回與給定 `XYZ` 向量互垂的 `XYZ` 向量。

`public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

此方法會傳回與給定 Dynamo `Vector` 互垂的 Dynamo `Vector`。

`public static Vector GetPerpendicular(this Vector vector)`

#### X 與 UZ 的範例用法

此範例示範使用 .GetPerpendicular 方法快速簡單取得與輸入向量互垂之向量的方法。

![取得互垂向量](Images/get-perpendicular-vector.png)

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
