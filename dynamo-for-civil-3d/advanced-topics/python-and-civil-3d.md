# Python a Civil 3D

Aplikace Dynamo je sice jako nástroj pro [vizuální programování](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) extrémně výkonná, ale můžete jít také nad rámec uzlů a drátů a psát kód v textové podobě. To lze provést dvěma způsoby:

1. Psát v jazyce **DesignScript** pomocí bloku kódu.
2. Psát v jazyce **Python** pomocí uzlu jazyka Python.

V této části se zaměříme na to, jak lze pomocí jazyka Python v prostředí aplikace Civil 3D efektivně využívat rozhraní .NET API aplikací AutoCAD a Civil 3D.

{% hint style="info" %} Další obecné informace o používání jazyka Python v aplikaci Dynamo naleznete v části [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") . {% endhint %}

## Dokumentace k rozhraní API

Aplikace AutoCAD i Civil 3D mají k dispozici několik rozhraní API, která umožňují vývojářům, jako jste vy, rozšířit základní produkt o vlastní funkce. V kontextu aplikace Dynamo jsou relevantní **spravovaná rozhraní .NET API**. Následující odkazy jsou důležité pro pochopení struktury rozhraní API a způsobu jejich fungování.

[Příručka pro vývojáře rozhraní .NET API pro aplikaci AutoCAD](https://help.autodesk.com/view/OARX/2024/CSY/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[Referenční příručka rozhraní .NET API pro aplikaci AutoCAD](https://help.autodesk.com/view/OARX/2024/CSY/?guid=OARX-ManagedRefGuide-What_s_New)

[Příručka pro vývojáře rozhraní .NET API pro aplikaci Civil 3D](https://help.autodesk.com/view/CIV3D/2024/CSY/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Referenční příručka rozhraní .NET API pro aplikaci Civil 3D](https://help.autodesk.com/view/CIV3D/2024/CSY/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %} Při procházení této části se můžete setkat s pojmy, které neznáte, jako jsou databáze, transakce, metody, vlastnosti atd. Mnoho z těchto pojmů tvoří základ pro práci s rozhraním .NET API a nejsou specifické pro aplikaci Dynamo nebo jazyk Python. Podrobné rozebírání těchto pojmů přesahuje rámec této části příručky Primer, proto doporučujeme často vyhledávat další informace na výše uvedených odkazech. {% endhint %}

## Šablona kódu

Při první úpravě nového uzlu jazyka Python bude tento uzel předvyplněn kódem šablony, abyste mohli začít. Zde je rozpis šablony s vysvětlením jednotlivých bloků.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Výchozí šablona jazyka Python v aplikaci Civil 3D</p></figcaption></figure>

> 1. Importuje moduly `sys` a `clr`, které jsou nezbytné pro správnou funkci interpretu jazyka Python. Modul `clr` zejména umožňuje, aby se se jmennými prostory .NET zacházelo v podstatě jako s balíčky jazyka Python.
> 2. Načte standardní sestavy (tj. knihovny DLL) pro práci se spravovanými rozhraními .NET API pro aplikace AutoCAD a Civil 3D.
> 3. Přidá odkazy na standardní jmenné prostory aplikací AutoCAD a Civil 3D. Jsou ekvivalentní direktivám `using` nebo `Imports` v jazyce C# nebo VB.NET (v uvedeném pořadí).
> 4. Vstupní porty uzlu jsou přístupné pomocí předdefinovaného seznamu s názvem `IN`. K datům v určitém portu můžete přistupovat pomocí jeho čísla indexu, například `dataInFirstPort = IN[0]`.
> 5. Vrátí aktivní dokument a editor.
> 6. Uzamkne dokument a zahájí transakci databáze.
> 7. Zde byste měli umístit většinu logiky skriptu.
> 8. Zrušte komentář tohoto řádku, aby se po dokončení hlavní práce provedla transakce.
> 9. Pokud chcete z uzlu získat výstup libovolných dat, přiřaďte je na konci skriptu proměnné `OUT`.

{% hint style="info" %} **Chcete si šablonu přizpůsobit?**\
 Výchozí šablonu jazyka Python si můžete přizpůsobit úpravou souboru `PythonTemplate.py`, který se nachází v umístění `C:\ProgramData\Autodesk\C3D <version>\Dynamo`. {% endhint %}

## Příklad

Pojďme si na příkladu ukázat některé základní koncepty psaní skriptů jazyka Python v aplikaci Dynamo for Civil 3D.

### Cíl

> :dart: Získejte geometrii hranic všech povodí ve výkresu.

### Datová sada

Zde jsou příklady souborů, na které se můžete odkazovat v tomto cvičení.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Přehled řešení

Zde je uveden přehled logiky tohoto grafu.

> 1. Zkontrolujte dokumentaci rozhraní API aplikace Civil 3D.
> 2. Vyberte všechna povodí v dokumentu podle názvu hladiny.
> 3. „Rozbalte“ objekty aplikace Dynamo a získejte přístup k interním prvkům rozhraní API aplikace Civil 3D.
> 4. Vytvořte body aplikace Dynamo z bodů aplikace AutoCAD.
> 5. Vytvořte pomocí bodů objekty PolyCurve.

Pojďme na to!

### Kontrola dokumentace rozhraní API

Než začneme vytvářet graf a psát kód, je vhodné se podívat do dokumentace rozhraní API aplikace Civil 3D a získat představu o tom, co nám rozhraní API zpřístupňuje. V tomto případě existuje [vlastnost ve třídě Catchment](https://help.autodesk.com/view/CIV3D/2024/CSY/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c), která vrátí hraniční body povodí. Všimněte si, že tato vlastnost vrací objekt `Point3dCollection`, který aplikace Dynamo nezná. Jinými slovy, z objektu `Point3dCollection` nebude možné vytvořit objekt PolyCurve, takže nakonec bude nutné vše převést na body aplikace Dynamo. K tomu se vrátíme později.

### Načtení všech povodí

Nyní můžeme začít vytvářet logiku grafu. Nejprve je nutné získat seznam všech povodí v dokumentu. Pro tuto operaci jsou k dispozici uzly, takže ji nemusíme zahrnovat do skriptu jazyka Python. Použití uzlů nabízí lepší přehlednost pro někoho, kdo by mohl graf číst (místo procházení velkého množství kódu ve skriptu jazyka Python), a také udržuje skript jazyka Python zaměřený na jednu věc: vrácení hraničních bodů povodí.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Získání všech povodí v dokumentu podle hladiny</p></figcaption></figure>

Všimněte si, že výstup z uzlu **All Objects on Layer** je seznam objektů CivilObject. Je tomu tak proto, že aplikace Dynamo for Civil 3D aktuálně nemá žádné uzly pro práci s povodími, což je celý důvod, proč přistupovat k rozhraní API prostřednictvím jazyka Python.

### Rozbalení objektů

Než budeme pokračovat, musíme se krátce zastavit u důležitého pojmu. V části [node-library.md](../node-library.md "mention") jsme se zmínili, jak spolu souvisí objekty (Object) a objekty aplikace Civil 3D (CivilObject). Abychom to dále upřesnili, **objekt aplikace Dynamo** je obálka kolem **entity aplikace AutoCAD**. Podobně platí, že objekt **Dynamo CivilObject** je obálka kolem **entity aplikace Civil 3D**. Objekt můžete „rozbalit“ přístupem k jeho vlastnostem `InternalDBObject` nebo `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Typ aplikace Dynamo</th><th width="373">Zabalení</th></tr></thead><tbody><tr><td><strong>Objekt</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entita</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entita</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} Obecně je bezpečnější získat ID objektu pomocí vlastnosti `InternalObjectId` a poté získat přístup k zabalenému objektu v transakci. Důvodem je, že vlastnost `InternalDBObject` vrátí objekt DBObject aplikace AutoCAD, který není v zapisovatelném stavu. {% endhint %}

### Skript jazyka Python

Zde je úplný skript jazyka Python, který provádí přístup k vnitřním objektům povodí a získává jejich hraniční body. Zvýrazněné řádky jsou upraveny/přidány oproti výchozímu kódu šablony.

{% hint style="info" %} Kliknutím na podtržený text ve skriptu zobrazíte popis k jednotlivým řádkům. {% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Načtení standardní knihovny jazyka Python a knihovny jazyka DesignScript
import sys
import clr

# Přidání sestav pro aplikace AutoCAD a Civil3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Import referencí z aplikace AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Import referencí z aplikace Civil 3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# Vstupy do tohoto uzlu budou uloženy jako seznam v proměnných IN.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit(„Vstup je nulový nebo prázdný.“)</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Provést před ukončením transakce
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Přiřazení výstupu k proměnné OUT.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} Pravidlem je, že většinu logiky skriptu je vhodné zahrnout do transakce. Tím zajistíte bezpečný přístup k objektům, které váš skript čte/zapisuje. V mnoha případech může vynechání transakce způsobit závažnou chybu. {% endhint %}

### Vytvoření objektů PolyCurves

V této fázi by měl skript jazyka Python vygenerovat seznam bodů aplikace Dynamo, který si můžete prohlédnout v náhledu na pozadí. Posledním krokem je jednoduché vytvoření objektů PolyCurve z těchto bodů. To lze provést také přímo ve skriptu jazyka Python, ale záměrně jsme tento krok umístili mimo skript do uzlu kvůli lepší přehlednosti. Zde je výsledný graf.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Výsledný graf</p></figcaption></figure>

### Výsledek

A zde je výsledná geometrie aplikace Dynamo.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Výsledné objekty PolyCurve aplikace Dynamo pro hranice povodí</p></figcaption></figure>

> :tada: Úkol splněn!

## IronPython vs. CPython

Jen stručná poznámka, než skončíme toto téma. V závislosti na používané verzi aplikace Civil 3D může být uzel jazyka Python konfigurován odlišně. V **aplikacích Civil 3D 2020 a 2021** používala aplikace Dynamo k přesunu dat mezi objekty .NET a skripty jazyka Python nástroj **IronPython**. V aplikaci **Civil 3D 2022** však aplikace Dynamo používá standardní nativní interpret jazyka Python (neboli **CPython**), který používá jazyk Python 3. Mezi výhody přechodu na tento interpret patří přístup k oblíbeným moderním knihovnám a novým funkcím platformy, nezbytná údržba a bezpečnostní opravy.

{% hint style="info" %} Další informace o tomto přechodu a o upgradu starších skriptů naleznete na [blogu aplikace Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Pokud chcete i nadále používat nástroj IronPython, stačí nainstalovat balíček **DynamoIronPython2.7** pomocí nástroje Dynamo Package Manager. {% endhint %}

[^1]: Ve výchozím nastavení není knihovna geometrie aplikace Dynamo přidána do prostředí jazyka Python. Cílem tohoto skriptu je vytvořit seznam bodů aplikace Dynamo pro hranice povodí, takže je nutné přidat tento řádek, abychom mohli později vytvořit body.

[^2]: Tento řádek získá konkrétní třídu, kterou potřebujeme, z knihovny geometrie aplikace Dynamo. Všimněte si, že zde uvádíme `import Point as DynPoint` místo `import *`, protože druhá možnost by mohla způsobit kolizi názvů.

[^3]: Zde přejmenujeme výchozí proměnnou `dataEnteringNode` na popisnější `objs`.

[^4]: Zde přesně určíme, který vstupní port obsahuje požadovaná data, namísto výchozího zadání `IN`, které odkazuje na celý seznam všech vstupů.

[^5]: Tímto se vytvoří prázdný seznam, do kterého později přidáme výstupní data.

[^6]: Ochrana proti případnému prázdnému nebo nulovému vstupu do skriptu.

[^7]: Místo přerušení skriptu se zobrazí zpráva, která vysvětlí, proč skript nemůže pokračovat.

[^8]: Zbytek skriptu předpokládá, že máme k dispozici seznam objektů, se kterými můžeme pracovat (na rozdíl od jednoho objektu). Tento řádek je zde právě pro případ, že existuje pouze jeden objekt (tj. výkres s jedním povodím), a my chceme, aby skript mohl být i tak spuštěn.

[^9]: Pokud vstupem není seznam (tj. vstupem je jediný objekt), pak jednoduše vytvoříme nový seznam s objektem jako jedinou položkou.

[^10]: Spustí smyčku pro každý objekt v seznamu.

[^11]: „Rozbalí“ objekt aplikace Dynamo získáním jeho ID objektu.

[^12]: Načte „zabalený“ objekt z databáze aplikace AutoCAD. Všimněte si, že režim OpenMode je zde nastaven na hodnotu `ForRead`, protože neplánujeme provádět žádné úpravy objektů. Pouze se „dotazujeme“ na data.

[^13]: Je možné, že vstupní seznam objektů obsahuje kombinaci položek povodí a jiných položek, které nejsou položkami povodí. Tuto situaci musíme zkontrolovat a vhodně ji ošetřit (tj. pokračovat v této iteraci smyčky pouze v případě, že položka je skutečně povodí).

[^14]: Pokud se skript dostane až sem, pak víme, že objekt je skutečně povodí. Zde přidáme novou proměnnou, abychom zachovali přehlednost a srozumitelnost pojmenování.

[^15]: Zde načteme hraniční body povodí pomocí příslušné vlastnosti, kterou jsme dříve vyhledali v dokumentaci k rozhraní API. Jak bylo zmíněno dříve, získáte tak objekt `Point3dCollection`, který je v podstatě seznamem bodů aplikace AutoCAD. Aby byly tyto body užitečné, je nutné je „převést“ na body aplikace Dynamo.

[^16]: Vytvoří prázdný seznam k uložení hraničních bodů tohoto povodí.

[^17]: Spustí smyčku pro každý objekt `Point3d` v `Point3dCollection`.

[^18]: Vytvoří bod aplikace Dynamo pomocí souřadnic bodu aplikace AutoCAD.

[^19]: Přidá bod do seznamu.

[^20]: Po dokončení převodu všech bodů aplikace AutoCAD na body aplikace Dynamo přidáme výsledný seznam do výstupu. Poté se vnější smyčka přesune k dalšímu objektu v seznamu vstupů.

[^21]: Zrušením komentáře tohoto řádku potvrďte transakci.

[^22]: A na závěr vytvoříme výstup seznamu bodů aplikace Dynamo.
