# Příklad balíčku – sada nástrojů pro sítě

Balíček Dynamo Mesh Toolkit poskytuje nástroje k vytváření sítí z objektů geometrie aplikace Dynamo a ručnímu vytváření sítí pomocí jejich vrcholů a indexů. Knihovna také obsahuje nástroje pro úpravy sítí a extrahování vodorovných řezů pro použití ve výrobě. Ačkoli lze tuto sadu nástrojů využít také k importu a dotazování externích souborů sítě, v tomto příkladu je síť zahrnuta ve skriptu jako sada vrcholů a indexů, takže není potřeba žádný další soubor. Místo externího souboru sítě využívá tento graf uzly **Data.Remember** k uložení všech informací potřebných k opětovnému vytvoření slavné sítě s názvem Stanfordský zajíček.

\![](<../../.gitbook/assets/meshToolkit case study 01.jpg>)

Balíček Dynamo Mesh Toolkit je součástí probíhajícího výzkumu společnosti Autodesk a proto se bude v nadcházejících letech dále rozvíjet. Do sady budou často přidávány nové metody, tým aplikace Dynamo ocení jakékoliv komentáře, hlášení chyb nebo nápady na nové funkce.

### Sítě vs. tělesa

V následujícím cvičení budou demonstrovány základní operace pomocí sady nástrojů pro sítě. V tomto cvičení protneme síť řadou rovin, což by u těles bylo výpočetně náročné. Na rozdíl od tělesa má síť „rozlišení“, které není definováno matematicky, ale topologicky. Toto rozlišení můžeme definovat podle aktuální úlohy. Další podrobnosti o vztahu mezi sítí a tělesem naleznete v kapitole [Geometrie pro výpočetní návrh](../../5_essential_nodes_and_concepts/5-2_geometry-for-computational-design/) v této příručce. Další informace o balíčku Mesh Toolkit naleznete na [stránce Wiki k aplikaci Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit). Cvičení níže demonstruje práci s tímto balíčkem.

### Instalace balíčku Mesh Toolkit

V horní nabídce aplikace Dynamo vyberte možnost Balíčky > Package Manager. Do vyhledávacího pole zadejte MeshToolkit. Jedná se o jedno slovo. Klikněte na tlačítko Instalovat a potvrďte, že chcete zahájit stahování. Je to tak jednoduché.

<figure><img src="../../.gitbook/assets/install-mesh-toolkit.png" alt=""><figcaption></figcaption></figure>

## Cvičení: Průnik sítě

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../../.gitbook/assets/MeshToolkit.zip" %}

V tomto příkladu se podíváme na uzel průniku v sadě nástrojů pro sítě. Z uložených vrcholů vygenerujeme síť a protneme ji řadou vstupních rovin, čímž vytvoříme řezy. Tím začne příprava modelu na výrobu, řezání laserovým nebo vodním paprskem či CNC frézování.

Začněte otevřením souboru _Mesh-Toolkit_Intersect-Mesh.dyn v aplikaci Dynamo_.

\![](<../../.gitbook/assets/meshToolkit case study - exercise 01.jpg>)

> 1. **Data.Remember:** Tyto dva uzly obsahují vrcholy sítě, které jsou uloženy jako body, a indexy sítě, což jsou řady celých čísel. Dohromady poskytují všechny potřebné informace k novému vytvoření sítě.
> 2. **Mesh.ByVerticesAndIndices:** Propojením vrcholů a indexů z datových uzlů vytvořte síť.

\![](<../../.gitbook/assets/meshToolkit case study - exercise 02.jpg>)

> 1. **Point.ByCoordinates:** Vytvořte bod, který bude středem oblouku.
> 2. **Arc.ByCenterPointRadiusAngle:** Vytvořte oblouk kolem bodu. Tato křivka bude použita k umístění řady rovin. __ Nastavení jsou následující: __ `radius: 40, startAngle: -90, endAngle:0`

Vytvořte řadu rovin orientovaných podél oblouku.

\![](<../../.gitbook/assets/meshToolkit case study - exercise 03.jpg>)

> 1. **Code Block**: Vytvořte 25 čísel v rozmezí od 0 do 1.
> 2. **Curve.PointAtParameter:** Připojte oblouk ke vstupu _curve_ a výstup bloku s kódem ke vstupu _param_, čímž získáte řadu bodů na křivce.
> 3. **Curve.TangentAtParameter:** Připojte stejné vstupy jako u předchozího uzlu.
> 4. **Plane.ByOriginNormal:** Připojte body ke vstupu _origin_ a vektory ke vstupu _normal_, čímž v jednotlivých bodech vytvoříte řadu rovin.

Nyní tyto roviny použijeme k protnutí sítě.

\![](<../../.gitbook/assets/meshToolkit case study - exercise 04.jpg>)

> 1. **Mesh.Intersect:** Vytvořte průnik rovin s generovanou sítí, čímž vznikne řada kontur objektů polycurve. Klikněte pravým tlačítkem myši na uzel a nastavte vázání na nejdelší.
> 2. **PolyCurve.Curves:** Rozdělte objekty polycurve na fragmenty křivek.
> 3. **Curve.EndPoint:** Extrahujte koncové body jednotlivých křivek.
> 4. **NurbsCurve.ByPoints:** Pomocí bodů vytvořte křivku nurbs. K uzavření křivek použijte uzel Boolean nastavený na _True_.

Než budete pokračovat, vypněte náhled některých uzlů, například Mesh.ByVerticesAndIndices, Curve.EndPoint, Plane.ByOriginNormal a Arc.ByCenterPointRadiusAngle, abyste lépe viděli výsledek.

\![](<../../.gitbook/assets/meshToolkit case study - exercise 05.jpg>)

> 1. **Surface.ByPatch:** Vytvořte záplaty ploch pro každou konturu, čímž vytvoříte „řezy“ sítě.

Přidejte druhou řadu řezů, čímž vznikne efekt podobný vaflím.

\![](<../../.gitbook/assets/meshToolkit case study - exercise 06.jpg>)

Možná jste si všimli, že operace průniku se u sítí počítají rychleji než u těles. Pracovní postupy podobné těm jako v tomto cvičení fungují se sítěmi velmi dobře.
