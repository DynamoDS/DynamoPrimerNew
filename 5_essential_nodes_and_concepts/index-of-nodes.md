# Rejstřík uzlů

Tento rejstřík nabízí dodatečné informace o všech uzlech použitých v této příručce a také dalších komponentách, které mohou být užitečné. Jedná se pouze o představení některých z 500 uzlů dostupných v aplikaci Dynamo.

## Zobrazení

### Barva

|                                                  |                                                                                                                       |                                                                 |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
|                                                  | TVORBA                                                                                                                |                                                                 |
| ![](images/ColorbyARGB.jpg)          | <p><strong>Color.ByARGB</strong><br>Umožňuje vytvořit barvu pomocí alfa, červené, zelené a modré složky.</p>                  | \![](<images/index of nodes - color byargb.jpg>)     |
| \![](<images/Color Range.jpg>)        | <p><strong>Color Range</strong><br>Vrací barvu z barevného gradientu mezi počáteční a koncovou barvou.</p>      | \![](<images/Color Range.jpg>)        |
|                                                  | AKCE                                                                                                               |                                                                 |
| ![](images/ColorBrightness.jpg)      | <p><strong>Color.Brightness</strong><br>Vrací hodnotu jasu této barvy.</p>                                 | ![](images/ColorBrightness.jpg)   |
| ![](<images/ColorComponent.jpg>) | <p><strong>Color.Components</strong><br>Zobrazí seznam složek barvy v pořadí: alfa, červená, zelená a modrá.</p> | \![](<images/index of nodes - color component.jpg>)  |
| ![](images/ColorSaturation.jpg)      | <p><strong>Color.Saturation</strong><br>Vrací hodnotu sytosti této barvy.</p>                                  | \![](<images/index of nodes - color saturation.jpg>) |
| ![](images/ColorHue.jpg)             | <p><strong>Color.Hue</strong><br>Vrací hodnotu odstínu této barvy.</p>                                               | \![](<images/index of nodes - color hue.jpg>)        |
|                                                  | DOTAZ                                                                                                                 |                                                                 |
| ![](<images/ColorAlpha.jpg>)     | <p><strong>Color.Alpha</strong><br>Umožňuje najít alfa složku barvy, 0 až 255.</p>                                 | \![](<images/index of nodes - color alpha.jpg>)      |
| ![](images/ColorBlue.jpg)            | <p><strong>Color.Blue</strong><br>Umožňuje zjistit modrou složku barvy, 0 až 255.</p>                                   | \![](<images/index of nodes - color blue.jpg>)       |
| ![](images/ColorGreen.jpg)           | <p><strong>Color.Green</strong><br>Umožňuje zjistit zelenou složku barvy, 0 až 255.</p>                                 | \![](<images/index of nodes - color green.jpg>)      |
| ![](images/ColorRed.jpg)             | <p><strong>Color.Red</strong><br>Umožňuje zjistit červenou složku barvy, 0 až 255.</p>                                     | \![](<images/index of nodes - color red.jpg>)        |

|                                                               |                                                                                           |                                                                                 |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
|                                                               | TVORBA                                                                                    |                                                                                 |
| \![](<images/index of nodes - geometry color by geometry color.jpg>) | <p><strong>GeometryColor.ByGeometryColor</strong><br>Zobrazit geometrii v barvě.</p> | \![](<images/index of nodes - geometry color by geometry color.jpg>) |

### Watch

|                                             |                                                                               |                                                                 |
| ------------------------------------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------- |
|                                             | AKCE                                                                       |                                                                 |
| \![](<images/View watch.jpg>)    | <p><strong>View.Watch</strong><br>Vizualizuje výstup uzlu.</p>           | \![](<images/index of nodes - view watch.jpg>)       |
| \![](<images/View watch 3d.jpg>) | <p><strong>View.Watch 3D</strong><br>Zobrazí dynamický náhled geometrie.</p> | \![](<images/index of nodes - view watch.3Djpg.jpg>) |

## Vstup

