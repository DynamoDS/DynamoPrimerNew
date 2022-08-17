# Úpravy

Výkonnou funkcí aplikace Dynamo je, že můžete upravovat parametry na parametrické úrovni. Například generativní algoritmus nebo výsledky simulace lze použít k řízení parametrů pole prvků. Tímto způsobem může sada instancí ze stejné rodiny obsahovat uživatelské vlastnosti v projektu aplikace Revit.

### Parametry typu a instance

![Exercise](<../.gitbook/assets/32 (2).jpg>)

> 1. Parametry instance definují otvor panelů na povrchu střechy v rozsahu Poměr otvoru od 0.1 do 0.4.
> 2. Parametry založené na typu jsou použity u každého prvku na povrchu, protože se jedná o stejný typ rodiny. Materiál každého panelu může být například řízen parametrem založeným na typu.

![Cvičení](../.gitbook/assets/params.jpg)

> 1. Pokud jste již vytvořili rodinu aplikace Revit, nezapomeňte, že je nutné přiřadit typ parametru (řetězec, číslo, kóta atd.) Při přiřazování parametrů z aplikace Dynamo zkontrolujte, zda používáte správný typ dat.
> 2. Aplikaci Dynamo můžete použít také v kombinaci s parametrickými vazbami definovanými ve vlastnostech rodiny aplikace Revit.

Jako rychlý přehled parametrů v aplikaci Revit si ukážeme, že existují parametry typu a parametry instance. Oba lze upravovat pomocí aplikace Dynamo, ale v níže uvedeném cvičení budeme pracovat s parametry instance.

{% hint style="info" %} Až zjistíte, jak široké jsou možnosti použití parametrů úprav, možná budete chtít upravovat velké množství prvků v aplikaci Revit pomocí aplikace Dynamo. Může to být _výpočetně nákladná_ operace, což znamená, že může být pomalá. Pokud upravujete velký počet prvků, můžete použít funkci uzlu „zmrazit“, aby bylo možné pozastavit provádění operací aplikace Revit při vytváření grafu. Další informace o zmrazení uzlů najdete v části [Zmrazení](../essential-nodes-and-concepts/5\_geometry-for-computational-design/5-6\_solids.md#freezing) v kapitole Tělesa. {% endhint %}

### Jednotky

Od verze 0.8 je aplikace Dynamo v zásadě bez jednotek. Díky tomu může aplikace Dynamo zůstat abstraktním vizuálním programovacím prostředím. Uzly aplikace Dynamo, které spolupůsobí s kótami aplikace Revit, budou odkazovat na jednotky projektu aplikace Revit. Pokud například nastavujete parametr délky v aplikaci Revit z aplikace Dynamo, číslo v aplikaci Dynamo pro danou hodnotu bude odpovídat výchozím jednotkám v projektu aplikace Revit. Níže uvedené cvičení funguje v metrech.

K rychlému převodu jednotek použijte uzel _Convert Between Units_. Jedná se o užitečný nástroj pro konverzi délkových, plošných a objemových jednotek za pochodu.

![](<images/3/editing - units.jpg>)

## Cvičení

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="datasets/3/Revit-Editing.zip" %}

{% hint style="warning" %}V níže uvedeném cvičení jsou použity metry. {% endhint %}

Toto cvičení je zaměřeno na úpravy prvků aplikace Revit bez provedení geometrické operace v aplikaci Dynamo. Zde neimportujeme geometrii aplikace Dynamo, pouze upravujeme parametry v projektu aplikace Revit. Toto cvičení je základní, pokročilejší uživatelé aplikace Revit si mohou všimnout, že se jedná o parametry instance objemu, ale stejnou logiku lze použít na pole prvků, které chcete přizpůsobit ve velkém měřítku. To je vše s uzlem Element.SetParameterByName.

### Úprava parametrů objemu budovy

Začněte s ukázkovým souborem aplikace Revit pro tuto část. Z předchozího řezu jsme odstranili konstrukční prvky a adaptivní příhradové nosníky. V tomto cvičení se zaměříme na parametrický nýt v aplikaci Revit a v aplikaci Dynamo provedeme manipulaci.

Po výběru budovy v položce Objem v aplikaci Revit se na panelu vlastností zobrazí pole parametrů instance.

![](<../.gitbook/assets/editing - exercise 01.jpg>)

