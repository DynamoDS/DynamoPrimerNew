# Vůle obalových křivek

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

Vývoj kinematických obálek pro ověření průjezdnosti je důležitou součástí návrhu železnice. Aplikaci Dynamo lze použít k vytvoření těles pro obalovou křivku místo vytváření a správy složitých podsestav koridoru pro tuto úlohu.

## Cíl

> :dart: Pomocí bloku profilu vozidla vygenerujte 3D tělesa s volným prostorem podél koridoru.

## Klíčové koncepty

> * Práce s návrhovými liniemi koridoru
> * Transformace geometrie mezi souřadnicovými systémy
> * Vytvoření těles šablonováním
> * Řízení chování uzlu pomocí nastavení vázání

## Kompatibilita verzí

{% hint style="success" %} Tento graf bude funkční v aplikaci **Civil 3D 2020** a vyšších verzích. {% endhint %}

## Datová sada

Začněte stažením níže uvedených vzorových souborů a poté otevřete soubor DWG a graf aplikace Dynamo.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## Řešení

Zde je uveden přehled logiky tohoto grafu.

> 1. Získejte návrhové linie ze zadané základny koridoru.
> 2. Vytvořte souřadnicové systémy podél návrhové linie koridoru s požadovanou roztečí.
> 3. Transformujte geometrii bloku profilu do souřadnicových systémů.
> 4. Šablonujte těleso mezi profily.
> 5. Vytvořte tělesa v aplikaci Civil 3D.

Pojďme na to!

### Získání dat koridoru

Prvním krokem je získání dat koridoru. Model koridoru vybereme podle jeho názvu, v rámci koridoru vybereme konkrétní základnu a poté získáme návrhovou linii v rámci základny podle jejího kódu bodu.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>Výběr koridoru, základny a návrhové linie</p></figcaption></figure>

### Vytvoření souřadnicových systémů

Nyní vytvoříme **souřadnicové systémy** podél návrhových linií koridoru mezi daným počátečním a koncovým staničením. Tyto souřadnicové systémy se použijí k zarovnání geometrie bloku profilu vozidla s koridorem.

{% hint style="info" %} Pokud jsou pro vás souřadnicové systémy novinkou, přečtěte si část [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") . {% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>Získání souřadnicových systémů podél návrhových linií koridoru</p></figcaption></figure>

> 1. Všimněte si malých písmen **XXX** v pravém dolním rohu uzlu. Tato písmena znamenají, že nastavení vázání uzlu je nastaveno na hodnotu _Vektorový součin_, což je nutné k vytvoření souřadnicových systémů ve stejných hodnotách staničení pro obě návrhové linie.

{% hint style="info" %} Pokud je vázání uzlu pro vás novinkou, přečtěte si část [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention") . {% endhint %}

### Transformace geometrie bloku

Nyní je třeba nějakým způsobem vytvořit pole profilů vozidel podél návrhových linií. Provedeme transformaci geometrie z definice bloku profilu vozidla pomocí uzlu **Geometry.Transform**. Tento koncept je složitý na vizualizaci, takže než se podíváme na uzly, zde je grafické znázornění toho, co se stane.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>Vizualizace transformace geometrie mezi souřadnicovými systémy.</p></figcaption></figure>

V podstatě tedy přebíráme geometrii aplikace Dynamo z _jedné_ definice bloku a přesouváme/otáčíme ji, přičemž vytváříme pole podél návrhové linie. Skvělá věc! Takto vypadá posloupnost uzlů.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. Tento uzel získá definici bloku z dokumentu.
> 2. Tyto uzly získají geometrii objektů aplikace Dynamo v rámci bloku.
> 3. Tyto uzly v podstatě definují souřadnicový systém, _ze kterého_ transformujeme geometrii.
> 4. A konečně tento uzel provádí vlastní transformaci geometrie.
> 5. Všimněte, že v tomto uzlu je definováno vázání _Nejdelší_.

A tady vidíme výsledek v aplikaci Dynamo.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>Geometrie bloku profilu vozidla po transformaci</p></figcaption></figure>

### Vytvoření těles

Máme pro vás skvělou zprávu. To nejtěžší je za námi. Nyní je třeba pouze vygenerovat tělesa mezi profily. Toho snadno dosáhneme pomocí uzlu **Solid.ByLoft**.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

Zde je výsledek. Nezapomeňte, že se jedná o tělesa aplikace Dynamo – musíme je ještě vytvořit v aplikaci Civil 3D.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>Tělesa aplikace Dynamo po šablonování</p></figcaption></figure>

### Výstup těles do aplikace Civil 3D

Posledním krokem je vytvoření vygenerovaných těles v modelovém prostoru. Také jim přiřadíme barvu, aby byly dobře viditelné.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Výstup těles do aplikace Civil 3D</p></figcaption></figure>

### Výsledek

Zde je příklad spuštění grafu pomocí **Přehrávače skriptů Dynamo**.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Spuštění grafu pomocí Přehrávače skriptů Dynamo a zobrazení výsledků v aplikaci Civil 3D</p></figcaption></figure>

{% hint style="info" %} Pokud je pro vás Přehrávač skriptů Dynamo novinkou, přečtěte si část [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Úkol splněn!

## Nápady

Zde je několik nápadů, jak byste mohli rozšířit možnosti tohoto grafu.

{% hint style="info" %} Přidejte možnost používat **různé rozsahy staničení** pro každou trasu zvlášť. {% endhint %}

{% hint style="info" %} **Rozdělte tělesa** na menší segmenty, které by bylo možné jednotlivě analyzovat z hlediska kolizí. {% endhint %}

{% hint style="info" %} Zkontrolujte, zda se obálka těles ** protíná s návrhovými liniemi** a vybarvěte ty, které se střetávají. {% endhint %}
