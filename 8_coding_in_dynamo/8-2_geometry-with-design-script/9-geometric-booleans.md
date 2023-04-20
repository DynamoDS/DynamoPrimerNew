# Booleanos geométricos

_Intersect_, _Trim_ e _SelectTrim_ são principalmente usados na geometria bidimensional menor como Pontos, Curvas e Superfícies. A geometria sólida, por outro lado, tem um conjunto adicional de métodos para modificar a forma após sua construção, subtraindo o material de uma forma similar a _Trim_ e combinando elementos para formar um todo maior.

### Union

O método _Union_ usa dois objetos sólidos e cria um único objeto sólido fora do espaço coberto por ambos os objetos. O espaço sobreposto entre os objetos é combinado na forma final. Este exemplo combina uma esfera e um cuboide em uma única forma sólida de cubo de esfera:

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

O método _Difference_, como _Trim_, subtrai o conteúdo do sólido da ferramenta de entrada do sólido base. Neste exemplo, nós colocamos um pequeno recuo fora de uma esfera:

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

O método _Intersect_ retorna o sólido sobreposto entre duas entradas de sólidos. No exemplo a seguir, o método _Difference_ foi alterado para _Intersect_, e o sólido resultante é o vazio ausente inicialmente entalhado:

![](../images/8-2/9/GeometricBooleans\_03.png)

```js
s = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin, 6);

tool = Sphere.ByCenterPointRadius(
    CoordinateSystem.Identity().Origin.Translate(10, 0,
    0), 6);

result = s.Intersect(tool);
```