|                                              |                                                                                                          |                                                               |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
|                                              | AKCE                                                                                                  |                                                               |
| ![](images/Boolean.jpg)          | <p><strong>Boolean</strong><br>Výběr mezi hodnotami true a false.</p>                                   | \![](<images/index of nodes - boolean.jpg>)        |
| ![](<images/CodeBlock.jpg>)  | <p><strong>Code Block</strong><br>Umožňuje přímou tvorbu kódu DesignScript.</p>              | \![](<images/index of nodes - code block.jpg>)     |
| \![](<images/Directory Path.jpg>) | <p><strong>Directory Path</strong><br>Umožňuje vybrat adresář v systému a načíst jeho cestu.</p> | \![](<images/index of nodes - directory path.jpg>) |
| \![](<images/File Path.jpg>)      | <p><strong>Cesta k souboru</strong><br>Umožňuje výběr souboru v systému a získá jeho název.</p>       | \![](<images/index of nodes - file path.jpg>)      |
| \![](<images/Integer slider.jpg>) | <p><strong>Integer Slider</strong><br>Posuvník, který vytváří celočíselné hodnoty.</p>                         | \![](<images/index of nodes - integer slider.jpg>) |
| ![](images/number.jpg)           | <p><strong>Number</strong><br>Vytvoří číslo.</p>                                                      | ![](images/number.jpg)          |
| \![](<images/Number slider.jpg>)  | <p><strong>Number Slider</strong><br>Posuvník, který vytváří číselné hodnoty.</p>                          | \![](<images/index of nodes - number slider.jpg>)  |
| ![](images/string.jpg)           | <p><strong>String</strong><br>Vytvoří řetězec.</p>                                                      | \![](<images/index of nodes - string.jpg>)         |
| \![](<images/Object is Null.jpg>) | <p><strong>Object.IsNull</strong><br>Určuje, zda má zadaný objekt hodnotu null.</p>                         | \![](<images/index of nodes - object is null.jpg>) |

## Seznam

|                                                          |                                                                                                                                                                                                                                               |                                                                         |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
|                                                          | TVORBA                                                                                                                                                                                                                                        |                                                                         |
| \![](<images/List Create.jpg>)                | <p><strong>List.Create</strong><br>Vytvoří nový seznam ze zadaných vstupů.</p>                                                                                                                                                              | \![](<images/index of nodes - list create.jpg>)          |
| \![](<images/List Combine.jpg>)               | <p><strong>List.Combine</strong><br>Použije kombinátor na každý prvek ve dvou posloupnostech.</p>                                                                                                                                                 | \![](<images/index of nodes - list combine.jpg>)             |
| ![](images/Range.jpg)                        | <p><strong>Number Range</strong><br>Vytvoří posloupnost čísel v zadaném rozsahu.</p>                                                                                                                                                  | ![](images/Range.jpg)                     |
| ![](images/Sequence.jpg)                     | <p><strong>Number Sequence</strong><br>Vytvoří posloupnost čísel.</p>                                                                                                                                                                     | \![](<images/index of nodes - sequence.jpg>)                 |
|                                                          | AKCE                                                                                                                                                                                                                                       |                                                                         |
| \![](<images/List Chop.jpg>)                  | <p><strong>List.Chop</strong><br>Rozdělí seznam do sady seznamů, z nichž každý obsahuje dané množství položek.</p>                                                                                                                               | \![](<images/index of nodes - list chop.jpg>)                |
| \![](<images/index of nodes - count.jpg>)                   | <p><strong>List.Count</strong><br>Vrací počet položek uložených v daném seznamu.</p>                                                                                                                                                   | \![](<images/index of nodes - count.jpg>)                    |
| \![](<images/List Flatten.jpg>)               | <p><strong>List.Flatten</strong><br>Vyrovná vnořený seznam seznamů o určitou hodnotu.</p>                                                                                                                                                  | \![](<images/index of nodes - list flatten.jpg>)             |
| \![](<images/List Filter by Bool Mask.jpg>)   | <p><strong>List.FilterByBoolMask</strong><br>Filtruje posloupnost na základě vyhledávání příslušných indexů v samostatném seznamu logických hodnot.</p>                                                                                                       | \![](<images/index of nodes - list filter by bool mask.jpg>) |
| \![](<images/List Get Item At Index.jpg>)     | <p><strong>List.GetItemAtIndex</strong><br>Vrací položku z daného seznamu, která se nachází na určeném indexu.</p>                                                                                                                        | \![](<images/index of nodes - list get item at index.jpg>)   |
|                                                          | <p><strong>List.Map</strong><br>Použije funkci na všechny prvky v seznamu, čím z výsledků vytvoří nový seznam.</p>                                                                                                                    | \![](<images/index of nodes - list map.jpg>)                 |
|                                                          | <p><strong>List.Reverse</strong><br>Vytvoří nový seznam obsahující položky daného seznamu, ale v obráceném pořadí.</p>                                                                                                                        | \![](<images/index of nodes - list reverse.jpg>)             |
| \![](<images/List Replace Item At Index.jpg>) | <p><strong>List.ReplaceItemAtIndex</strong><br>Nahradí položku z daného seznamu, která se nachází na daném indexu.</p>                                                                                                                  | \![](<images/index of nodes - replace item at index.jpg>)    |
| \![](<images/List Shift Indices.jpg>)         | <p><strong>List.ShiftIndices</strong><br>Posune indexy v seznamu doprava o zadané množství.</p>                                                                                                                                      | \![](<images/index of nodes - list shift indices.jpg>)       |
| \![](<images/List Take Every Nth Item.jpg>)   | <p><strong>List.TakeEveryNthItem</strong><br>Načte položky ze zadaného seznamu na indexech, které jsou násobky dané hodnoty s daným odsazením.</p>                                                                                  | \![](<images/index of nodes - list take every nth item.jpg>) |
| \![](<images/List Transpose.jpg>)             | <p><strong>List.Transpose</strong><br>Prohodí řádky a sloupce v seznamu seznamů. Pokud jsou některé řádky kratší než jiné, budou jako zástupné znaky do výsledného pole vloženy hodnoty null, tak aby pole stále bylo pravoúhlé.</p> | \![](<images/index of nodes - list transpose.jpg>)           |

