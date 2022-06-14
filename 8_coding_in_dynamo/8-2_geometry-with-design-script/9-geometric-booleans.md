# ジオメトリのブール演算

_Intersect_、_Trim_、および _SelectTrim_ は、点、曲線、サーフェスなどの低い次元のジオメトリに主に使用されます。一方、ソリッド ジオメトリには、_Trim_ と同様の方法でマテリアルを取り除くことおよび要素を結合して全体を大きくすることの両方によって構築後に形状を修正するための、一連のメソッドが追加されています。

### 論理和

_Union_ メソッドでは、2 つのソリッド オブジェクトが使用され、両方のオブジェクトによってカバーされる空間から単一のソリッド オブジェクトが作成されます。オブジェクト間で重複する空間は最終形状に結合されます。この例では、球と直方体が結合して単一のソリッドの球-立方体形状となります。

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### 差の絶対値

_Difference_ メソッドでは、_Trim_ のように、ベースとなるソリッドから入力されたツールとなるソリッドの内容が取り除かれます。この例では、球から小さなくぼみが削り出されています。

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### [交差]

_Intersect_ メソッドでは、2 つのソリッド入力間の重複するソリッドが生成されます。次の例では、_Difference_ が _Intersect_ に変更され、結果として得られるソリッドは、最初に削り出されていた空間がなくなっています。

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
