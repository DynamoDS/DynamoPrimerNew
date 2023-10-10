# Barva

Barva je skvělý datový typ k tvorbě působivých vizuálních prvků a k rozlišení částí výstupu ve vizuálním programu. Při práci s abstraktními daty a proměnlivými čísly je někdy obtížné zjistit, co se do jaké míry mění. Toto je skvělé využití pro barvy.

### Tvorba barev

Barvy v aplikaci Dynamo jsou tvořeny pomocí vstupů ARGB. To odpovídá kanálům Alfa, Červená, Zelená a Modrá. Alfa představuje _průhlednost_ barvy, zatímco ostatní tři se používají jako primární barvy k tvorbě celého spektra barev.

| Ikona                                     | Název (Syntaxe)                 | Vstupy  | Výstupy |
| ---------------------------------------- | ----------------------------- | ------- | ------- |
| ![](<../images/5-1/ColorbyARGB (2).jpg>) | Barva ARGB (**Color.ByARGB**) | A,R,G,B | barva   |

### Dotazování se na hodnoty barev

Barvy v níže uvedené tabulce se dotazují na vlastnosti, které se používají k definování barvy: Alfa, Červená, Zelená a Modrá. Všimněte si, že uzel Color.Components nám předá všechny čtyři kanály v samostatných výstupech, což je lepší k dotazování vlastností barvy.

| Ikona                                              | Název (Syntaxe)                     | Vstupy | Výstupy    |
| ------------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](<../images/5-1/ColorAlpha(1)(1) (2) (2).jpg>) | Alfa (**Colour.Alpha**)           | barva  | A          |
| ![](../images/5-1/ColorRed.jpg)                   | Červená (**Colour.Red**)               | barva  | R          |
| ![](<../images/5-1/ColorGreen(1)(1) (2) (1).jpg>) | Zelená (**Color.Green**)           | barva  | G          |
| ![](../images/5-1/ColorBlue.jpg)                  | Modrá (**Color.Blue**)             | barva  | B          |
| ![](<../images/5-1/ColorComponent (2).jpg>)       | Komponenty (**Colour.Components**) | barva  | A,R,G,B |

Barvy v tabulce níže odpovídají **barevnému prostoru HSB**. Rozdělení barvy na odstín, sytost a jas je pravděpodobně intuitivnější pro interpretaci barvy: Jaká barva by to měla být? Jak moc sytá má být? A jak moc světlá, či tmavá má být? Toto je rozbor odstínu, respektive sytosti, respektive jasu.

| Ikona                                         | Název (Syntaxe)                     | Vstupy | Výstupy    |
| -------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](../images/5-1/ColorHue.jpg)              | Odstín (**Colour.Hue**)               | barva  | Odstín        |
| ![](<../images/5-1/ColorSaturation (2).jpg>) | Sytost (**Colour.Saturation**) | barva  | Sytost |
| ![](<../images/5-1/ColorBrightness (2).jpg>) | Jas (**Colour.Brightness**) | barva  | Jas |

### Rozsah barev