## Logika

|                                |                                                                                                                                                                                                              |                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- |
|                                | AKCE                                                                                                                                                                                                      |                                                   |
| ![](images/If.jpg) | <p><strong>Pokud</strong><br>Podmíněný výraz. Zkontroluje booleovskou hodnotu testovacího vstupu. Pokud má testovací vstup hodnotu true, výsledný výstup bude mít hodnotu true, v opačném případě bude mít hodnotu false.</p> | \![](<images/index of nodes - if.jpg>) |

## Matematika

|                                                       |                                                                                                                            |                                                                        |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
|                                                       | AKCE                                                                                                                    |                                                                        |
| \![](<images/Math cos.jpg>)                | <p><strong>Math.Cos</strong><br>Vrací kosinus úhlu.</p>                                                          | \![](<images/index of nodes - math cos.jpg>)                |
| \![](<images/Math degrees to radians.jpg>) | <p><strong>Math.DegreesToRadians</strong><br>Převede úhel ve stupních na úhel v radiánech.</p>                      | \![](<images/index of nodes - math degrees to radians.jpg>) |
| \![](<images/Math pow.jpg>)                | <p><strong>Math.Pow</strong><br>Umocní číslo na danou mocninu.</p>                                                | \![](<images/index of nodes - math pow.jpg>)                |
| \![](<images/Math radians to degrees.jpg>) | <p><strong>Math.RadiansToDegrees</strong><br>Převede úhel v radiánech na úhel ve stupních.</p>                      | \![](<images/index of nodes - math radians to degrees.jpg>) |
| \![](<images/Math remap range.jpg>)        | <p><strong>Math.RemapRange</strong><br>Upraví rozsah seznamu čísel při zachování poměru rozložení.</p> | \![](<images/index of nodes - math remap range.jpg>)        |
| \![](<images/Math sin.jpg>)                | <p><strong>Math.Sin</strong><br>Najde sinus úhlu.</p>                                                            | \![](<images/index of nodes - math sin.jpg>)                |
| ![](images/Map.jpg)                  | <p><strong>Map</strong><br>Mapuje hodnotu do vstupního rozsahu.</p>                                                            | \![](<images/index of nodes - math map.jpg>)                |

## Řetězec

