# Změny jazyka

V části Změny jazyka je uveden přehled aktualizací a úprav jazyka používaného v aplikaci Dynamo v jednotlivých verzích. Tyto změny můžou mít vliv na funkčnost, výkon a použití a tato příručka pomůže uživatelům pochopit, kdy a proč se těmto aktualizacím přizpůsobit.

## Změny jazyka aplikace Dynamo 2.0

1. Změna syntaxe list@level z "@-1" na "@L1.
* Nová syntaxe pro list@level, která používá list@L1 místo list@-1.
* Důvod: Sladění syntaxe kódu s náhledem/uživatelským rozhraním. Uživatelské testování ukazuje, že tato nová syntaxe je srozumitelnější.

2. Implementace typů Int a Double v TS pro sladění s typy aplikace Dynamo.

3. Nejsou povoleny přetížené funkce, jejichž argumenty se liší pouze kardinalitou.
* Staré grafy používající přetížení, která byla odebrána, by měly ve výchozím nastavení používat přetížení s vyšší úrovní.
* Důvod: Odstranění nejasností ohledně toho, která konkrétní funkce se provádí.

4. Zakázání povýšení pole při použití vodítek replikace.

5. Proměnné v imperativních blocích jsou místní pro obor imperativního bloku.
* Hodnoty proměnných definované uvnitř imperativních bloků kódu nebudou dotčeny změnami uvnitř imperativních bloků, které na ně odkazují.  

6. Proměnné nastaveny jako neměnné, aby se zakázala asociativní aktualizace v uzlech bloku kódu.

7. Všechny uzly uživatelského rozhraní jsou kompilovány jako statické metody.

8. Podpora návratových výrazů bez přiřazení.
* Výraz "=" není potřeba ani v definicích funkcí, ani v imperativním kódu.

9. Migrace starých názvů metod v uzlu bloku kódu.
* Mnoho uzlů bylo přejmenováno, aby se zvýšila jejich čitelnost a umístění v uživatelském rozhraní Prohlížeče knihoven.

10. Oddělení seznamu od slovníku.

