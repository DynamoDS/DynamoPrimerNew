# Publikování balíčku

V předchozích částech jsme se zabývali podrobnostmi o tom, jak je balíček _MapToSurface_ nastaven pomocí vlastních uzlů a vzorových souborů. Ale jak publikujeme balíček, který byl vyvinut místně? Tato případová studie ukazuje, jak publikovat balíček ze sady souborů v místní složce.

\![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

Balíček lze publikovat mnoha způsoby. Níže je popsán proces, který doporučujeme: **publikovat místně, vyvíjet místně a poté publikovat online**. Začneme složkou obsahující všechny soubory v balíčku.

### Odinstalace balíčku

Před přechodem k publikování balíčku MapToSurface nejprve odinstalujte balíček z předchozí lekce, abyste nepracovali se stejnými balíčky.

Začněte přechodem na kartu Balíčky > Package Manager > Instalované balíčky > vedle položky MapToSurface klikněte na nabídku se svislými tečkami > Odstranit.

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

Poté restartujte aplikaci Dynamo. Při opakovaném otevření by se v okně _Správa balíčků_ již neměla nacházet položka _MapToSurface_. Teď jsme připraveni začít od začátku.

### Místní publikování balíčku

{% hint style="warning" %} Vlastní uzly a balíčky z aplikace Dynamo Sandbox ve verzi 2.17 a novějších můžete publikovat, pokud nemají žádné závislosti na hostitelském rozhraní API. Ve starších verzích je publikování vlastních uzlů a balíčků povoleno pouze v aplikacích Dynamo for Revit a Dynamo for Civil 3D. {% endhint %}

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Toto je první odeslání pro náš balíček a všechny ukázkové soubory a vlastní uzly souborů jsme umístili do jedné složky. Když je tato složka připravena, jsme připraveni k odeslání do správce Dynamo Package Manager.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Tato složka obsahuje pět vlastních uzlů (.dyf).
> 2. Tato složka také obsahuje pět vzorových souborů (.dyn) a jeden importovaný vektorový soubor (.svg). Tyto soubory budou sloužit jako úvodní cvičení, která uživateli ukážou, jak pracovat s vlastními uzly.

V aplikaci Dynamo začněte kliknutím na kartu _Balíčky > Package Manager > Publikovat nový balíček_.

Na kartě _Publikovat balíček_ vyplňte příslušná pole v levé části okna.

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

Dále přidáme soubory balíčku. Soubory můžete přidat po jednom nebo můžete přidat celou složku výběrem možnosti Přidat adresář (1). Chcete-li přidat soubory, které nejsou soubory .dyf, změňte typ souboru v okně prohlížeče na **Všechny soubory (**_._**)**. Všimněte si, že budeme přidávat soubor, vlastní uzel (.dyf) nebo ukázkový soubor (.dyn), bez rozdílu. Aplikace Dynamo tyto položky kategorizuje při publikování balíčku.

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

Po výběru složky MapToSurface nástroj Package Manager zobrazí obsah složky. Pokud nahráváte vlastní balíček se složitou strukturou složek a nechcete, aby aplikace Dynamo provedla změny ve struktuře složek, můžete povolit přepínač Zachovat strukturu složek. Tato možnost je určena pro pokročilé uživatele, a pokud balíček není záměrně nastaven určitým způsobem, je nejlepší nechat tento přepínač vypnutý a umožnit aplikaci Dynamo uspořádat soubory podle potřeby. Pokračujte kliknutím na tlačítko Další.

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

Zde si můžete prohlédnout, jak aplikace Dynamo uspořádá soubory balíčku před publikováním. Pokračujte kliknutím na tlačítko Dokončit.

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

Publikujte balíček kliknutím na tlačítko Publikovat místně (1).. Pokud pracujete s námi, klikněte na tlačítko _Publikovat místně_ a **ne** _Publikovat online_, abychom v nástroji Package Manager neměli duplicitní balíčky.

Po publikování by měly být vlastní uzly dostupné ve skupině DynamoPrimer nebo v knihovně aplikace Dynamo.

\![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Nyní se podívejme na kořenový adresář a uvidíme, jak aplikace Dynamo formátovala balíček, který jsme právě vytvořili. Proveďte to tak, že přejdete na kartu Nainstalované balíčky > vedle položky MapToSurface klikněte na nabídku se svislými tečkami > vyberte možnost Zobrazit kořenový adresář.

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

Všimněte si, že kořenový adresář se nachází v místním umístění balíčku (balíček jsme publikovali „místně“). Aplikace Dynamo aktuálně odkazuje na tuto složku pro čtení vlastních uzlů. Proto je důležité místně publikovat adresář do trvalého umístění složky (například ne na plochu). Zde je struktura složky balíčku Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. Složka _bin_ obsahuje soubory .dll vytvořené pomocí knihoven C# nebo Zero-Touch. Pro tento balíček žádné nemáme, proto je tato složka v tomto příkladu prázdná.
> 2. Složka _dyf_ slouží k umístění vlastních uzlů. Otevřením tohoto okna se zobrazí všechny vlastní uzly (soubory .dyf) pro tento balíček.
> 3. Složka navíc obsahuje všechny další soubory. Tyto soubory budou pravděpodobně soubory aplikace Dynamo (.dyn) nebo jakékoli další požadované soubory (.svg, .xls, .jpeg, .sat atd.).
> 4. Soubor pkg je základní textový soubor, který definuje nastavení balíčku. To je v aplikaci Dynamo automatické, ale pokud se chcete dostat do detailů, můžete je upravit.

### Publikování balíčku online

{% hint style="warning" %} Poznámka: Tento krok neprovádějte, pokud skutečně nepublikujete vlastní balíček! {% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. Až budete připraveni k publikování, v okně Balíčky > Package Manager > Instalované balíčky vyberte tlačítko vpravo od balíčku, který chcete publikovat, a zvolte možnost Publikovat.
2. Pokud aktualizujete balíček, který již byl publikován, klikněte na tlačítko Publikovat verzi a aplikace Dynamo aktualizuje balíček online podle nových souborů v kořenovém adresáři daného balíčku. Je to tak jednoduché.

### Publikování verze...

Při aktualizaci souborů v kořenové složce publikovaného balíčku můžete také publikovat novou verzi balíčku výběrem možnosti _Publikovat verzi_ na kartě _Moje balíčky_. Jedná se o snadný způsob, jak provést nezbytné aktualizace vašeho obsahu a sdílet jej s komunitou. Možnost _Publikovat verzi_ bude fungovat pouze v případě, že udržujete balíček.

### Převod vlastnictví balíčku

V současné době nelze převést vlastnictví balíčku pomocí nástroje Package Manager. Můžete ale požádat tým aplikace Dynamo, aby přidal dalšího vlastníka. Všimněte si, že nemůžeme odebrat existující vlastníky, pouze přidat další správce balíčku. Pokud si přejete přidat účet jako vlastníka k existujícího balíčku, pošlete e-mail na adresu [dynamoteam@dynamobim.org](mailto:dynamoteam@dynamobim.org). Nezapomeňte uvést název balíčku a název účtu, který chcete přidat.
