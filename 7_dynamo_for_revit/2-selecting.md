# Výběr

### Výběr prvků aplikace Revit

Aplikace Revit je prostředí obsahující velké množství dat. Díky tomu máte k dispozici celou řadu možností, které obnášejí více než jen najetí kurzorem a kliknutí. Při provádění parametrických operací můžete zadat dotaz do databáze aplikace Revit a dynamicky propojit prvky aplikace Revit s geometrií aplikace Dynamo.

Knihovna aplikace Revit v uživatelském rozhraní nabízí kategorii Selection (Výběr), která umožňuje výběr geometrie několika způsoby.

![](../.gitbook/assets/select\_revit\_elements\_01.jpg)

### Hierarchie aplikace Revit

Chcete-li prvky aplikace Revit správně vybírat, je důležité plně pochopit hierarchii prvků aplikace Revit. Chcete vybrat všechny stěny v projektu? Vyberte je podle kategorie. Chcete vybrat každou židli Eames v moderní hale z poloviny století? Vyberte podle rodiny.

Zobrazme si stručný přehled hierarchie aplikace Revit.

![](images/2/hierarchy.png)

Pamatujete si taxonomii z biologie? Říše, kmen, třída, řád, čeleď, rod, druh? Prvky aplikace Revit jsou uspořádány do kategorií podobným způsobem. Na základní úrovni je možné hierarchii aplikace Revit rozdělit na kategorie, rodiny, typy* a instance. Instance je samostatný prvek modelu (s jedinečným ID), zatímco kategorie definuje obecnou skupinu (například „stěny“ nebo „podlahy“). Pokud je databáze aplikace Revit tímto způsobem uspořádána, můžete vybrat jeden prvek a vybrat všechny podobné prvky podle určené úrovně hierarchie.

{% hint style="warning" %}
 *Typy v aplikaci Revit jsou definovány jinak než typy u programování. Typ v aplikaci Revit odkazuje na větev hierarchie, nikoli na „datový typ“. 
{% endhint %}

### Navigace v databázi pomocí uzlů aplikace Dynamo

Tři níže uvedené obrázky rozebírají hlavní kategorie výběru prvků aplikace Revit v aplikaci Dynamo. Jsou to skvělé nástroje použitelné v kombinaci, a některé z nich budou rozebrány během následujících cvičení.

_Najetí kurzorem a kliknutí_ je nejjednodušší způsob, jak přímo vybrat prvek aplikace Revit. Můžete vybrat prvek úplného modelu nebo součásti jeho topologie (jako je plocha nebo hrana). Prvek zůstane dynamicky propojený s objektem aplikace Revit, čili jakmile u souboru aplikace Revit dojde k aktualizaci umístění nebo parametrů, odkazovaný prvek aplikace Dynamo se v grafu aktualizuje.

![](../.gitbook/assets/selecting\_database\_navigation\_with\_dynamo\_nodes\_01.jpg)

_Rozevírací nabídky_ slouží k tvorbě seznamu všech dostupných prvků v projektu aplikace Revit. Pomocí tohoto nástroje můžete odkazovat na prvky aplikace Revit, které nemusí být v pohledu viditelné. Jedná se o skvělý nástroj k dotazování existujících prvků nebo tvorbě nových v projektu aplikace Revit nebo v editoru rodin.

![](<../.gitbook/assets/selecting - database navigation with dynamo nodes 02.png>)

Prvek aplikace Revit můžete také vybrat podle konkrétních vrstev v _hierarchii aplikace Revit_. Toto je účinná možnost přizpůsobení velkých polí dat při přípravě dokumentace nebo generativních instancí a přizpůsobení.

![UI](../.gitbook/assets/allelements.jpg)

Se třemi výše zmíněnými obrázky stále na paměti se pusťte do cvičení, které vybere prvky ze základního projektu aplikace Revit při přípravě pro parametrickou aplikaci, kterou vytvoříte ve zbývajících částech této kapitoly.

## Cvičení

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="datasets/2/Revit-Selecting.zip" %}

V tomto vzorovém souboru aplikace Revit jsou k dispozici tři typy prvků jednoduché budovy. Tento soubor bude sloužit jako příklad výběru prvků aplikace Revit v rámci kontextu hierarchie aplikace Revit.

![](<../.gitbook/assets/selecting_exercise_01 (1) (1).jpg>)

> 1. Objem budovy
> 2. Nosníky (rámová konstrukce)
> 3. Příhradové nosníky (adaptivní komponenty)

