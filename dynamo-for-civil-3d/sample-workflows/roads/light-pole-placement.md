# Umístění sloupů osvětlení

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Jedním z mnoha případů skvělého použití aplikace Dynamo je dynamické umísťování samostatných objektů podél modelu koridoru. Často je potřeba objekty umístit na místa, která jsou nezávislá na vložených sestavách podél koridoru, což je velmi zdlouhavý úkol, který je třeba provést ručně. A pokud se změní horizontální nebo vertikální geometrie koridoru, je nutné provést značné množství předělávek.

## Cíl

> :dart: Umístěte reference bloků sloupů osvětlení podél koridoru v hodnotách staničení určených v souboru aplikace Excel. 

## Klíčové koncepty

> * Načítání dat z externího souboru (v tomto případě soubor aplikace Excel)
> * Uspořádání dat ve slovnících
> * Použití souřadnicových systémů k řízení polohy, měřítka nebo natočení
> * Umístění referencí bloků
> * Vizualizace geometrie v aplikaci Dynamo

## Kompatibilita verzí

{% hint style="success" %} Tento graf bude funkční v aplikaci **Civil 3D 2020** a vyšších verzích. 
{% endhint %} 

## Datová sada

Začněte stažením níže uvedených vzorových souborů a poté otevřete soubor DWG a graf aplikace Dynamo.

{% hint style="info" %}
 Doporučujeme soubor aplikace Excel uložit ve stejném adresáři jako graf aplikace Dynamo. 
{% endhint %} 

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Řešení

Zde je uveden přehled logiky tohoto grafu.

> 1. Načtěte soubor aplikace Excel a importujte data do aplikace Dynamo.
> 2. Získejte návrhové linie ze zadané základny koridoru.
> 3. Vytvořte souřadnicové systémy podél návrhové linie koridoru v požadovaných staničeních.
> 4. Pomocí souřadnicových systémů umístěte reference bloků v modelovém prostoru.

Pojďme na to!

### Získání dat ze souboru aplikace Excel

V tomto vzorovém grafu použijeme soubor aplikace Excel k uložení dat, která aplikace Dynamo použije k umístění referencí bloků sloupů osvětlení. Tabulka vypadá následovně.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Struktura tabulky souboru aplikace Excel</p></figcaption></figure>

{% hint style="info" %}
 Použití aplikace Dynamo k načtení dat z externího souboru (například souboru aplikace Excel) je skvělou strategií, zejména pokud je potřeba data sdílet s ostatními členy týmu. 
{% endhint %} 

Data ze souboru aplikace Excel se do aplikace Dynamo importují následujícím způsobem. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Import dat aplikace Excel do aplikace Dynamo</p></figcaption></figure>

Nyní, když máme data, je nutné je rozdělit podle sloupců (_Corridor_, _Baseline_, _PointCode_ atd.), aby je bylo možné použít ve zbývající části grafu. Běžně se to dělá pomocí uzlu **List.GetItemAtIndex** a zadáním čísla indexu každého sloupce, který chceme použít. Například sloupec _Corridor_ má index 0, sloupec _Baseline_ má index 1 atd.

Vypadá to dobře, že? Ale tento přístup v sobě skrývá možný problém. Co když se v budoucnu změní pořadí sloupců v souboru aplikace Excel? Nebo se mezi dva sloupce přidá nový sloupec? Potom graf nebude správně fungovat a bude vyžadovat aktualizaci. Budoucí fungování grafu můžeme zajistit vložením dat do **slovníku**, přičemž záhlaví sloupců v souboru aplikace Excel budou sloužit jako _klíče_ a zbývající data jako _hodnoty_.

