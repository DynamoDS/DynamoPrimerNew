# Přejmenování stavebních objektů

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

Při přidávání potrubí a stavebních objektů do potrubní sítě používá aplikace Civil 3D šablonu k automatickému přiřazení názvů. To je obvykle dostačující během počátečního umístění, ale názvy se budou v budoucnu nevyhnutelně měnit s tím, jak se bude návrh vyvíjet. Kromě toho existuje mnoho různých vzorů pojmenování, které mohou být vyžadovány, například postupné pojmenování stavebních objektů v potrubní trase počínaje nejvzdálenějším stavebním objektem, nebo podle vzoru pojmenování, který je v souladu s datovým schématem vyžadovaným místními úřady. V tomto příkladu si ukážeme, jak lze aplikaci Dynamo použít k definování libovolného typu strategie pojmenování a jejímu důslednému používání.

## Cíl

> :dart: Přejmenujte stavební objekty potrubní sítě v pořadí podle staničení trasy.

## Klíčové koncepty

> * Práce s ohraničujícími kvádry
> * Filtrování dat pomocí uzlu **List.FilterByBoolMask**
> * Třídění dat pomocí uzlu **List.SortByKey**
> * Generování a úprava textových řetězců

## Kompatibilita verzí

{% hint style="success" %} Tento graf bude funkční v aplikaci **Civil 3D 2020** a vyšších verzích. {% endhint %}

## Datová sada

Začněte stažením níže uvedených vzorových souborů a poté otevřete soubor DWG a graf aplikace Dynamo.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Řešení

Zde je uveden přehled logiky tohoto grafu.

> 1. Vyberte stavební objekty podle hladiny.
> 2. Získejte umístění stavebních objektů.
> 3. Filtrujte stavební objekty podle odsazení a pak je uspořádejte podle staničení.
> 4. Vytvořte nové názvy.
> 5. Přejmenujte stavební objekty.

Pojďme na to!

### Výběr stavebních objektů

Nejprve je třeba vybrat všechny stavení objekty, se kterými chceme pracovat. Provedeme to tak, že jednoduše vybereme všechny objekty v určité hladině, což znamená, že můžeme vybrat stavební objekty z různých potrubních sítí (za předpokladu, že sdílejí stejnou hladinu).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Výběr stavebních objektů v dané hladině</p></figcaption></figure>

> 1. Tento uzel zajišťuje, že neúmyslně nevybereme žádné nežádoucí typy objektů, které by mohly sdílet stejnou hladinu jako stavební objekty.

### Získání umístění stavebních objektů

Nyní, když máme stavební objekty, musíme zjistit jejich polohu v prostoru, abychom je mohli seřadit podle jejich umístění. K tomu využijeme ohraničující kvádr každého objektu. **Ohraničující kvádr** objektu je kvádr minimální velikosti, který zcela obsahuje geometrické rozměry objektu. Výpočtem středu ohraničujícího kvádru získáte poměrně dobrou aproximaci bodu vložení stavebního objektu.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Použití ohraničujících kvádrů k získání přibližného bodu vložení každého stavebního objektu</p></figcaption></figure>

Tyto body použijeme k získání staničení a odsazení stavebních objektů vzhledem k vybrané trase.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filtrování a řazení

Tady to začíná být trochu složitější. V této fázi máme velký seznam všech stavebních objektů na hladině, kterou jsme určili, a vybrali jsme trasu, podle které jsme je chtěli seřadit. Problém je v tom, že v seznamu mohou být stavební objekty, které nechceme přejmenovat. Nemusí například být součástí konkrétní trasy, která nás zajímá.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. Vybraná trasa
> 2. Stavební objekty, které chceme přejmenovat
> 3. Stavební objekty, které mají být ignorovány

Proto je nutné seznam stavebních objektů filtrovat, aby nebyly brány v úvahu stavební objekty, jejichž odsazení od trasy je větší než zadaná hodnota. To lze nejlépe provést pomocí uzlu **List.FilterByBoolMask**. Po filtrování seznamu stavebních objektů je pomocí uzlu **List.SortByKey** uspořádáme podle hodnot staničení.

{% hint style="info" %} Pokud se seznamy pracujete poprvé, přečtěte si část [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtrování a řazení stavebních objektů</p></figcaption></figure>

> 1. Zkontroluje, zda je odsazení stavebního objektu menší než prahová hodnota.
> 2. Nahradí nulové hodnoty hodnotou _false_.
> 3. Filtruje seznam stavebních objektů a staničení.
> 4. Uspořádá stavební objekty podle staničení.

### Generování nových názvů

Poslední část práce, kterou je třeba udělat, je vytvoření nových názvů struktur. Formát, který použijeme, je `<alignment name>-STRC-<number>`. Je zde několik dalších uzlů, které v případě potřeby doplní čísla dalšími nulami (například 01 místo 1).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Generování nových názvů stavebních objektů</p></figcaption></figure>

### Přejmenování stavebních objektů

A v neposlední řadě přejmenujeme stavební objekty.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Nastavení názvů pro stavební objekty</p></figcaption></figure>

### Výsledek

Zde je příklad spuštění grafu pomocí **Přehrávače skriptů Dynamo**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Spuštění grafu pomocí Přehrávače skriptů Dynamo a zobrazení výsledků v aplikaci Civil 3D</p></figcaption></figure>

{% hint style="info" %} Pokud je pro vás Přehrávač skriptů Dynamo novinkou, přečtěte si část [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Úkol splněn!

### Bonus: Vizualizace v aplikaci Dynamo

Pro účely vizualizace dočasných výstupů grafu místo pouze konečného výsledku může být užitečné využít 3D náhled na pozadí v aplikaci Dynamo. Jednou z jednoduchých věcí, kterou můžeme udělat, je zobrazit ohraničující kvádry pro stavební objekty. Tato konkrétní datová sada navíc obsahuje v dokumentu koridor, takže můžeme přenést geometrii návrhové linie koridoru do aplikace Dynamo a získat tak určitý kontext pro umístění stavebních objektů v prostoru. Pokud by graf byl použit v datové sadě, která nemá žádné koridory, pak tyto uzly jednoduše nebudou provádět žádné akce.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Vizualizace geometrie pro stavební objekty a návrhové linie koridoru</p></figcaption></figure>

Nyní lépe rozumíme tomu, jak funguje proces filtrování stavebních objektů podle odsazení.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Úprava prahové hodnoty odsazení trasy a zobrazení ovlivněných stavebních objektů v aplikaci Dynamo</p></figcaption></figure>

## Nápady

Zde je několik nápadů, jak byste mohli rozšířit možnosti tohoto grafu.

{% hint style="info" %} Přejmenujte stavební objekty podle jejich **nejbližší trasy** místo výběru konkrétní trasy. {% endhint %}

{% hint style="info" %} Kromě stavebních objektů **přejmenujte také potrubí**. {% endhint %}

{% hint style="info" %} **Nastavte hladiny** stavebních objektů podle jejich spuštění. {% endhint %}