----
Známé problémy:
- Konflikty jmenného prostoru v imperativních blocích způsobují zobrazení neočekávaných vstupních portů. Další informace najdete v tomto článku popisujícím [problém na Githubu](https://github.com/DynamoDS/Dynamo/issues/8796). Chcete-li tento problém obejít, definujte funkci mimo imperativní blok takto:
```
pnt = Autodesk.Point.ByCoordinates;
lne = Autodesk.Line.ByStartPointEndPoint;

[Imperative]
{
    x = 1;
    start = pnt(0,0,0);
    end = pnt(x,x,x);
    line = lne(start,end);
    return = line;
};
```

## Vysvětlení změn jazyka v aplikaci Dynamo 2.0

V aplikaci Dynamo verze 2.0 byla provedena řada vylepšení jazyka. Hlavním důvodem bylo zjednodušení jazyka. Důraz byl kladen na to, aby byl DesignScript srozumitelnější a snadněji použitelný a nabídnul větší výkon a flexibilitu s cílem zlepšit srozumitelnost pro koncové uživatele.

Následuje seznam změn ve verzi 2.0 spolu s vysvětlením:
* Zjednodušená syntaxe List@Level
* Přetížené metody s parametry, které se liší pouze úrovní, jsou neplatné. 
* Všechny uzly uživatelského rozhraní jsou kompilovány jako statické metody.
* Zakázání povýšení seznamu při použití vodítek replikace / vázáním.
* Proměnné v asociativních blocích jsou neměnné, aby se zabránilo asociativní aktualizaci.
* Proměnné v imperativních blocích jsou místní pro imperativní obor.
* Provedeno oddělení seznamů a slovníků.

## 1\. Zjednodušená syntaxe list@level syntax. 

Nová syntaxe pro list@level používá `list@L1` místo `list@-1` ![](../images/8-4/1/lang2_1.png)


## 2\. Přetížené funkce s parametry, které se liší pouze úrovní, jsou neplatné.
Přetížené funkce jsou problematické z několika důvodů:
* Přetížená funkce označená uzlem uživatelského rozhraní v grafu nemusí být stejným přetížením, které se provede za běhu.
* Rozlišení metod je nákladné a pro přetížené funkce nefunguje dobře.
* Je obtížné pochopit chování replikace u přetížených funkcí.

Vezměme si jako příklad funkci `BoundingBox.ByGeometry`. Ve starších verzích Dynamo byly dvě přetížené funkce, z nichž jedna přijímala argument s jednou hodnotou a druhá seznam geometrií:
```
BoundingBox BoundingBox.ByGeometry(geometry: Geometry) {...}
BoundingBox BoundingBox.ByGeometry(geometry: Geometry[]) {...}
```
Pokud uživatel přetáhl první uzel na kreslicí plochu a připojil seznam geometrií, očekávat by, že se spustí replikace, ale k tomu nikdy nedošlo, protože za běhu by místo toho bylo voláno druhé přetížení, jak je znázorněno na obrázku: ![](../images/8-4/1/lang2_2.png)
 
Ve verzi 2.0 jsme z tohoto důvodu zakázali přetížené funkce, které se liší pouze kardinalitou parametrů. To znamená, že u přetížených funkcí, které mají stejný počet a typy parametrů, ale mají jeden nebo více parametrů, které se liší pouze úrovní, vítězí vždy přetížení, které je definováno jako první, zatímco ostatní jsou překladačem vyřazena. Hlavní výhodou tohoto zjednodušení je zjednodušení logiky rozlišení metody díky rychlé cestě k výběru kandidátů na funkce.

V knihovně geometrie pro verzi 2.0 bylo první přetížení v příkladu `BoundingBox.ByGeometry` zrušeno a druhé bylo zachováno, takže pokud je uzel určen k replikaci, tj. používá se v kontextu prvního, je nutné jej použít s nejkratší (nebo nejdelší) možností vázání nebo v bloku kódu s vodítky replikace: 
```
BoundingBox.ByGeometry(geometry<1>);
```
V tomto příkladu vidíme, že uzel s vyšší úrovní lze použít v replikovaném i nereplikovaném volání, a proto je vždy upřednostněn před přetížením s nižší úrovní. Jako pravidlo se proto **autorům uzlů vždy doporučuje, aby vyřadili přetížení s nižší úrovní ve prospěch metod s vyšší úrovní**, aby překladač jazyka DesignScript vždy volal metodu s vyšší úrovní jako první a jedinou, kterou najde.

### Příklady:
V následujícím příkladu jsou definována dvě přetížení funkce `foo`. Ve verzi 1.x je nejednoznačné, které přetížení se spustí za běhu. Uživatel může očekávat, že se spustí druhé přetížení `foo(a:int, b:int)`. V takovém případě by se očekávalo, že se metoda bude třikrát replikovat a třikrát vrátí hodnotu `10`. Ve skutečnosti je vrácena jedna hodnota `10`, protože je místo toho voláno první přetížení s parametrem seznamu.

### Druhé přetížení je ve verzi 2.0 vynecháno:
Ve verzi 2.0 je to vždy první definovaná metoda, která je vybrána před ostatními. Kdo dřív přijde, ten dřív mele.

![](../images/8-4/1/lang2_3.png)

Pro každý z následujících případů bude použito první definované přetížení. Všimněte si, že toto chování je založeno čistě na pořadí definování funkcí, nikoli na úrovni parametrů, i když se u uživatelem definovaných uzlů a uzlů Zero Touch doporučuje upřednostňovat metody s parametry s vyšší úrovní.
```
1)
foo(a: int[], b: int); ✓
foo(a: int, b: int); ✕
```
```
2) 
foo(x: int, y: int); ✓
foo(x: int[], y: int[]); ✕
```
## 3\. Všechny uzly uživatelského rozhraní jsou kompilovány jako statické metody.
V aplikaci Dynamo verze 1.x byly uzly uživatelského rozhraní (bez bloků kódu) kompilovány do metod a vlastností instancí. Například uzel `Point.X` byl zkompilován do `pt.X` a uzel `Curve.PointAtParameter` byl zkompilován do `curve.PointAtParameter(param)`. Toto chování mělo dva problémy:

__A. Funkce, kterou představoval uzel uživatelského rozhraní, nebyla vždy stejná jako funkce, která byla spuštěna za běhu.__

Typickým příkladem je uzel `Translate`. Existuje více uzlů `Translate`, které přijímají stejný počet a typy argumentů, například: `Geometry.Translate`, `Mesh.Translate` a `FamilyInstance.Translate`. Vzhledem k tomu, že uzly byly kompilovány jako metody instancí, předání argumentu `FamilyInstance` do uzlu `Geometry.Translate` by překvapivě stále fungovalo, protože za běhu by odeslalo volání do metody instance `Translate` na `FamilyInstance`. To bylo pro uživatele zjevně zavádějící, protože uzel nedělal to, co podle svého názvu měl.

__B. Druhým problémem bylo, že metody instancí nefungovaly s heterogenními poli.__

Za běhu musí prováděcí modul zjistit, do které funkce má odesílat. Pokud je vstupem seznam, řekněme `list.Translate()`, bylo by chování následující: protože je nákladné procházet každý prvek v seznamu a vyhledávat metody podle jeho typu, logika rozlišení metody by jednoduše předpokládala, že cílový typ je stejný jako typ prvního prvku, a pokusila by se vyhledat metodu `Translate()` definovanou pro tento typ. V důsledku toho, pokud by první typ prvku neodpovídal cílovému typu metody (nebo by se jednalo o `null` nebo prázdný seznam), celý seznam by selhal, i kdyby v seznamu byly další typy, které by odpovídaly. 

Pokud by byl například do uzlu `Arc.CenterPoint` předán vstup seznamu s následujícími typy `[Arc, Line]`, výsledek by podle očekávání obsahoval středový bod oblouku a hodnotu `null` pro čáru. Pokud by však bylo pořadí obrácené, celý výsledek byl nulový, protože by první prvek neprošel kontrolou rozlišení metody:
### Dynamo 1.x: V rámci kontroly rozlišení metody je testován pouze první prvek seznamu vstupů
![](../images/8-4/1/lang2_4.png)
```
x = [arc, line];
y = x.CenterPoint; // y = [centerpoint, null] ✓
```
```
x = [line, arc];
y = x.CenterPoint; // y = null ✕
```
Ve verzi 2.0 jsou oba tyto problémy vyřešeny kompilací uzlů uživatelského rozhraní jako statických vlastností a statických metod. 

U statických metod je rozlišení metody za běhu přímočařejší a všechny prvky ve vstupním seznamu jsou iterovány. Příklad:

Sémantika `foo.Bar()` (metoda instance) musí zkontrolovat typ `foo` a také zkontrolovat, zda se jedná o seznam či nikoli, a poté jej porovnat s kandidátskými funkcemi. To je nákladné. Na druhou stranu sémantika `Foo.Bar(foo)` (statická metoda) potřebuje zkontrolovat pouze jednu funkci s parametrem typu `foo`!

Ve verzi 2.0 se stane toto:
* Uzel vlastnosti uživatelského rozhraní je zkompilován do statického getteru: Modul generuje statickou verzi getteru pro každou vlastnost. Například uzel `Point.X` je zkompilován do statického getteru `Point.get_X(pt)`. Všimněte si, že statický getter lze volat také pomocí jeho aliasu: `Point.X(pt)` v uzlu bloku kódu.
* Uzel metody uživatelského rozhraní je zkompilován do statické verze: Modul vygeneruje pro uzel odpovídající statickou metodu. Například uzel `Curve.PointAtParameter` se zkompiluje do `Curve.PointAtParameter(curve: Curve, parameter:double)` místo `curve.PointAtParameter(parameter)`. 

**Poznámka:** Touto změnou jsme neodstranili podporu metod instancí, takže existující metody instancí používané v uzlu bloku kódu, jako jsou `pt.X` a `curve.PointAtParameter(parameter)` ve výše uvedených příkladech, budou stále fungovat.

Tento příklad by dříve fungoval ve verzi 1.x, protože graf by se zkompiloval do `point.X;` a našel by vlastnost `X` u objektu point. Nyní ve verzi 2.0 selhává, protože zkompilovaný kód `Vector.X(point)` očekává pouze typ `Vector`:

![](../images/8-4/1/lang2_5.png)

### Výhody:
**Srozumitelnost:** Statické metody odstraní všechny nejasnosti ohledně toho, která metoda se spustí za běhu. Metoda vždy odpovídá uzlu uživatelského rozhraní použitému v grafu, jehož volání uživatel očekává.

**Kompatibilita:** Mezi kódem a vizuálním programem je lepší korelace.

**Instruktáž:** Předávání vstupů heterogenních seznamů uzlům má nyní za následek hodnoty jiné než null pro typy, které jsou uzlem přijaty, a hodnoty null pro typy, které uzel neimplementuje. Výsledky jsou předvídatelnější a lépe ukazují, které typy jsou pro uzel přípustné.

### Upozornění: Nevyřešené nejasnosti s přetíženými metodami

Vzhledem k tomu, že Dynamo obecně podporuje přetížení funkcí, může stále docházet k nejasnostem, pokud existuje jiná přetížená funkce se stejným počtem parametrů. Pokud například v následujícím grafu připojíme číselnou hodnotu ke vstupu `direction` uzlu `Curve.Extrude` a vektor ke vstupu `distance` uzlu `Curve.Extrude`, budou oba uzly nadále fungovat, což je neočekávané. V tomto případě, i když se uzly zkompilují do statických metod, modul stále nedokáže zjistit rozdíl za běhu a vybere jednu z nich v závislosti na typu vstupu. ![](../images/8-4/1/lang2_6.png)
 
### Vyřešené problémy:
Přechod na sémantiku statických metod přinesl následující vedlejší efekty, které stojí za to zmínit jako související změny jazyka ve verzi 2.0.

**1\. Ztráta polymorfního chování:**

Podívejme se na příklad z uzlů `TSpline` v `ProtoGeometry` (všimněte si, že `TSplineTopology` dědí ze základního typu `Topology`): Uzel `Topology.Edges`, který byl dříve zkompilován do metody instance `object.Edges`, je nyní zkompilován do statické metody `Topology.Edges(object)`. Předchozí volání by se polymorfně přeložilo na odvozenou metodu třídy `TsplineTopology.Edges` po odeslání metody přes typ objektu za běhu. 

![](../images/8-4/1/lang2_7.png)

Kdežto nové statické chování bylo nuceno volat metodu základní třídy `Topology.Edges`, V důsledku toho tento uzel vrátil základní třídu, objekty `Edge` místo odvozených objektů třídy typu `TSplineEdge`.
 
![](../images/8-4/1/lang2_8.png)

V důsledku toho došlo k regresi, protože navazující uzly `TSpline`, které očekávaly `TSplineEdges`, začaly selhávat. 

Problém byl vyřešen přidáním kontroly za běhu do logiky odesílání metody, která kontroluje typ instance proti typu nebo podtypu prvního parametru metody. V případě vstupního seznamu jsme zjednodušili metodu odesílání tak, aby jednoduše kontrolovala typ prvního prvku. Konečným řešením byl tedy kompromis mezi částečně statickým a částečně dynamickým vyhledáváním metod.

**Nové polymorfní chování ve verzi 2.0:**

![](../images/8-4/1/lang2_9.png)

V tomto případě, protože první prvek `a` je `TSpline`, je za běhu vyvolána odvozená metoda `TSplineTopology.Edges`. V důsledku toho vrátí `null` pro základní typ `Topology` pro vstup `b`. 

Ve druhém případě, protože prvním prvkem je obecný typ `Topology` na vstupu `b`, je volána základní metoda `Topology.Edges`. Protože metoda `Topology.Edges` přijímá jako vstup také odvozený typ `TSplineTopology`, `a` jako vstup vrátí `Edges`pro oba vstupy, `a` i `b`.

![](../images/8-4/1/lang2_10.png)
 
**2\. Regrese z vytváření nadbytečných vnějších seznamů**

Pokud jde o chování vodítek replikace, existuje mezi metodami instance a statickými metodami jeden hlavní rozdíl. U metod instance nejsou vstupy s jednou hodnotou s vodítky replikace povýšeny na seznamy, zatímco u statických metod povýšeny jsou.

Zvažte příklad uzlu `Surface.PointAtParameter` s křížovým vázáním a s jedním vstupem povrchu a poli hodnot parametrů `u` a `v`. Metoda instance se zkompiluje takto:
```
surface<1>.PointAtParameter(u<1>, v<2>);
```
Výsledkem je 2D pole bodů.
 
Statická metoda se zkompiluje takto:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
Výsledkem je 3D seznam bodů s nadbytečným vnějším seznamem.

Tento vedlejší účinek kompilace uzlů uživatelského rozhraní na statické metody by mohl potenciálně způsobit regresi v takových existujících případech použití. Tento problém byl vyřešen zakázáním povýšení vstupů s jednou hodnotou na seznam při použití vodítek replikace/vázání (viz následující bod).
 
**4\. Zakázání povýšení na seznam při použití vodítek replikace / vázáním.**

Ve verzi 1.x existovaly dva případy, kdy byly jednotlivé hodnoty povýšeny na seznamy:

* Když byly vstupy s nižší úrovní předány funkcím, které očekávaly vstupy s vyšší úrovní.
* Když byly vstupy s nižší úrovní předány funkcím, které očekávaly stejnou úroveň, ale vstupní argumenty byly doplněny vodítky replikace nebo používaly šněrování.

Ve verzi 2.0 již nepodporujeme druhý případ tím, že v takových scénářích bráníme povýšení na seznam.

V následujícím grafu verze 1.x si jedna úroveň vodítek replikace pro každou osu `y` a `z` vynutila povýšení pole o 1 úroveň pro každou z nich, což je důvod, proč měl výsledek 3 úrovně (po 1 pro osu `x`, `y` a `z`). Místo toho by uživatel očekával, že výsledek bude mít 1 úroveň, protože není zcela zřejmé, že by přítomnost vodítek replikace pro vstupy s jednou hodnotou přidala k výsledku další úrovně.
```
x = 1..5;
y = 0;
z = 0;
p = Point.ByCoordinates(x<1>, y<2>, z<3>); // cross-lacing
```

### Dynamo 1.x: 3D seznam bodů

![](../images/8-4/1/lang2_11.png)

Ve verzi 2.0 přítomnost vodítek replikace pro každý z argumentů s jednou hodnotou `y` a `z` nezpůsobí povýšení, jehož výsledkem je seznam, který má stejnou dimenzi jako vstupní 1D seznam pro `x`. 

### Dynamo 2.0: 1D seznam bodů

![](../images/8-4/1/lang2_12.png)

Touto změnou jazyka byla také vyřešena výše zmíněná regrese způsobená kompilací statické metody s generováním nadbytečných vnějších seznamů.

Pokračujeme-li ve stejném příkladu výše, vidíme, že volání statické metody, jako je:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>); 
```
vytvořilo v aplikaci Dynamo verze 1.x 3D seznam bodů. K tomu došlo kvůli povýšení prvního argumentu povrchu s jedinou hodnotou na seznam při použití vodítek replikace.
 
### Dynamo 1.x: Povýšení argumentu na seznam při použití vodítek replikace

![](../images/8-4/1/lang2_13.png)

Ve verzi 2.0 jsme zakázali povýšení argumentů s jednou hodnotou na seznamy při použití s vodítky replikace nebo vázáním. Nyní tedy volání:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
jednoduše vrátí 2D seznam, protože povrch není povýšen.

### Dynamo 2.0.x: Zakázání povýšení argumentu s jednou hodnotou na seznam při použití vodítek replikace

![](../images/8-4/1/lang2_14.png)

Tato změna nyní odstraňuje přidání nadbytečné úrovně seznamu a také řeší regresi způsobenou přechodem na kompilaci statické metody.

### Výhody:

**Čitelnost: ** Výsledky odpovídají očekáváním uživatelů a jsou srozumitelnější.

**Kompatibilita:** Uzly uživatelského rozhraní (s možností vázání) a uzly bloku kódu používající vodítka replikace poskytují kompatibilní výsledky.

**Konzistence:** 
* Metody instance a statické metody jsou konzistentní (což opravuje problémy se sémantikou statické metody).
* Uzly se vstupy a výchozími argumenty se chovají konzistentně (viz níže).

![](../images/8-4/1/lang2_15.png)

## 5\. Proměnné jsou v uzlech bloku kódu neměnné, aby se zabránilo asociativní aktualizaci. 

DesignScript historicky podporuje dvě programovací paradigmata – asociativní a imperativní programování. Asociativní kód vytváří graf závislostí z příkazů programu, kde jsou proměnné na sobě závislé. Aktualizace proměnné může vyvolat aktualizace všech ostatních proměnných, které na této proměnné závisí. To znamená, že posloupnost provádění příkazů v asociativním bloku není založena na jejich pořadí, ale na vztazích závislosti mezi proměnnými.

V následujícím příkladu je posloupnost provádění kódu takováto: 1 -> 2 -> 3 -> 2. Protože `b` je závislé na `a`, při aktualizaci `a` na řádku 3 provádění znovu přeskočí na řádek 2, aby se `b` aktualizovalo novou hodnotou `a`. 
```
1. a = 1; 
2. b = a * 2;
3. a = 2;
```
Naproti tomu pokud se stejný kód provádí v imperativním kontextu, příkazy se provádějí lineárně, shora dolů. Imperativní bloky kódu jsou proto vhodné pro sekvenční provádění konstrukcí kódu, jako jsou smyčky a podmínky if-else.

### Nejasnosti asociativní aktualizace:

**1\. Proměnné s cyklickou závislostí:**

V některých případech nemusí být cyklická závislost mezi proměnnými tak zřejmá jako v následujícím případě. V takových případech, kdy překladač nemůže detekovat cyklus staticky, by to mohlo vést k neomezenému cyklu běhu.
```
a = 1;
b = a;
a = b;
```
**2\. Proměnné závislé na sobě samých:**

Pokud proměnná závisí sama na sobě, měla by se její hodnota kumulovat nebo by měla být při každé aktualizaci obnovena její původní hodnota?
```
a = 1;
b = 1;
b = b + a + 2; // b = 4
a = 4;         // b = 10 or b = 7?
```
Protože v tomto příkladu geometrie závisí krychle `b` sama na sobě i na válci `a`, měl by pohyb posuvníku způsobit, že se otvor posune podél kvádru, nebo by se měl při každé aktualizaci polohy posuvníku vytvořit kumulativní efekt vyhloubení několika otvorů podél jeho dráhy?

![](../images/8-4/1/lang2_16.gif)

**3\. Aktualizace vlastností proměnných:**

```
1: def foo(x: A) { x.prop = ...; return x; }
2: a = A.A();
3: p = a.prop;
4: a1 = foo(a);  // will p update?
```

**4\. Aktualizace funkcí:**

```
1: def foo(v: double) { return v * 2; }// define “foo”
2: x = foo(5);                         // first definition of “foo” called
3: def foo(v: int) { return v * 3; }   // overload of “foo” defined, will x update?
```
Na základě zkušeností jsme zjistili, že asociativní aktualizace se v uzlech bloků kódu v kontextu grafu toku dat založeného na uzlech neosvědčuje. Než bylo k dispozici jakékoli vizuální programovací prostředí, jediným způsobem, jak zkoumat možnosti, bylo explicitně měnit hodnoty některých proměnných v programu. V textovém programu je k dispozici celá historie aktualizací proměnné, zatímco ve vizuálním programovacím prostředí se zobrazuje pouze poslední hodnota proměnné. 

Pokud ji vůbec někteří uživatelé použili, s největší pravděpodobností ji použili nevědomky a způsobili tím více škody než užitku. Proto jsme se rozhodli ve verzi 2.0 skrýt asociativitu při použití uzlů bloků kódu tím, že jsme proměnné učinili neměnnými, zatímco asociativní aktualizaci jsme nadále zachovali pouze jako nativní funkci modulu DS. Jedná se o další změnu provedenou s myšlenkou zjednodušit uživatelům skriptování.

**Asociativní aktualizace je v uzlu bloku kódu zakázána, aby se zabránilo redefinici proměnné:** ![](../images/8-4/1/lang2_17.png)

**Indexování seznamu je v blocích kódu nadále povoleno.**

Byla vytvořena výjimka pro indexování seznamu, které je ve verzi 2.0 stále povoleno pomocí přiřazení operátoru indexu.

V následujícím příkladu vidíme, že seznam `a` je inicializován, ale později může být přepsán přiřazením operátoru indexu, a že všechny proměnné závislé na `a` by se aktualizovaly asociativně, jak je vidět na hodnotě `c`. Také náhled uzlu zobrazuje aktualizované hodnoty `a` po předefinování jednoho nebo více prvků.

![](../images/8-4/1/lang2_18.png)

## 6\. Proměnné v imperativních blocích jsou místní pro obor imperativního bloku.

Ve verzi 2.0 jsme provedli změny pravidel pro obor imperativního bloku, abychom zakázali složité scénáře mezijazykové aktualizace.

V aplikaci Dynamo 1.x by posloupnost provádění následujícího skriptu probíhala v řádcích 1 -> 2 -> 4 -> 6 -> 4, kdy se změna šíří z vnějšího do vnitřního oboru jazyka Protože `y` je aktualizováno ve vnějším asociativním bloku a protože `x` v imperativním bloku je závislé na `y`, přesune se řízení z vnějšího asociativního programu do imperativního jazyka na řádku 4. 
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3;
```

