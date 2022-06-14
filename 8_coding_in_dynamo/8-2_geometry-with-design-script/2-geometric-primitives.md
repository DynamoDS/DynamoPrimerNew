# Primitives géométriques

### Système de coordonnées

Bien que Dynamo soit capable de créer diverses formes géométriques complexes, les primitives géométriques simples constituent la base de toute conception informatique. Elles sont soit directement exprimées dans la forme finale de la conception, soit utilisées comme armature à partir de laquelle une géométrie plus complexe est générée.

Bien qu'il ne s'agisse pas d'une partie de la géométrie à strictement parler, le CoordinateSystem est un outil important pour la construction d'une géométrie. Un objet CoordinateSystem conserve une trace des transformations de position et géométriques, telles que la rotation, la transvection et la mise à l'échelle.

La création d'un CoordinateSystem centré sur un point avec x = 0, y = 0, z = 0, sans rotation ni transformation de transvection ni de mise à l'échelle, requiert simplement l'appel du constructeur Identity :

![](../images/8-2/2/GeometricPrimitives\_01.png)

```js
// create a CoordinateSystem at x = 0, y = 0, z = 0,
// no rotations, scaling, or sheering transformations

cs = CoordinateSystem.Identity();
```

Les CoordinateSystems avec transformations géométriques dépassent la portée de ce chapitre, bien qu'un autre constructeur vous permette de créer un système de coordonnées à un point spécifique, à savoir _CoordinateSystem.ByOriginVectors_ :

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

### Point

La primitive géométrique la plus simple est un point, représentant un emplacement à zéro dimension dans un espace tridimensionnel. Comme indiqué précédemment, il existe plusieurs méthodes pour créer un point dans un système de coordonnées particulier : _Point.ByCoordinates_ crée un point avec les coordonnées x, y et z spécifiées ; _Point.ByCartesianCoordinates_ crée un point avec les coordonnées x, y et z dans un système de coordonnées spécifique ; _Point.ByCylindricalCoordinates_ crée un point se trouvant sur un cylindre, avec rayon, angle de rotation et hauteur ; et _Point.BySphericalCoordinates_ crée un point situé sur une sphère avec un rayon et deux angles de rotation.

Cet exemple montre des points créés sur différents systèmes de coordonnées :

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

### Ligne&#x20;

La primitive Dynamo dimensionnelle supérieure suivante est un segment de ligne, représentant un nombre infini de points entre deux extrémités. Les lignes peuvent être créées en spécifiant explicitement les deux points de limite avec le constructeur _Line.ByStartPointEndPoint_, ou en spécifiant un point de départ, une direction et une longueur dans cette direction, _Line.ByStartPointDirectionLength_.

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

### Primitives 3D : cuboïde, cône, cylindre, sphère, etc.

Dynamo comporte des objets représentant les principaux types de primitives géométriques de base en trois dimensions : des cuboïdes créés avec _Cuboid.ByLengths_ ; des cônes créés avec _Cone.ByPointsRadius_ et _Cone.ByPointsRadii_ ; des cylindres créés avec _Cylindre.ByRadiusHeight_ ; et des sphères créées avec _Sphere.ByCenterPointRadius_.

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
