# Python a Revit

### Python a Revit

Nyní, když jsme ukázali, jak používat skripty jazyka Python v aplikaci Dynamo, podívejme se na připojení knihoven aplikace Revit do prostředí skriptování. Pamatujte, importovali jsme standardní knihovny jazyka Python a hlavní uzly aplikace Dynamo pomocí prvních čtyř řádků v níže uvedeném bloku kódu. Chcete-li importovat uzly aplikace Revit, prvky aplikace Revit a správce dokumentů aplikace Revit, stačí přidat pouze několik dalších řádků:

```
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# Import RevitNodes
clr.AddReference("RevitNodes")
import Revit

# Import Revit elements
from Revit.Elements import *

# Import DocumentManager
clr.AddReference("RevitServices")
import RevitServices
from RevitServices.Persistence import DocumentManager

import System
```

To nám poskytuje přístup k rozhraní API aplikace Revit a nabízí vlastní skriptování pro libovolnou úlohu aplikace Revit. Díky kombinaci procesu vizuálního programování se skriptováním rozhraní API aplikace Revit se spolupráce a vývoj nástrojů výrazně zlepšují. Například správce BIM i konstruktér schémat mohou spolupracovat na stejném grafu. Při této spolupráci mohou zlepšit návrh a provedení modelu.

\![](<../../.gitbook/assets/python & revit - 01.jpg>)

### Rozhraní API pro konkrétní platformu