|                                                |                                                                                                                                                      |                                                                 |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
|                                                | AKCE                                                                                                                                              |                                                                 |
| \![](<images/String concat.jpg>)    | <p><strong>String.Concat</strong><br>Zřetězí více řetězců do jediného řetězce.</p>                                                         | \![](<images/index of nodes - string concat.jpg>)    |
| \![](<images/String contains.jpg>)  | <p><strong>String.Contains</strong><br>Určuje, zda zadaný řetězec obsahuje daný dílčí řetězec.</p>                                              | \![](<images/index of nodes - string contains.jpg>)  |
| \![](<images/String join.jpg>)      | <p><strong>String.Join</strong><br>Zřetězí více řetězců do jediného řetězce, přičemž vloží daný oddělovač mezi každý spojený řetězec.</p> | \![](<images/index of nodes - string join.jpg>)      |
| \![](<images/String split.jpg>)     | <p><strong>String.Split</strong><br>Rozdělí jeden řetězec na seznam řetězců, s dělením určeným podle daných oddělovacích řetězců.</p>    | \![](<images/index of nodes - string split.jpg>)     |
| \![](<images/String to number.jpg>) | <p><strong>String.ToNumber</strong><br>Převádí řetězec na celé číslo nebo hodnotu typu double.</p>                                                              | \![](<images/index of nodes - string to number.jpg>) |

## Geometrie

### Kružnice

|                                                             |                                                                                                                                                          |                                                                                     |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
|                                                             | TVORBA                                                                                                                                                   |                                                                                     |
| \![](<images/Circle by center point radius.jpg>) | <p><strong>Circle.ByCenterPointRadius</strong><br>Vytvoří kružnici se zadaným středem a poloměrem v globální rovině XY, s rovinou Z jako normálou.</p> | \![](<images/index of nodes - circle by center point radius normal.jpg>) |
| \![](<images/Circle by plane radius.jpg>)        | <p><strong>Circle.ByPlaneRadius</strong><br>Vytvoří kružnici vystředěnou na počátku vstupní roviny (kořenu), ležící ve vstupní rovině, se zadaným poloměrem.</p>  | \![](<images/index of nodes - circle by plane radius.jpg>)               |

|                                                                           |                                                                                                                                                                                                    |                                                                                              |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
|                                                                           | TVORBA                                                                                                                                                                                             |                                                                                              |
| \![](<images/Coordinate system by origin.jpg>)                 | <p><strong>CoordinateSystem.ByOrigin</strong><br>Vytvoří systém CoordinateSystem s počátkem ve vstupním bodu, s osami X a Y nastavenými jako osy X a Y v GSS.</p>                                               | \![](<images/index of nodes - coordinates system by origin.jpg>)                  |
| \![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) | <p><strong>CoordinateSystem.ByCylindricalCoordinates</strong><br>Vytvoří systém CoordinateSystem v zadaných válcových souřadnicových parametrech s ohledem na zadaný souřadnicový systém.</p> | \![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) |

### Cuboid

|                                                                  |                                                                                                                                            |                                                                                    |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
|                                                                  | TVORBA                                                                                                                                     |                                                                                    |
| \![](<images/Cuboid by length.jpg>)                   | <p><strong>Cuboid.ByLengths</strong><br>Vytvoří kvádr vystředěný na počátek GSS, se šířkou, délkou a výškou.</p>                        | \![](<images/index of nodes - cuboid by lengths.jpg>)                   |
| \![](<images/index of nodes - cuboid by lengths origin.jpg>)            | <p><strong>Cuboid.ByLengths</strong> (origin)</p><p>Vytvoří kvádr vystředěný na vstupním bodu, s určenou šířkou, délkou a výškou.</p> | \![](<images/index of nodes - cuboid by lengths origin.jpg>)            |
| \![](<images/index of nodes - cuboid by lengths coordinate system.jpg>) | <p><strong>Cuboid.ByLengths</strong> (coordinateSystem)</p><p>Vytvoří kvádr vystředěný na počátek GSS, se šířkou, délkou a výškou.</p>  | \![](<images/index of nodes - cuboid by lengths coordinate system.jpg>) |
| \![](<images/index of nodes - cuboid by corners.jpg>)                 | <p><strong>Cuboid.ByCorners</strong></p><p>Vytvoří kvádr s rozsahem od dolního bodu po horní bod.</p>                                      | \![](<images/index of nodes - cuboid by corners.jpg>)                   |
| \![](<images/Cuboid length.jpg>)                      | <p><strong>Cuboid.Length</strong></p><p>Vrátí vstupní rozměry kvádru, NE skutečné globální rozměry prostoru. **</p>           | \![](<images/index of nodes - cuboid length.jpg>)                       |
| \![](<images/index of nodes - cuboid width.jpg>)                     | <p><strong>Cuboid.Width</strong></p><p>Vrátí vstupní rozměry kvádru, NE skutečné globální rozměry prostoru. **</p>            | \![](<images/index of nodes - cuboid width.jpg>)                        |
| \![](<images/index of nodes - cuboid height.jpg>)                    | <p><strong>Cuboid.Height</strong></p><p>Vrátí vstupní rozměry kvádru, NE skutečné globální rozměry prostoru. **</p>           | \![](<images/index of nodes - cuboid height.jpg>)                       |
| \![](<images/Bounding box to cuboid.jpg>)             | <p><strong>BoundingBox.ToCuboid</strong></p><p>Získá hraniční kvádr jako objemový kvádr.</p>                                                  | \![](<images/index of nodes - bounding box to cuboid.jpg>)              |

