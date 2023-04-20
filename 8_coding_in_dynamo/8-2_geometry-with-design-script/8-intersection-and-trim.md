# 相交和修剪

目前，许多示例都关注从较少维的对象构造较多维的几何体。相交方法允许此较高维度的几何图形生成较低维度的对象，而“修剪”和“选择修剪”命令允许脚本在创建几何形状后对其进行大量修改。

_Intersect_ 方法在 Dynamo 中的所有几何图形上定义，这意味着理论上，任何几何图形都可以与任何其他几何图形相交。通常，由于结果对象将始终是输入点本身，因此某些交点没有意义（例如涉及点的交点）。下图概述了对象之间可能存在的交点组合。下图概述了各种相交操作的结果：

### **交集**

| _其中：_     | 曲面 | 曲线 | 平面        | 实体   |
| ----------- | ------- | ----- | ------------ | ------- |
| **曲面** | 曲线   | 点 | 点，曲线 | 曲面 |
| **曲线**   | 点   | 点 | 点        | 曲线   |
| **平面**   | 曲线   | 点 | 曲线        | 曲线   |
| **实体**   | 曲面 | 曲线 | 曲线        | 实体   |

下面非常简单的示例演示了平面与 NurbsSurface 的交集。该交集会生成 NurbsCurve 数组，可像使用任何其他 NurbsCurve 一样使用。

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

_Trim_ 方法与“Intersect”方法非常相似，因为它几乎为每个几何图形都定义了该方法。但是，与 _Intersect_ 相比，_Trim_ 存在更多限制。

### **修剪**

|             | _使用：_点 | 曲线 | 平面 | 曲面 | 实体 |
| ----------- | -------------- | ----- | ----- | ------- | ----- |
| _开：_曲线 | 是            | 否    | 否    | 否      | 否    |
| 多边形     | -              | 否    | 是   | 否      | 否    |
| 曲面     | -              | 是   | 是   | 是     | 是   |
| 实体       | -              | -     | 是   | 是     | 是   |

关于 _Trim_ 方法需要注意的是，需要“选择”点、确定要丢弃哪些几何图形的点以及要保留哪些部分。Dynamo 会查找并放弃与选择点最近的已修剪几何图形。

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
