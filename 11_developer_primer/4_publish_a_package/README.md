# Publikování balíčku 

### Publikování balíčku <a href="#publish-a-package" id="publish-a-package"></a>

Balíčky představují pohodlný způsob ukládání a sdílení uzlů s komunitou aplikace Dynamo. Balíček může obsahovat vše od vlastních uzlů vytvořených v pracovním prostoru aplikace Dynamo po uzly odvozené z uzlů NodeModel. Balíčky jsou publikovány a nainstalovány pomocí nástroje Package Manager. Kromě této stránky naleznete v příručce [Primer](https://primer2.dynamobim.org/v/cs/6_custom_nodes_and_packages/6-2_packages/1-introduction) také obecné pokyny k balíčkům.

#### Co je nástroj Package Manager? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Nástroj Dynamo Package Manager je softwarový registr (podobně jako nástroj npm), který je přístupný z aplikace Dynamo nebo z webového prohlížeče. Nástroj Package Manager umožňuje instalaci, publikování, aktualizaci a prohlížení balíčků. Stejně jako nástroj npm uchovává různé verze balíčků. Pomáhá také spravovat závislosti projektu.

V prohlížeči si můžete vyhledat balíčky a zobrazit statistiku: [https://dynamopackages.com/](https://dynamopackages.com)

* Nástroj Package Manager v aplikaci Dynamo umožňuje instalaci, publikování a aktualizaci balíčků.

![Vyhledání balíčků](images/dynamopackagemanager.jpg)

> 1. Vyhledejte balíčky online: `Packages > Search for a Package...`
> 2. Zobrazte nebo upravte nainstalované balíčky: `Packages > Manage Packages...`
> 3. Publikujte nový balíček: `Packages > Publish New Package...`

#### Publikování balíčku <a href="#publishing-a-package" id="publishing-a-package"></a>

Balíčky jsou publikovány pomocí nástroje Package Manager v aplikaci Dynamo. Doporučeným postupem je publikovat balíček místně, otestovat jej a poté jej publikovat online a sdílet s komunitou. Pomocí případové studie uzlu NodeModel si ukážeme kroky potřebné k publikování uzlu RectangularGrid jako balíčku místně a poté online.

Spusťte aplikaci Dynamo a výběrem možnosti `Packages > Publish New Package...` otevřete okno `Publish a Package`.

![Publikování balíčku](images/dyn-publish-package-add-files.jpg)

> 1. Vyberte `Add file...` a vyhledejte soubory, které chcete přidat do balíčku.
> 2. Vyberte dva soubory `.dll` z případové studie uzlu NodeModel.
> 3. Klikněte na tlačítko `Ok`.

Po přidání souborů do obsahu balíčku zadejte název, popis a verzi balíčku. Publikováním balíčku pomocí aplikace Dynamo se automaticky vytvoří soubor `pkg.json`.

![Nastavení balíčku](images/dyn-publish-package.jpg)

> Balíček připravený k publikování.
>
> 1. Zadejte požadované informace: název, popis a verzi.
> 2. Publikujte kliknutím na tlačítko Publikovat místně a vyberte složku balíčku aplikace Dynamo `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages`, aby byl uzel dostupný v infrastruktuře Core. Vždy publikujte místně, dokud nebude balíček připraven ke sdílení.

Po publikování balíčku budou uzly dostupné v knihovně aplikace Dynamo v kategorii `CustomNodeModel`.

![Balíček v knihovně aplikace Dynamo](images/dyn-publish-package-library.jpg)

> 1. Právě vytvořený balíček v knihovně aplikace Dynamo

Když je balíček připraven k publikování online, otevřete nástroj Package Manager a vyberte možnost `Publish` a poté `Publish Online`.

![Publikování balíčku v nástroji Package Manager](images/dyn-publish-package-directory.jpg)

> 1. Chcete-li se podívat, jak aplikace Dynamo naformátovala balíček, klikněte na tři svislé tečky vpravo od položky CustomNodeModel a vyberte možnost Zobrazit kořenový adresář.
> 2. V okně Publikovat balíček aplikace Dynamo vyberte možnost `Publish` a poté `Publish Online`.
> 3. Chcete-li balíček odstranit, vyberte možnost `Delete`.

#### Jak lze balíček aktualizovat? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

Aktualizace balíčku probíhá podobně jako publikování. Otevřete nástroj Package Manager, vyberte položku `Publish Version...` u balíčku, který je třeba aktualizovat, a zadejte vyšší verzi.

![Publikování verze balíčku](images/dyn-publish-package-version.jpg)

> 1. Výběrem možnosti `Publish Version` aktualizujte existující balíček novými soubory v kořenovém adresáři a poté vyberte, zda má být balíček publikován místně nebo online.

#### Webový klient Package Manager <a href="#package-manager-web-client" id="package-manager-web-client"></a>

Webový klient Package Manager umožňuje uživatelům vyhledávat a prohlížet data balíčku, včetně správy verzí, statistik stahování a dalších relevantních informací. Kromě toho se autoři balíčků mohou přihlásit a aktualizovat podrobnosti o svých balíčcích, například informace o kompatibilitě, přímo prostřednictvím webového klienta.

Další informace o těchto funkcích najdete v blogovém příspěvku na následujícím odkazu: [https://dynamobim.org/discover-the-new-dynamo-package-management-experience/](https://dynamobim.org/discover-the-new-dynamo-package-management-experience/).

K webovému klientu Package Manager lze získat přístup pomocí tohoto odkazu: [https://dynamopackages.com/](https://dynamopackages.com)

![Webový klient Package Manager](images/packagemanager-browser.jpg)

##### Aktualizace podrobností balíčku

Autoři mohou upravit popis balíčku, odkaz na web a odkaz na úložiště pomocí následujících kroků:  

> 1. V části **Moje balíčky** vyberte balíček a klikněte na možnost **Upravit podrobnosti balíčku**.  
> 2. Pomocí příslušných polí přidejte nebo upravte odkazy na **webové stránky** a **úložiště**.  
> 3. Podle potřeby aktualizujte **popis balíčku**.  
> 4. Kliknutím na tlačítko **Uložit změny** aktualizace použijte.  

 **Poznámka**: Aktualizace v nástroji Package Manager v aplikaci Dynamo mohou trvat až 15 minut, protože aktualizace serveru nějakou dobu trvají. Snažíme se toto zpoždění zkrátit.  

 ![Nové uživatelské rozhraní pro aktualizaci podrobností o publikovaných balíčcích](images/Package-Manager_Image_5.png)

##### Úprava informací o kompatibilitě publikovaných verzí balíčků  

Informace o kompatibilitě lze zpětně aktualizovat pro dříve publikované verze balíčků. Postupujte podle následujících kroků:  

![Úprava informací o kompatibilitě publikovaných balíčků – krok 1](images/Package-Manager_Image_6.png)

**Krok 1:**  

1. Klikněte na verzi balíčku, kterou chcete aktualizovat.  
2. Seznam **Závisí na** bude automaticky vyplněn balíčky, na kterých váš balíček závisí.  
3. Kliknutím na ikonu tužky vedle položky **Kompatibilita** otevřete okno pracovního postupu **Upravit informace o kompatibilitě**.  

**Krok 2:**  

Postupujte podle níže uvedeného vývojového diagramu a podívejte se na tabulku níže, jež vám pomůže pochopit, která možnost je pro váš balíček nejvhodnější.

![Kterou možnost zvolit pro pracovní postup Upravit informace o kompatibilitě](images/Package-Manager_Image_7.png)

Projděme si na příkladech několik scénářů:

**Příklad balíčku č. 1** – Civil Connection: Tento balíček obsahuje závislosti rozhraní API pro aplikace Revit i Civil 3D a neobsahuje kolekci základních uzlů (např. geometrické funkce, matematické funkce nebo správu seznamů). V tomto případě by tedy bylo ideální zvolit možnost 1. Balíček se zobrazí jako kompatibilní v aplikacích Revit a Civil 3D, které odpovídají rozsahu verzí nebo seznamu jednotlivých verzí.

**Příklad balíčku č. 2** – Rhythm: Tento balíček obsahuje kolekci uzlů specifických pro aplikaci Revit a kolekci základních uzlů. V tomto případě má balíček závislosti hostitele. Obsahuje ale také základní uzly, které budou fungovat v komponentě Dynamo Core. V tomto případě by tedy bylo ideální zvolit možnost 2. Balíček se zobrazí jako kompatibilní v aplikaci Revit a prostředí Dynamo Core (označovaném také jako Dynamo Sandbox), které odpovídají rozsahu verzí nebo seznamu jednotlivých verzí.

**Příklad balíčku č. 3** – Mesh Toolkit: Tento balíček je balíček Dynamo Core, což je kolekce uzlů geometrie, která nemá žádné závislosti na hostiteli. V tomto případě by tedy bylo ideální zvolit možnost 3. Balíček se zobrazí jako kompatibilní v aplikaci Dynamo a ve všech hostitelských prostředích, které odpovídají rozsahu verzí nebo seznamu jednotlivých verzí.

![Možnosti úprav informací o kompatibilitě](images/Package-Manager_Image_8.png)

V závislosti na vybrané možnosti se zobrazí pole specifická pro aplikaci Dynamo a/nebo hostitele, jak je znázorněno na obrázku níže.

![Úprava informací o kompatibilitě – krok 2](images/Package-Manager_Image_9.png)