{% hint style="warning" %} **Jinými slovy, pokud vytvoříte šířku kvádru (osa X) o délce 10 a transformujete ji na souřadnicový systém s 2krát větším měřítkem v ose X, šířka bude stále 10. ASM neumožňuje extrahovat vrcholy tělesa v předvídatelném pořadí, takže po transformaci není možné určit rozměry. {% endhint %}

### Křivka

|                                                        |                                                                                                                                                  |                                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
|                                                        | AKCE                                                                                                                                          |                                                                         |
| \![](<images/Curve extrude.jpg>)            | <p><strong>Curve.Extrude</strong> (distance)<br>Vysune křivku ve směru normálového vektoru.</p>                                             | \![](<images/index of nodes - curve extrude.jpg>)            |
| \![](<images/Curve point at parameter.jpg>) | <p><strong>Curve.PointAtParameter</strong><br>Získá bod na křivce v určeném parametru mezi objekty StartParameter() a EndParameter().</p> | \![](<images/index of nodes - curve point at parameter.jpg>) |

### Modifikátory geometrie

|                                                        |                                                                                                                                    |                                                                         |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
|                                                        | AKCE                                                                                                                            |                                                                         |
| \![](<images/Geometry distance to.jpg>)     | <p><strong>Geometry.DistanceTo</strong><br>Získá vzdálenost od této geometrie k jiné.</p>                                  | \![](<images/index of nodes - geometry distance to.jpg>)     |
| \![](<images/Geometry explode.jpg>)         | <p><strong>Geometry.Explode</strong><br>Rozdělí složené nebo neoddělené prvky do součástí jejich komponent.</p>                | \![](<images/index of nodes - geometry explode.jpg>)         |
| \![](<images/Geometry import from SAT.jpg>) | <p><strong>Geometry.ImportFromSAT</strong><br>Seznam importovaných geometrií</p>                                                      | \![](<images/index of nodes - geometry import from sat.jpg>) |
| \![](<images/Geometry rotate.jpg>)          | <p><strong>Geometry.Rotate</strong> (basePlane)<br>Otočí objekt kolem počátku roviny a normály o zadaný počet stupňů.</p> | \![](<images/index of nodes - geometry rotate.jpg>)          |
| \![](<images/Geometry translate.jpg>)       | <p><strong>Geometry.Translate</strong><br>Posune libovolný typ geometrie o zadanou vzdálenost v daném směru.</p>           | \![](<images/index of nodes - geometry translate.jpg>)       |

### Čára

