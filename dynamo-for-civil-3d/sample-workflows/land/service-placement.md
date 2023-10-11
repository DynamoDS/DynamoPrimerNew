# Umístění služeb

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

Inženýrský návrh typické bytové výstavby zahrnuje práci s několika podzemními inženýrskými sítěmi, jako je splašková kanalizace, dešťová kanalizace, rozvody pitné vody nebo jiné. V tomto příkladu si ukážeme, jak lze aplikaci Dynamo použít k nakreslení servisních přípojek z rozvodné sítě k danému pozemku (tj. parcely). Je běžné, že každá parcela vyžaduje servisní přípojku, což vyžaduje značné množství zdlouhavé práce při umisťování všech služeb. Aplikace Dynamo může tento proces urychlit automatickým přesným nakreslením nezbytné geometrie a také poskytuje flexibilní vstupy, které lze upravit tak, aby vyhovovaly místním standardům.

## Cíl

> :dart: Umístěte reference bloků vodoměrů ve stanovených vzdálenostech od linie pozemku a nakreslete čáru pro každou servisní přípojku kolmo na rozvodnou síť.

## Klíčové koncepty

> * Použití uzlu **Select Object** pro uživatelský vstup
> * Práce se souřadnicovými systémy
> * Použití geometrických operací, jako jsou například **Geometry.DistanceTo** a **Geometry.NearestPointTo**
> * Vytvoření referencí bloků
> * Řízení nastavení vazeb objektů

## Kompatibilita verzí

{% hint style="success" %}
 Tento graf bude funkční v aplikaci **Civil 3D 2020** a vyšších verzích. 
{% endhint %}

## Datová sada

Začněte stažením níže uvedených vzorových souborů a poté otevřete soubor DWG a graf aplikace Dynamo.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Řešení

Zde je uveden přehled logiky tohoto grafu.

> 1. Získání geometrie křivky pro rozvodnou síť.
> 2. Získání geometrie křivky pro uživatelem vybranou linii pozemku, v případě potřeby obrácení směru.
> 3. Generování bodů vložení pro měřiče služeb.
> 4. Získání bodů na rozvodné síti, které jsou nejblíže umístění měřičů služeb.
> 5. Vytvoření referencí bloků a čar v modelovém prostoru.

Pojďme na to!

### Získání geometrie rozvodné sítě

Prvním krokem je získání geometrie rozvodné sítě do aplikace Dynamo. Místo výběru jednotlivých čar nebo křivek získáme všechny objekty v určité hladině a spojíme je dohromady jako objekt PolyCurve aplikace Dynamo.

