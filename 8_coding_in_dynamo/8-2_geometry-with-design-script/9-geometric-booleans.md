# Operazioni booleane geometriche

_Intersect_, _Trim_ e _SelectTrim_ vengono utilizzati principalmente nella geometria dimensionale minore come punti, curve e superfici. La geometria solida, invece, presenta un insieme aggiuntivo di metodi per la modifica della forma dopo la costruzione, sottraendo il materiale in modo simile a _Trim_ e combinando gli elementi insieme per formare un intero più grande.

### Unione

Il metodo _Unione_ utilizza due oggetti solidi e crea un singolo oggetto solido partendo dallo spazio coperto da entrambi gli oggetti. Lo spazio sovrapposto tra oggetti viene combinato nella forma finale. In questo esempio si combina una sfera e un cuboide in un'unica forma solida sfera-cubo:

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### Differenza

Il metodo _Difference_, come _Trim_, sottrae il contenuto del solido strumento di input dal solido di base. In questo esempio, viene ritagliata una piccola rientranza da una sfera:

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### Interseca

Il metodo _Intersect_ restituisce il solido sovrapposto tra due input solidi. Nel seguente esempio, _Difference_ è stato modificato in _Intersect_ e il solido risultante è il vuoto mancante inizialmente ritagliato:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