{% hint style="info" %}
 Pokud jsou pro vás slovníky novinkou, přečtěte si část [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention") . 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Vložení dat aplikace Excel do slovníku</p></figcaption></figure>

Graf je díky tomu odolnější, protože umožňuje flexibilně měnit pořadí sloupců v souboru aplikace Excel. Dokud záhlaví sloupců zůstanou stejná, pak lze data jednoduše načíst ze slovníku pomocí jeho _klíče_ (tj. záhlaví sloupce), což nyní provedeme.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Načítání dat ze slovníku</p></figcaption></figure>

### Získání návrhových linií koridorů

Nyní, když máme importovaná a připravená data aplikace Excel, začneme je používat k získávání informací o modelech koridorů z aplikace Civil 3D.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Vybere model koridoru podle jeho názvu.
> 2. Získá konkrétní základnu v rámci koridoru.
> 3. Získá návrhovou linii v rámci základny podle kódu bodu.

### Vytvoření souřadnicových systémů

Nyní vytvoříme **souřadnicové systémy** podél návrhových linií koridoru v hodnotách staničení, které jsme zadali v souboru aplikace Excel. Tyto souřadnicové systémy budou použity k definování polohy, otočení a měřítka referencí bloků sloupů osvětlení.

{% hint style="info" %}
 Pokud jsou pro vás souřadnicové systémy novinkou, přečtěte si část [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") . 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Získání souřadnicových systémů podél návrhových linií koridoru</p></figcaption></figure>

Všimněte si, že je zde použit blok kódu k otočení souřadnicových systémů v závislosti na tom, na které straně základny se nacházejí. Toho lze dosáhnout pomocí posloupnosti několika uzlů, ale toto je dobrý příklad situace, kdy je jednodušší to prostě sepsat.

{% hint style="info" %}
 Pokud jsou bloky kódu pro vás novinkou, přečtěte si část [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention"). 
{% endhint %} 

### Vytvoření referencí bloků

Blížíme se k cíli! Máme všechny informace, které potřebujeme, abychom mohli skutečně umístit reference bloků. Nejprve je nutné získat požadované definice bloků pomocí sloupce _BlockName_ v souboru aplikace Excel.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Získání požadovaných definic bloků z dokumentu</p></figcaption></figure>

Posledním krokem je vytvoření referencí bloků.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Vytvoření referencí bloků v modelovém prostoru</p></figcaption></figure>

### Výsledek

Po spuštění grafu by se měly v modelovém prostoru podél koridoru zobrazit nové reference bloků. A tady je ta skvělá část – pokud je režim spuštění grafu nastaven na Automaticky a upravíte soubor aplikace Excel, reference bloků se automaticky aktualizují!

{% hint style="info" %}
 Další informace o režimech spouštění grafů naleznete v části [3_user_interface](../../../3\_user\_interface/ "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Provádění změn v souboru aplikace Excel a rychlé zobrazení výsledků v aplikaci Civil 3D</p></figcaption></figure>

Zde je příklad spuštění grafu pomocí **Přehrávače skriptů Dynamo**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Spuštění grafu pomocí Přehrávače skriptů Dynamo a zobrazení výsledků v aplikaci Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Pokud je pro vás Přehrávač skriptů Dynamo novinkou, přečtěte si část [dynamo-player.md](../../dynamo-player.md "mention"). 
{% endhint %} 

> :tada: Úkol splněn!

### Bonus: Vizualizace v aplikaci Dynamo

Vizualizace geometrie koridoru v aplikaci Dynamo může být užitečná kvůli získání kontextu. Tento konkrétní model má tělesa koridoru již extrahovaná v modelovém prostoru, takže je přeneseme do aplikace Dynamo. 

Je tu však ještě něco, co musíme vzít v úvahu. Tělesa jsou relativně „těžkým“ typem geometrie, což znamená, že tato operace zpomalí graf. Proto by se hodilo, kdyby existoval jednoduchý způsob, jak _zvolit_, zda chceme tělesa zobrazit, nebo ne. Zřejmou odpovědí je odpojení uzlu **Corridor.GetSolids**, což však vytvoří upozornění pro všechny navazující uzly, což je trochu nepřehledné. To je situace, kdy vítězoslavně použijeme uzel **ScopeIf**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Všimněte si šedé čáry v dolní části uzlu **Object.Geometry**. To znamená, že náhled uzlu je vypnutý (přístupný po kliknutí pravým tlačítkem myši na uzel), což umožňuje, aby uzel **GeometryColor.ByGeometryColor** „nebojoval“ s jinou geometrií o prioritu zobrazení v náhledu na pozadí.
> 2. Uzel **ScopeIf** v zásadě umožňuje selektivně spustit celou větev uzlů. Pokud je vstup _test_ nastaven na hodnotu false, potom se nespustí žádné uzly připojené k uzlu **ScopeIf**.

Zde je výsledek v náhledu na pozadí v aplikaci Dynamo.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Vizualizace geometrie koridoru v aplikaci Dynamo</p></figcaption></figure>

## Nápady

Zde je několik nápadů, jak byste mohli rozšířit možnosti tohoto grafu.

{% hint style="info" %}
 Přidejte do souboru aplikace Excel sloupec **rotation** a použijte jej k řízení otáčení souřadnicových systémů. 
{% endhint %} 

{% hint style="info" %}
 Přidejte do souboru aplikace Excel **horizontální nebo vertikální odsazení**, aby se sloupy osvětlení mohly v případě potřeby odchýlit od návrhové linie koridoru. 
{% endhint %} 

{% hint style="info" %}
 Místo použití souboru aplikace Excel s hodnotami staničení vygenerujte hodnoty staničení **přímo v aplikaci Dynamo** pomocí počátečního staničení a typické rozteče. 
{% endhint %} 
