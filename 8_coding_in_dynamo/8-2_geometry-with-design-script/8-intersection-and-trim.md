# Intersección y recorte

Muchos de los ejemplos mostrados hasta ahora se han centrado en la creación de geometría dimensional superior a partir de objetos dimensionales inferiores. Los métodos de intersección permiten que esta geometría dimensional superior genere objetos dimensionales inferiores, mientras que los comandos de recorte y de selección de recorte permiten que el archivo de comandos modifique las formas geométricas una vez creadas.

El método _Intersect_ se define en todas las partes de la geometría de Dynamo, lo que significa que, en teoría, cualquier elemento de geometría se puede intersecar con cualquier otro. Naturalmente, algunas intersecciones no tienen sentido, como las intersecciones que involucran puntos, ya que el objeto resultante siempre será el propio punto de entrada. Las demás combinaciones posibles de intersecciones entre objetos se describen en el siguiente gráfico. En la siguiente tabla, se muestra el resultado de varias operaciones de intersección:

### **Intersecar**

| _Con:_     | Superficie | Curva | Plano        | Sólido   |
| ----------- | ------- | ----- | ------------ | ------- |
| **Superficie** | Curva   | Punto | Punto, curva | Superficie |
| **Curva**   | Punto   | Punto | Punto        | Curva   |
| **Plano**   | Curva   | Punto | Curva        | Curva   |
| **Sólido**   | Superficie | Curva | Curva        | Sólido   |

En el siguiente ejemplo muy sencillo, se muestra la intersección de un plano con una NurbsSurface. La intersección genera una matriz de NurbsCurves, que se puede utilizar como cualquier otra NurbsCurve.

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

El método _Trim_ es muy similar al método Intersect, ya que se define para casi cada elemento de geometría. Sin embargo, existen muchas más limitaciones en el método _Trim_ que en _Intersect_.

### **Recortar**

|             | _Uso:_ Punto | Curva | Plano | Superficie | Sólido |
| ----------- | -------------- | ----- | ----- | ------- | ----- |
| _En:_ Curva | Sí            | No    | No    | No      | No    |
| Polígono     | -              | No    | Sí   | No      | No    |
| Superficie     | -              | Sí   | Sí   | Sí     | Sí   |
| Sólido       | -              | -     | Sí   | Sí     | Sí   |

Algo que se debe tener en cuenta en los métodos _Trim_ es el requisito de un punto de "selección", un punto que determina la geometría que se descartará y las partes que se deben conservar. Dynamo busca y descarta la geometría recortada más cercana al punto de selección.

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
