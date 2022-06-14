# Перенос, поворот и другие преобразования

Определенные геометрические объекты можно создавать путем непосредственного указания их координат по осям X, Y и Z в трехмерном пространстве. Однако в большинстве случаев итоговое положение геометрии задается путем преобразований, применяемых либо к самому объекту, либо к его базовой системе координат (CoordinateSystem).

### Перемещение

Простейшим геометрическим преобразованием является перенос, в результате которого объект перемещается на заданное число делений вдоль осей X, Y и Z.

![](../images/8-2/5/Transformations\_01.png)

```js
// create a point at x = 1, y = 2, z = 3
p = Point.ByCoordinates(1, 2, 3);

// translate the point 10 units in the x direction,
// -20 in y, and 50 in z
// p2’s new position is x = 11, y = -18, z = 53
p2 = p.Translate(10, -20, 50);
```

### Поворот

Любой объект в Dynamo можно перенести, просто добавив метод _.Translate_ после имени объекта, однако более сложные преобразования требуют изменения базовой системы координат объекта (CoordinateSystem). Например, чтобы повернуть объект на 45 градусов вокруг оси X, потребуется заменить существующую систему координат (без поворота) на систему координат, повернутую на 45 градусов вокруг оси X с помощью метода _.Transform_.

![](../images/8-2/5/Transformations\_02.png)

```js
cube = Cuboid.ByLengths(CoordinateSystem.Identity(),
    10, 10, 10);

new_cs = CoordinateSystem.Identity();
new_cs2 = new_cs.Rotate(Point.ByCoordinates(0, 0),
    Vector.ByCoordinates(1,0,0.5), 25);

// get the existing coordinate system of the cube
old_cs = CoordinateSystem.Identity();

cube2 = cube.Transform(old_cs, new_cs2);
```

### Масштабировать

Помимо операций переноса и поворота к объектам CoordinateSystem также можно применять операции масштабирования и сдвига. Для масштабирования объектов CoordinateSystem используется метод _.Scale_.

![](../images/8-2/5/Transformations\_03.png)

```js
cube = Cuboid.ByLengths(CoordinateSystem.Identity(),
    10, 10, 10);

new_cs = CoordinateSystem.Identity();
new_cs2 = new_cs.Scale(20);

old_cs = CoordinateSystem.Identity();

cube2 = cube.Transform(old_cs, new_cs2);
```

Для сдвига объекта CoordinateSystem используется конструктор CoordinateSystem, в который нужно ввести неортогональные векторы.

![](../images/8-2/5/Transformations\_04.png)

```js
new_cs = CoordinateSystem.ByOriginVectors(
    Point.ByCoordinates(0, 0, 0),
	Vector.ByCoordinates(-1, -1, 1),
	Vector.ByCoordinates(-0.4, 0, 0));

old_cs = CoordinateSystem.Identity();

cube = Cuboid.ByLengths(CoordinateSystem.Identity(),
    5, 5, 5);

new_curves = cube.Transform(old_cs, new_cs);
```

По сравнению с поворотом и переносом операции масштабирования и сдвига являются более сложными и поддерживаются не всеми объектами Dynamo. Объекты Dynamo, которые поддерживают операции изменения масштаба и сдвига системы координат, перечислены в следующей таблице.

| Класс | Объект CoordinateSystem с измененным масштабом | Объект CoordinateSystem со сдвигом |
| ------------ | ------------------------------------- | ------------------------ |
| Дуга | Нет | Нет  |
| Объект NurbsCurve | Да | Да |
| Объект NurbsSurface | Нет | Нет  |
| Окружность | Нет | Нет  |
| Отрезок | Да | Да |
| Плоскость | Нет | Нет  |
| Точка | Да | Да |
| Полигон | Нет | Нет  |
| Тело | Нет | Нет  |
| Поверхность | Нет | Нет  |
| Текст | Нет | Нет  |
