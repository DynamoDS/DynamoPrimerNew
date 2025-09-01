# Přizpůsobení

Zatímco v předchozích částech byl kladen důraz na úpravu základního objemu budovy, tato část se zabývá více propojením aplikací Dynamo a Revit, konkrétně úpravou velkého počtu prvků najednou. Přizpůsobení ve velkém měřítku je čím dál složitější tím, jak datové struktury vyžadují čím dál pokročilejší operace seznamů. Základní principy za jejich prováděním však zůstávají v podstatě stejné. Následuje popis několika příležitostí k analýze v sadě adaptivních komponent.

### Umístění bodu

Řekněme, že byl vytvořen určitý počet adaptivních komponent a vy chcete upravit parametry podle jejich umístění bodů. Body by mohly například řídit parametr tloušťky, který souvisí s plochou prvku. Nebo by mohly řídit parametr průhlednosti, který souvisí s vystavením slunečnímu záření po celý rok. Aplikace Dynamo umožňuje analýzu parametrů v několika snadných krocích, přičemž základní verze analýzy je popsána ve cvičení níže.

![](images/5/customizing-pointlocation.jpg)

> Zadejte dotaz na adaptivní body vybrané adaptivní komponenty pomocí uzlu **AdaptiveComponent.Locations**. Toto vám umožní pracovat během analýzy s abstrahovanou verzí prvku aplikace Revit.

Extrahování umístění bodu adaptivních komponent vám umožní spustit celou řadu analýz pro daný prvek. Například čtyřbodová adaptivní komponenta umožní studovat odchylku od roviny u daného panelu.

### Analýza orientace slunečního záření

![](images/5/customizing-solarorientationanalysis.jpg)

> Pomocí přemapování můžete namapovat sadu dat na rozsah parametru. Jedná se o základní nástroj používaný v parametrických modelech a je znázorněn v níže uvedeném cvičení.

V aplikaci Dynamo je možné pomocí umístění bodů adaptivních komponent vytvořit nejlépe přizpůsobenou rovinu pro každý prvek. Také můžete zadat dotaz na pozici slunce v souboru aplikace Revit a studovat orientaci roviny vzhledem ke slunci v porovnání s jinými adaptivními komponentami. V níže uvedeném cvičení toto nastavíme vytvořením algoritmických střech.

## Cvičení

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="datasets/5/Revit-Customizing.zip" %}

Toto cvičení rozšiřuje techniky znázorněné v předchozí části. V tomto případě definujeme parametrický povrch z prvků aplikace Revit, dále vytvoříme instance čtyřbodových adaptivních komponent a poté je upravíme podle orientace vzhledem ke slunci.

![](images/5/customizing-exercise01.jpg)

> 1. Začněte výběrem dvou hran pomocí uzlu _Select Edge_. Tyto dvě hrany jsou dvě dlouhá rozpětí atria.
> 2. Spojte dvě hrany do jednoho seznamu pomocí uzlu _List.Create_.
> 3. Vytvořte mezi dvěma hranami povrch pomocí uzlu _Surface.ByLoft_.

![](images/5/customizing-exercise02.jpg)

> 1. Pomocí _bloku kódu_ definujte rozsah od 0 do 1 s 10 rovnoměrně rozmístěnými hodnotami: `0..1..#10;`.
> 2. Připojte _blok kódu_ ke vstupům *u* a _v_ uzlu _Surface.PointAtParameter_ a připojte uzel _Surface.ByLoft_ ke vstupu _surface_. Klikněte pravým tlačítkem na uzel a změňte _vázání_ na _Kartézský součin_. Tím se vytvoří osnova bodů na povrchu.

Tato osnova bodů slouží jako řídicí body parametricky definovaného povrchu. Je třeba extrahovat pozice u a v každého z těchto bodů, aby bylo možné je připojit k parametrickému vzorci a zachovat stejnou datovou strukturu. Toho dosáhnete zadáním dotazu na umístění parametrů bodů, které jste právě vytvořili.

![](images/5/customizing-exercise03.jpg)

