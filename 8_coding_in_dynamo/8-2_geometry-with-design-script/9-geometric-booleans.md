# Geometryczne wartości logiczne

Metody _Intersect_, _Trim_ i _SelectTrim_ są używane przede wszystkim na geometriach niższych wymiarów, takich jak punkty, krzywe i powierzchnie. Natomiast geometria brył ma dodatkowy zestaw metod do modyfikowania postaci po jej utworzeniu, zarówno przez odjęcie materiału w sposób podobny do zastosowania metody _Trim_, jak i przez połączenie elementów w celu utworzenia większej części.

### Union

Metoda _Union_ pobiera dwa obiekty brył i tworzy pojedynczy obiekt bryły z przestrzeni objętej tymi dwoma obiektami. Przestrzeń wspólna między obiektami jest łączona w postać końcową. W tym przykładzie kula i prostopadłościan łączą się w pojedynczy kształt bryły złożonej z kuli i sześcianu:

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### Difference

Metoda _Difference_, podobnie jak metoda _Trim_, odejmuje zawartość bryły wejściowej od bryły bazowej. W tym przykładzie tworzymy małe wcięcie w sferze:

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### Intersect

Metoda _Intersect_ zwraca bryłę wspólną między dwiema bryłami wejściowymi. W poniższym przykładzie metodę _Difference_ zmieniono na metodę _Intersect_, a wynikowa bryła to brakująca wcześniej wycięta pustka:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
