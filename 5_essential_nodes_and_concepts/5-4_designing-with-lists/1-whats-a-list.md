# Co je to seznam

### Co je to seznam?

Seznam je kolekce prvků nebo položek. Vezměte si například trs banánů. Každý banán je položka v seznamu (nebo v trsu). Je jednodušší sebrat celý trs banánů, než brát každý banán jednotlivě, a to samé platí pro seskupení prvků podle parametrických vztahů v datové struktuře.

![Banány](../images/5-4/1/Bananas\_white\_background\_DS.jpg)

> Autor fotografie: [Augustus Binu](https://commons.wikimedia.org/wiki/File:Bananas\_white\_background\_DS.jpg?fastcci\_from=11404890\&c1=11404890\&d1=15\&s=200\&a=list).

Při nákupu potravin naskládáme všechny zakoupené položky do tašky. Tato taška je také seznamem. Pokud chceme vyrobit banánový chléb, potřebujeme 3 trsy banánů (chceme vyrobit _hodně_ banánového chleba). Taška představuje seznam trsů banánů a každý trs představuje seznam banánů. Taška je seznam seznamů (dvourozměrný) a trs banánů je seznam (jednorozměrný).

V aplikaci Dynamo jsou data seznamu seřazena a první položka v každém seznamu má index „0“. Níže rozebereme způsob, jak v aplikaci Dynamo definovat seznamy a jak spolu více seznamů vzájemně souvisí.

### Nulové indexy

Jedna věc, která se může zdát podivnou, je, že první index seznamu je vždy 0, nikoli 1. Čili pokud je řeč o první položce seznamu, ve skutečnosti máme na mysli položku s indexem 0.

Pokud byste například měli spočítat prsty na pravé ruce, existuje šance, že byste napočítali od 1 do 5. Pokud byste však vložili prsty do seznamu, aplikace Dynamo by jim přiřadila index 0 až 4. Ačkoli se toto může zdát začátečníkům v programování velmi zvláštní, nulové indexy jsou ve většině výpočetních systémů běžnou praxí.

Všimněte si, že v seznamu stále máme 5 položek; je to proto, že seznam používá systém počítání od nuly. A položky uložené v seznamu nemusí být jen čísla. Mohou to být položky jakéhokoli datového typu, který aplikace Dynamo podporuje, například body, křivky, povrchy, rodiny atd.

![](<../images/5-4/1/what's a list - zero based indices.jpg>)

> a. Index
>
> b. Bod
>
> c. Položka

Nejjednodušším způsobem, jak se je možné se dívat na datový typ uložený v seznamu je nejčastěji propojení uzlu Watch s výstupem jiného uzlu. Ve výchozím nastavení uzel Watch automaticky zobrazí všechny indexy na levé straně seznamu a data zobrazí vpravo.

Tyto indexy jsou velice důležitým prvkem při práci se seznamy.

### Vstupy a výstupy

Vstupy a výstupy náležející k seznamům se liší podle toho, jaký uzel aplikace Dynamo se použije. Jako příklad použijte seznam bodů a připojte tento výstup ke dvěma různým uzlům aplikace Dynamo: **PolyCurve.ByPoints** a **Circle.ByCenterPointRadius**:

![Input Examples](<../images/5-4/1/what's a list - inputs and outputs.jpg>)

> 1. Vstup _points_ uzlu **PolyCurve.ByPoints** hledá strukturu _„Point[]“_. Tato struktura představuje seznam bodů.
> 2. Výstup uzlu **PolyCurve.ByPoints** je samostatný objekt PolyCurve vytvořený ze seznamu pěti bodů.
> 3. Vstup _centerPoint_ uzlu **Circle.ByCenterPointRadius** žádá o objekt _„Point“_.
> 4. Výstup uzlu **Circle.ByCenterPointRadius** je seznam pěti kružnic, jejichž středy odpovídají původnímu seznamu bodů.

Vstupní data pro uzly **PolyCurve.ByPoints** a **Circle.ByCenterPointRadius** jsou stejná, uzel **PolyCurve.ByPoints** však předává jeden objekt PolyCurve, zatímco uzel **Circle.ByCenterPointRadius** předává 5 kružnic se středy v každém bodu. Je to intuitivní: Objekt PolyCurve je vykreslen jako křivka spojující 5 bodů, zatímco kružnice v každém bodu vytvářejí jinou kružnici. Co se tedy děje s daty?

Po přejetí umístění kurzoru nad vstupem _points_ uzlu **Polycurve.ByPoints** zjistíte, že vstup hledá strukturu _„Point[]“_. Všimněte si závorek na konci. To představuje seznam bodů a k vytvoření objektu PolyCurve je třeba na vstupu seznam pro každý objekt PolyCurve. Tento uzel proto zhušťuje každý seznam do jednoho objektu PolyCurve.

Na druhou stranu vstup _centerPoint_ uzlu **Circle.ByCenterPointRadius** žádá objekt _„Point“_. Tento uzel hledá jeden bod jako položku k definování středu kružnice. Proto ze vstupních dat vznikne pět kružnic. Rozlišování těchto vstupů v aplikaci Dynamo vám pomůže pochopit, jak uzly při zpracování dat fungují.

### Vázání

Porovnávání dat je problém bez čistého řešení. Dochází k němu, pokud má uzel přístup různě velkým vstupům. Změna algoritmu porovnávání dat může vést k naprosto odlišným výsledkům.

Představte si uzel, který tvoří segmenty úseček mezi body (**Line.ByStartPointEndPoint**). Bude mít dva vstupní parametry, které oba předávají souřadnice bodů:

#### Nejkratší seznam

Nejjednodušším způsobem je spojovat vstupy jedna ku jedné, dokud jeden z datových proudů nedojde na konec. Tomuto se říká algoritmus „Nejkratší seznam“. Jedná se o výchozí chování uzlů aplikace Dynamo:

![](<../images/5-4/1/what's a list - lacing - shortest.jpg>)

#### Nejdelší seznam

Algoritmus „Nejdelší seznam“ připojuje vstupy a opakovaně využívá prvky tak dlouho, dokud všechny datové proudy nedojdou na konec:

![](<../images/5-4/1/what's a list - lacing - longest.jpg>)

#### Kartézský součin

Metoda „Kartézský součin“ provede všechna možná připojení:

![](<../images/5-4/1/what's a list - lacing - cross.jpg>)

Jak vidíte, existují různé způsoby kreslení čar mezi těmito množinami bodů. Možnosti vázání naleznete po kliknutí pravým tlačítkem na střed uzlu a výběru nabídky „Vázání“.

![](<../images/5-4/1/what's a list - right click lacing opt.jpg>)

## Cvičení

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/5-4/1/Lacing.dyn" %}

Pomocí tohoto základního souboru znázorníme níže operace vázání tím, že definujeme nejkratší seznam, nejdelší seznam a kartézský součin.

U uzlu **Point.ByCoordinates** se změní vázání, ale nic jiného se u grafu výše nezmění.

### Nejkratší seznam

Pokud jako možnost vázání vyberete _nejkratší seznam_ (což je také výchozí možnost), získáte základní diagonální čáru složenou z pěti bodů. Pět bodů je délka kratšího seznamu, takže vázání nejkratšího seznamu se zastaví, jakmile dorazí na konec jednoho ze seznamů.

![Input Examples](<../images/5-4/1/what's a list - lacing exercise 01.jpg>)

### **Nejdelší seznam**

Změnou vázání na _nejdelší seznam_ získáte diagonální čáru, která se vertikálně rozšíří. Poslední položka v seznamu 5 položek bude opakována tak dlouho, dokud nebude dosaženo délky delšího seznamu, což je stejné chování jako metoda koncepčního diagramu.

![Input Examples](<../images/5-4/1/what's a list - lacing exercise 02.jpg>)

### **Kartézský součin**

Změnou vázání na _Kartézský součin_ získáte všechny kombinace mezi všemi seznamy, což vytvoří osnovu bodů o rozměrech 5x10. Jedná se o datovou strukturu ekvivalentní ke kartézskému součinu, jak je ukázáno v koncepčním diagramu výše, až na to, že data jsou nyní seznamy seznamů. Pokud připojíme objekt PolyCurve, uvidíme, že každý seznam je definovaný hodnotou X, což znamená, že máme řadu vertikálních čar.

![Input Examples](<../images/5-4/1/what's a list - lacing exercise 03.jpg>)
