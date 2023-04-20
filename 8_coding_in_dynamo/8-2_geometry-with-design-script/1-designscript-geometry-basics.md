# Geometria DesignScript — podstawy

### Punkt

Najprostszym obiektem geometrycznym w standardowej bibliotece geometrii dodatku Dynamo jest punkt. Cała geometria jest tworzona za pomocą specjalnych funkcji zwanych konstruktorami, które zwracają nowe wystąpienia danego typu geometrii. W dodatku Dynamo konstruktory zaczynają się od nazwy typu obiektu, w tym przypadku Point (punkt), a następnie podawana jest metoda konstrukcji. Aby utworzyć trójwymiarowy punkt określony przez współrzędne kartezjańskie x, y i z, użyj konstruktora _ByCoordinates_ („na podstawie współrzędnych”):

![](../images/8-2/1/GeometryBasics\_01.png)

```js
// create a point with the following x, y, and z
// coordinates:
x = 10;
y = 2.5;
z = -6;

p = Point.ByCoordinates(x, y, z);
```

Konstruktory w dodatku Dynamo są zazwyczaj oznaczone prefiksem „_By_”. Wywołanie takiej funkcji zwraca nowo utworzony obiekt danego typu. Nowo utworzony obiekt jest przechowywany w zmiennej o nazwie podanej po lewej stronie znaku równości.

Większość obiektów ma wiele różnych konstruktorów. Można na przykład użyć konstruktora _BySphericalCoordinates_, aby utworzyć punkt leżący na sferze, określony przez promień sfery, kąt pierwszego obrotu i kąt drugiego obrotu (określone w stopniach):

![](../images/8-2/1/GeometryBasics\_02.png)

```js
// create a point on a sphere with the following radius,
// theta, and phi rotation angles (specified in degrees)
radius = 5;
theta = 75.5;
phi = 120.3;
cs = CoordinateSystem.Identity();

p = Point.BySphericalCoordinates(cs, radius, theta,
    phi);
```

### Od punktu do linii

Punkty mogą być używane do tworzenia geometrii wyższych wymiarów, np. linii. Za pomocą konstruktora _ByStartPointEndPoint_ można utworzyć obiekt linii między dwoma punktami:

![](../images/8-2/1/GeometryBasics\_03.png)

```js
// create two points:
p1 = Point.ByCoordinates(3, 10, 2);
p2 = Point.ByCoordinates(-15, 7, 0.5);

// construct a line between p1 and p2
l = Line.ByStartPointEndPoint(p1, p2);
```

### Od linii do powierzchni

Podobnie linie mogą być używane do tworzenia geometrii powierzchni wyższych wymiarów, na przykład za pomocą konstruktora _Loft_, który wykorzystuje szereg linii lub krzywych i interpoluje między nimi powierzchnię.

![](../images/8-2/1/GeometryBasics\_04.png)

```js
// create points:
p1 = Point.ByCoordinates(3, 10, 2);
p2 = Point.ByCoordinates(-15, 7, 0.5);

p3 = Point.ByCoordinates(5, -3, 5);
p4 = Point.ByCoordinates(-5, -6, 2);

p5 = Point.ByCoordinates(9, -10, -2);
p6 = Point.ByCoordinates(-11, -12, -4);

// create lines:
l1 = Line.ByStartPointEndPoint(p1, p2);
l2 = Line.ByStartPointEndPoint(p3, p4);
l3 = Line.ByStartPointEndPoint(p5, p6);

// loft between cross section lines:
surf = Surface.ByLoft([l1, l2, l3]);
```

### Od powierzchni do bryły

Także powierzchnie mogą być używane do tworzenia geometrii brył wyższych wymiarów, na przykład przez pogrubienie powierzchni o określoną odległość. Do wielu obiektów są dołączone funkcje zwane metodami, które umożliwiają programistom wykonywanie poleceń na tych konkretnych obiektach. Wspólne dla wszystkich elementów geometrii są metody _Translate_ i _Rotate_, które odpowiednio przekształcają (przesuwają) i obracają geometrię o określoną wartość. Powierzchnie mają metodę _Thicken_, która pobiera pojedyncze dane wejściowe: liczbę określającą nową grubość powierzchni.

![](../images/8-2/1/GeometryBasics\_05.png)

```js
p1 = Point.ByCoordinates(3, 10, 2);
p2 = Point.ByCoordinates(-15, 7, 0.5);

p3 = Point.ByCoordinates(5, -3, 5);
p4 = Point.ByCoordinates(-5, -6, 2);

l1 = Line.ByStartPointEndPoint(p1, p2);
l2 = Line.ByStartPointEndPoint(p3, p4);

surf = Surface.ByLoft([l1, l2]);

// true indicates to thicken both sides of the Surface:
solid = surf.Thicken(4.75, true);
```

### Przecięcie

Polecenia _Intersection_ mogą wyodrębniać geometrię niższych wymiarów z obiektów wyższych wymiarów. Ta wyodrębniona geometria niższych wymiarów może stanowić podstawę dla geometrii wyższych wymiarów w cyklicznym procesie tworzenia, wyodrębniania i ponownego tworzenia geometrii. W tym przykładzie używamy wygenerowanej bryły do utworzenia powierzchni, a następnie używamy tej powierzchni do utworzenia krzywej.

![](../images/8-2/1/GeometryBasics\_06.png)

```js
p1 = Point.ByCoordinates(3, 10, 2);
p2 = Point.ByCoordinates(-15, 7, 0.5);

p3 = Point.ByCoordinates(5, -3, 5);
p4 = Point.ByCoordinates(-5, -6, 2);

l1 = Line.ByStartPointEndPoint(p1, p2);
l2 = Line.ByStartPointEndPoint(p3, p4);

surf = Surface.ByLoft([l1, l2]);

solid = surf.Thicken(4.75, true);

p = Plane.ByOriginNormal(Point.ByCoordinates(2, 0, 0),
    Vector.ByCoordinates(1, 1, 1));

int_surf = solid.Intersect(p);

int_line = int_surf.Intersect(Plane.ByOriginNormal(
    Point.ByCoordinates(0, 0, 0),
    Vector.ByCoordinates(1, 0, 0)));
```
