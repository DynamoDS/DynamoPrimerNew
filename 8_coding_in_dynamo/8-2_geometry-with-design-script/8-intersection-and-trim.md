# 交差およびトリム

これまでの多くの例では、低い次元のオブジェクトから高い次元のジオメトリを構築することに注目してきました。交差メソッドを使用すると、この高い次元のジオメトリから低い次元のオブジェクトを生成でき、一方でトリム コマンドを選択すると、ジオメトリ形状を作成した後にスクリプトによって大幅に修正できます。

_Intersect_ メソッドは Dynamo のジオメトリのすべてのピース上で定義されます。つまり、理論的にはジオメトリのどのピースもジオメトリの他の任意のピースと交差できます。点に関する交差では結果として得られるオブジェクトは常に入力された点そのものとなるなど、意味のない交差も当然あります。オブジェクト間での交差のその他の可能な組み合わせについて、次の表で概要を示します。次の表はさまざまな交差の操作の結果の概要を示しています。

### **交差**

| _対象:_    | サーフェス | 曲線 | 平面        | ソリッド   |
| ----------- | ------- | ----- | ------------ | ------- |
| **サーフェス** | 曲線   | 点 | 点、曲線 | サーフェス |
| **曲線**   | 点   | 点 | 点        | 曲線   |
| **平面**   | 曲線   | 点 | 曲線        | 曲線   |
| **ソリッド**   | サーフェス | 曲線 | 曲線        | ソリッド   |

次の非常に簡単な例では、平面と NURBS 曲面の交差を示しています。交差によって NURBS 曲線の配列が生成され、これを他の NURBS 曲線のように使用できます。

![](../images/8-2/8/IntersectionAndTrim\_01.png)

```js
// python_points_5 is a set of Points generated with
// a Python script found in Chapter 12, Section 10

surf = NurbsSurface.ByPoints(python_points_5, 3, 3);

WCS = CoordinateSystem.Identity();

pl = Plane.ByOriginNormal(WCS.Origin.Translate(0, 0,
    0.5), WCS.ZAxis);

// intersect surface, generating three closed curves
crvs = surf.Intersect(pl);

crvs_moved = crvs.Translate(0, 0, 10);
```

_Trim_ メソッドは Intersect メソッドに非常によく似ており、ジオメトリのほぼすべてのピースに対して定義されます。ただし _Trim_ には _Intersect_ よりもはるかに多くの制限があります。

### **トリム**

|             | _使用:_ 点 | 曲線 | 平面 | サーフェス | ソリッド |
| ----------- | -------------- | ----- | ----- | ------- | ----- |
| _対象:_ 曲線 | 可            | 不可    | 不可    | 不可      | 不可    |
| ポリゴン     | -              | 不可    | 可   | 不可      | 不可    |
| サーフェス     | -              | 可   | 可   | 可     | 可   |
| ソリッド       | -              | -     | 可   | 可     | 可   |

_Trim_ メソッドについては、「選択」点が必要であることに注意が必要です。この点によってどのジオメトリを破棄してどの部分を保持するかが決まります。Dynamo は、選択点に最も近いトリムされたジオメトリを見つけて破棄します。

![](../images/8-2/8/IntersectionAndTrim\_02.png)

```js
// python_points_5 is a set of Points generated with
// a Python script found in Chapter 12, Section 10

surf = NurbsSurface.ByPoints(python_points_5, 3, 3);

tool_pts = Point.ByCoordinates((-10..20..10)<1>,
    (-10..20..10)<2>, 1);

tool = NurbsSurface.ByPoints(tool_pts);

pick_point = Point.ByCoordinates(8, 1, 3);

result = surf.Trim(tool, pick_point);
```