|                                                                    |                                                                                                                                                          |                                                                                     |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
|                                                                    | TVORBA                                                                                                                                                   |                                                                                     |
| \![](<images/Line by best fit through points.jpg>)      | <p><strong>Line.ByBestFitThroughPoints</strong><br>Vytvoří čáru nejlépe aproximující rozptýlené vykreslení bodů.</p>                                       | \![](<images/index of nodes - line by best fit through points.jpg>)      |
| \![](<images/Line by start point direction length.jpg>) | <p><strong>Line.ByStartPointDirectionLength</strong><br>Vytvoří přímou čáru od počátečního bodu, která se prodlouží ve směru vektoru o zadanou délku.</p> | \![](<images/index of nodes - line by start point direction length.jpg>) |
| ![](<images/Linebystartpointendpoint.jpg>)         | <p><strong>Line.ByStartPointEndPoint</strong><br>Vytvoří rovnou čáru mezi dvěma vstupními body.</p>                                                   | \![](<images/index of nodes - line by start point end point.jpg>)        |
| \![](<images/Line by tangency.jpg>)                     | <p><strong>Line.ByTangency</strong><br>Vytvoří tečnu ke vstupní křivce, umístěnou v bodu parametru vstupní křivky.</p>               | \![](<images/index of nodes - line by tangency.jpg>)                     |
|                                                                    | DOTAZ                                                                                                                                                    |                                                                                     |
| \![](<images/Line direction.jpg>)                       | <p><strong>Line.Direction</strong><br>Směr křivky.</p>                                                                                    | \![](<images/index of nodes - line direction.jpg>)                       |

### NurbsCurve

|                                                             |                                                                                                               |                                                                              |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
|                                                             | Tvorba                                                                                                        |                                                                              |
| \![](<images/Nurbs curve by control points.jpg>) | <p><strong>NurbsCurve.ByControlPoints</strong><br>Pomocí explicitních řídicích bodů vytvoří objekt BSplineCurve.</p> | \![](<images/index of nodes - nurbs curve by control points.jpg>) |
| \![](<images/Nurbs curve by points.jpg>)         | <p><strong>NurbsCurve.ByPoints</strong><br>Vytvoří objekt BSplineCurve pomocí interpolace mezi body.</p>          | \![](<images/index of nodes - nurbs curve by points.jpg>)         |

### NurbsSurface

|                                                               |                                                                                                                                                                                            |                                                                                |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
|                                                               | Tvorba                                                                                                                                                                                     |                                                                                |
| \![](<images/Nurbs surface by control points.jpg>) | <p><strong>NurbsSurface.ByControlPoints</strong><br>Vytvoří objekt NurbsSurface pomocí explicitních řídicích bodů se zadanými stupni U a V.</p>                                             | \![](<images/index of nodes - nurbs surface by control points.jpg>) |
| \![](<images/Nurbs surface by points.jpg>)         | <p><strong>NurbsSurface.ByPoints</strong><br>Vytvoří objekt NurbsSurface s určenými interpolovanými body a stupni U a V. Výsledný povrch bude procházet všemi body.</p> | \![](<images/index of nodes - nurbs surface by points.jpg>)         |

### Rovina

|                                                      |                                                                                                                  |                                                                       |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
|                                                      | TVORBA                                                                                                           |                                                                       |
| \![](<images/Plane by origin normal.jpg>) | <p><strong>Plane.ByOriginNormal</strong><br>Vytvoří rovinu vystředěnou na kořenový bod pomocí vstupního normálového vektoru.</p> | \![](<images/index of nodes - plane by origin normal.jpg>) |
| \![](<images/Plane XY.jpg>)               | <p><strong>Plane.XY</strong><br>Vytvoří rovinu v prostoru XY.</p>                                              | \![](<images/index of nodes - plane xy.jpg>)               |

### Bod

