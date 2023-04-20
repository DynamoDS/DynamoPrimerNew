# Przecięcie i ucinanie

Wiele z dotychczasowych przykładów koncentrowało się na konstruowaniu geometrii wyższych wymiarów z obiektów niższych wymiarów. Metody przecięcia umożliwiają generowanie obiektów niższych wymiarów za pomocą geometrii wyższych wymiarów, natomiast polecenia ucinania i ucinania selektywnego umożliwiają skryptowi znaczne modyfikowanie form geometrycznych po ich utworzeniu.

Metoda _Intersect_ jest zdefiniowana dla wszystkich elementów geometrii w dodatku Dynamo, co oznacza, że teoretycznie każdy element geometrii może zostać przecięty za pomocą dowolnego innego elementu geometrii. Naturalnie niektóre przecięcia są bez znaczenia, na przykład przecięcia z punktami, ponieważ obiekt wynikowy będzie zawsze punktem wejściowym. Pozostałe możliwe kombinacje przecięć między obiektami zostały przedstawione w poniższym zestawieniu. W poniższej tabeli przedstawiono wyniki różnych operacji przecięcia:

### **Intersect**

| _Z:_     | Powierzchnia | Łuk | Płaszczyzna        | Bryła   |
| ----------- | ------- | ----- | ------------ | ------- |
| **Powierzchnia** | Krzywa   | Punkt | Punkt, krzywa | Powierzchnia |
| **Krzywa**   | Punkt   | Punkt | Punkt        | Krzywa   |
| **Płaszczyzna**   | Krzywa   | Punkt | Łuk        | Krzywa   |
| **Bryła**   | Powierzchnia | Łuk | Łuk        | Wypełnienie   |

Poniższy bardzo prosty przykład przedstawia przecięcie płaszczyzny z powierzchnią NurbsSurface. To przecięcie generuje szyk NurbsCurve, który może być używany jak każdy inny obiekt NurbsCurve.

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

Metoda _Trim_ jest bardzo podobna do metody Intersect pod tym względem, że jest zdefiniowana dla niemal każdego elementu geometrii. Jednak dla metody _Trim_ istnieje znacznie więcej ograniczeń niż dla metody _Intersect_.

### **Trim**

|             | _Za pomocą:_ punkt | Krzywa | Płaszczyzna | Powierzchnia | Bryła |
| ----------- | -------------- | ----- | ----- | ------- | ----- |
| _Na:_ krzywa | Tak            | Nie    | Nie    | Nie      | Nie    |
| Wielobok     | -              | Nie    | Tak   | Nie      | Nie    |
| Powierzchnia     | -              | Tak   | Tak   | Tak     | Tak   |
| Wypełnienie       | -              | -     | Tak   | Tak     | Tak   |

W przypadku metod _Trim_ warto pamiętać o wymaganiu punktu „wyboru”, określającego, która geometria zostanie odrzucona, a które elementy zostaną zachowane. Dodatek Dynamo znajduje i odrzuca uciętą geometrię najbliżej wybranego punktu.

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