{% hint style="info" %}
 Pokud je pro vás geometrie křivek aplikace Dynamo novinkou, přečtěte si část [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Získání objektů z aplikace Civil 3D a jejich spojení do jednoho objektu PolyCurve</p></figcaption></figure>

### Získání geometrie linie pozemku

Nyní je nutné načíst geometrii vybrané linie pozemku do aplikace Dynamo, abychom s ní mohli pracovat. Správným nástrojem pro tuto úlohu je uzel **Select Object**, který umožňuje uživateli grafu vybrat konkrétní objekt v aplikaci Civil 3D.

Musíme se také vypořádat s potenciálním problémem, který může nastat. Linie pozemku má počáteční a koncový bod, což znamená, že má směr. Aby graf poskytoval konzistentní výsledky, je nutné, aby všechny linie pozemku měly konzistentní směr. Tuto podmínku můžeme zohlednit přímo v logice grafu, díky čemuž je graf odolnější. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Výběr linie pozemku a zajištění jejího správného směru</p></figcaption></figure>

> 1. Získá počáteční a koncový bod linie pozemku.
> 2. Změří vzdálenost od každého bodu k rozvodné síti a pak zjistí, která vzdálenost je větší.
> 3. Požadovaným výsledkem je, aby počáteční bod linie byl co nejblíže k rozvodné síti. Pokud tomu tak není, obrátíme směr linie pozemku. V opačném případě jednoduše vrátíme původní linii pozemku.

### Vytvoření bodů vložení

Je čas zjistit, kde budou umístěny měřiče služeb. Umístění je obvykle určeno požadavky místních úřadů, takže pouze zadáme vstupní hodnoty, které lze změnit tak, aby vyhovovaly různým podmínkám. Jako referenci pro vytvoření bodů použijeme **souřadnicový systém** podél linie pozemku. To velmi usnadní definování odsazení vzhledem k linii pozemku, bez ohledu na její orientaci.

{% hint style="info" %}
 Pokud jsou pro vás souřadnicové systémy novinkou, přečtěte si část [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Vytvoření bodů vložení pro měřiče služeb</p></figcaption></figure>

### Získání bodů připojení

Nyní je potřeba získat body na rozvodné síti, které jsou nejblíže umístění měřičů služeb. To nám umožní nakreslit servisní přípojky v modelovém prostoru tak, aby byly vždy kolmé k rozvodné síti Ideálním řešením je uzel **Geometry.NearestPointTo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Získání kolmých bodů na rozvodné síti</p></figcaption></figure>

> 1. Toto je křivka PolyCurve rozvodné sítě.
> 2. Toto jsou body vložení měřičů služeb.

### Vytvoření objektů

Posledním krokem je vytvoření objektů v modelovém prostoru. Dříve vytvořené body vložení použijeme k vytvoření referencí bloků a pak pomocí bodů na rozvodné síti nakreslíme čáry k servisním přípojkám.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Výsledek

Při spuštění grafu byste měli vidět nové reference bloků a čáry servisních přípojek v modelovém prostoru. Zkuste změnit některé vstupy a sledujte, jak se vše automaticky aktualizuje!

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Úprava vstupních parametrů v aplikaci Dynamo a okamžité zobrazení výsledků v aplikaci Civil 3D</p></figcaption></figure>

### Bonus: Povolení sekvenčního umístění

Možní jste si všimli, že po umístění objektů pro jednu linii pozemku se při výběru jiné linie pozemku objekty „přesunou“.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Chování při zapnutí vazby objektů</p></figcaption></figure>

Toto je výchozí chování aplikace Dynamo, které je v mnoha případech velmi užitečné. Může se však stát, že budete chtít postupně umístit několik servisních přípojek a nechat aplikaci Dynamo při každém spuštění vytvořit nové objekty, místo aby upravovala ty původní. Toto chování můžete ovládat změnou nastavení vazby objektů.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Nastavení vazby objektů aplikace Dynamo</p></figcaption></figure>

{% hint style="info" %}
 Další informace naleznete v části [object-binding.md](../../advanced-topics/object-binding.md "mention"). 
{% endhint %}

Změna tohoto nastavení způsobí, že aplikace Dynamo „zapomene“ na objekty, které vytvoří při každém spuštění. Zde je příklad spuštění grafu s vypnutou vazbou objektů pomocí **Přehrávače skriptů Dynamo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Spuštění grafu pomocí Přehrávače skriptů Dynamo a zobrazení výsledků v aplikaci Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Pokud je pro vás Přehrávač skriptů Dynamo novinkou, přečtěte si část [dynamo-player.md](../../dynamo-player.md "mention"). 
{% endhint %}

> :tada: Úkol splněn!

## Nápady

Zde je několik nápadů, jak byste mohli rozšířit možnosti tohoto grafu.

{% hint style="info" %}
 Umístěte **několik servisních přípojek** současně místo výběru jednotlivých linií pozemku. 
{% endhint %}

{% hint style="info" %}
 Upravte vstupy tak, aby místo vodoměrů byly umístěny **přípojky pro čištění kanalizace** . 
{% endhint %}

{% hint style="info" %}
 **Přidejte přepínač** , který umožní umístit jednu servisní přípojku na určitou stranu linie pozemku místo na obě strany. 
{% endhint %}
