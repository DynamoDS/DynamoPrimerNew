# Body

## Body v aplikaci Dynamo

### Co je to bod?

[Bod](5-3\_points.md#point-as-coordinates) není definován ničím jiným než jednou nebo více hodnotami, které se nazývají souřadnice. Počet hodnot souřadnic, které je potřeba definovat, závisí na souřadnicovém systému nebo kontextu, ve kterém se bod nachází.

### 2D a 3D bod

Nejběžnější typ bodu v aplikaci Dynamo existuje v našem trojrozměrném globálním souřadnicovém systému a má tři souřadnice \[X,Y,Z] (3D bod v aplikaci Dynamo).

![](<../images/5-2/3/points - 3d point in dynamo.jpg>)

2D bod v aplikaci Dynamo má dvě souřadnice \[X,Y].

![](<../images/5-2/3/points - 2d point in dynamo.jpg>)

### Bod na křivkách a površích

Parametry křivek i povrchů jsou spojité a přesahují hranu dané geometrie. Protože tvary, které definují parametrický prostor, se nacházejí v trojrozměrném globálním souřadnicovém systému, můžeme vždy převést parametrickou souřadnici do „Globální“ souřadnice. Například bod \[0.2, 0.5] na povrchu je stejný jako bod \[1.8, 2.0, 4.1] v globálních souřadnicích.

![](<../images/5-2/3/points - xyz vs coord sys vs uv.jpg>)

> 1. Bod v předpokládaných globálních souřadnicích XYZ
> 2. Bod relativní k danému souřadnicovému systému (válcový)
> 3. Bod jako souřadnice UV na povrchu

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/5-2/3/Geometry for Computational Design - Points.dyn" %}

## Podrobné informace...

Pokud je geometrie jazykem modelu, pak body jsou abecedou. Body jsou základem, na kterém je vytvořena veškerá další geometrie – k vytvoření křivky potřebujeme alespoň dva body, k vytvoření polygonu nebo plochy sítě potřebujeme alespoň tři body, a tak dále. Definování polohy, pořadí a vztahu mezi body (zkuste funkci sinus) umožňuje definovat geometrii vyššího řádu, například věci, které známe jako kružnice nebo křivky.

![Od bodu ke křivce](../images/5-2/3/PointsAsBuildingBlocks-1.jpg)

> 1. Kružnice vytvořená pomocí funkcí `x=r*cos(t)` a `y=r*sin(t)`.
> 2. Sinusoida vytvořená pomocí funkcí `x=(t)` a `y=r*sin(t)`.

### Bod jako souřadnice

Body mohou existovat také v dvojrozměrném souřadnicovém systému. Konvence má různý zápis písmen v závislosti na tom, s jakým prostorem pracujeme – můžeme použít \[X,Y] na rovině nebo \[U,V] na povrchu.

![Bod jako souřadnice](../images/5-2/3/Coordinates.jpg)

> 1. Bod v euklidovském souřadnicovém systému: \[X,Y,Z]
> 2. Bod v souřadnicovém systému parametru křivky: \[t]
> 3. Bod v souřadnicovém systému parametru povrchu: \[U,V]
