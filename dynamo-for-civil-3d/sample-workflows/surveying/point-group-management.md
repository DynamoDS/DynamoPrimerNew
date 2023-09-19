# Správa skupin bodů

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Práce s body COGO a skupinami bodů v aplikaci Civil 3D je základním prvkem mnoha komplexních procesů využívajících data získaná v terénu. Aplikace Dynamo skutečně vyniká v oblasti správy dat a v tomto příkladu si ukážeme jeden z případů možného použití.  

## Cíl

> :dart: Vytvořte skupinu bodů pro každý jedinečný popis bodu COGO. 

## Klíčové koncepty

> * Práce se seznamy
> * Seskupení podobných objektů pomocí uzlu **List.GroupByKey**
> * Zobrazení vlastního výstupu v Přehrávači skriptů Dynamo

## Kompatibilita verzí

{% hint style="success" %} Tento graf bude funkční v aplikaci **Civil 3D 2020** a vyšších verzích. {% endint %}

## Datová sada

Začněte stažením níže uvedených vzorových souborů a poté otevřete soubor DWG a graf aplikace Dynamo.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Řešení

Zde je uveden přehled logiky tohoto grafu.

> 1. Získejte všechny body COGO v dokumentu.
> 2. Seskupte body COGO podle popisu.
> 3. Vytvořte skupiny bodů.
> 4. Odešlete souhrn do Přehrávače skriptů Dynamo.

Pojďme na to!

### Získání bodů COGO

V prvním kroku získáme všechny skupiny bodů v dokumentu a potom všechny body COGO v každé skupině. Tím získáme _vnořený seznam_ neboli „seznam seznamů“, se kterým se nám bude později lépe pracovat, pokud vše sloučíme do jediného seznamu pomocí uzlu **List.Flatten**.

{% hint style="info" %} Pokud se seznamy pracujete poprvé, přečtěte si část [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). {% enddhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Získání všech skupin bodů a bodů COGO </p></figcaption></figure>

### Seskupení bodů podle popisu

Nyní, když máme všechny body COGO, je třeba je rozdělit do skupin podle jejich popisů. Přesně to dělá uzel **List.GroupByKey**. V podstatě seskupuje všechny položky, které sdílejí stejný klíč.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Seskupení bodů COGO podle popisu</p></figcaption></figure>

### Vytvořte skupiny bodů.

To nejtěžší je za námi! Posledním krokem je vytvoření nových skupin bodů aplikace Civil 3D ze seskupených bodů COGO.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Vytvoření nové skupiny bodů</p></figcaption></figure>

### Výstupní souhrn

Při spuštění grafu není v náhledu na pozadí v aplikaci Dynamo nic vidět, protože nepracujeme s žádnou geometrií. Takže jediný způsob, jak zjistit, zda byl graf správně proveden, je zkontrolovat prostor nástrojů nebo se podívat na náhledy výstupu uzlu. Pokud však graf spustíme pomocí **Přehrávače skriptů Dynamo**, můžeme získat další zpětnou vazbu o výsledcích grafu vypsáním přehledu vytvořených skupin bodů. Stačí kliknout pravým tlačítkem myši na uzel a nastavit jej na možnost _Je výstup_. V tomto případě zobrazíme výsledky pomocí přejmenovaného uzlu **Watch**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>Nastavení uzlu na hodnotu <em>Je výstup</em> zobrazí jeho obsah ve výstupu Přehrávače skriptů Dynamo.</p></figcaption></figure>

### Výsledek

Zde je příklad spuštění grafu pomocí **Přehrávače skriptů Dynamo**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Spuštění grafu pomocí Přehrávače skriptů Dynamo a zobrazení výsledků v prostoru nástrojů</p></figcaption></figure>

{% hint style="info" %} Pokud je pro vás Přehrávač skriptů Dynamo novinkou, přečtěte si část [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Úkol splněn!

## Nápady

Zde je několik nápadů, jak byste mohli rozšířit možnosti tohoto grafu.

{% hint style="info" %} Upravte seskupení bodů tak, aby bylo založeno na **úplném popisu** místo hrubého popisu. {% enddhint %}

{% hint style="info" %} Seskupte body podle dalších **předdefinovaných kategorií**, které vyberete (například „Pozemní snímky“, „Vztažné body“ atd.) {% enddhint %}

{% hint style="info" %} Automaticky vytvořte povrchy TIN pro body v určitých skupinách. {% enddhint %}
