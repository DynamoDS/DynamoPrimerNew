# Логические операции с геометрическими объектами

Методы _Intersect_, _Trim_ и _SelectTrim_ в основном используются при работе с простыми геометрическими объектами, такими как точки, кривые и поверхности. Для твердотельных геометрических объектов доступны дополнительные методы изменения формы после ее построения. Эти методы включают как удаление материала аналогично методу _Trim_, так и объединение нескольких элементов для получения единого большого элемента.

### Объединение

Метод _Union_ позволяет создать новый твердотельный объект на основе двух исходных объектов. Итоговый объект занимает в пространстве столько же места, сколько занимали оба исходных. Если объекты накладываются друг на друга в пространстве, то в итоговой форме накладывающиеся участки объединяются. В этом примере из сферы и кубоида путем объединения была получена единая кубо-сферическая твердотельная форма:

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### Разница

Метод _Difference_, аналогично методу _Trim_, позволяет удалить из базового тела материал, объем которого соответствует используемому на входе твердотельному инструменту. В этом примере в сфере был создан небольшой вырез:

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### Пересечение

Результатом использования метода _Intersect_ является тело, образованное наложением двух других тел. В следующем примере вместо метода _Difference_ был использован метод _Intersect_, в результате чего было получено тело, объем которого соответствует вырезу в предыдущем примере:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
