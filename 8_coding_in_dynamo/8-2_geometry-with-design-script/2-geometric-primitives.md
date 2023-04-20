# Geometrische Grundkörper

### Koordinatensystem

Zwar ist Dynamo in der Lage, eine Reihe komplexer geometrischer Formen zu erstellen, jedoch bilden einfache geometrische Grundkörper die Grundlage jedes computergestützten Entwurfs: entweder als direkter Ausdruck in der endgültigen Entwurfsform oder als Gerüst für die Erstellung einer komplexeren Geometrie.

Ein Koordinatensystem ist streng genommen kein Geometrieobjekt, dient jedoch als wichtiges Hilfsmittel zum Erstellen von Geometrie. Ein CoordinateSystem-Objekt verfolgt Positions- und Geometrietransformationen wie Drehen, Scheren und Skalieren.

Zum Erstellen eines an einem Punkt mit x = 0, y = 0, z = 0 zentrierten Koordinatensystems ohne Rotations-, Skalierungs- oder Schertransformationen kann einfach der Konstruktor Identity aufgerufen werden:

![](../images/8-2/2/GeometricPrimitives\_01.png)

```js
// create a CoordinateSystem at x = 0, y = 0, z = 0,
// no rotations, scaling, or sheering transformations

cs = CoordinateSystem.Identity();
```

Koordinatensysteme mit geometrischen Transformationen gehen über den Umfang dieses Kapitels hinaus, auch wenn Sie mit einem anderen Konstruktor, namentlich _CoordinateSystem.ByOriginVectors_, ein Koordinatensystem an einem bestimmten Punkt erstellen können:

![](../images/8-2/2/GeometricPrimitives\_02.png)

```js
// create a CoordinateSystem at a specific location,
// no rotations, scaling, or sheering transformations
x_pos = 3.6;
y_pos = 9.4;
z_pos = 13.0;

origin = Point.ByCoordinates(x_pos, y_pos, z_pos);
identity = CoordinateSystem.Identity();

cs = CoordinateSystem.ByOriginVectors(origin,
    identity.XAxis, identity.YAxis, identity.ZAxis);
```

### Punkt

Der einfachste geometrische Grundkörper ist ein Punkt, der eine nulldimensionale Position im dreidimensionalen Raum darstellt. Wie bereits erwähnt gibt es verschiedene Möglichkeiten zum Erstellen eines Punkts in einem bestimmten Koordinatensystem: _Point.ByCoordinates_ erstellt einen Punkt mithilfe der angegebenen Koordinaten x, y und z, _Point.ByCartesianCoordinates_ erstellt einen Punkt mithilfe der angegebenen Koordinaten x, y und z in einem bestimmten Koordinatensystem, _Point.ByCylindricalCoordinates_ erstellt einen Punkt, der auf einem Zylinder mit angegebenen Werten für Radius, Drehwinkel und Höhe liegt, und _Point.BySphericalCoordinates_ erstellt einen Punkt auf einer Kugel mit einem angegebenen Radius und zwei Drehwinkeln.

Dieses Beispiel zeigt Punkte, die in unterschiedlichen Koordinatensystemen erstellt wurden:

![](../images/8-2/2/GeometricPrimitives\_03.png)

```js
// create a point with x, y, and z coordinates
x_pos = 1;
y_pos = 2;
z_pos = 3;

pCoord = Point.ByCoordinates(x_pos, y_pos, z_pos);

// create a point in a specific coordinate system
cs = CoordinateSystem.Identity();
pCoordSystem = Point.ByCartesianCoordinates(cs, x_pos,
    y_pos, z_pos);

// create a point on a cylinder with the following
// radius and height
radius = 5;
height = 15;
theta = 75.5;

pCyl = Point.ByCylindricalCoordinates(cs, radius, theta,
    height);

// create a point on a sphere with radius and two angles

phi = 120.3;

pSphere = Point.BySphericalCoordinates(cs, radius,
    theta, phi);
```

### Linie 

Der nächsthöherdimensionale Dynamo-Grundkörper ist ein Liniensegment, das eine unendliche Anzahl von Punkten zwischen zwei Endpunkten darstellt. Linien können erstellt werden, indem Sie die beiden Grenzpunkte mit dem Konstruktor _Line.ByStartPointEndPoint_ explizit angeben, oder durch Angabe eines Startpunkts, einer Richtung und Länge in dieser Richtung mittels _Line.ByStartPointDirectionLength_.

![](../images/8-2/2/GeometricPrimitives\_04.png)

```js
p1 = Point.ByCoordinates(-2, -5, -10);
p2 = Point.ByCoordinates(6, 8, 10);

// a line segment between two points
l2pts = Line.ByStartPointEndPoint(p1, p2);

// a line segment at p1 in direction 1, 1, 1 with
// length 10
lDir = Line.ByStartPointDirectionLength(p1,
    Vector.ByCoordinates(1, 1, 1), 10);
```

### 3D-Grundkörper - Quader, Kegel, Zylinder, Kugel usw.

Dynamo verfügt über Objekte, die grundlegende Typen geometrischer Grundkörper in drei Dimensionen darstellen: Quader, erstellt mit _Cuboid.ByLengths_; Kegel, erstellt mit _Cone.ByPointsRadius_ und _Cone.ByPointsRadii_; Zylinder, erstellt mit _Cylinder.ByRadiusHeight_ sowie Kugeln, erstellt mit _Sphere.ByCenterPointRadius_.

![](../images/8-2/2/GeometricPrimitives\_05.png)

```js
// create a cuboid with specified lengths
cs = CoordinateSystem.Identity();

cub = Cuboid.ByLengths(cs, 5, 15, 2);

// create several cones
p1 = Point.ByCoordinates(0, 0, 10);
p2 = Point.ByCoordinates(0, 0, 20);
p3 = Point.ByCoordinates(0, 0, 30);

cone1 = Cone.ByPointsRadii(p1, p2, 10, 6);
cone2 = Cone.ByPointsRadii(p2, p3, 6, 0);

// make a cylinder
cylCS = cs.Translate(10, 0, 0);

cyl = Cylinder.ByRadiusHeight(cylCS, 3, 10);

// make a sphere
centerP = Point.ByCoordinates(-10, -10, 0);

sph = Sphere.ByCenterPointRadius(centerP, 5);
```
