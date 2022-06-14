# Opérations booléennes géométriques

Les méthodes _Intersect_, _Trim_ et _SelectTrim_ sont principalement utilisées sur la géométrie dimensionnelle inférieure, comme les points, les courbes et les surfaces. La géométrie solide, en revanche, dispose d'un ensemble de méthodes supplémentaires pour modifier la forme après sa construction, en soustrayant de la matière de manière comparable à la méthode _Trim_ et en combinant les éléments pour former un ensemble plus grand.

### Réunion

La méthode _Union_ prend deux objets solides et crée un objet solide unique à partir de l'espace couvert par les deux objets. L'espace de chevauchement entre les objets est combiné dans la forme finale. Cet exemple combine une sphère et un cuboïde en une forme de sphère-cube solide unique :

![](../images/8-2/9/GeometricBooleans\_01.png)

```js
s1 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

s2 = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(4, 0,
    0), 6);

combined = s1.Union(s2);
```

### Différence

La méthode _Difference_, comme _Trim_, soustrait le contenu du solide de l'outil d'entrée du solide de base. Dans cet exemple, nous allons creuser une petite indentation dans une sphère :

![](../images/8-2/9/GeometricBooleans\_02.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Difference(tool);
```

### Intersecter

La méthode _Intersect_ renvoie le solide se chevauchant entre deux entrées de solide. Dans l'exemple suivant, la méthode _Difference_ a été changée en _Intersect_ et le solide résultant correspond au vide manquant initialement creusé :

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
