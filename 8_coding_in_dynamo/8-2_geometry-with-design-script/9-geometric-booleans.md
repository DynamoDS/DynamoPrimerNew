# 几何布尔

_Intersect_、_Trim_ 和 _SelectTrim_ 主要用于较少维的几何体，例如点、曲线和曲面。另一方面，实体几何体还有一组附加方法用于在构造后修改形状，方法是以与 _Trim_ 类似的方式减去材质，并将图元合并到一起以形成更大的整体。

### 活接头

_Union_ 方法可获取两个实体对象，并在两个对象覆盖的空间之外创建单个实体对象。对象之间的重叠空间将合并为最终形式。本例将球体和立方体合并为单个实体球体-立方体形状：

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### 差异

_Difference_ 方法类似 _Trim_，从基础实体中减去输入工具实体的内容。在本例中，我们从球体中穿凿出一个小凹穴：

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### 交集

_Intersect_ 方法将返回两个实体输入之间的重叠实体。在以下示例中，_Difference_ 已更改为 _Intersect_，并且生成的实体是最初穿凿的缺失空心：

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
