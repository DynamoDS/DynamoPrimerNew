# Úvod do práce s balíčky

Aplikace Dynamo nabízí velké množství funkcí, které jsou k dispozici ihned po instalaci, a také spravuje rozsáhlou knihovnu balíčků, která může možnosti aplikace Dynamo podstatně rozšířit. Balíček je kolekce vlastních uzlů nebo dalších funkcí. Nástroj Dynamo Package Manager je portál pro komunitu, kde lze stáhnout všechny balíčky, které byly publikovány online. Tyto sady nástrojů jsou vyvinuty třetími stranami, aby bylo možné rozšířit základní funkce aplikace Dynamo, jsou přístupné všem a připraveny ke stažení kliknutím na tlačítko.

![Web Package Manager](../images/6-2/1/dpm.jpg)

Projektu s otevřeným zdrojovým kódem, jako je Dynamo, tento druh zapojení komunity prospívá. Díky specializovaným vývojářům třetích stran je aplikace Dynamo schopna rozšířit svůj dosah na pracovní postupy napříč různými odvětvími. Z tohoto důvodu tým aplikace Dynamo vyvinul společné úsilí, aby zefektivnil vývoj a zveřejňování balíčků (které budou podrobněji popsány v následujících částech).

### Instalace balíčku

Nejjednodušším způsobem instalace balíčku je použití příkazu nabídky Balíčky v rozhraní aplikace Dynamo. Pojďme se do toho hned pustit a nainstalovat ho. V tomto rychlém příkladu nainstalujeme oblíbený balíček k vytvoření čtyřúhelníkových panelů v osnově.

V aplikaci Dynamo přejděte na kartu _Balíčky > Package Manager_.

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

Na panelu hledání vyhledejte „quads from rectangular grid“. Za chvíli by se měly zobrazit všechny balíčky, které odpovídají tomuto vyhledávacímu dotazu. Chceme vybrat první balíček s odpovídajícím názvem.

Kliknutím na tlačítko Instalovat přidejte tento balíček do knihovny a potvrďte jej. Hotovo!

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

Všimněte si, že v knihovně aplikace Dynamo je nyní k dispozici další skupina s názvem „buildz“. Tento název odkazuje na vývojáře balíčku a uživatelský uzel je umístěn do této skupiny. Můžete ji začít ihned používat.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

Pomocí uzlu **Code Block** rychle definujte pravoúhlou osnovu, výsledek odešlete do uzlu **Polygon.ByPoints** a následně do uzlu **Surface.ByPatch**, čímž zobrazíte seznam právě vytvořených obdélníkových panelů.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### Instalace složky balíčku – DynamoUnfold

Výše uvedený příklad se zaměřuje na balíček s jedním uživatelským uzlem, ale stejný postup se používá ke stahování balíčků s několika uživatelskými uzly a podpůrnými datovými soubory. Nyní si předvedeme složitější balíček: Dynamo Unfold.

Stejně jako ve výše uvedeném příkladu začněte výběrem položek _Balíčky > Package Manager_.

Tentokrát vyhledáme výraz _DynamoUnfold_. Jedná se o jedno slovo. Když se balíčky zobrazí, stáhněte je kliknutím na tlačítko Instalovat a přidejte balíček Dynamo Unfold do své knihovny aplikace Dynamo.

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

V knihovně aplikace Dynamo je k dispozici skupina aplikace _DynamoUnfold_ s více kategoriemi a s vlastními uzly.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

Nyní se podíváme na strukturu souborů balíčku. 

1. Nejprve přejděte do nabídky Balíčky > Package Manager > Instalované balíčky.
2. Vedle položky DynamoUnfold vyberte nabídku možností <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">.
3. Poté kliknutím na možnost Zobrazit kořenový adresář otevřete kořenovou složku pro tento balíček.

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

Tak se dostaneme do kořenového adresáře balíčku. Všimněte si, že máme tři složky a soubor.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. Složka _bin_ slouží k ukládání souborů .dll. Tento balíček Dynamo byl vyvinut pomocí možnosti Zero-Touch, proto jsou vlastní uzly uloženy v této složce.
> 2. Složka _dyf_ slouží k umístění vlastních uzlů. Tento balíček nebyl vytvořen pomocí vlastních uzlů aplikace Dynamo, proto je tato složka pro tento balíček prázdná.
> 3. Složka extra obsahuje všechny další soubory včetně vzorových souborů.
> 4. Soubor pkg je základní textový soubor, který definuje nastavení balíčku. Můžeme ho zatím ignorovat.

Při otevření složky extra se zobrazí několik vzorových souborů, které byly staženy spolu s instalací. Ne všechny balíčky mají vzorové soubory, ale zde je můžete najít, pokud jsou součástí balíčku.

Otevřeme položku SphereUnfold.

![](../images/6-2/1/rd2.jpg)

Po otevření souboru a stisknutí tlačítka Spustit na řešiči máme rozvinutou kouli. Ukázkové soubory, jako jsou tyto, jsou užitečné při studiu práce s novým balíčkem Dynamo.

