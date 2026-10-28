# Miniatury grafů pro domovskou stránku aplikace Dynamo

Na domovské stránce aplikace Dynamo se miniatury grafů zobrazují na široké kartě. Tato karta má poměr stran 16:9. Obrázek je zarovnaný na střed a oříznutý nahoře i dole.

Čtvercová miniatura může v generativním návrhu vypadat kompletně, protože se zde zobrazuje celý čtvercový obrázek. Na domovské stránce aplikace Dynamo se však ze stejného obrázku mohou oříznout názvy nebo loga.

To platí pro libovolný graf (`.dyn`), nikoli pouze pro grafy v balíčku. Obrázek nastavte ve vlastnostech grafu. Ukládá se do souboru grafu.

{% hint style="info" %}
Nadpisy, loga a hlavní obrázek ponechte ve střední **bezpečné oblasti**. Horní a dolní část čtvercové miniatury používejte pouze jako pozadí.
{% endhint %}

## Šablony miniatur

Stáhněte si šablonu a navrhněte obrázek podle ní. Tmavší pruh uprostřed představuje oblast, která se zobrazí na domovské stránce. Světlejší pruhy nahoře a dole jsou na domovské stránce oříznuty. Generativní návrh zobrazuje celý čtverec.

> Stáhněte si šablonu ve formátu SVG a upravte ji v aplikaci pro úpravy vektorové grafiky, případně šablonu ve formátu PNG a vložte grafiku v obrazovém editoru.

{% file src="../.gitbook/assets/Template_Thumbnail.svg" %}

{% file src="../.gitbook/assets/DynamoThumbnailTemplate.png" %}

<figure><img src="../.gitbook/assets/DynamoThumbnailTemplate.png" alt="Šablona čtvercové miniatury. Tmavší střední pruh je viditelný na domovské stránce aplikace Dynamo. Světlejší horní a dolní pruhy jsou na domovské stránce oříznuty a v generativním návrhu zůstanou viditelné."><figcaption><p>Tmavší střední pruh je viditelný na domovské stránce. Světlejší horní a dolní pruhy jsou oříznuty.</p></figcaption></figure>

## Vytvoření miniatury

1. Otevřete šablonu ve formátu SVG nebo PNG v editoru obrázků.
2. Umístěte grafiku na růžový čtverec v šabloně.
3. Název, logo a hlavní obrázek ponechte uvnitř tmavšího středního pruhu.
4. Světlejší horní a dolní pruh použijte pouze pro barvu pozadí nebo doplňkovou grafiku.
5. Skryjte nebo odstraňte růžové vodítko a poté exportujte soubor PNG ve **čtvercovém** formátu.

Použijte **čtvercový ** obrázek (poměr stran 1:1). Domovská stránka vždy zobrazuje tmavší střední pruh jako výřez s poměrem stran 16:9. Velikost v pixelech nemá na oříznutí vliv. Pokud místo šablony začínáte s prázdným souborem, nastavte čtvercový formát. Samotná šablona miniatury má velikost 840 × 840 pixelů.

## Přidání miniatury do grafu

Domovská stránka načítá miniaturu ze souboru grafu (`.dyn`), nikoli z formuláře pro publikování nástroje Package Manager.

1. Otevřete graf v aplikaci Dynamo.
2. Přejděte do nabídky **Soubor > Zobrazit vlastnosti grafu > Obecné**.
3. Přidejte exportovaný soubor PNG do pole pro obrázek.
4. Uložte graf.

Pokud se graf dodává jako součást balíčku, přiložte při publikování také soubor`.dyn`. Viz část [Publikování balíčku](../6_custom_nodes_and_packages/6-2_packages/4-publishing.md).

## Kontrola výsledku na domovské stránce

1. Otevřete domovskou stránku aplikace Dynamo.
2. V části Nedávné vyhledejte graf, který jste právě uložili.
3. Ověřte, že název a logo jsou na kartě plně viditelné.

Pokud je text oříznut, posuňte jej směrem ke středu, znovu exportujte obrázek a aktualizujte vlastnosti grafu. Nespoléhejte se pouze na generativní návrh. Generativní návrh zobrazuje celý čtverec.