Plán projektu aplikace Dynamo spočívá v rozšíření rozsahu implementace platformy. Protože aplikace Dynamo přidává další programy do objektu docket, uživatelé budou mít přístup k programům API specifickým pro platformu z prostředí skriptování v jazyce Python. Zatímco v aplikaci Revit se jedná o případovou studii, můžeme předvídat další kapitoly v budoucnosti nabízející komplexní výukové programy zaměřené na skriptování v jiných platformách. Navíc je nyní k dispozici mnoho knihoven [IronPython](http://ironpython.net), které lze importovat do aplikace Dynamo.

Níže uvedené příklady ukazují způsoby implementace operací specifických pro aplikaci Revit z aplikace Dynamo pomocí jazyka Python. Podrobnější informace o vztahu jazyka Python s aplikacemi Dynamo a Revit naleznete na [stránce Wiki k aplikaci Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Python-0.6.3-to-0.7.x-Migration). Dalším užitečným zdrojem pro aplikace Python a Revit je projekt [prostředí Python Shell](https://github.com/architecture-building-systems/revitpythonshell) aplikace Revit.

## Cvičení 1

> Vytvořte nový projekt aplikace Revit.
>
> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/8-2/2/Revit-Doc.dyn" %}

V těchto cvičeních prozkoumáme základní skripty jazyka Python v rozhraní Dynamo pro aplikaci Revit. Toto cvičení se zaměří na práci se soubory a prvky aplikace Revit a také na komunikaci mezi aplikacemi Revit a Dynamo.

Jedná se o jednoduchou metodu získávání souborů _doc_, _uiapp_ a _app_ aplikace Revit připojených k vaší relaci Dynamo. Programátoři, kteří předtím pracovali v rozhraní API aplikace Revit, si mohou všimnout položek v seznamu sledovaných položek. Pokud se vám to nezdá povědomé, tak to nevadí. Použijeme další příklady ve cvičeních níže.

Zde je způsob importu služeb aplikace Revit a získání dat dokumentu v aplikaci Dynamo.

\![](<../images/8-3/2/python & revit - exercise 01 - 01.jpg>)

Podívejte se na uzel Python v aplikaci Dynamo. Kód můžete také najít níže:

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr

#Import DocumentManager
clr.AddReference("RevitServices")
import RevitServices
from RevitServices.Persistence import DocumentManager

#Place your code below this line
doc = DocumentManager.Instance.CurrentDBDocument
uiapp = DocumentManager.Instance.CurrentUIApplication
app = uiapp.Application

#Assign your output to the OUT variable
OUT = [doc,uiapp,app]
```

## Cvičení 2

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/8-2/2/Revit-ReferenceCurve.dyn" %}

V tomto cvičení vytvoříme pomocí uzlu Python aplikace Dynamo jednoduchou křivku modelu v aplikaci Revit.

Začněte vytvořením nové rodiny Koncepční objem v aplikaci Revit.

\![](<../images/8-3/2/python & revit - exercise 02 - 01.jpg>)

Otevřete _složku rodiny Koncepční objem_ a použijte soubor šablony _Metric Mass.rft_.

\![](<../images/8-3/2/python & revit - exercise 02 - 02.jpg>)

V aplikaci Revit pomocí klávesové zkratky **`un`** vyvolejte nastavení jednotek projektu a změňte jednotku délky na metry.

\![](<../images/8-3/2/python & revit - exercise 02 - 03.jpg>)

Spusťte aplikaci Dynamo a vytvořte sadu uzlů jako na obrázku níže. Nejprve vytvoříme dva referenční body v aplikaci Revit z uzlů aplikace Dynamo.

\![](<../images/8-3/2/python & revit - exercise 02 - 04.jpg>)

> 1. Vytvořte **blok kódu** a zadejte hodnotu `"0;"`.
> 2. Tuto hodnotu připojte ke vstupům X, Y a Z uzlu **ReferencePoint.ByCoordinates**.
> 3. Vytvořte tři posuvníky v rozsahu od -100 do 100 s velikostí kroku 1.
> 4. Připojte všechny posuvníky k uzlu **ReferencePoint.ByCoordinates**.
> 5. Přidejte do pracovního prostoru uzel **Python**, kliknutím na tlačítko „+“ v uzlu přidejte další vstup a připojte dva referenční body, jeden ke každému vstupu. Otevřete uzel **Python**.

Podívejte se na uzel Python v aplikaci Dynamo. Celý kód najdete níže.

\![](<../images/8-3/2/python & revit - exercise 02 - 05.jpg>)

> 1. **System.Array**: Aplikace Revit vyžaduje jako vstup **systémové pole** (místo seznamu jazyka Python). Jedná se pouze o jeden další řádek kódu, ale zohlednění typů argumentů usnadní programování v aplikaci Revit v jazyce Python.

```
import sys
import clr

# Import RevitNodes
clr.AddReference("RevitNodes")
import Revit
#Import Revit elements
from Revit.Elements import *
import System

#define inputs
startRefPt = IN[0]
endRefPt = IN[1]

#define system array to match with required inputs
refPtArray = System.Array[ReferencePoint]([startRefPt, endRefPt])

#create curve by reference points in Revit
OUT = CurveByPoints.ByReferencePoints(refPtArray)
```

V aplikaci Dynamo jsme pomocí jazyka Python vytvořili dva referenční body a čáru, která je spojuje. V dalším cvičení zkusíme něco složitějšího.

\![](<../images/8-3/2/python & revit - exercise 02 - 06.jpg>)

## Cvičení 3

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/8-2/2/Revit-StructuralFraming.zip" %}

Toto cvičení vysvětluje témata připojení dat a geometrie z aplikace Revit do aplikace Dynamo a zpět. Začneme otevřením souboru Revit-StructuralFraming.rvt. Po otevření spusťte aplikaci Dynamo a otevřete soubor Revit-StructuralFraming.dyn.

\![](<../../.gitbook/assets/python & revit - exercise 03 - 01.jpg>)

Tento soubor aplikace Revit je jeden z nejzákladnějších. Dvě referenční křivky: jedna nakreslená na Podlaží 1 a druhá na Podlaží 2. Chceme tyto křivky dostat do aplikace Dynamo a zachovat živé propojení.

V tomto souboru máme sadu uzlů zapojených do pěti vstupů uzlu Python.

\![](<../images/8-3/2/python & revit - exercise 03 - 02.jpg>)

> 1. Uzly **Select Model Element**: Klikněte na tlačítko Vybrat pro každý prvek a vyberte odpovídající křivku v aplikaci Revit.
> 2. Uzel **Code Block**: Pomocí syntaxe `0..1..#x;`_,_ připojte posuvník celého čísla v rozsahu od 0 do 20 ke vstupu _x_. Označuje počet nosníků, které se mají kreslit mezi dvěma křivkami.
> 3. Uzel **Structural Framing Types**: Zde vybereme v rozevírací nabídce výchozí nosník W12x26.
> 4. Uzel **Levels**: Vyberte možnost Podlaží 1.

Tento kód v jazyce Python je trochu hustší, ale komentáře v kódu popisují, co se v procesu děje.

\![](<../images/8-3/2/python & revit - exercise 03 - 03.jpg>)

```
import clr
#import Dynamo Geometry
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *
# Import RevitNodes
clr.AddReference("RevitNodes")
import Revit
# Import Revit elements
from Revit.Elements import *
import System

#Query Revit elements and convert them to Dynamo Curves
crvA=IN[0].Curves[0]
crvB=IN[1].Curves[0]

#Define input Parameters
framingType=IN[3]
designLevel=IN[4]

#Define "out" as a list
OUT=[]

for val in IN[2]:
	#Define Dynamo Points on each curve
	ptA=Curve.PointAtParameter(crvA,val)
	ptB=Curve.PointAtParameter(crvB,val)
	#Create Dynamo line
	beamCrv=Line.ByStartPointEndPoint(ptA,ptB)
	#create Revit Element from Dynamo Curves
	beam = StructuralFraming.BeamByCurve(beamCrv,designLevel,framingType)
	#convert Revit Element into list of Dynamo Surfaces
	OUT.append(beam.Faces)
```

V aplikaci Revit je k dispozici pole nosníků, které pokrývají dvě křivky jako konstrukční prvky. Poznámka: Toto není realistický příklad... Konstrukční prvky se používají jako příklad pro nativní instance aplikace Revit vytvořené z aplikace Dynamo.

Výsledky jsou v aplikaci Dynamo zobrazeny také. Nosníky v uzlu **Watch3D** odkazují na geometrii dotazovanou z prvků aplikace Revit.

\![](<../images/8-3/2/python & revit - exercise 03 - 05.jpg>)

Všimněte si, že máme nepřetržitý proces převodu dat z prostředí aplikace Revit do prostředí aplikace Dynamo. Toto je souhrn průběhu procesu:

1. Vybrat prvek aplikace Revit
2. Převést prvek aplikace Revit na křivku aplikace Dynamo
3. Rozdělit křivku aplikace Dynamo na řadu bodů aplikace Dynamo
4. Vytvoření čar aplikace Dynamo pomocí bodů aplikace Dynamo mezi dvěma křivkami
5. Vytvořit nosníky aplikace Revit pomocí odkazů na čáry aplikace Dynamo
6. Vytvořit výstup povrchů aplikace Dynamo pomocí dotazů na geometrii nosníků aplikace Revit

Může to znít složitě, ale díky skriptu je to stejně jednoduché jako úprava křivky v aplikaci Revit a opětovné spuštění řešiče (i když je možné, že při tom budete muset odstranit předchozí nosníky). _Důvodem je skutečnost, že nosníky umísťujeme do jazyka Python, a tím porušujeme asociaci uzlů OOTB._

Pomocí aktualizace referenčních křivek v aplikaci Revit získáte nové pole nosníků.

\![](<../images/8-3/2/python & revit - ex 03 - 06.gif>)
