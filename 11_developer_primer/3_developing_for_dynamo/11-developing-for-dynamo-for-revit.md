# Developing For Dynamo for Revit

## Using Methods in `GeometryPrimitiveConverter.cs`

The [GeometryPrimitiveConverter](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodes/GeometryConversion/GeometryPrimitiveConverter.cs) class in DynamoRevit's code library provides various methods to convert between Revit and Dynamo geometric types. These methods are useful when working with geometry in Dynamo scripts that interact with Revit models.

### Method Categories

The methods in `GeometryPrimitiveConverter.cs` can be grouped into four main categories:

1. **Proto to Revit Types**: Methods that convert Dynamo (Proto) types to Revit types.
2. **Revit to Proto Types**: Methods that convert Revit types to Dynamo (Proto) types.
3. **Degrees & Radians**: Methods that convert between degrees and radians.
4. **X & UZ**: Methods that deal with getting perpendicular vectors. 

### Proto to Revit Types

#### ToRevitBoundingBox

Creates a Revit BoundingBoxXYZ from a Dynamo coordinate system and two defining points (minimum and maximum).

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitBoundingBox(
            Autodesk.DesignScript.Geometry.CoordinateSystem cs,
            Autodesk.DesignScript.Geometry.Point minPoint,
            Autodesk.DesignScript.Geometry.Point maxPoint, bool convertUnits = true)`

#### ToRevitType (BoundingBox)

Converts a Dynamo BoundingBox to a Revit BoundingBoxXYZ.

The convertUnits flag (defaulted to true) determines if the coordinates should be converted from Dynamo’s unit system to Revit’s internal units.

`public static Autodesk.Revit.DB.BoundingBoxXYZ ToRevitType(this Autodesk.DesignScript.Geometry.BoundingBox bb, bool convertUnits = true)`

#### ToRevitType (Point)

Converts a Dynamo Point to a Revit XYZ.

The convertUnits flag (defaulted to true) converts the coordinates if necessary.

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToRevitType (Vector)

Converts a Dynamo Vector to a Revit XYZ.

Notice that the convertUnits flag is defaulted to false because vectors represent direction and magnitude, which usually do not require unit conversion. Covnersion might effect the direction and length of the vector. 

`public static Autodesk.Revit.DB.XYZ ToRevitType(this Vector vec, bool convertUnits = false)`

#### ToXyz (Point)

Converts a Dynamo Point to a Revit XYZ.

`public static Autodesk.Revit.DB.XYZ ToXyz(this Autodesk.DesignScript.Geometry.Point pt, bool convertUnits = true)`

#### ToXyz (Vector)

Converts a Dynamo Vector to a Revit XYZ.

Notice that the convertUnits flag is defaulted to false because vectors represent direction and magnitude, which usually do not require unit conversion. Covnersion might effect the direction and length of the vector. 

`public static Autodesk.Revit.DB.XYZ ToXyz(this Vector vec, bool convertUnits = false)`

#### ToTransform

Converts a Dynamo CoordinateSystem to a Revit Transform.

`public static Autodesk.Revit.DB.Transform ToTransform(this CoordinateSystem cs, bool convertUnits = true)`

#### ToPlane

Converts a Dynamo Plane to a Revit Plane.

`public static Autodesk.Revit.DB.Plane ToPlane(this Autodesk.DesignScript.Geometry.Plane plane, bool convertUnits = true)`

#### ToXyzs (Points)

Converts collections of Dynamo Point objects into Revit XYZ collections.

Return a List of XYZs.
`public static List<XYZ> ToXyzs(this List<Autodesk.DesignScript.Geometry.Point> list, bool convertUnits = true)`

Return an array of XYZs.
`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Point[] list, bool convertUnits = true)`

#### ToXyzs (Vectors)

Converts an array of Dynamo Vector objects into an array of Revit XYZ vectors.

`public static XYZ[] ToXyzs(this Autodesk.DesignScript.Geometry.Vector[] list, bool convertUnits = false)`

#### ToDoubleArray

Converts an array of double values into Revit’s DoubleArray.

`public static DoubleArray ToDoubleArray(this double[] list)`

#### ToUvs

Converts a two-dimensional array (double[][]) where each inner array represents a pair of values (U and V) into an array of Revit UV objects.

`internal static Autodesk.Revit.DB.UV[] ToUvs(this double[][] uvArr)`

#### ToDSUvs

Converts a two-dimensional array (double[][]) where each inner array represents a pair of values (U and V) into an array of Dynamo UV objects.

`internal static Autodesk.DesignScript.Geometry.UV[] ToDSUvs(this double[][] uvArr)`

#### Example Using Proto To Revit Types

This example shows a quick and easy method to use the .ToXyz (Point) method to convert a Dynamo Point.ByCoordinates to a Revit XYZ. 

![Converting a Dynamo Point.ByCoordinates into a Revit XYZ](Images/dynamo-point-to-revit-xyz.png)

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



### Revit to Proto Types

#### ToProtoType (BoundingBox)

Converts a Revit BoundingBoxXYZ to a Dynamo BoundingBox.

`public static Autodesk.DesignScript.Geometry.BoundingBox ToProtoType(this Autodesk.Revit.DB.BoundingBoxXYZ xyz, bool convertUnits = true)`

#### ToPoint (XYZ)

Converts a Revit XYZ to a Dynamo Point.

`public static Autodesk.DesignScript.Geometry.Point ToPoint(this XYZ xyz, bool convertUnits = true)`

#### ToProtoType (Point)

Converts a Revit Point to a Dynamo Point.

`public static Autodesk.DesignScript.Geometry.Point ToProtoType(this Autodesk.Revit.DB.Point point, bool convertUnits = true)`

#### ToVector (XYZ)

Converts a Revit XYZ to a Dynamo Vector.

`public static Vector ToVector(this XYZ xyz, bool convertUnits = false)`

#### ToProtoType (UV)

Converts a Revit UV to a Dynamo UV.

`public static Autodesk.DesignScript.Geometry.UV ToProtoType(this Autodesk.Revit.DB.UV uv)`

#### ToPlane (Revit Plane)

Converts a Revit Plane to a Dynamo Plane.

`public static Autodesk.DesignScript.Geometry.Plane ToPlane(this Autodesk.Revit.DB.Plane plane, bool convertUnits = true)`

#### ToCoordinateSystem

Converts a Revit Transform to a Dynamo CoordinateSystem.

`public static CoordinateSystem ToCoordinateSystem(this Transform t, bool convertUnits = true)`

#### ToPoints

Converts a list of Revit XYZ points into a list of Dynamo Points.

`public static List<Autodesk.DesignScript.Geometry.Point> ToPoints(this List<XYZ> list, bool convertUnits = true)`

#### Example Using Revit To Proto Types

This example shows a quick and easy method to use the .ToPoint (XYZ) method to convert a Revit XYZ into a Dynamo Point. 

![Converting a Revit XYZ into a Dynamo Point.ByCoordinates](Images/revit-xyz-to-dynamo-point.png)

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

### Degrees & Radians

#### ToRadians

Converts degrees to radians.

`public static double ToRadians(this double degrees)
{
    return degrees * Math.PI / 180.0;
}`

#### ToDegrees

Converts radians to degrees.

`public static double ToDegrees(this double degrees)
{
    return degrees * 180.0 / Math.PI;
}`

#### Example Usage of Degrees & Radians

This example shows a quick and easy method to use the .ToRadians method to convert from degrees to radians. 

![Degrees to Radians](Images/degrees-to-radians.png)

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

This method returns a perpendicular `XYZ` vector to the given `XYZ` vector.

` public static XYZ GetPerpendicular(this XYZ xyz)`

#### GetPerpendicular (Vector)

This method returns a perpendicular Dynamo `Vector` to the given Dynamo `Vector`.

` public static Vector GetPerpendicular(this Vector vector)`

#### Example Usage of X & UZ

This example shows a quick and easy method to use the .GetPerpendicular method to get the perpendicular vector to an input vector. 

![Get Perpendicular Vector](Images/get-perpendicular-vector.png)

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