Posloupnost provádění v tomto dalším příkladu by probíhala v řádcích 1 -> 2 -> 4 -> 2, kdy by se změna rozšířila z vnitřního oboru jazyka do vnějšího.
```
1: x = 1;
2: y = x * 2;
3: [Imperative] {
4:     x = 3;
5: }
```
Výše uvedené scénáře se týkají mezijazykových aktualizací, které stejně jako asociativní aktualizace nejsou v uzlech bloků kódu příliš užitečné. Abychom zakázali složité scénáře mezijazykové aktualizace, učinili jsme proměnné v imperativním oboru místními. 

Podívejme se na následující příklad v aplikaci Dynamo 2.0:
```
x = 1;
y = x * 2;
i = [Imperative] {
     x = 3;
     return x;
}
```
* `x` definované v imperativním bloku je nyní místní pro imperativní obor.
* Hodnoty `x` a `y` ve vnějším oboru zůstávají `1` a `2`.

Jakákoli místní proměnná uvnitř imperativního bloku musí být vrácena, pokud má být její hodnota přístupná ve vnějším oboru.

Prozkoumejme následující příklad:
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3; // x = 1, y = 3
```
* `y` je zkopírováno místně uvnitř imperativního oboru.  
* Hodnota `x`, která je místní pro imperativní obor, je `4`.
* Aktualizace hodnoty `y` ve vnějším oboru nadále způsobuje aktualizaci `x` z důvodu mezijazykové aktualizace, ale v blocích kódu ve verzi 2.0 je zakázána kvůli neměnnosti proměnných.
* Hodnoty `x` a `y` ve vnějším asociativním oboru zůstávají `1` a `2`

## 7\. Seznamy a slovníky

V aplikaci Dynamo 1.x byly seznamy a slovníky reprezentovány jedním sjednoceným kontejnerem, který mohl být indexován jak celočíselným, tak neceločíselným klíčem. Následující tabulka shrnuje oddělení seznamů a slovníků ve verzi 2.0 a pravidla nového datového typu Dictionary:

|                               |    1.x                      |    2.0                                   |
| :---------------------------- | --------------------------- | ---------------------------------------- |
| **Inicializace seznamu**       | `a = {1, 2, 3};`            | `a = [1, 2, 3];`                         |
| **Prázdný seznam**                | `a = {};`                   | `a = [];`                                |
| **Inicializace slovníku** | **Lze dynamicky připojit ke stejnému slovníku:** | **Lze vytvořit pouze nové slovníky:** |
|                           | `a = {};`                   | `a = {“foo” : 1, “bar” : 2};`            |
|                           | `a[“foo”] = 1;`             | `b = {“foo” : 1, “bar” : 2, “baz” : 3};` |
|                           | `a[“bar”] = 2;`             | `a = {};` // Vytvoří prázdný slovník |
|                           | `a[“baz”] = 3;`             |                                          |
| **Indexování slovníku**   | **Indexování klíče**            | **Syntaxe indexování zůstává stejná**     |
|                           | `b = a[“bar”];`             | `b = a[“bar”];`                          |
| **Klíče slovníku**       | **Jakýkoli typ klíče byl platný**  | **Platné jsou pouze řetězcové klíče**           |
|                           | `a = {};`                   | `a  = {“false” : 23, “point” : 12};`     |
|                           | `a[false] = 23;`            |                                          |
|                           | `a[point] = 12;`            |                                          |

### Nová syntaxe seznamu `[]`
Syntaxe inicializace seznamu byla ve verzi 2.0 změněna ze složených závorek `{}` na hranaté závorky `[]`. Všechny skripty verze 1.x jsou při otevření ve verzi 2.0 automaticky migrovány na novou syntaxi. 

**Poznámka k výchozím atributům argumentů u uzlů Zero Touch:**

Všimněte si, že automatická migrace nebude fungovat se starou syntaxí používanou ve výchozích atributech argumentů. Autoři uzlů budou muset v případě potřeby ručně aktualizovat definice metod Zero Touch tak, aby používaly novou syntaxi ve výchozích atributech argumentů `DefaultArgumentAttribute`.

**Poznámka k indexaci:**

Nové chování indexování bylo v některých případech změněno. Indexování do seznamu/slovníku s libovolným seznamem indexů/klíčů pomocí operátoru `[]` nyní zachová strukturu seznamu vstupního seznamu indexů/klíčů. Dříve se vždy vracel 1D seznam hodnot:
```
Given:
a = {“foo” : 1, “bar” : 2};

