# Vývoj pro aplikaci Dynamo

Platforma Dynamo je navržena tak, aby se přispěvateli mohli stát všichni uživatelé, bez ohledu na úroveň jejich zkušeností. Existuje několik možností vývoje, které se zaměřují na různé schopnosti a úrovně dovedností, přičemž každá má své silné a slabé stránky v závislosti na cíli. Níže uvádíme různé možnosti a způsob, jak si vybrat jednu z nich.

![Tři vývojová prostředí](images/developing-for-dynamo.png)

> Tři vývojová prostředí: Visual Studio, Editor jazyka Python a Blok kódů s jazykem DesignScript

#### Jaké máte možnosti? <a href="#what-are-my-options" id="what-are-my-options"></a>

Možnosti vývoje pro aplikaci Dynamo spadají primárně do dvou kategorií: _pro_ aplikaci Dynamo a _v_ aplikaci Dynamo. Tyto dvě kategorie si lze představit takto: „v“ aplikaci Dynamo znamená obsah vytvořený pomocí vývojového prostředí aplikace Dynamo, který bude použit v aplikaci Dynamo, a „pro“ aplikaci Dynamo znamená použití externích nástrojů k vytvoření obsahu, který bude importován do aplikace Dynamo a poté zde použit. Ačkoliv je tato příručka zaměřena na vývoj _pro_ aplikaci Dynamo, níže jsou popsány zdroje pro všechny procesy.

#### Pro aplikaci Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

Tyto uzly umožňují nejvyšší stupeň přizpůsobení. Touto metodou je sestaveno mnoho balíčků a je nezbytná pro přispívání do zdrojů aplikace Dynamo. Proces jejich sestavení bude popsán v této příručce.

* Uzly Zero Touch
* Uzly odvozené z uzlů NodeModel
* Rozšíření

