# Operaciones booleanas geométricas

Los métodos _Intersect_, _Trim_ y _SelectTrim_ se utilizan principalmente en geometrías dimensionales inferiores como puntos, curvas y superficies. Por otra parte, la geometría sólida cuenta con un conjunto adicional de métodos para modificar la forma después de su creación, tanto sustrayendo material de forma similar a _Trim_ como combinando elementos para formar un todo mayor.

### Unión

El método _Union_ utiliza dos objetos sólidos para crear un único objeto sólido a partir del espacio cubierto por ambos objetos. El espacio solapado entre los objetos se combina en la forma final. Este ejemplo combina una esfera y un cubo en una forma de esfera-cubo sólida:

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### Diferencia

El método _Difference_, al igual que _Trim_, resta el contenido del sólido de la herramienta de entrada del sólido base. En este ejemplo, se extrae una pequeña hendidura de una esfera:

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### Intersecar

El método _Intersect_ devuelve el sólido solapado entre dos entradas sólidas. En el siguiente ejemplo, _Difference_ se ha cambiado a _Intersect_ y el sólido resultante es el vacío que falta que se ha eliminado inicialmente:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
