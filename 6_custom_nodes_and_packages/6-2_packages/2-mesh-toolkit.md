# Příklad balíčku – sada nástrojů pro sítě

Sada nástrojů pro sítě obsahuje nástroje pro import sítí ze souborů různých formátů, tvorbu sítě z objektů geometrií aplikace Dynamo a ruční tvorbu sítí z bodů a indexů. Knihovna také obsahuje nástroje pro úpravy sítí a extrahování vodorovných řezů pro použití ve výrobě.

![](<../images/6-2/2/meshToolkit case study 01.jpg>)

Sada nástrojů pro sítě aplikace Dynamo je součástí výzkumu společnosti Autodesk a v budoucnu budou její funkce přibývat. Do sady budou často přidávány nové metody, tým aplikace Dynamo ocení jakékoliv komentáře, hlášení chyb nebo nápady na nové funkce.

### Sítě vs. tělesa

V následujícím cvičení budou demonstrovány základní operace pomocí sady nástrojů pro sítě. V tomto cvičení protneme síť řadou rovin, což by u těles bylo výpočetně náročné. Na rozdíl od tělesa má síť „rozlišení“, které není definováno matematicky, ale topologicky, a je možné ho definovat podle aktuální úlohy. Další podrobnosti o vztahu mezi sítí a tělesem naleznete v kapitole [Geometrie pro návrh výpočtu](../../a-closer-look-at-dynamo-essential-nodes-and-concepts/5\_geometry-for-computational-design/) v této příručce Primer. Další informace o sadě nástrojů pro sítě naleznete na [wiki stránce aplikace Dynamo.](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit) Cvičení níže demonstruje práci s tímto balíčkem.

### Instalace sady nástrojů pro sítě

V horní nabídce aplikace Dynamo vyberte možnost _Balíčky > Hledat balíčky..._. Do pole pro hledání zadejte slovo _„MeshToolkit“_, dbejte na velikost písmen. Kliknutím na tlačítko Instalovat zahájíte stahování. Je to tak jednoduché.

![](<../images/6-2/2/meshToolkit case study - install package.jpg>)

## Cvičení: Průnik sítě

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/6-2/2/MeshToolkit.zip" %}

V tomto příkladu se podíváme na uzel průniku v sadě nástrojů pro sítě. Provedeme import sítě a protneme ji řadou vstupních rovin, čímž vytvoříme řezy. Tím začne příprava modelu na výrobu, řezání laserovým nebo vodním paprskem či CNC frézování.

Začněte otevřením souboru _Mesh-Toolkit\_Intersect-Mesh.dyn v aplikaci Dynamo._

![](<../images/6-2/2/meshToolkit case study - exercise 01.jpg>)

> 1. **File Path:** Vyhledejte soubor sítě, který chcete importovat (_stanford\_bunny\_tri.obj_). Podporované typy souborů jsou .mix a .obj
> 2. **Mesh.ImportFile:** Připojte cestu k souboru, aby došlo k importu sítě

![](<../images/6-2/2/meshToolkit case study - exercise 02.jpg>)

> 1. **Point.ByCoordinates:** Vytvořte bod, ten bude středem oblouku.
> 2. **Arc.ByCenterPointRadiusAngle:** Vytvořte oblouk kolem bodu. Tato křivka se použije k umístění řady rovin. \_\_ Nastavení jsou následující: \_\_ `radius: 40, startAngle: -90, endAngle:0`

Vytvořte řadu rovin orientovaných podél oblouku.

![](<../images/6-2/2/meshToolkit case study - exercise 03.jpg>)

> 1. **Code Block**: Vytvořte 25 čísel v rozmezí od 0 do 1.
> 2. **Curve.PointAtParameter:** Připojte oblouk na vstup _„curve“_ a výstup bloku s kódem na vstup _„param“_, čímž získáte řadu bodů na křivce.
> 3. **Curve.TangentAtParameter:** Připojte stejné vstupy jako u předchozího uzlu.
> 4. **Plane.ByOriginNormal:** Připojte body na vstup _„origin“_ a vektory na vstup _„normal“_, čímž v jednotlivých bodech vytvoříte řadu rovin.

Nyní tyto roviny použijeme k protnutí sítě.

![](<../images/6-2/2/meshToolkit case study - exercise 04.jpg>)

> 1. **Mesh.Intersect:** Vytvořte průnik rovin s importovanou sítí, čímž vznikne řada polykřivkových kontur. Klikněte pravým tlačítkem myši na uzel a nastavte vázání na nejdelší.
> 2. **PolyCurve.Curves:** Rozdělte polykřivky na jejich křivkové fragmenty.
> 3. **Curve.EndPoint:** Extrahujte koncové body jednotlivých křivek.
> 4. **NurbsCurve.ByPoints:** Pomocí bodů vytvořte křivku nurbs. K uzavření křivek použijte uzel Boolean nastavený na _True_.

Než budete pokračovat, vypněte náhled některých uzlů, například Mesh.ImportFile, Curve.EndPoint, Plane.ByOriginNormal a Arc.ByCenterPointRadiusAngle, abyste lépe viděli výsledek.

![](<../images/6-2/2/meshToolkit case study - exercise 05.jpg>)

> 1. **Surface.ByPatch:** Vytvořte záplaty ploch pro každou konturu, čímž vytvoříte „řezy“ sítě.

Přidejte druhou řadu řezů, čímž vznikne efekt podobný vaflím.

![](<../images/6-2/2/meshToolkit case study - exercise 06.jpg>)

Možná jste si všimli, že operace průniku se u sítí počítají rychleji než u těles. Pracovní postupy podobné těm jako v tomto cvičení fungují se sítěmi velmi dobře.
