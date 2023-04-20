# Geometrische boolesche Operationen

_Intersect_, _Trim_ und _SelectTrim_ werden hauptsächlich für niedrigerdimensionale Geometrien wie Punkte, Kurven und Oberflächen genutzt. Volumenkörper-Geometrie verfügt über einen zusätzlichen Satz von Methoden zum Ändern der Form nach ihrer Erstellung, sowohl durch Subtraktion von Material, ähnlich wie _Trim_, als auch durch Kombination von Elementen zu einem größeren Ganzen.

### Union

Die Methode _Union_ nimmt zwei Volumenkörper-Objekte und erstellt ein einzelnes Volumenkörper-Objekt aus dem Raum, der von beiden Objekten abgedeckt wird. Der Überlappungsbereich zwischen den einzelnen Objekten wird zur endgültigen Form kombiniert. In diesem Beispiel werden eine Kugel und ein Quader in einer einzigen Volumenkörper-Kugel-Quader-Form kombiniert:

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

Die Methode _Difference_ subtrahiert ähnlich wie _Trim_ die Inhalte des eingegebenen Werkzeug-Volumenkörpers vom Basis-Volumenkörper. In diesem Beispiel kerben wir eine Kugel leicht ein:

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

Die Methode _Intersect_ gibt den überlappenden Volumenkörper zwischen zwei Eingabe-Volumenkörpern zurück. Im folgenden Beispiel wurde _Difference_ in _Intersect_ geändert, und der resultierende Volumenkörper entspricht dem ursprünglich eingekerbten Leerraum:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