![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### Procházení a zobrazení informací o balíčku

V nástroji Package Manager můžete procházet balíčky pomocí možností třídění a filtrování na kartě Vyhledat balíčky. K dispozici je několik filtrů pro hostitelský program, stav (nový, odmítnutý nebo neodmítnutý) a informace o tom, zda má balíček závislosti.

Seřazením balíčků můžete identifikovat vysoce hodnocené nebo nejčastěji stahované balíčky nebo vyhledat balíčky s nejnovějšími aktualizacemi. 

Další podrobnosti o jednotlivých balíčcích můžete také zobrazit kliknutím na možnost Zobrazit podrobnosti. Tím se v nástroji Package Manager otevře postranní panel, kde najdete podrobné informace o balíčku, jako je správa verzí a závislosti, adresa URL webu nebo úložiště, informace o licenci atd.

### Web nástroje Dynamo Package Manager

Další způsob, jak objevovat balíčky aplikace Dynamo, je prozkoumat web nástroje nástroj [Dynamo Package Manager](http://dynamopackages.com). Zde najdete závislosti balíčků a informace o kompatibilitě hostitelů a verzí, které poskytují autoři balíčků. Soubory balíčku můžete také stáhnout z aplikace Dynamo Package Manager, ale přímo z aplikace Dynamo je proces jednodušší.

![](../images/6-2/1/dpm2.jpg)

### Kde jsou soubory balíčků uloženy místně?

Pokud chcete zjistit, kde jsou uloženy soubory balíčku, klikněte v horní části navigace na možnosti Dynamo > Předvolby > Nastavení balíčku > Umístění souborů uzlů a balíčků, kde najdete aktuální adresář kořenové složky.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

Ve výchozím nastavení jsou balíčky nainstalovány do umístění podobného této cestě: _C:/Users/[uživatelské jméno]/AppData/Roaming/Dynamo/[verze aplikace Dynamo]_.

### Nastavení sdíleného umístění pro balíčky v organizaci

Informace pro uživatele, kteří se ptají, zda je možné nasadit aplikaci Dynamo (v libovolné formě) pomocí předem připojených balíčků: Chcete-li tento problém vyřešit a umožnit kontrolu v centrálním umístění pro všechny uživatele s instalacemi aplikace Dynamo, přidejte ke každé instalaci vlastní cestu k balíčku.

**Přidání síťové složky, ve které může správce BIM nebo jiní uživatelé dohlížet na plnění složky balíčky schválenými organizací**  

V uživatelském rozhraní jednotlivých aplikací přejděte do nabídky *Dynamo -> Předvolby -> Nastavení balíčku -> Umístění souborů uzlů a balíčků*. V dialogu klikněte na tlačítko Přidat cestu a přejděte do síťového umístění sdíleného prostředku balíčku. 
 
Jedná se o automatizovaný proces, který by zahrnoval přidání informací do konfiguračního souboru, který je nainstalován s aplikací Dynamo:  
 `C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]\DynamoSettings.xml`

Ve výchozím nastavení je konfigurace aplikace Dynamo pro Revit následující:
 
 
`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`</CustomPackageFolders>`

Přidání vlastního umístění by vypadalo takto:  

`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`<string>N:\OfficeFiles\Dynamo\Packages_Limited</string>`  

`</CustomPackageFolders>`


Centrální správu této složky lze také ovládat jednoduše nastavením složky pouze ke čtení.

### Načítání balíčků s binárními soubory ze síťového umístění

#### Scénář

Organizace může chtít standardizovat balíčky nainstalované různými pracovními stanicemi a uživateli. Způsobem, jak toho dosáhnout, by mohlo být nainstalovat tyto balíčky z nabídky *Dynamo -> Předvolby -> Nastavení balíčku -> Umístění souborů uzlů a balíčků*, vybrat síťovou složku jako umístění instalace a přimět pracovní stanice, aby tuto cestu přidaly do `Manage Node and Package Paths`.

#### Problém

I když tento scénář funguje správně pro balíčky, které obsahují pouze vlastní uzly, nemusí fungovat pro balíčky obsahující binární soubory, jako jsou uzly Zero-Touch. Tento problém je způsoben [bezpečnostními opatřeními](https://stackoverflow.com/questions/5328274/load-assembly-from-network-location), které rozhraní .NET Framework uplatňuje při načítá sestav, které pocházejí ze síťového umístění. Bohužel, použití konfiguračního prvku `loadFromRemoteSources`, jak je navrženo v odkazovaném vlákně, není pro aplikaci Dynamo možným řešením, protože je distribuován jako komponenta, nikoli jako aplikace.

#### Řešení

Jedním z možných alternativních řešení je použít namapovanou síťovou jednotku odkazující na síťové umístění a nechat pracovní stanice odkazovat na tuto cestu. Postup vytvoření mapované síťové jednotky je popsán [zde](https://support.microsoft.com/cs-cz/help/4026635/windows-10-map-a-network-drive).

### Další práce s balíčky

Komunita aplikace Dynamo neustále roste a vyvíjí se. Průběžně prohlížením aplikace Dynamo Package Manager najdete nové zajímavé informace. V následujících částech se podrobněji podíváme na balíčky, z pohledu koncového uživatele na tvorbu vlastního balíčku aplikace Dynamo.
