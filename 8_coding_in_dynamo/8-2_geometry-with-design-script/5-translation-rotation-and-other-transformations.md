# Transformações de conversão, rotação e outras

Determinados objetos de geometria podem ser criados explicitamente especificando as coordenadas x, y e z no espaço tridimensional. Mais frequentemente, no entanto, a geometria é movida para sua posição final usando transformações geométricas no próprio objeto ou em seu CoordinateSystem subjacente.

### Conversão

A transformação geométrica mais simples é uma conversão, que move um objeto um número especificado de unidades nas direções x, y e z.

![](../images/8-2/5/Transformations\_01.png)

```js
// create a point at x = 1, y = 2, z = 3
p = Point.ByCoordinates(1, 2, 3);

// translate the point 10 units in the x direction,
// -20 in y, and 50 in z
// p2’s new position is x = 11, y = -18, z = 53
p2 = p.Translate(10, -20, 50);
```

### Rotação

Embora todos os objetos no Dynamo possam ser convertidos anexando o método _.Translate_ ao final do nome do objeto, as transformações mais complexas exigem a transformação do objeto de um CoordinateSystem subjacente em um novo CoordinateSystem. Por exemplo, para rotacionar um objeto 45° em torno do eixo x, vamos transformar o objeto de seu CoordinateSystem existente sem rotação em um CoordinateSystem que foi rotacionado 45° em torno do eixo x com o método _.Transform_:

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

### Dimensionar

Além de serem convertidos e rotacionados, também é possível criar os CoordinateSystems com escala ou cisalhamento. Um CoordinateSystem pode ser dimensionado com o método _.Scale_:

![](../images/8-2/5/Transformations\_03.png)

```js
cube = Cuboid.ByLengths(CoordinateSystem.Identity(),
    10, 10, 10);

new_cs = CoordinateSystem.Identity();
new_cs2 = new_cs.Scale(20);

old_cs = CoordinateSystem.Identity();

cube2 = cube.Transform(old_cs, new_cs2);
```

Os CoordinateSystems de cisalhamento são criados inserindo vetores não ortogonais no construtor CoordinateSystem.

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

A escala e o cisalhamento são, comparativamente, transformações geométricas mais complexas do que a rotação e a conversão, de modo que nem todos os objetos do Dynamo podem sofrer essas transformações. A tabela a seguir descreve quais objetos do Dynamo podem ter CoordinateSystems com escala não uniforme e CoordinateSystems com cisalhamento.

| Classe        | CoordinateSystem com escala não uniforme | CoordinateSystem com cisalhamento |
| ------------ | ------------------------------------- | ------------------------ |
| Arco          | Não                                    | Não                       |
| NurbsCurve   | Sim                                   | Sim                      |
| NurbsSurface | Não                                    | Não                       |
| Círculo       | Não                                    | Não                       |
| Linha         | Sim                                   | Sim                      |
| Plano        | Não                                    | Não                       |
| Ponto        | Sim                                   | Sim                      |
| Polígono      | Não                                    | Não                       |
| Sólido        | Não                                    | Não                       |
| Superfície      | Não                                    | Não                       |
| Texto         | Não                                    | Não                       |
