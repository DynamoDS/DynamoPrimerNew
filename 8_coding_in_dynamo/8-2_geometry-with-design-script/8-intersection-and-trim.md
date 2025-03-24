# Průnik a oříznutí

Mnoho příkladů se dosud soustředilo na tvorbu vícerozměrných geometrií z méněrozměrných geometrií. Pomocí metod průsečíků je možné z vícerozměrných geometrií vygenerovat méněrozměrné objekty a po vytvoření geometrií lze jejich tvar dále upravit pomocí příkazů oříznutí.

Metoda _Průnik_ je definována u všech geometrií aplikace Dynamo, teoreticky lze tedy provést průnik libovolných dvou geometrií. Některé průniky samozřejmě nedávají smysl, například průnik s body, protože výsledným objektem bude vždy samotný vstupní bod. Další možné kombinace průniků mezi objekty jsou uvedeny v následujícím diagramu. Následující tabulka uvádí výsledky různých operací průniku:

### **Průnik**

| _S hodnotami:_     | Povrch | Křivka | Rovina        | Těleso   |
| ----------- | ------- | ----- | ------------ | ------- |
| **Povrch** | Křivka   | Bod | Bod, křivka | Povrch |
| **Křivka**   | Bod   | Bod | Bod        | Křivka   |
| **Rovina**   | Křivka   | Bod | Křivka        | Křivka   |
| **Těleso**   | Povrch | Křivka | Křivka        | Těleso   |

Následující velmi jednoduchý příklad ukazuje průnik roviny s plochou Nurbs. Průnik vygeneruje pole objektů NurbsCurve, které lze používat jako kterékoliv jiné objekty NurbsCurve.

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

Metoda _Oříznutí_ je velmi podobná metodě Průnik v tom, že je definována u téměř všech geometrií. Metoda _Oříznutí_ je však omezenější než metoda _Průnik_.

### **Oříznutí**

|             | _Používá:_ Bod | Křivka | Rovina | Plocha | Těleso |
| ----------- | -------------- | ----- | ----- | ------- | ----- |
| _Na:_ Křivka | Ano            | Ne    | Ne    | Ne      | Ne    |
| Polygon     | -              | Ne    | Ano   | Ne      | Ne    |
| Plocha     | -              | Ano   | Ano   | Ano     | Ano   |
| Těleso       | -              | -     | Ano   | Ano     | Ano   |

U metody _Oříznutí_ je nutné zadat výběrový bod, který určuje, která geometrie má být zahozena a která má být zachována. Aplikace vyhledá a zahodí oříznutou geometrii, která bude výběrovému bodu nejblíže.

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