> V této příručce naleznete pokyny k [importu knihoven Zero-Touch](https://primer2.dynamobim.org/v/cs/6_custom_nodes_and_packages/6-2_packages/5-zero-touch).

V následujícím pojednání je jako vývojové prostředí pro uzly Zero-Touch a NodeModel použita aplikace Visual Studio.

![Rozhraní aplikace Visual Studio](images/vs-devenv.jpg)

> Rozhraní aplikace Visual Studio s projektem, který budeme vyvíjet.

#### V aplikaci Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

Ačkoli tyto procesy existují v pracovním prostoru vizuálního programování a jsou poměrně přímočaré, všechny představují vhodné možnosti pro přizpůsobení aplikace Dynamo. Tato příručka se jimi podrobně zabývá a v kapitole [Strategie skriptování](../../9\_best\_practices/2-scripting-strategies.md) poskytuje tipy a osvědčené postupy.

*   Bloky kódu zobrazují jazyk DesignScript ve vizuálním programovacím prostředí a umožňují flexibilní práci s textovým skriptem a uzly. Funkci v bloku kódu může volat cokoli v pracovním prostoru.

    > Stáhněte si příklad bloku kódu (klikněte pravým tlačítkem a uložte jej) nebo si v této [příručce](https://primer2.dynamobim.org/v/cs/8_coding_in_dynamo/8-1_code-blocks-and-design-script/1-what-is-a-code-block) prohlédněte podrobnou ukázku.
*   Vlastní uzly jsou kontejnery pro kolekce uzlů nebo dokonce celých grafů. Jsou účinným způsobem, jak shromažďovat často používané postupy a sdílet je s komunitou.

    > Stáhněte si příklad vlastního uzlu (klikněte pravým tlačítkem a uložte jej) nebo si v této [příručce](https://primer2.dynamobim.org/v/cs/6_custom_nodes_and_packages/6-1_custom-nodes/1-introduction) prohlédněte podrobnou ukázku.
*   Uzly jazyka Python jsou skriptovací rozhraní v pracovním prostoru vizuálního programování, podobně jako bloky kódu. Knihovny Autodesk.DesignScript používají tečkovou notaci podobnou jazyku DesignScript.

    > Stáhněte si příklad uzlu jazyka Python (klikněte pravým tlačítkem a uložte jej) nebo si v této [příručce](https://primer2.dynamobim.org/v/cs/8_coding_in_dynamo/8-3_python) prohlédněte podrobnou ukázku.

Vývoj v pracovním prostoru Dynamo představuje výkonný nástroj pro získání okamžité zpětné vazby.

![Vývoj v pracovním prostoru aplikace Dynamo pomocí uzlu jazyka Python](images/python-example.jpg)

> Vývoj v pracovním prostoru aplikace Dynamo pomocí uzlu jazyka Python

#### Jaké jsou výhody/nevýhody jednotlivých možností vývoje? <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Možnosti vývoje aplikace Dynamo byly navrženy tak, aby řešily složitost potřeby přizpůsobení. Ať už je cílem napsat rekurzivní skript v jazyce Python, nebo sestavit vlastní uživatelské rozhraní uzlu, existují možnosti implementace kódu, které zahrnují pouze to, co je nezbytné pro spuštění.

**Bloky kódu, uzel jazyka Python a vlastní uzly v aplikaci Dynamo**

Toto jsou přímé možnosti pro psaní kódu ve vizuálním programovacím prostředí aplikace Dynamo. Pracovní prostor vizuálního programování aplikace Dynamo poskytuje přístup k jazyku Python a jazyku DesignScript a umožňuje zahrnout více uzlů do vlastního uzlu.

![Blok kódu, skript jazyka Python a vlastní uzel](images/Development-Icons.png)

Pomocí těchto metod můžeme:

* Začít psát kód v jazyce Python nebo DesignScript bez větších nároků na nastavení.
* Importovat knihovny jazyka Python do aplikace Dynamo.
* Sdílet bloky kódu, uzly jazyka Python a vlastní uzly s komunitou aplikace Dynamo v rámci balíčku.

**Uzly Zero Touch**

Zero-Touch označuje jednoduchou metodu importu knihoven C# pomocí najetí kurzoru a kliknutí. Aplikace Dynamo přečte veřejné metody knihovny `.dll` a převede je na uzly aplikace Dynamo. Pomocí funkce Zero-Touch můžete vyvíjet své vlastní uzly a balíčky.

![Uzly Zero-Touch](images/ZTImport.png)

Pomocí této metody můžeme:

* Importovat knihovnu, která nebyla nezbytně vyvinuta pro aplikaci Dynamo, a automaticky vytvořit sadu nových uzlů, viz [příklad A-Forge](../../6\_custom\_nodes\_and\_packages/6-2\_packages/5-zero-touch.md#case-study-importing-aforge) v příručce Primer.
* Psát metody C# a snadno je používat jako uzly v aplikaci Dynamo.
* Sdílet knihovny C# jako uzly s komunitou aplikace Dynamo v balíčku

**Uzly odvozené z uzlů NodeModel**

Tyto uzly jsou krokem hlouběji do struktury aplikace Dynamo. Jsou založeny na třídě `NodeModel` a napsány v jazyce C#. Tato metoda sice poskytuje největší flexibilitu a výkon, ale většina aspektů uzlu musí být explicitně definována a funkce musí být umístěny v samostatné sestavě.

![Uzly odvozené z uzlů NodeModel](images/Development-Icons-NodeModel.png)

Pomocí této metody můžeme:

* Vytvářet plně přizpůsobitelné uživatelské rozhraní uzlu s posuvníky, obrázky, barvami atd. (např. uzel ColorRange).
* Přistupovat k tomu, co se děje na kreslicí ploše aplikace Dynamo, a ovlivňovat to.
* Přizpůsobit vázání.
* Načítat uzly do aplikace Dynamo jako balíček.

#### Informace o změnách verzí aplikace Dynamo a rozhraní API (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Protože je aplikace Dynamo pravidelně aktualizována, mohou být provedeny změny v části rozhraní API, které jsou používány v balíčku. Sledování těchto změn je důležité pro zajištění správné funkce stávajících balíčků.

Změny rozhraní API jsou sledovány na [stránce Wiki aplikace Dynamo na Githubu](https://github.com/DynamoDS/Dynamo/wiki/API-Changes). Sledovány jsou změny v jádru aplikace Dynamo, knihovnách a pracovních prostorech.

![Dokument změn rozhraní API aplikace Dynamo](images/api-changes.jpg)

Příkladem nadcházející významné změny je přechod z formátu XML na formátu souboru JSON ve verzi 2.0. Uzly odvozené z uzlu NodeModel nyní potřebují [konstruktor JSON](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node), jinak se neotevřou v aplikaci Dynamo 2.0.

Dokumentace rozhraní API aplikace Dynamo aktuálně pokrývá hlavní funkce: [http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI)

![Dokumentace k rozhraní API](images/api-docs.jpg)

#### Oprávnění k distribuci binárních souborů v balíčku <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Dávejte pozor na soubory DLL obsažené v balíčku, který je nahráván do správce balíčků. Pokud autor balíčku nevytvořil knihovnu .dll, musí mít práva na její sdílení.

Jestliže balíček obsahuje binární soubory, je nutné uživatele při stahování upozornit, že balíček obsahuje binární soubory.