> 1. Přidejte na kreslicí plochu uzel _Surface.ParameterAtPoint_ a připojte vstupy, jak je znázorněno výše.
> 2. Zadejte dotaz na hodnoty _u_ těchto parametrů pomocí uzlu UV.U.
> 3. Zadejte dotaz na hodnoty _v_ těchto parametrů pomocí uzlu UV.V.
> 4. Výstup zobrazuje odpovídající hodnoty _u_ a _v_ pro každý bod povrchu. Nyní existuje rozsah od _0_ do _1_ pro každou hodnotu ve správné datové struktuře, čili je vše připraveno k použití parametrického algoritmu.

![](images/5/customizing-exercise04.jpg)

> 1. Přidejte na kreslicí plochu _blok kódu_ a zadejte tento kód: `Math.Sin(u*180)*Math.Sin(v*180)*w;`. Jedná se o parametrickou funkci, která z plochého povrchu vytvoří sinusový oblouk.
> 2. Připojte uzel _UV.U_ ke vstupu _u_ a UV.V ke vstupu _v_.
> 3. Vstup _w_ představuje _amplitudu_ tvaru, takže k němu připojíme _posuvník čísel_.

![](images/5/customizing-exercise05.jpg)

> 1. Nyní máme seznam hodnot definovaných algoritmem. Pomocí tohoto seznamu hodnot přesuňte body nahoru ve směru _+Z_. Pomocí uzlu _Geometry.Translate_ připojte *blok kódu* ke vstupu _zTranslation_ a uzel _Surface.PointAtParameter_ připojte ke vstupu _geometry_. Nyní by se v náhledu aplikace Dynamo měly zobrazit nové body.
> 2. Nakonec vytvořte povrch pomocí uzlu _NurbsSurface.ByPoints_ tím, že ke vstupu bodů připojíte uzel z předchozího kroku. Nyní máme parametrický povrch. Můžete volně přetahovat posuvník a pozorovat, jak se oblouk zmenšuje a zvětšuje.

U parametrického povrchu nyní chceme definovat způsob, jak z něj vytvořit panely a umístit tak čtyřbodové adaptivní komponenty do pole. Aplikace Dynamo nemá ve výchozím stavu funkci k vytvoření panelů z povrchu, čili je třeba prozkoumat komunitu a najít užitečné balíčky pro aplikaci Dynamo.

![](images/5/customizing-exercise06.jpg)

> 1. Přejděte do nabídky _Balíčky > Vyhledat balíček_.
> 2. Vyhledejte řetězec _LunchBox_ a nainstalujte si balíček _LunchBox for Dynamo_. Jedná se o skutečně užitečnou sadu nástrojů pro operace geometrií, jako je právě tato.

> 1. Po stažení máte nyní plný přístup k sadě LunchBox. Vyhledejte řetězec _Quad Grid_ a vyberte položku _LunchBox Quad Grid By Face_. Připojte parametrický povrch ke vstupu _surface_ a nastavte podíly _U_ a _V_ na hodnotu _15_. Nyní byste v náhledu aplikace Dynamo měli vidět povrch se čtyřmi panely.

> Pokud vás zajímá nastavení tohoto balíčku, klikněte dvakrát na uzel _Lunch Box_ a zjistěte, jak funguje.

> Opět v aplikaci Revit se v rychlosti podíváme na adaptivní komponentu, kterou používáme. Není třeba ji stále sledovat, ale toto je o panel střechy, od kterého vytvoříme instanci. Jedná se o čtyřbodovou adaptivní komponentu, která je jednoduchou reprezentací systému ETFE. Otvor středového vybrání je u parametru s názvem _ApertureRatio_.