Jaké závěry je možné vyvodit z prvků aktuálně zobrazených v pohledu projektu aplikace Revit? A jak hluboko do hierarchie bude třeba jít, aby bylo možné vybrat vhodné prvky? Tyto problémy se samozřejmě stanou mnohem složitějšími, pokud pracujete na velkých projektech. K dispozici je mnoho možností: prvky je možné vybrat podle kategorií, úrovní, rodin, instancí atd.

### Výběr objemu a povrchů

![](../.gitbook/assets/selecting\_exercise\_02.jpg)

> 1. Vzhledem k tomu, že nyní pracujete se základním nastavením, vyberte objem budovy kliknutím na položku _Objem_ v rozevíracím uzlu Categories. Tyto položky naleznete na kartě Revit > Výběr.
> 2. Výstup kategorie Objem je pouze samotná kategorie. Je třeba vybrat prvky. K tomuto účelu použijte uzel _All Elements of Category_.

V tuto chvíli si všimněte, že v aplikaci Dynamo není zobrazena žádná geometrie. Vybrali jste prvek aplikace Revit, ale nepřevedli jste jej na geometrii aplikace Dynamo. Toto rozdělení je důležité. Pokud byste chtěli vybrat velký počet prvků, určitě byste nechtěli zobrazit všechny jejich náhledy v aplikaci Dynamo, protože tím by se vše zpomalilo. Aplikace Dynamo je nástroj ke správě projektu aplikace Revit bez nutnosti provádění operací geometrie, tomuto je věnována následující část této kapitoly.

V tomto případě pracujete s jednoduchou geometrií, čili je užitečné zobrazit v náhledu aplikace Dynamo geometrii. Položka „BldgMass“ ve výše zobrazeném uzlu Watch má vedle sebe zelené číslo. To představuje ID prvku a sděluje, že se zabýváme prvkem aplikace Revit, nikoli geometrií aplikace Dynamo. Dalším krokem je převedení tohoto prvku aplikace Revit na geometrii v aplikaci Dynamo.

![](../.gitbook/assets/selecting\_exercise\_03.jpg)

> 1. Pomocí uzlu _Element.Faces_ získáme seznam povrchů představujících každou plochu objemu aplikace Revit. Nyní je geometrie zobrazena ve výřezu aplikace Dynamo a je možné začít s odkazováním plochy pro parametrické operace.

Zde je alternativní metoda. Tentokrát se vyhneme výběru přes hierarchii aplikace Revit _(All Elements of Category)_ a budeme vybírat, aby byla explicitně vybrána geometrie v aplikaci Revit.

![](../.gitbook/assets/selecting\_exercise\_04.jpg)

> 1. V uzlu _Select Model Element_ klikněte na tlačítko *Vybrat *(nebo _Změnit_). Ve výřezu aplikace Revit vyberte požadovaný prvek. V tomto případě vybíráme objem budovy.
> 2. Místo uzlu _Element.Faces_ můžete k výběru plného objemu jako jedné geometrie tělesa použít uzel _Element.Geometry_. Tím vyberete veškerou geometrii obsaženou v daném objemu.
> 3. Pomocí uzlu _Geometry.Explode_ můžete zase získat zpět seznam povrchů. Tyto dva uzly fungují stejně jako uzel _Element.Faces_, ale nabízejí alternativní možnosti, jak proniknout do geometrie prvku aplikace Revit.

Pomocí některých základních operací se seznamem se můžete dotazovat na určitou plochu.

![](<images/2/selecting - exercise 05.jpg>)

> 1. Nejprve odešlete dříve vybrané prvky do uzlu Element.Faces.
> 2. Uzel _List.Count_ následně zobrazí, že pracujete s 23 povrchy v objemu.
> 3. Na základě tohoto čísla změňte hodnotu Maximum u *celočíselného posuvníku *na _22_.
> 4. Pomocí uzlu _List.GetItemAtIndex_ zadejte jako vstupy seznamy a *celočíselný posuvník *pro vstup _index_. Při procházení vybraných prvků se zastavte na _indexu 9_ a izolujte hlavní fasádu od příhradových nosníků.

Předchozí krok byl trochu těžkopádný. Činnost tohoto kroku je možné provést mnohem rychleji pomocí uzlu _Select Face_. Můžete tak izolovat plochu, která není samotným prvkem v projektu aplikace Revit. Stejná interakce platí i pro uzel _Select Model Element_, s tím rozdílem, že se vybírá povrch, nikoli celý prvek.

![](../.gitbook/assets/selecting\_exercise\_06.jpg)

Řekněme, že chcete izolovat stěny hlavní fasády budovy. Toho můžete dosáhnout pomocí uzlu _Select Faces_. Klikněte na tlačítko Vybrat a poté v aplikaci Revit vyberte čtyři hlavní fasády.