|                                                              |                                                                                                                                           |                                                                               |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
|                                                              | TVORBA                                                                                                                                    |                                                                               |
| \![](<images/Point by cartesian coordinates.jpg>) | <p><strong>Point.ByCartesianCoordinates</strong><br>Vytvoří bod v daném souřadnicovém systému pomocí 3 kartézských souřadnic.</p>          | \![](<images/index of nodes - point by cartesian coordinates.jpg>) |
| \![](<images/Point by coordinates 2D.jpg>)        | <p><strong>Point.ByCoordinates</strong> (2d)<br>Vytvoří bod v rovině XY pomocí dvou kartézských souřadnic. Komponenta Z je 0.</p> | \![](<images/index of nodes - point by coordinates 2D.jpg>)        |
| \![](<images/Point by coordinates 3D.jpg>)        | <p><strong>Point.ByCoordinates</strong> (3d)<br>Vytvoří bod daný 3 kartézskými souřadnicemi.</p>                                           | \![](<images/index of nodes - point by coordinates 3D.jpg>)        |
| \![](<images/Point origin.jpg>)                   | <p><strong>Point.Origin</strong><br>Získá bod počátku (0,0,0).</p>                                                                      | \![](<images/index of nodes - point origin.jpg>)                   |
|                                                              | AKCE                                                                                                                                   |                                                                               |
| \![](<images/Point add.jpg>)                      | <p><strong>Point.Add</strong><br>Přidá k bodu vektor. Stejné jako Translate (Vector).</p>                                             | \![](<images/index of nodes - point add.jpg>)                      |
|                                                              | DOTAZ                                                                                                                                     |                                                                               |
| \![](<images/Point x.jpg>)                        | <p><strong>Point.X</strong><br>Získá komponentu X bodu.</p>                                                                         | \![](<images/index of nodes - point x.jpg>)                        |
| \![](<images/Point y.jpg>)                        | <p><strong>Point.Y</strong><br>Získá komponentu Y bodu.</p>                                                                         | \![](<images/index of nodes - point y.jpg>)                        |
| \![](<images/Point z.jpg>)                        | <p><strong>Point.Z</strong><br>Získá komponentu Z bodu.</p>                                                                         | \![](<images/index of nodes - point z.jpg>)                        |

### Polycurve

|                                                   |                                                                                                                                                                                       |                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
|                                                   | TVORBA                                                                                                                                                                                |                                                                    |
| \![](<images/Polycurve by points.jpg>) | <p><strong>Polycurve.ByPoints</strong><br>Vytvoří objekt PolyCurve z posloupnosti čar propojujících body. U uzavřené křivky by měl poslední bod být ve stejném umístění jako počáteční bod.</p> | \![](<images/index of nodes - polycurve by points.jpg>) |

### Obdélník

|                                                         |                                                                                                                                                                               |                                                                          |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
|                                                         | TVORBA                                                                                                                                                                        |                                                                          |
| \![](<images/Rectangle by width length.jpg>) | <p><strong>Rectangle.ByWidthLength</strong> (Plane)<br>Vytvoří obdélník vystředěný na kořen vstupní roviny se vstupní šířkou (délka osy X roviny) a délkou (délka osy Y roviny).</p> | \![](<images/index of nodes - rectangle by width length.jpg>) |

### Koule

|                                                             |                                                                                                                             |                                                                              |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
|                                                             | TVORBA                                                                                                                      |                                                                              |
| \![](<images/Sphere by center point radius.jpg>) | <p><strong>Sphere.ByCenterPointRadius</strong><br>Vytvoří těleso (kouli) vystředěné na vstupní bod se zadaným poloměrem.</p> | \![](<images/index of nodes - sphere by center point radius.jpg>) |

### Povrch

|                                                           |                                                                                                                                                      |                                                                           |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
|                                                           | TVORBA                                                                                                                                               |                                                                           |
| ![](<images/Surfacebyloft.jpg>) | <p><strong>Surface.ByLoft</strong><br>Vytvoří povrch pomocí šablonování mezi křivkami vstupního příčného řezu.</p>                                             | \![](<images/index of nodes - surface by loft.jpg>)            |
| ![](<images/Surfacebyloft.jpg>) | <p><strong>Surface.ByPatch</strong><br>Vytvoří povrch vyplněním vnitřní části uzavřené hranice definované vstupními křivkami.</p>                 | ![](images/Surface.ByPatch.png)              |
|                                                           | AKCE                                                                                                                                              |                                                                           |
| \![](<images/Surface offset.jpg>)              | <p><strong>Surface.Offset</strong><br>Odsadí povrch ve směru normály povrchu o zadanou vzdálenost.</p>                                        | \![](<images/index of nodes - surface offset.jpg>)             |
| ![](images/Surfacepointatparameter.jpg)  | <p><strong>Surface.PointAtParameter</strong><br>Vrátí bod v zadaných parametrech U a V.</p>                                              | \![](<images/index of nodes - surface point at parameter.jpg>) |
| ![](images/Surfacethicken.jpg)           | <p><strong>Surface.Thicken</strong><br>Rozšíří plochu na těleso vysunutím ve směru normál povrchu na obou stranách povrchu.</p> | \![](<images/index of nodes - surface thicken.jpg>)            |