V aplikaci Dynamo můžeme načíst parametry výběrem cílového prvku.

![](<images/3/editing - exercise 02.jpg>)

> 1. Vyberte objem budovy pomocí uzlu _Select Model Element_.
> 2. Všechny parametry tohoto objemu lze dotazovat pomocí uzlu _Element.Parameters_. To zahrnuje parametry typu a instance.

![](<images/3/editing - exercise 03.jpg>)

> 1. Odkazujte na uzel _Element. Parameters_ kvůli vyhledání cílových parametrů. Nebo si můžeme v předchozím kroku prohlédnout panel vlastností a vybrat si názvy parametrů, které chcete upravit. V tomto případě hledáme parametry, které ovlivňují velké geometrické pohyby na objemu budovy.
> 2. Provedeme změny prvku aplikace Revit pomocí uzlu _Element.SetParameterByName_.
> 3. Pomocí bloku kódu definujte seznam parametrů, s uvozovkami kolem každé položky označujícími řetězec. Můžeme také použít uzel List.Create s řadou uzlů _string_ připojených k několika vstupům, ale blok kódu je rychlejší a jednodušší. Ujistěte se, že řetězec odpovídá přesnému názvu v aplikaci Revit, přičemž se rozlišují malá a velká písmena: `{"BldgWidth","BldgLength","BldgHeight", "AtriumOffset", "InsideOffset","LiftUp"};`

![](<images/3/editing - exercise 04.jpg>)

> 1. Také chceme určit hodnoty pro každý parametr. Přidejte na kreslicí plochu šest _posuvníků celých čísel_ a každý přejmenujte na odpovídající parametr v seznamu. Dále nastavte hodnoty každého posuvníku podle výše uvedeného obrázku. V pořadí shora dolů: 62, 92, 25, 22, 8 a12
> 2. Definujte další _blok kódu_ se seznamem stejné délky jako názvy parametrů. V tomto případě pojmenujeme proměnné (bez uvozovek), které vytvářejí vstupy pro _blok kódu_. Připojte _posuvníky_ ke každému odpovídajícímu vstupu: `{bw,bl,bh,ao,io,lu};`
> 3. Připojte blok kódu ke vstupu value uzlu _Element.SetParameterByName*_. Po automatické kontrole spuštění se výsledky automaticky zobrazí.

{% hint style="warning" %}*Tato ukázka funguje s parametry instance, ale ne s parametry typu. {% endhint %}

Stejně jako v aplikaci Revit je mnoho těchto parametrů na sobě závislých. Samozřejmě existují kombinace, ve kterých se může geometrie přerušit. Tento problém můžeme vyřešit definovanými vzorci ve vlastnostech parametru nebo můžeme nastavit podobnou logiku pomocí matematických operací v aplikaci Dynamo (pokud si chcete cvičení rozšířit, je to další výzva).

![](<images/3/editing - exercise 05.jpg>)

> 1. Tato kombinace poskytuje nový návrh objemu budovy: 100, 92, 100, 25, 13, 51.

### Úprava parametrů fasády

Nyní se podívejme, jak můžeme pomocí podobného postupu upravit fasádu.

![](<images/3/editing - exercise 06.jpg>)

> 1. Zkopírujme graf a zaměřme se na zasklení fasády, ve kterém bude umístěn systém příhradových nosníků. V tomto případě izolujeme čtyři parametry: `{"DblSkin_SouthOffset","DblSkin_MidOffset","DblSkin_NorthOffset","Facade Bend Location"};`
> 2. Dále vytvoříme _posuvníky čísel_ a přejmenujeme je na příslušné parametry. První tři posuvníky shora dolů by měly být přemapovány na doménu s obsahem [0,10], zatímco poslední posuvník _Facade Bend Location_ by měl být přemapován na doménu s obsahem [0,1]. Tyto hodnoty by měly odshora dolů vycházet z těchto hodnot (i když jsou libovolné): 2.68, 2.64, 2.29, 0.5.
> 3. Definujte nový blok kódu a připojte posuvníky: `{so,mo,no,fbl};`

![](<images/3/editing - exercise 07.jpg>)

> 1. Změnou _posuvníků_ v této části grafu můžeme dosáhnout toho, že zasklení fasády bude mnohem výraznější: 9.98, 10.0, 9.71 ,0.31.
