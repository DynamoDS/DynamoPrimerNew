# Booleovské operace geometrií

Metody _Intersect_, _Trim_ a _SelectTrim_ se používají zejména u méněrozměrných geometrií, například u bodů, křivek a ploch. Geometrie těles obsahují další sadu metod, které umožňují upravovat jejich tvar, například odebráním materiálu podobně jako u metody _Trim_, nebo prvky kombinovat a vytvářet tak větší celky.

### Sjednocení

Metoda _Union_ přijímá dvě tělesa a z prostoru, který tyto objekty zaujímají, vytváří jedno těleso. Překrývající se prostor mezi objekty se zkombinuje do konečného tvaru. Tento příklad kombinuje kouli a kvádr do jednoho tvaru:

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### Rozdíl

Metoda _Difference_ funguje podobně jako metoda _Trim_, odečítá obsah vstupního tělesa od základního tělesa. V tomto příkladu odřízneme od koule malý kus:

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### Průnik

Metoda _Intersect_ vrací těleso tvořené překrývajícím se prostorem dvou vstupních těles. V následujícím příkladu je metoda _Difference_ změněna na _Intersect_, výsledným tělesem je kus, který byl v předchozím příkladu odříznut:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