> 1. Chystáte se vytvořit instanci geometrie v aplikaci Revit, proto se ujistěte, že je řešič aplikace Dynamo nastaven na možnost _Ruční_.
> 2. Přidejte na kreslicí plochu uzel _Family Types_ a vyberte položku _ROOF-PANEL-4PT_.
> 3. Přidejte na kreslicí plochu uzel _AdaptiveComponent.ByPoints_ a připojte _Panel Pts_ z výstupu balíčku _LunchBox Quad Grid by Face_ ke vstupu _points_. Připojte uzel _Family Types_ ke vstupu _familySymbol_.
> 4. Klikněte na možnost _Spustit_. Během tvorby geometrie bude aplikace Revit chvíli _zaneprázdněna_. Pokud to trvá příliš dlouho, snižte hodnotu v _bloku kódu z 15_ na nižší hodnotu. Tím se sníží počet panelů na střeše.

_Poznámka: Pokud aplikaci Dynamo trvá výpočet uzlů dlouho, bude možná užitečné využít funkce uzlu „freeze“, která pozastaví provádění operací aplikace Revit, zatímco vyvíjíte graf. Další informace o zmrazení uzlů najdete v části Zmrazení v kapitole Tělesa._

> Zpátky v aplikaci Revit máme pole panelů na střeše.

> Po přiblížení se můžete blíže podívat na kvality jejich povrchů.

### Analýza

> 1. V rámci pokračování z předchozího kroku nastavte otvor každého panelu podle jeho vystavení slunci. Pokud přiblížíte pohled v aplikaci Revit a vyberete jeden panel, uvidíte na panelu vlastností, že je zde parametr s názvem _Poměr otvoru_. Rodina je nastavena, tak aby měl otvor rozsah zhruba od _0.05_ do _0.45_.

> 2. Pokud zapnete dráhu slunce, uvidíte v aplikaci Revit aktuální polohu slunce.

> 3. Na tuto polohu slunce se můžete odkázat pomocí uzlu _SunSettings.Current_.

1. Připojením nastavení slunce k uzlu _Sunsetting.SunDirection_ získejte vektor slunečního záření.
2. U výstupu _Panel Pts_ použitého k vytvoření adaptivních komponent aproximujte rovinu pro komponentu pomocí uzlu _Plane.ByBestFitThroughPoints_.
3. Zadejte dotaz na _normálu_ této roviny.
4. K výpočtu orientace slunečního záření použijte _skalární součin_. Skalární součin je vzorec, který určuje, jak mohou dva vektory být rovnoběžné nebo nerovnoběžné. Čili vezmete normálu roviny každé adaptivní komponenty a porovnáte ji s vektorem slunečního záření, čímž zhruba nasimulujete orientaci slunečního záření.
5. Použijte _absolutní hodnotu_ výsledku. Tím zajistíte, že bude skalární součin přesný, pokud je normála roviny otočena opačným směrem.
6. Klikněte na možnost _Spustit_.

> 1) Při pohledu na _skalární součin_ je vidět, že máme k dispozici velký rozsah čísel. Chceme použít jejich relativní rozložení, ale je potřeba zhustit čísla do vhodného rozsahu parametru _Poměr otvoru_, který chceme upravit.

1. K tomuto účelu se skvěle hodí uzel _Math.RemapRange_. Ten vezme vstupní seznam a přemapuje jeho hranice na dvě cílové hodnoty.
2. Definujte cílové hodnoty _0.15_ a _0.45_ v _bloku kódu_.
3. Klikněte na možnost _Spustit_.

> 1) Připojte přemapované hodnoty k uzlu _Element.SetParameterByName_.

1. Připojte řetězec _Aperture Ratio_ (Poměr otvoru) ke vstupu _parameterName_.
2. Připojte _adaptivní komponenty_ ke vstupu _element_.
3. Klikněte na možnost _Spustit_.

> Zpět v aplikaci Revit můžeme z dálky rozeznat vliv orientace slunečního záření na otvor v panelech ETFE.

> Pokud přiblížíte pohled, zjistíte, že panely ETFE jsou více uzavřené, pokud směřují ke slunci. Cílem je snížit míru přehřátí při vystavení slunečnímu záření. Pokud chcete vpustit dovnitř více světla na základě slunečního záření, prostě jen přepněte doménu v uzlu _Math.RemapRange_.

