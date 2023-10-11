# Začínáme

Když jste si teď udělali základní obrázek, pojďme se rovnou pustit do vytváření prvního grafu aplikace Dynamo v aplikaci Civil 3D!

{% hint style="info" %}
 Jedná se o jednoduchý příklad, který má demonstrovat základní funkce aplikace Dynamo. Doporučujeme postupovat v novém prázdném dokumentu aplikace Civil 3D. {% enddhint %}

## Otevření aplikace Dynamo

Nejprve otevřete prázdný dokument v aplikaci Civil 3D. V tomto dokumentu poté na pásu karet aplikace Civil 3D přejděte na kartu **Správa** a vyhledejte panel **Vizuální programování**.

![](<../.gitbook/assets/image (7).png>)

Kliknutím na tlačítko **Dynamo** spusťte aplikaci Dynamo v samostatném okně.

{% hint style="info" %}
 **Jaký je rozdíl mezi aplikací Dynamo a Přehrávačem skriptů Dynamo?**

Aplikace Dynamo se používá k vytváření a spouštění grafů. Přehrávač skriptů Dynamo umožňuje snadno spouštět grafy, aniž by bylo nutné je otevírat v aplikaci Dynamo.

Až si jej budete chtít vyzkoušet, přejděte do části [dynamo-player.md](dynamo-player.md "mention"). 
{% endhint %}

## Zahájení nového grafu

Po otevření aplikace Dynamo se zobrazí úvodní obrazovka. Kliknutím na tlačítko **Nový** otevřete prázdný pracovní prostor.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Úvodní obrazovka aplikace Dynamo</p></figcaption></figure>

{% hint style="info" %}
 **Jsou dostupné nějaké ukázky?**

Aplikace Dynamo for Civil 3D obsahuje několik předdefinovaných grafů, které vám pomohou podnítit některé další nápady, jak používat aplikaci Dynamo. Doporučujeme se na ně někdy podívat a také si přečtěte část [sample-workflowstitlemention v této příručce Primer. 
{% endhint %}

## Přidání uzlů

Nyní byste měli vidět prázdný pracovní prostor. Vyzkoušejme si aplikaci Dynamo v akci! Zde je náš cíl:

>  :dart: **Vytvořte graf aplikace Dynamo, který vloží text do modelového prostoru.**

Vypadá to jednoduše, že? Než však začneme, musíme se seznámit s několika základními pojmy.

Základní stavební bloky grafu aplikace Dynamo se nazývají **uzly**. Uzel je jako malý počítač – vložíte do něj data, on s nimi provede nějakou práci a vygeneruje výsledek. Aplikace Dynamo for Civil 3D obsahuje **knihovnu** uzlů, které můžete propojit pomocí **drátů** a vytvořit tak **graf**, který dokáže větší a lepší věci než kterýkoli uzel sám o sobě.

{% hint style="info" %}
 **Počkat, co když jsem úplně nový uživatel aplikace Dynamo?**

Některé z těchto informací pro vás mohou být zcela nové – to je v pořádku! Tyto části vám pomohou.

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") 
{% endhint %}

Dobrá. Pojďme nyní sestavit náš graf. Zde je seznam všech uzlů, které budeme potřebovat.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

Tyto uzly můžete najít zadáním jejich názvu do vyhledávacího řádku v knihovně nebo kliknutím pravým tlačítkem myši kdekoli na kreslicí ploše a následným vyhledáním.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>Uzly lze umístit z knihovny nebo kliknutím pravým tlačítkem na kreslicí plochu.</p></figcaption></figure>

{% hint style="info" %}
 **Jak poznám, které uzly použít a kde je najít?**

Uzly v knihovně jsou seskupeny do logických kategorií podle toho, co dělají. Přečtěte si část [node-library.md](node-library.md "mention"), ve které naleznete podrobnější informace. 
{% endhint %}

Takto by měl vypadat výsledný graf.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>Dokončený graf</p></figcaption></figure>

Shrňme si, co jsme zde udělali:

> 1. Vybrali jsme, ve kterém dokumentu budeme pracovat. V tomto případě (a v mnoha dalších případech) chceme pracovat v aktivním dokumentu v aplikaci Civil 3D.
> 2. Definovali jsme cílový blok, ve kterém má být vytvořen textový objekt (v tomto případě modelový prostor).
> 3. Pomocí uzlu _String_ jsme určili, do které hladiny má být text umístěn.
> 4. Pomocí uzlu _Point.ByCoordinates_ jsme vytvořili bod, který definuje umístění textu.
> 5. Pomocí dvou uzlů _Number Slider_ jsme definovali souřadnice X a Y bodu vložení textu.
> 6. Pomocí dalšího uzlu _String_ jsme definovali obsah textového objektu.
> 7. Nakonec jsme vytvořili textový objekt.

Podívejme se na výsledky našeho nového krásného grafu!

## Zobrazení výsledku

V aplikaci Civil 3D zkontrolujte, zda je vybrána karta **Model**. Měl by se zobrazit nový textový objekt vytvořený aplikací Dynamo.

{% hint style="info" %}
 Pokud text nevidíte, možná budete muset spustit příkaz ZOOM -> EXTENTS, abyste se přiblížili na správné místo. 
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

Výborně! Nyní provedeme několik aktualizací textu.

Vraťte se do grafu aplikace Dynamo a změňte několik vstupních hodnot, například textový řetězec, souřadnice bodu vložení atd. Text by se měl v aplikaci Civil 3D automaticky aktualizovat. Všimněte si také, že pokud odpojíte některý ze vstupních portů, text se odstraní. Pokud vše připojíte zpět, text se znovu vytvoří. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>Dokončený graf v akci</p></figcaption></figure>

</div>

{% hint style="info" %}
 **Proč aplikace Dynamo nevloží nový textový objekt při každém spuštění grafu?**

Ve výchozím nastavení si aplikace Dynamo „pamatuje“ objekty, které vytvoří. Pokud změníte vstupní hodnoty uzlu, objekty v aplikaci Civil 3D se aktualizují místo vytváření zcela nových objektů. Další informace o tomto chování naleznete v části [object-binding.md](advanced-topics/object-binding.md "mention"). 
{% endhint %}

> :tada: Úkol splněn!

## Další postup

Tento příklad je pouze malou ukázkou toho, co všechno lze s aplikací Dynamo for Civil 3D dělat. Čtěte dál a dozvíte se více!