1.x:
b = a[{“foo”, {“bar”}}];
returns {1, 2}

2.0:
b = a[[“foo”, [“bar”]]];
returns [1, [2]];
```

### Syntaxe inicializace slovníku:

Syntaxi složených závorek `{}` lze k inicializaci slovníku použít pouze ve 
```
dict = {<key> : <value>, …}; 
```
formátu dvojice klíč-hodnota, kde je pro `<key>` povolen pouze řetězec a více dvojic klíč-hodnota je odděleno čárkami.

![](../images/8-4/1/lang2_19.png)

Metodu `Dictionary.ByKeysValues` Zero Touch lze použít jako univerzálnější způsob inicializace slovníku předáním seznamu klíčů a hodnot, který má k dispozici všechny funkce a možnosti metod Zero Touch, jako jsou vodítka replikace atd.

![](../images/8-4/1/lang2_20.png)

### Proč jsme pro syntaxi inicializace slovníku nepoužili libovolné výrazy?

Experimentovali jsme s myšlenkou použití libovolných výrazů pro klíče v syntaxi inicializace slovníku „klíč-hodnota“ a zjistili jsme, že by to mohlo vést k matoucím výsledkům, zvláště když syntaxe jako `{keys : vals}` (kde `keys` i `vals` reprezentují seznamy) kolidovala s jinými funkcemi jazyka DesignScript, jako je replikace, a poskytovala jiné výsledky než inicializační uzel Zero Touch. 

Mohou například existovat další případy, jako je tento příkaz, kdy by bylo obtížné definovat očekávané chování:
```
dict = {["foo", "bar"] : "baz" };
```
Další přidávání syntaxe vodítek replikací atd., nejen identifikátorů, by bylo v rozporu s myšlenkou jednoduchosti jazyka. 

V budoucnu bychom _mohli_ rozšířit klíče slovníku tak, aby podporovaly libovolné výrazy, ale museli bychom také zajistit, aby interakce s ostatními funkcemi jazyka byla konzistentní a srozumitelná, a to za cenu zvýšení složitosti, nikoli za cenu toho, aby byl systém o něco méně výkonný, ale snadno pochopitelný. Vzhledem k tomu, že vždy existuje alternativní způsob, jak to vyřešit pomocí metody `Dictionary.ByKeysValues(keyList, valueList)`, což není tak náročné.

### Interakce s uzly Zero Touch:

__1\. Uzel Zero Touch vracející slovník .NET je vrácen jako slovník Dynamo.__

**Uvažujme následující metodu Zero Touch jazyka C#, která vrací IDictionary:** ![](../images/8-4/1/lang2_21.png)

**Odpovídající návratová hodnota uzlu Zero Touch je převedena na slovník Dynamo:** ![](../images/8-4/1/lang2_22.png)

__2\. Uzly vracející více hodnot jsou zobrazeny jako slovníky__

**Uzel Zero Touch vracející IDictionary s atributem MultiReturn vrací slovník Dynamo:** ![](../images/8-4/1/lang2_23.png)

![](../images/8-4/1/lang2_24.png)

__3\. Slovník Dynamo je možné předat jako vstup do uzlu Zero Touch, který přijímá slovník .NET.__

**Metoda Zero Touch s parametrem IDictionary:** ![](../images/8-4/1/lang2_25.png)

**Uzel Zero Touch přijímá jako vstup slovník Dynamo:** ![](../images/8-4/1/lang2_26.png)

### Náhled slovníku v uzlech vracejících více hodnot

Slovníky jsou neuspořádané dvojice klíč-hodnota. V souladu s touto myšlenkou proto není zaručeno, že náhledy dvojic klíč-hodnota uzlů vracejících slovníky budou seřazeny v pořadí návratových hodnot uzlů. 

Udělali jsme však výjimku pro uzly vracející více hodnot, které mají definován atribut `MultiReturnAttribute`. V následujícím příkladu je uzel `DateTime.Components` uzel vracející více hodnot a náhled uzlu odráží jeho dvojice klíč-hodnota tak, aby byly ve stejném pořadí jako výstupní porty uzlu, což je také pořadí, ve kterém jsou výstupy zadány na základě atributu `MultiReturnAttribute` v definici uzlu.

Všimněte si také, že náhledy bloků kódu nejsou na rozdíl od uzlu uživatelského rozhraní seřazeny, protože informace o výstupním portu (ve formě atributu multireturn) pro uzel bloku kódu neexistují: ![](../images/8-4/1/lang2_27.png)