### UV

|                                                 |                                                                           |                                                                  |
| ----------------------------------------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------- |
|                                                 | TVORBA                                                                    |                                                                  |
| \![](<images/UV by coordinates.jpg>) | <p><strong>UV.ByCoordinates</strong><br>Vytvoří prvek UV ze dvou hodnot typu double.</p> | \![](<images/index of nodes - UV by coordinates.jpg>) |

### Vektor

|                                                         |                                                                                          |                                                                      |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
|                                                         | TVORBA                                                                                   |                                                                      |
| \![](<images/Vector by coordinates.jpg>) | <p><strong>Vector.ByCoordinates</strong><br>Vytvoří vektor pomocí 3 euklidovských souřadnic.</p> | \![](<images/index of nodes - vector by coordinates.jpg>) |
| ![](images/Vectorxaxis.jpg)            | <p><strong>Vector.XAxis</strong><br>Získá kanonický vektor osy X (1,0,0).</p>         | \![](<images/index of nodes - vector x.jpg>)              |
| ![](images/Vectoryaxis.jpg)            | <p><strong>Vector.YAxis</strong><br>Získá kanonický vektor osy Y (0,1,0).</p>         | \![](<images/index of nodes - vector y.jpg>)              |
| ![](images/Vectorzaxis.jpg)            | <p><strong>Vector.ZAxis</strong><br>Získá kanonický vektor osy Z (0,0,1).</p>         | \![](<images/index of nodes - vector z.jpg>)              |
|                                                         | AKCE                                                                                  |                                                                      |
| \![](<images/Vector normalized.jpg>)     | <p><strong>Vector.Normalized</strong><br>Získá normalizovanou verzi vektoru.</p>      | \![](<images/index of nodes - vector normalized.jpg>)     |

## CoordinateSystem

|                                                                           |                                                                                                                                                                                                    |                                                                                              |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
|                                                                           | TVORBA                                                                                                                                                                                             |                                                                                              |
| \![](<images/Coordinate system by origin.jpg>)                 | <p><strong>CoordinateSystem.ByOrigin</strong><br>Vytvoří systém CoordinateSystem s počátkem ve vstupním bodu, s osami X a Y nastavenými jako osy X a Y v GSS.</p>                                               | \![](<images/index of nodes - coordinates system by origin.jpg>)                  |
| \![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) | <p><strong>CoordinateSystem.ByCylindricalCoordinates</strong><br>Vytvoří systém CoordinateSystem v zadaných válcových souřadnicových parametrech s ohledem na zadaný souřadnicový systém.</p> | \![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) |

## Operátory

|                                                  |                                                                                                                         |                                                               |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| ![](<images/addition.jpg>)       | <p><strong>+</strong><br>Součet</p>                                                                                   | \![](<images/index of nodes - addition.jpg>)       |
| ![](<images/Subtraction.jpg>)    | <p><strong>-</strong><br>Odečítání</p>                                                                                | \![](<images/index of nodes - subtraction.jpg>)    |
| ![](<images/Multiplication.jpg>) | <p><strong>*</strong><br>Součin</p>                                                                             | \![](<images/index of nodes - multiplication.jpg>) |
| ![](<images/Division.jpg>)       | <p><strong>/</strong><br>Podíl</p>                                                                                   | \![](<images/index of nodes - division.jpg>)       |
| ![](images/modular.jpg)         | <p><strong>%</strong><br>Modulární dělení nalezne zbytek prvního vstupu po dělení druhým vstupem.</p> | \![](<images/index of nodes - %.jpg>)              |
| \![](<images/index of nodes - less than.jpg>)        | <p><strong><</strong><br>Menší než</p>                                                                             | \![](<images/index of nodes - less than.jpg>)      |
| \![](<images/greater than.jpg>)   | <p><strong>></strong><br>Větší než</p>                                                                               | \![](<images/index of nodes - greater than.jpg>)   |
| ![](<images/==.jpg>)             | <p><strong>==</strong><br>Zkoušky rovnosti pro rovnost mezi dvěma hodnotami.</p>                                           | \![](<images/index of nodes - ==.jpg>)             |
