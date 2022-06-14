# Uzly jazyka Python

Proč byste měli používat textové programování ve vizuálním programovacím prostředí aplikace Dynamo? [Vizuální programování](../../a\_appendix/visual-programming-and-dynamo.md) má mnoho výhod. Umožňuje vytvářet programy bez použití speciální syntaxe v intuitivním vizuálním rozhraní. Vizuální program však může být nepřehledný a v některých případech může mít menší funkčnost. Například jazyk Python nabízí mnohem jednodušší metody pro zápis podmínek (if/then) a cyklů. Python je výkonný nástroj, který umožňuje rozšířit možnosti aplikace Dynamo a umožňuje nahradit mnoho uzlů několika stručnými řádky kódů.

**Vizuální program:**

![](<../images/8-3/1/python node - visual vs textual programming.jpg>)

**Textový program:**

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

solid = IN[0]
seed = IN[1]
xCount = IN[2]
yCount = IN[3]

solids = []

yDist = solid.BoundingBox.MaxPoint.Y-solid.BoundingBox.MinPoint.Y
xDist = solid.BoundingBox.MaxPoint.X-solid.BoundingBox.MinPoint.X

for i in xRange:
	for j in yRange:
		fromCoord = solid.ContextCoordinateSystem
		toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin,Vector.ByCoordinates(0,0,1),(90*(i+j%val)))
		vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
		toCoord = toCoord.Translate(vec)
		solids.append(solid.Transform(fromCoord,toCoord))

OUT = solids
```

### Uzel Python

Stejně jako bloky kódu jsou uzly Python skriptovacím rozhraním v prostředí vizuálního programování. Uzel jazyka Python naleznete v knihovně v části Script > Editor > Python Script.

![](<../images/8-3/1/python node - the python node 01.jpg>)

Dvojitým kliknutím na uzel otevřete editor skriptů jazyka Python (můžete také kliknout pravým tlačítkem na uzel a vybrat možnost _Upravit..._). Všimněte si, že se v horní části nachází výchozí text, který vám má pomoci odkazovat na knihovny, které budete potřebovat. Vstupy jsou uloženy v poli IN. Hodnoty se vrátí do aplikace Dynamo jejich přiřazením k proměnné OUT.

![](<../images/8-3/1/python node - the python node 02.jpg>)

Knihovna Autodesk.DesignScript.Geometry umožňuje použití tečkové notace podobné blokům kódů. Další informace o syntaxi aplikace Dynamo naleznete v části [7-2\_design-script-syntaxe.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "mention") a také v příručce [DesignScript Guide](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf). (Chcete-li stáhnout tento dokument PDF, klikněte pravým tlačítkem na odkaz a vyberte možnost Uložit odkaz jako.) Zadání typu geometrie, například „Point.“ zobrazí seznam metod vytváření a dotazování bodů.

![](<../images/8-3/1/python node - the python node 03.jpg>)

> Metody zahrnují konstruktory, například _ByCoordinates_, akce, například _Add_, a dotazy, například souřadnice _X_, _Y_ a _Z_.

## Cvičení: Vlastní uzel se skriptem jazyka Python k vytváření vzorů z modulu tělesa

### Část I: Nastavení skriptu jazyka Python

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

V tomto příkladu napíšeme skript jazyka Python, který vytvoří vzory z modulu tělesa a převede je na vlastní uzel. Nejprve vytvoříme náš modul tělesa pomocí uzlů aplikace Dynamo.

![](<../images/8-3/1/python node - exercise pt I-01.jpg>)

> 1. **Rectangle.ByWidthLength:** Vytvořte obdélník, který bude základem našeho tělesa.
> 2. **Surface.ByPatch:** Spojte obdélník se vstupem _closedCurve_ a vytvořte tak dolní povrch.

![](<../images/8-3/1/python node - exercise pt I-02.jpg>)

> 1. **Geometry.Translate:** Připojte obdélník k vstupu _geometrie_, aby se posunul nahoru pomocí bloku kódu k určení tloušťky základny našeho tělesa.
> 2. **Polygon.Points:** Pomocí dotazu na převedený obdélník lze extrahovat rohové body.
> 3. **Geometry.Translate:** Pomocí bloku kódu vytvořte seznam čtyř hodnot odpovídajících čtyřem bodům. Tím posunete jeden roh tělesa nahoru.
> 4. **Polygon.ByPoints:** Pomocí převedených bodů lze rekonstruovat horní polygon.
> 5. **Surface.ByPatch:** Připojením polygonu vytvořte horní povrch.

Nyní, když máme horní a dolní povrch, vytvoříme boky tělesa šablonováním mezi dvěma profily.

![](<../images/8-3/1/python node - exercise pt I-03.jpg>)

> 1. **List.Create:** Spojte dolní obdélník a horní polygon se vstupy indexu.
> 2. **Surface.ByLoft:** Šablonováním dvou profilů vytvořte strany tělesa.
> 3. **List.Create:** Připojte horní, boční a dolní povrchy ke vstupům indexu a vytvořte tak seznam povrchů.
> 4. **Solid.ByConnectedSurfaces:** Spojením povrchů vytvořte modul tělesa.

Nyní, když máme naše těleso, přetáhneme do pracovního prostoru uzel skriptu jazyka Python.

![](<../images/8-3/1/python node - exercise pt I-04.jpg>)

> 1. Chcete-li do uzlu přidat další vstupy, klikněte na ikonu + v uzlu. Vstupy jsou pojmenovány IN\[0], IN\[1] atd. Představují položky v seznamu.

Začneme definováním našich vstupů a výstupu. Dvojitým kliknutím na uzel otevřete editor jazyka Python. Při úpravách kódu v editoru postupujte podle kódu uvedeného níže.

![](<../images/8-3/1/python node - exercise pt I-05.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []

# Place your code below this line


# Assign your output to the OUT variable.
OUT = solids
```

