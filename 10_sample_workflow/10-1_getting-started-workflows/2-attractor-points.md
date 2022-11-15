# Body atraktoru

Body atraktorů jsou skvělé pro experimentování s geometrickými vzory. Lze je použít k postupným změnám objektů na základě jejich vzdálenosti.

Tento pracovní postup vás naučí, jak:

* Vytvářet, spravovat a upravovat seznamy.
* Přesouvat body v 3D náhledu pomocí přímé manipulace.
* Změnit režim spuštění.

![](../images/10-1/2/attractor1.gif)

## Definování našich cílů

V tomto cvičení vytvoříte kružnici (_Cíl_), kde je vstup poloměru definován vzdáleností k blízkému bodu (_Vztah_).

![Ruční náčrt kružnice](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> Bod, který definuje vztah podle vzdálenosti, se obvykle označuje jako „Atraktor“. Zde bude vzdálenost k našemu bodu atraktoru použita k určení, jak velký by měl být náš kruh.

## Další postup

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

Když máme nyní načrtnuté cíle a vztahy, můžeme začít vytvářet náš graf. Potřebujeme uzly, které představují posloupnost akcí, které budou aplikací Dynamo provedeny. Začneme přidáním následujících uzlů: **Number**, **Number Slider**, **Point.ByCoordinates**, **Geometry.DistanceTo, Circle.ByCenterPointRadius.**

![](../images/10-1/2/attractor(2).png)

> 1. Input > Basic > **Number**
> 2. Input > Basic > **Number Slider**
> 3. Geometry > Points > Point > **By Coordinates(x,y,z)**
> 4. Geometry > Modifiers > Geometry > **DistanceTo**
> 5. Geometry > Curves > Circle > **ByCenterPointRadius**

### Propojení uzlů pomocí drátů

Nyní, když máme několik uzlů, je nutné propojit porty uzlů pomocí drátů. Tato připojení budou definovat tok dat.

![](../images/10-1/2/attractor(3).png)

> 1. **Number** k **Point.ByCoordinates**
> 2. **Number Sliders** k **Point.ByCoordinates**
> 3. **Point.ByCoordinates** (2) k **DistanceTo**
> 4. **Point.ByCoordinates** a **DistanceTo** k **Circle.ByCenterPointRadius**

### Spuštění programu

Když je definován náš tok programu, stačí říct aplikaci Dynamo, aby jej provedla. Po spuštění programu (buď automaticky, nebo po kliknutí na tlačítko Spustit v ručním režimu) budou data procházet přes dráty a výsledky by měly být zobrazeny v 3D náhledu.

![](../images/10-1/2/attractor(4).png)

> 1. (Klikněte na tlačítko Spustit) – pokud je panel spuštění v ručním režimu, je nutné graf spustit kliknutím na tlačítko Spustit.
> 2. Náhled uzlu – pozastavením ukazatele myši nad polem v pravém dolním rohu uzlu zobrazíte místní nabídku výsledků.
> 3. 3D náhled – pokud některý z našich uzlů vytvoří geometrii, uvidíte ji v 3D náhledu.
> 4. Výstupní geometrie při vytvoření uzlu.

### Přidání **bloku kódu**

Pokud náš program funguje, měli bychom vidět kružnici v 3D náhledu, která prochází naším bodem atraktoru. To je skvělé, ale možná budeme chtít přidat více detailů nebo ovládacích prvků. Upravíme vstup na uzel kružnice, abychom mohli kalibrovat vliv na poloměr. Přidejte do pracovního prostoru další položku **Number Slider** a poté dvojitým kliknutím na prázdnou oblast pracovního prostoru přidejte uzel **Code Block**. Upravte pole v bloku kódu zadáním `X/Y`.

![](../images/10-1/2/attractor(5).png)

> 1. **Code Block**
> 2. **DistanceTo** a **Number Slider** k **Code Block**
> 3. **Code Block** k **Circle.ByCenterPointRadius**

### Použití posloupností

Začít jednoduše a přidávat složitost je efektivní způsob, jak průběžně vytvářet náš program. Jakmile bude fungovat pro jeden kruh, použijeme sílu programu na více než jeden kruh. Pokud místo jednoho středového bodu použijeme rastr bodů a přizpůsobíme změnu ve výsledné datové struktuře, náš program nyní vytvoří mnoho kružnic – každou s jedinečnou hodnotou poloměru definovanou kalibrovanou vzdáleností k bodu atraktoru.

![](../images/10-1/2/attractor(6).png)

> 1. Přidejte uzel **Number Sequence** a nahraďte vstupy uzlu **Point.ByCoordinates** – klikněte pravým tlačítkem myši na uzel Point.ByCoordinates a vyberte možnost Vázání > Kartézský součin.
> 2. Přidejte uzel **Flatten** za položku Point.ByCoordinates. Chcete-li seznam zcela rozvinout, ponechte pro vstup `amt`výchozí hodnotu `-1`.
> 3. 3D náhled se aktualizuje s rastrem kružnic.

### Přizpůsobení pomocí přímé manipulace

Někdy číselná manipulace není správný přístup. Nyní můžete ručně tlačit a táhnout bodovou geometrii při procházení 3D náhledu v pozadí. Také můžeme ovládat další geometrii, která byla vytvořena pomocí bodu. Například uzel **Sphere.ByCenterPointRadius** je také schopen přímé manipulace. Umístění bodu lze řídit z posloupností hodnot X, Y a Z pomocí **Point.ByCoordinates**. Při použití přímé manipulace však můžete hodnoty posuvníků aktualizovat ručním přesunutím bodu v režimu **Navigace 3D náhledu**. To nabízí intuitivnější přístup k ovládání sady diskrétních hodnot, které určují umístění bodu.

![](../images/10-1/2/attractor(7).png)

> 1. Chcete-li použít možnost **Přímá manipulace**, vyberte panel bodu, který chcete přesunout – nad vybraným bodem se zobrazí šipky.
> 2. Přepněte do režimu **Navigace 3D náhledu**.

![](../images/10-1/2/attractor\(8\).png)

> 1. Přesuňte kurzor nad bod a zobrazí se osy X, Y a Z.
> 2. Kliknutím a přetažením barevné šipky přemístíte odpovídající osu, přičemž hodnoty uzlu **Number Slider** se aktualizují na místo s ručně přesunutým bodem.

![](../images/10-1/2/attractor(1).png)

> 1. Všimněte si, že před **přímou manipulací** byl do komponenty **Point.ByCoordinates** připojen pouze jeden posuvník. Když bod ve směru X ručně přesuneme, aplikace Dynamo automaticky vygeneruje pro vstup X nový uzel **Number Slider**.

