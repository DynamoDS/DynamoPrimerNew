# DesignScript ジオメトリの基本

### 点

Dynamo の標準のジオメトリ ライブラリで最も単純なジオメトリ オブジェクトは、点です。すべてのジオメトリはコンストラクタという特別な関数を使用して作成され、各関数はその特定のジオメトリ タイプの新しいインスタンスを返します。Dynamo では、コンストラクタの名前はオブジェクトのタイプ(この場合は Point)で始まり、構築メソッドが続きます。X、Y、Z のデカルト座標で指定された 3 次元の点を作成するには、次のように _ByCoordinates_ コンストラクタを使用します。

![](../images/8-2/1/GeometryBasics\_01.png)

```js
// create a point with the following x, y, and z
// coordinates:
x = 10;
y = 2.5;
z = -6;

p = Point.ByCoordinates(x, y, z);
```

Dynamo では、コンストラクタは通常、接頭表記「_By_」を使用して指定され、これらの関数を呼び出すと、そのタイプの新しく作成されたオブジェクトを返します。この新しく作成されたオブジェクトは、等号の左側にある名前の変数に格納されます。

ほとんどのオブジェクトには、多種多様なコンストラクタがあり、次のように _BySphericalCoordinates_ コンストラクタを使用すると、球の半径、1 番目の回転角度、2 番目の回転角度(度単位)を指定して、球体上の点を作成できます。

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

### 点から線分へ

点を使用して、線分などのより高い次元のジオメトリを構築できます。次のように _ByStartPointEndPoint_ コンストラクタを使用すると、2 つの点の間に Line オブジェクトを作成できます。

![](../images/8-2/1/GeometryBasics\_03.png)

```js
// create two points:
p1 = Point.ByCoordinates(3, 10, 2);
p2 = Point.ByCoordinates(-15, 7, 0.5);

// construct a line between p1 and p2
l = Line.ByStartPointEndPoint(p1, p2);
```

### 線分からサーフェスへ

同様に、線分を使用してより高い次元のサーフェス ジオメトリを作成できます。たとえば、_Loft_ コンストラクタを使用すると、一連の線分または曲線を使用してその間にサーフェスを補間します。

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

### サーフェスからソリッドへ

サーフェスを使用してより高い次元のソリッド ジオメトリを作成することもできます。たとえば指定した距離だけサーフェスに厚みを付けてソリッドを作成します。多くのオブジェクトには、メソッドと呼ばれる関数がアタッチされ、これを使用することでプログラマはその特定のオブジェクトに関するコマンドを実行できます。ジオメトリのすべてのピースに共通するメソッドには、_Translate_ および _Rotate_ があり、それぞれ指定した量だけジオメトリを移動および回転します。サーフェスには _Thicken_ メソッドがあり、1 つの数字を入力してサーフェスの新しい厚みを指定します。

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

### 交差

_Intersection_ コマンドによって、高い次元のオブジェクトから低い次元のジオメトリを抽出できます。この抽出された低い次元のジオメトリは、ジオメトリの作成、抽出、再作成を繰り返すことで、高い次元のジオメトリの基盤を形成できます。この例では、生成されたソリッドを使用してサーフェスを作成し、そのサーフェスを使用して曲線を作成します。

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