Tento kód začne dávat větší smysl, jak budeme cvičením procházet. Dále musíme přemýšlet o tom, jaké informace jsou potřeba k vytvoření pole našeho modulu tělesa. Nejprve je nutné znát rozměry tělesa, abychom mohli určit vzdálenost posunutí. Kvůli chybě ohraničujícího kvádru bude nutné k jeho vytvoření použít geometrii křivky hrany.

![](../images/8-3/1/python07.png)

> Podívejte se na uzel Python v aplikaci Dynamo. Všimněte si, že používáme stejnou syntaxi, jakou vidíme v názvech uzlů v aplikaci Dynamo. Prohlédněte si níže uvedený kód s komentáři.

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

# Assign your output to the OUT variable.
OUT = solids
```

Protože budeme převádět i otáčet moduly těles, použijeme operaci Geometry.Transform. Při pohledu na uzel Geometry.Transform zjistíme, že k transformaci tělesa budeme potřebovat zdrojový souřadnicový systém a cílový souřadnicový systém. Zdroj je kontextový souřadnicový systém našeho tělesa, zatímco cíl bude odlišný souřadnicový systém pro každý modul v poli. To znamená, že bude nutné projít hodnoty X a Y a transformovat souřadnicový systém pokaždé jinak.

![](<../images/8-3/1/python node - exercise pt I-06.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

#Get the source coordinate system
fromCoord = solid.ContextCoordinateSystem

#Loop through x and y
for i in range(xCount):
    for j in range(yCount):
        #Rotate and translate the coordinate system
        toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin, Vector.ByCoordinates(0,0,1), (90*(i+j%seed)))
        vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
        toCoord = toCoord.Translate(vec)
        #Transform the solid from the source coord syste, to the target coord system and append to the list
        solids.append(solid.Transform(fromCoord,toCoord))

# Assign your output to the OUT variable.
OUT = solids
```

Klikněte na tlačítko Spustit a poté uložte kód. Připojte uzel jazyka Python k existujícímu skriptu níže uvedeným způsobem.

![](<../images/8-3/1/python node - exercise pt I-07.jpg>)

> 1. Připojte výstup z uzlu **Solid.ByConnectedSurfaces** jako první vstup uzlu jazyka Python a pomocí uzlu CodeBlock definujte ostatní vstupy.
> 2. Vytvořte uzel **Topology.Edge** a jako vstup použijte výstup z uzlu jazyka Python.
> 3. Nakonec vytvořte uzel **Edge.CurveGeometry** a jako vstup použijte výstup z uzlu Topology.Edge.

Zkuste změnit výchozí hodnotu a vytvořte jiné vzory. Můžete také změnit parametry samotného modulu tělesa a dosáhnout tak různých efektů.

![](../images/8-3/1/python10.png)

### Část II: Převod uzlu Python Script na vlastní uzel

Nyní, když jsme vytvořili užitečný skript jazyka Python, uložte jej jako uživatelský uzel. Vyberte uzel Python Script, klikněte pravým tlačítkem na pracovní prostor a vyberte možnost Vytvořit vlastní uzel.

![](<../images/8-3/1/python node - exercise pt II-01.jpg>)

Přiřaďte název, popis a kategorii.

![](<../images/8-3/1/python node - exercise pt II-02.jpg>)

Tím se otevře nový pracovní prostor, ve kterém se má upravit uživatelský uzel.

![](<../images/8-3/1/python node - exercise pt II-03.jpg>)

> 1. **Vstupy:** Změňte vstupní názvy tak, aby byly popisnější, a přidejte typy dat a výchozí hodnoty.
> 2. **Výstup:** Změňte název výstupu.

Uzel uložte jako soubor .dyf a měli byste vidět, že vlastní uzel odráží změny, které jsme právě provedli.

![](<../images/8-3/1/python node - exercise pt II-04.jpg>)
