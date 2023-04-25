# Publikování balíčku

V předchozích částech jsme se zabývali podrobnostmi o tom, jak je balíček _MapToSurface_ nastaven pomocí vlastních uzlů a vzorových souborů. Ale jak publikujeme balíček, který byl vyvinut místně? Tato případová studie ukazuje, jak publikovat balíček ze sady souborů v místní složce.

![](<../images/6-2/3/develop package - custom nodes 01 (1) (6).jpg>)

Balíček lze publikovat mnoha způsoby. Níže je popsán proces, který doporučujeme: **publikovat místně, vyvíjet místně a poté publikovat online**. Začneme složkou obsahující všechny soubory v balíčku.

### Odinstalace balíčku

Před přechodem k publikování balíčku MapToSurface nejprve odinstalujte balíček z předchozí lekce, abyste nepracovali se stejnými balíčky.

Začněte přechodem do nabídky Dynamo > Předvolby > Package Manager > vedle položky MapToSurface klikněte na nabídku se svislými tečkami > Odstranit.

![](../images/6-2/4/publishapackage-deletepackage.jpg)

Poté restartujte aplikaci Dynamo. Při opakovaném otevření by se v okně _Správa balíčků_ již neměla nacházet položka _MapToSurface_. Teď jsme připraveni začít od začátku.

### Místní publikování balíčku

{% hint style="warning" %} Publikování balíčků aplikace Dynamo je povoleno pouze v aplikacích Dynamo for Revit a Dynamo pro aplikaci Civil 3D. Aplikace Dynamo Sandbox nemá funkce pro publikování. {% endhint %}

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Toto je první odeslání pro náš balíček a všechny ukázkové soubory a vlastní uzly souborů jsme umístili do jedné složky. Když je tato složka připravena, jsme připraveni k odeslání do správce Dynamo Package Manager.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Tato složka obsahuje pět vlastních uzlů (.dyf).
> 2. Tato složka také obsahuje pět vzorových souborů (.dyn) a jeden importovaný vektorový soubor (.svg). Tyto soubory budou sloužit jako úvodní cvičení, která uživateli ukážou, jak pracovat s vlastními uzly.

V aplikaci Dynamo začněte kliknutím na položku _Balíčky > Publikovat nový balíček_.

![](../images/6-2/4/publishapackage-publishlocally02.jpg)

V okně _Publikování balíčků aplikace Dynamo_ jsme vyplnili příslušné údaje v levé části okna.

![](../images/6-2/4/publishapackage-publishlocally03.jpg)

> 1. Kliknutím na tlačítko _Přidat soubor_ jsme také přidali soubory ze struktury složek na pravé straně obrazovky (pro přidání souborů, které nejsou soubory .dyf, změňte typ souboru v okně prohlížeče na **Všechny soubory(**_**.**_**)**. Všimněte si, že jsme přidali každý soubor, vlastní uzel (.dyf) nebo ukázkový soubor (.dyn), bez rozdílu. Aplikace Dynamo tyto položky kategorizuje při publikování balíčku.
> 2. Pole Skupina definuje, ve které skupině budou v uživatelském rozhraní aplikace Dynamo vyhledány vlastní uzly.
> 3. Publikujte kliknutím na tlačítko Publikovat místně. Pokud pracujete s námi, klikněte na tlačítko _Publikovat místně_ a **ne** _Publikovat online_. V nástroji Package Manager nechceme duplicitní balíčky.

Po publikování by měly být vlastní uzly dostupné ve skupině DynamoPrimer nebo v knihovně aplikace Dynamo.

![](<../images/6-2/3/develop package - install package 02 (1) (4).jpg>)

Nyní se podívejme na kořenový adresář a uvidíme, jak aplikace Dynamo formátovala balíček, který jsme právě vytvořili. Proveďte to kliknutím na nabídku Dynamo > Předvolby > Package Manager > vedle položky MapToSurface klikněte na nabídku se svislými tečkami > vyberte možnost Zobrazit kořenový adresář.

![](../images/6-2/4/publishapackage-publishlocally05.jpg)

Všimněte si, že kořenový adresář se nachází v místním umístění balíčku (balíček jsme publikovali „místně“). Aplikace Dynamo aktuálně odkazuje na tuto složku pro čtení vlastních uzlů. Proto je důležité místně publikovat adresář do trvalého umístění složky (například ne na plochu). Zde je struktura složky balíčku Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. Složka _bin_ obsahuje soubory .dll vytvořené pomocí knihoven C# nebo Zero-Touch. Pro tento balíček žádné nemáme, proto je tato složka v tomto příkladu prázdná.
> 2. Složka _dyf_ slouží k umístění vlastních uzlů. Otevřením tohoto okna se zobrazí všechny vlastní uzly (soubory .dyf) pro tento balíček.
> 3. Složka navíc obsahuje všechny další soubory. Tyto soubory budou pravděpodobně soubory aplikace Dynamo (.dyn) nebo jakékoli další požadované soubory (.svg, .xls, .jpeg, .sat atd.).
> 4. Soubor pkg je základní textový soubor, který definuje nastavení balíčku. To je v aplikaci Dynamo automatické, ale pokud se chcete dostat do detailů, můžete je upravit.

### Publikování balíčku online

{% hint style="warning" %} Poznámka: Tento krok neprovádějte, pokud skutečně nepublikujete vlastní balíček! {% endhint %}

![](../images/6-2/4/publishapackage-publishonline01.jpg)

> 1. Až budete připraveni k publikování, v okně Předvolby > Package Manager vyberte tlačítko napravo od položky MapToSurface a vyberte možnost _Publikovat_.
> 2. Pokud aktualizujete balíček, který již byl publikován, klikněte na tlačítko Publikovat verzi a aplikace Dynamo aktualizuje balíček online podle nových souborů v kořenovém adresáři daného balíčku. Je to tak jednoduché.

### Publikovat verzi...

Při aktualizaci souborů v kořenové složce publikovaného balíčku můžete publikovat novou verzi balíčku výběrem možnosti _Publikovat verzi_ v okně _Správa balíčků_. Jedná se o snadný způsob, jak provést nezbytné aktualizace vašeho obsahu a sdílet jej s komunitou. Možnost _Publikovat verzi_ bude fungovat pouze v případě, že udržujete balíček.