![](../.gitbook/assets/selecting\_exercise\_07.jpg)

Po výběru čtyř stěn se ujistěte, že jste v aplikaci Revit kliknuli na tlačítko Dokončit.

![](../.gitbook/assets/selecting\_exercise\_08.jpg)

Plochy jsou nyní importovány do aplikace Dynamo jako povrchy.

![](../.gitbook/assets/selecting\_exercise\_09.jpg)

### Výběr nosníků

Nyní se zaměřme na nosníky nad atriem.

![](../.gitbook/assets/selecting\_exercise\_10.jpg)

> 1. Pomocí uzlu _Select Model Element_ vyberte jeden z nosníků.
> 2. Připojte prvek nosníku k uzlu _Element.Geometry_ a nosník se zobrazí ve výřezu aplikace Dynamo.
> 3. Geometrii můžete přiblížit pomocí uzlu _Watch3D_ (pokud nosník není v uzlu Watch 3D zobrazen, klikněte pravým tlačítkem a klikněte na položku Přizpůsobit oknu).

Otázka, která se může často vyskytovat u pracovních postupů aplikace Revit/Dynamo: Jak mohu vybrat jeden prvek a získat všechny podobné prvky? Vzhledem k tomu, že vybraný prvek aplikace Revit obsahuje všechny hierarchické informace, můžete zadat dotaz na typ rodiny a vybrat všechny prvky tohoto typu.

![](../.gitbook/assets/selecting\_exercise\_11.jpg)

> 1. Připojte prvek nosníku k uzlu _Element.ElementType_.
> 2. Uzel _Watch_ ukazuje, že výstupem je nyní symbol rodiny, nikoli prvek aplikace Revit.
> 3. _Element.ElementType_ je jednoduchý dotaz, čili je možné jej provést stejně snadno i v bloku kódu pomocí výrazu `x.ElementType;` a získat tak stejné výsledky.

![](../.gitbook/assets/selecting\_exercise\_12.jpg)

> 1. K výběru zbývajících nosníků se použije uzel _All Elements of Family Type_.
> 2. Uzel Watch zobrazuje, že je vybráno pět prvků aplikace Revit.

![](../.gitbook/assets/selecting\_exercise\_13.jpg)

> 1. Všech pět prvků je také možné převést na geometrii aplikace Dynamo.

Co kdybyste pracovali s 500 nosníky? Převod všech těchto prvků na geometrii aplikace Dynamo by byl velmi pomalý. Pokud aplikaci Dynamo trvá výpočet uzlů dlouho, bude možná užitečné využít funkci „zmrazení“ uzlu, která pozastaví provádění operací aplikace Revit, zatímco vyvíjíte graf. Další informace o zmrazení uzlů najdete v části [Zmrazení](../essential-nodes-and-concepts/5\_geometry-for-computational-design/5-6\_solids.md#freezing) v kapitole Tělesa.

V každém případě, pokud chcete importovat 500 nosníků, potřebujete k provedení požadované parametrické operace všechny povrchy? Nebo je možné extrahovat z nosníků základní informace a provést generativní úlohy u základní geometrie? Tuto otázku mějte během procházení této kapitoly na paměti. Podívejme se dále například na systém příhradových nosníků

### Výběr příhradových nosníků

Pomocí stejného grafu uzlů vyberte prvek příhradového nosníku místo prvku nosníku. Ještě než to uděláte, odstraňte uzel Element.Geometry z předchozího kroku.

![](../.gitbook/assets/selecting\_exercise\_14.jpg)

Dále jsme připraveni extrahovat některé základní informace z typu rodiny příhradových nosníků.

![](../.gitbook/assets/selecting\_exercise\_15.jpg)

> 1. V uzlu _Watch_ je vidět, že výstupem je seznam adaptivních komponent vybraných v aplikaci Revit. Pokud chcete extrahovat základní informace, začněte adaptivními body.
> 2. Připojte uzel _All Elements of Family Type_ k uzlu _AdaptiveComponent.Location_. Tím se zobrazí seznam seznamů, z nichž každý má tři body představující umístění adaptivních bodů.
> 3. Připojením uzlu _Polygon.ByPoints_ pak vrátíte objekt PolyCurve. Ve výřezu aplikace Dynamo je to vidět. Touto metodou jste vizualizovali geometrii jednoho prvku a abstrahovali geometrii zbývajícího pole prvků (které mohlo být početně větší než v tomto příkladu).

{% hint style="info" %}
 Tip: Pokud v aplikaci Dynamo kliknete na zelené číslo u prvku aplikace Revit, výřez aplikace Revit se na tento prvek přiblíží. 
{% endhint %}