Rozsah barev je podobný uzlu **Remap Range** ve cvičení [\#part-ii-from-logic-to-geometry](3-logic.md#part-ii-from-logic-to-geometry "mention"): Přemapuje seznam čísel do jiné domény. Místo mapování do _číselné_ domény však mapuje _barevný gradient_ podle vstupních čísel v rozsahu od 0 do 1.

Aktuální uzel funguje dobře, pokud však všechno začne fungovat napoprvé, něco je zřejmě špatně. Nejlepší způsob, jak se s barevným gradientem seznámit, je provést interaktivní test. Nyní provedeme rychlé cvičení a probereme, jak nastavit gradient s výstupními barvami odpovídajícími číslům.

![](../images/5-3/5/color-colorrange.jpg)

> 1. Definujte tři barvy: Pomocí **bloku kódu** definujte _červenou, zelenou_ a _modrou_ zadáním příslušných kombinací hodnot _0_ a _255_.
> 2. **Vytvořte seznam:** Slučte tři barvy do jednoho seznamu.
> 3. Definujte indexy: Vytvořte seznam k definování umístění uzlů jednotlivých barev (v rozsahu od 0 do 1). Všimněte si, že u zelené barvy je hodnota 0.75. Tímto se zelená barva umístí do 3/4 přes vodorovný gradient na posuvníku rozsahu barev.
> 4. **Blok kódu:** Zadejte hodnoty (mezi 0 a 1), které chcete převést na barvy.

### Náhled barvy

Uzel **Display.ByGeometry** umožňuje vybarvit geometrii ve výřezu aplikace Dynamo. Toto je užitečné při oddělení různých typů geometrie, předvedení parametrické koncepce nebo definování legendy analýzy pro simulaci. Vstupy jsou jednoduché: geometrie a barva. Vstup color je za účelem vytvoření gradientu jako na obrázku výše připojen k uzlu **Color** **Range**.

![](../images/5-3/5/color-colorpreview.jpg)

### Barva na površích

Uzel **Display.BySurfaceColor** umožňuje mapovat data po celém povrchu pomocí barvy. Tato funkce nabízí určité zajímavé možnosti vizualizace dat obdržených přes diskrétní analýzu, jako je analýza slunečného světla, energetická analýza a analýza blízkosti. Použití barvy na povrch v aplikaci Dynamo je podobné jako použití textury na materiál v jiných prostředích CAD. Krátké cvičení níže znázorňuje použití tohoto nástroje.

![](../images/5-3/5/12\(1\).jpg)

## Cvičení

### Základní šroubovice s barvami

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/5-3/5/Building Blocks of Programs - Color.dyn" %}

Toto cvičení je zaměřeno na parametrické řízení barvy rovnoběžně s geometrií. Geometrie je základní šroubovice, kterou níže definujeme pomocí **bloku kódu**. Jedná se o rychlý a snadný způsob tvorby parametrické funkce; vzhledem k tomu, že se soustředíme na barvu (místo geometrie), můžeme efektivně vytvořit šroubovici pomocí bloku kódu, aniž by došlo k zaplnění kreslicí plochy. K čím složitějším materiálům se příručka Primer dostane, tím častěji se bude používat blok kódu.

![](../images/5-3/5/color-basichelixwithcolors01.jpg)

> 1. **Blok kódu:** Definujte dva bloky kódu s výše uvedenými vzorci. Toto je rychlá parametrická metoda tvorby spirály.
> 2. **Point.ByCoordinates:** Připojte tři výstupy z bloku kódu k souřadnicím uzlu.

Nyní je vidět pole bodů tvořících šroubovici. Dalším krokem je tvorba křivky procházející body, aby bylo možné vizualizovat šroubovici.

![](../images/5-3/5/color-basichelixwithcolors02.jpg)

> 1. **PolyCurve.ByPoints:** Připojte výstup **Point.ByCoordinates** ke vstupu _points_ u uzlu. Vznikne šroubovitá křivka.
> 2. **Curve.PointAtParameter:** Připojte výstup **PolyCurve.ByPoints** ke vstupu _curve_. Účelem tohoto kroku je vytvořit parametrický bod atraktoru, který se posouvá podél křivky. Vzhledem k tomu, že křivka vyhodnocuje bod v parametru, bude nutné zadat hodnotu _param_ v rozmezí od 0 do 1.
> 3. **Posuvník čísel:** Po přidání na kreslicí plochu změňte hodnotu _min_ na _0.0_, hodnotu _max_ na _1.0_ a hodnotu _step_ na _0.01_. Připojte výstup posuvníku ke vstupu _param_ u uzlu **Curve.PointAtParameter**. Nyní je vidět bod podél délky šroubovice, který je vyjádřen procentem posuvníku (0 v počátečním bodě, 1 v koncovém bodě).

Po vytvoření referenčního bodu nyní porovnáme vzdálenost od referenčního bodu k původním bodům, čímž se definuje šroubovice. Tato hodnota vzdálenosti bude řídit geometrii i barvu.

![](../images/5-3/5/color-basichelixwithcolors03.jpg)

> 1. **Geometry.DistanceTo:** Připojte výstup **Curve.PointAtParameter** ke _vstupu_. Připojte uzel **Point.ByCoordinates** ke vstupu geometrie.
> 2. **Watch:** Výsledný výstup zobrazuje seznam vzdáleností od každého bodu šroubovice k referenčnímu bodu podél křivky.

Dalším krokem je řízení parametrů pomocí seznamu vzdáleností od bodů šroubovice k referenčnímu bodu. Pomocí těchto hodnot vzdáleností se definují poloměry řady koulí podél křivky. Aby se koule udržely ve vhodné velikosti, je nutné _přemapovat_ hodnoty vzdálenosti.

![](../images/5-3/5/color-basichelixwithcolors04.jpg)

> 1. **Math.RemapRange:** Připojte výstup **Geometry.DistanceTo** ke vstupu čísel.
> 2. **Blok kódu:** připojte blok kódu s hodnotou _0.01_ ke vstupu _newMin_ a blok kódu s hodnotou _1_ ke vstupu _newMax_.
> 3. **Watch:** Připojte výstup **Math.RemapRange** k jednomu uzlu a výstup **Geometry.DistanceTo** k jinému. Porovnejte výsledky.

V tomto kroku došlo k přemapování seznamu vzdáleností do menšího rozsahu. Hodnoty _newMin_ a _newMax_ je možné upravit podle libosti. Hodnoty se přemapují a budou mít stejný _poměr rozložení_ v rámci celé domény.

![](../images/5-3/5/color-basichelixwithcolors05.jpg)

> 1. **Sphere.ByCenterPointRadius:** Připojte výstup **Math.RemapRange** ke vstupu _radius_ a původní výstup **Point.ByCoordinates** připojte ke vstupu _centerPoint_.

Změňte hodnotu posuvníku čísel a sledujte, jak se aktualizuje velikost koulí. Vytvořili jsme parametrický objekt.

![](../images/5-3/5/color-basichelixwithcolors06.gif)

Velikost koulí ukazuje parametrické pole definované referenčním bodem podél křivky. Použijeme stejnou koncepci u poloměru koule, abychom mohli řídit jejich barvu.

![](../images/5-3/5/color-basichelixwithcolors07.jpg)

> 1. **Color Range:** Přidejte horní část kreslicí plochy. Při přejetí kurzoru nad vstupem _value_ si všimněte, že požadovaná čísla jsou v rozsahu 0 až 1. Čísla z výstupu **Geometry.DistanceTo** je nutné přemapovat, aby byla kompatibilní s touto doménou.
> 2. **Sphere.ByCenterPointRadius:** V tuto chvíli vypněte náhled u tohoto uzlu. (_Klikněte pravým tlačítkem myši > Náhled_.)

![](../images/5-3/5/color-basichelixwithcolors08.jpg)

> 1. **Math.RemapRange:** Tento proces by vám měl být známý. Připojte výstup **Geometry.DistanceTo** ke vstupu čísel.
> 2. **Blok kódu:** Podobně jako v předchozím kroku vytvořte hodnotu _0_ pro vstup _newMin_ a hodnotu _1_ pro zadání _newMax_. Všimněte si, že v tomto případě je možné definovat dva výstupy z jednoho bloku kódu.
> 3. **Color Range:** Připojte výstup **Math.RemapRange** ke vstupu _value_.

![](../images/5-3/5/color-basichelixwithcolors09.jpg)

> 1. **Color.ByARGB:** Toto je akce, pomocí které se vytvoří dvě barvy. I přesto, že tento proces může vypadat neobvykle, je stejný jako u barev RGB v jiném softwaru, jen se přitom využije vizuální programování.
> 2. **Blok kódu:** Vytvořte dvě hodnoty _0_ a _255_. Připojte dva výstupy ke dvěma vstupům **Color.ByARGB** podle výše uvedeného obrázku (případně vytvořte jakékoli dvě požadované barvy).
> 3. **Color Range:** Vstup _colors_ vyžaduje seznam barev. Tento seznam je potřeba vytvořit ze dvou barev vytvořených v předchozím kroku.
> 4. **List.Create:** Slučte dvě barvy do jednoho seznamu. Připojte výstup ke vstupu _colors_ u uzlu **Color Range**.

![](../images/5-3/5/color-basichelixwithcolors10.jpg)

> 1. **Display.ByGeometryColor:** Připojte položku **Sphere.ByCenterPointRadius** ke vstupu _geometry_ a uzel _Color Range_ připojte ke vstupu _color_. Nyní máme hladký gradient v celé doméně křivky.

Pokud změníme hodnotu **posuvníku čísel** z dřívější definice, barvy a velikosti se aktualizují. V tomto případě spolu barvy a velikost poloměru přímo souvisí: nyní existuje vizuální propojení mezi dvěma parametry.

![](../images/5-3/5/color-basichelixwithcolors11.gif)

### Cvičení barev na površích

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/5-3/5/BuildingBlocks of Programs - ColorOnSurface.zip" %}

Nejprve je třeba vytvořit (nebo odkázat) povrch, který se použije jako vstup pro uzel **Display.BySurfaceColor**. V tomto příkladu šablonujeme mezi sinusovou a kosinusovou křivkou.

![](../images/5-3/5/color-coloronsurface01.jpg)

> 1. Tato skupina uzlů vytváří body podél osy Z a poté je posunuje podle sinových a kosinových funkcí. Pomocí dvou seznamů bodů se poté vygenerují křivky NURBS.
> 2. **Surface.ByLoft**: Vygenerujte interpolovaný povrch mezi křivkami NURBS v seznamu.

![](../images/5-3/5/color-coloronsurface02.jpg)

> 1. **File Path**: Vyberte soubor obrázku, který se bude vzorkovat pro následná data pixelů.
> 2. Pomocí uzlu **File.FromPath** převeďte cestu k souboru na soubor a poté jej předejte do uzlu **Image.ReadFromFile**. Tím vytvoříte obrázek pro vzorkování.
> 3. **Image.Pixels**: Zadejte obrázek a zadejte hodnotu vzorku, která se má použít ve směru rozměrů X a Y obrázku.
> 4. **Posuvník**: Zadejte hodnoty vzorků pro uzel **Image.Pixels**
> 5. **Display.BySurfaceColor**: Namapujte pole hodnot barev přes celý povrch podél směru X, respektive Y.

Podrobný náhled výstupního povrchu s rozlišením 400x300 vzorků.

![](../images/5-3/5/color-coloronsurface03.jpg)
