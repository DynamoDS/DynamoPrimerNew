# Příklad balíčku – sada nástrojů pro sítě

Sada nástrojů pro sítě obsahuje nástroje pro import sítí ze souborů různých formátů, tvorbu sítě z objektů geometrií aplikace Dynamo a ruční tvorbu sítí z bodů a indexů. Knihovna také obsahuje nástroje pro úpravy sítí a extrahování vodorovných řezů pro použití ve výrobě.

\![](<../images/6-2/2/meshToolkitcasestudy01 (2).jpg>)

Balíček Dynamo Mesh Toolkit je součástí probíhajícího výzkumu společnosti Autodesk a proto se bude v nadcházejících letech dále rozvíjet. Do sady budou často přidávány nové metody, tým aplikace Dynamo ocení jakékoliv komentáře, hlášení chyb nebo nápady na nové funkce.

### Sítě vs. tělesa

V následujícím cvičení budou demonstrovány základní operace pomocí sady nástrojů pro sítě. V tomto cvičení protneme síť řadou rovin, což by u těles bylo výpočetně náročné. Na rozdíl od tělesa má síť „rozlišení“, které není definováno matematicky, ale topologicky, a je možné ho definovat podle aktuální úlohy. Další podrobnosti o vztahu mezi sítí a tělesem naleznete v kapitole [Geometrie pro výpočetní návrh](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/) v této příručce Primer. Další informace o balíčku Mesh Toolkit naleznete na [stránce Wiki k aplikaci Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit). Cvičení níže demonstruje práci s tímto balíčkem.

### Instalace balíčku Mesh Toolkit

V horní nabídce aplikace Dynamo vyberte možnost _Balíčky > Vyhledat balíčky_. Do pole pro hledání zadejte výraz _MeshToolkit_.Jedná se o jedno slovo, dbejte na velikost písmen. Kliknutím na tlačítko Instalovat zahájíte stahování. Je to tak jednoduché.

![](../images/6-2/2/meshToolkitcasestudy-installpackage.jpg)

## Cvičení: Průnik sítě

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/6-2/2/MeshToolkit.zip" %}

V tomto příkladu se podíváme na uzel průniku v sadě nástrojů pro sítě. Provedeme import sítě a protneme ji řadou vstupních rovin, čímž vytvoříme řezy. Tím začne příprava modelu na výrobu, řezání laserovým nebo vodním paprskem či CNC frézování.

Začněte otevřením souboru _Mesh-Toolkit_Intersect-Mesh.dyn v aplikaci Dynamo_.

![](../images/6-2/2/meshToolkitcasestudy-exercise01.jpg)

> 1. **File Path:** Vyhledejte soubor sítě, který chcete importovat (_stanford_bunny_tri.obj_). Podporované typy souborů jsou .mix a .obj
> 2. **Mesh.ImportFile:** Připojte cestu k souboru, aby došlo k importu sítě.

![](../images/6-2/2/meshToolkitcasestudy-exercise02.jpg)

> 1. **Point.ByCoordinates:** Vytvořte bod, který bude středem oblouku.
> 2. **Arc.ByCenterPointRadiusAngle:** Vytvořte oblouk kolem bodu. Tato křivka bude použita k umístění řady rovin. __ Nastavení jsou následující: __ `radius: 40, startAngle: -90, endAngle:0`

Vytvořte řadu rovin orientovaných podél oblouku.

![](../images/6-2/2/meshToolkitcasestudy-exercise03.jpg)

> 1. **Code Block**: Vytvořte 25 čísel v rozmezí od 0 do 1.
> 2. **Curve.PointAtParameter:** Připojte oblouk ke vstupu _curve_ a výstup bloku s kódem ke vstupu _param_, čímž získáte řadu bodů na křivce.
> 3. **Curve.TangentAtParameter:** Připojte stejné vstupy jako u předchozího uzlu.
> 4. **Plane.ByOriginNormal:** Připojte body ke vstupu _origin_ a vektory ke vstupu _normal_, čímž v jednotlivých bodech vytvoříte řadu rovin.

Nyní tyto roviny použijeme k protnutí sítě.

![](../images/6-2/2/meshToolkitcasestudy-exercise04.jpg)

> 1. **Mesh.Intersect:** Vytvořte průnik rovin s importovanou sítí, čímž vznikne řada kontur objektů polycurve. Klikněte pravým tlačítkem myši na uzel a nastavte vázání na nejdelší.
> 2. **PolyCurve.Curves:** Rozdělte objekty polycurve na fragmenty křivek.
> 3. **Curve.EndPoint:** Extrahujte koncové body jednotlivých křivek.
> 4. **NurbsCurve.ByPoints:** Pomocí bodů vytvořte křivku nurbs. K uzavření křivek použijte uzel Boolean nastavený na _True_.

Než budete pokračovat, vypněte náhled některých uzlů, například Mesh.ImportFile, Curve.EndPoint, Plane.ByOriginNormal a Arc.ByCenterPointRadiusAngle, abyste lépe viděli výsledek.

![](../images/6-2/2/meshToolkitcasestudy-exercise05.jpg)

> 1. **Surface.ByPatch:** Vytvořte záplaty ploch pro každou konturu, čímž vytvoříte „řezy“ sítě.

Přidejte druhou řadu řezů, čímž vznikne efekt podobný vaflím.

![](../images/6-2/2/meshToolkitcasestudy-exercise06.jpg)

Možná jste si všimli, že operace průniku se u sítí počítají rychleji než u těles. Pracovní postupy podobné těm jako v tomto cvičení fungují se sítěmi velmi dobře.
