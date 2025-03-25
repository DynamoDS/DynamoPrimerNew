# Python und Civil 3D

Dynamo ist als [visuelles Programmierwerkzeug](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) zwar sehr leistungsstark, es ist jedoch auch möglich, nicht nur Blöcke und Drähte zu verwenden, sondern Code in Textform zu schreiben. Es gibt zwei Möglichkeiten, dies zu tun:

1. Schreiben von **DesignScript** mithilfe eines Codeblocks
2. Schreiben von **Python** mithilfe eines Python-Blocks

In diesem Abschnitt wird beschrieben, wie Sie Python in der Civil 3D-Umgebung einsetzen können, um die Vorteile der .NET-APIs von AutoCAD und Civil 3D zu nutzen.

{% hint style="info" %}
 Weitere allgemeine Informationen zur Verwendung von Python in Dynamo finden Sie im Abschnitt [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") . 
{% endhint %} 

## API-Dokumentation

AutoCAD und Civil 3D verfügen über mehrere APIs, mit denen Entwickler wie Sie das Kernprodukt um benutzerdefinierte Funktionen erweitern können. Im Kontext von Dynamo sind die **verwalteten .NET-APIs** relevant. Die folgenden Links sind wichtig, um die Struktur der APIs und ihre Funktionsweise zu verstehen.

[AutoCAD-.NET-API - Entwicklerhandbuch](https://help.autodesk.com/view/OARX/2024/DEU/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[AutoCAD-.NET-API - Referenzhandbuch](https://help.autodesk.com/view/OARX/2024/DEU/?guid=OARX-ManagedRefGuide-What_s_New)

[Civil 3D-.NET-API - Entwicklerhandbuch](https://help.autodesk.com/view/CIV3D/2024/DEU/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Civil 3D-.NET-API - Referenzhandbuch](https://help.autodesk.com/view/CIV3D/2024/DEU/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %}
 Wenn Sie diesen Abschnitt durcharbeiten, werden Ihnen möglicherweise einige Konzepte begegnen, mit denen Sie nicht vertraut sind, z. B. Datenbanken, Transaktionen, Methoden, Eigenschaften usw. Viele dieser Konzepte sind für die Arbeit mit .NET-APIs wichtig und nicht spezifisch für Dynamo oder Python. Da diese Konzepte in diesem Abschnitt der Einführung nicht behandelt werden, empfehlen wir für weitere Informationen die oben genannten Links. 
{% endhint %} 

## Code-Vorlage

Wenn Sie einen neuen Python-Block zum ersten Mal bearbeiten, wird er automatisch mit Vorlagencode gefüllt, um Ihnen den Einstieg zu erleichtern. Hier sehen Sie eine Aufschlüsselung der Vorlage mit Erläuterungen zu den einzelnen Blöcken.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Die vorgabemäßige Python-Vorlage in Civil 3D</p></figcaption></figure>

> 1. Importiert die Module `sys` und `clr`, die beide für die korrekte Funktionsweise des Python-Interpreters erforderlich sind. Insbesondere ermöglicht das `clr`-Modul, dass .NET-Namensbereiche im Wesentlichen wie Python-Pakete behandelt werden.
> 2. Lädt die Standardbaugruppen (d. h. DLLs) für die Arbeit mit den verwalteten .NET-APIs für AutoCAD und Civil 3D.
> 3. Fügt Verweise zu den standardmäßigen AutoCAD- und Civil 3D-Namensbereichen hinzu. Diese entsprechen den Anweisungen `using` oder `Imports` in C# bzw. VB.NET.
> 4. Auf die Eingabeanschlüsse des Blocks kann über eine vordefinierte Liste mit dem Namen `IN` zugegriffen werden. Sie können auf die Daten in einem bestimmten Anschluss über die Indexnummer zugreifen, z. B. `dataInFirstPort = IN[0]`.
> 5. Ruft das aktive Dokument und den aktiven Editor ab.
> 6. Sperrt das Dokument und initiiert eine Datenbanktransaktion.
> 7. Hier sollten Sie den Großteil der Skriptlogik platzieren.
> 8. Heben Sie die Auskommentierung dieser Zeile auf, um die Transaktion nach der Hauptarbeit zu bestätigen.
> 9. Wenn Sie Daten aus dem Block ausgeben möchten, weisen Sie diese der Variablen `OUT` am Ende des Skripts zu.

{% hint style="info" %}
 **Sie möchten die Software anpassen?**\
 Sie können die vorgabemäßige Python-Vorlage ändern, indem Sie die Datei `PythonTemplate.py` in `C:\ProgramData\Autodesk\C3D <Version>\Dynamo` bearbeiten. 
{% endhint %} 

## Beispiel

Im Folgenden werden anhand eines Beispiels einige der grundlegenden Konzepte zum Schreiben von Python-Skripten in Dynamo for Civil 3D demonstriert.

### Ziel

> :dart: Ruft die Umgrenzungsgeometrie aller Einzugsgebiete in einer Zeichnung ab.

### Datensatz

Hier sehen Sie Beispieldateien, die für diese Übung als Referenz dienen können.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Lösungsübersicht

Hier sehen Sie einen Überblick über die Logik in diesem Diagramm.

> 1. Dokumentation zur Civil 3D-API ansehen
> 2. Alle Einzugsgebiete im Dokument nach Layernamen auswählen
> 3. Dynamo-Objekte "abwickeln", um auf die internen Civil 3D-API-Elemente zuzugreifen
> 4. Dynamo-Punkte aus AutoCAD-Punkten erstellen
> 5. PolyCurves aus den Punkten erstellen

Los gehts!

### Überprüfen der API-Dokumentation

Bevor wir beginnen, unser Diagramm zu erstellen und Code zu schreiben, sollten wir uns die Dokumentation zur Civil 3D-API ansehen und einen Eindruck davon gewinnen, was die API uns zur Verfügung stellt. In diesem Fall gibt es eine [Eigenschaft in der Einzugsgebietklasse](https://help.autodesk.com/view/CIV3D/2024/DEU/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c), die die Umgrenzungspunkte des Einzugsgebiets zurückgibt. Beachten Sie, dass diese Eigenschaft ein `Point3dCollection`-Objekt zurückgibt. Dynamo kann diese Objektart nicht verarbeiten. Mit anderen Worten: Wir können keine PolyCurve aus einer `Point3dCollection` erstellen, daher müssen wir irgendwann alles in Dynamo-Punkte umwandeln. Doch mehr dazu später.

### Abrufen aller Einzugsgebiete

Jetzt können wir mit der Erstellung der Diagrammlogik beginnen. Als Erstes müssen Sie eine Liste aller Einzugsgebiete im Dokument abrufen. Hierfür sind Blöcke verfügbar, sodass Sie dies nicht in das Python-Skript aufnehmen müssen. Die Verwendung von Blöcken bietet eine bessere Sichtbarkeit für andere Benutzer, die das Diagramm möglicherweise lesen (im Gegensatz zu in einem Python-Skript "verstecktem" Code). Außerdem konzentriert sich das Python-Skript auf eine Sache: die Rückgabe der Umgrenzungspunkte der Einzugsgebiete.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Abrufen aller Einzugsgebiete im Dokument nach Layer</p></figcaption></figure>

Beachten Sie, dass die Ausgabe des Blocks **All Objects on Layer** eine Liste von CivilObjects ist. Dies liegt daran, dass Dynamo for Civil 3D derzeit keine Blöcke für die Arbeit mit Einzugsgebieten hat. Aus diesem Grund müssen wir über Python auf die API zugreifen.

### Abwickeln von Objekten

Bevor wir fortfahren, müssen wir noch kurz auf ein wichtiges Konzept eingehen. Im Abschnitt [node-library.md](../node-library.md "mention") wurde erläutert, wie Objekte und CivilObjects miteinander verbunden sind. Eine weitere relevante Information ist, dass ein **Dynamo-Objekt** als Wrapper um ein **AutoCAD-Objekt** herum fungiert. In ähnlicher Weise ist ein **Dynamo-CivilObject** ein Wrapper um ein **Civil 3D-Objekt** herum. Sie können ein Objekt abwickeln, indem Sie auf seine `InternalDBObject`- oder `InternalObjectId`-Eigenschaften zugreifen.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Dynamo-Typ</th><th width="373">Enthält im Wrapper</th></tr></thead><tbody><tr><td><strong>Objekt</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Objekt</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Objekt</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} Als Faustregel gilt: Es ist in der Regel sicherer, die Objekt-ID über die Eigenschaft `InternalObjectId` abzurufen und dann in einer Transaktion auf das im Wrapper enthaltene Objekt zuzugreifen. Dies liegt daran, dass die Eigenschaft `InternalDBObject` ein AutoCAD-DBObject zurückgibt, das sich nicht in einem schreibbaren Status befindet. 
{% endhint %} 

### Python-Skript

Hier sehen Sie das vollständige Python-Skript, das für den Zugriff auf die internen Einzugsgebietobjekte deren Umgrenzungspunkte abruft. Die hervorgehobenen Zeilen stehen für die Zeilen, die vom Vorgabevorlagencode geändert/hinzugefügt wurden.

{% hint style="info" %}
 Klicken Sie auf den unterstrichenen Text im Skript, um eine Erläuterung für jede Zeile anzuzeigen. 
{% endhint %} 

<pre class="language-python" data-line-numbers><code class="lang-python"># Python-Standard- und DesignScript-Bibliotheken laden
import sys
import clr

# Baugruppen für AutoCAD und Civil3D hinzufügen
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Referenzen aus AutoCAD importieren
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Referenzen aus Civil3D importieren
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# Die Eingaben für diesen Block werden als Liste in den IN-Variablen gespeichert.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("Die Eingabe ist null oder leer.")</a>
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
            # Commit vor Beenden der Transaktion
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Weisen Sie Ihre Ausgabe der OUT-Variable zu.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} Als Faustregel sollten Sie den Großteil Ihrer Skriptlogik in eine Transaktion einschließen. Dadurch wird der sichere Zugriff auf die Objekte sichergestellt, die das Skript lesen und schreiben kann. In vielen Fällen kann das Auslassen einer Transaktion einen schwerwiegenden Fehler verursachen. 
{% endhint %} 

### Erstellen von PolyCurves

An dieser Stelle sollte das Python-Skript eine Liste von Dynamo-Punkten ausgeben, die Sie in der Hintergrundvorschau sehen können. Der letzte Schritt besteht darin, einfach PolyCurves aus den Punkten zu erstellen. Beachten Sie, dass dies auch direkt im Python-Skript erfolgen kann. Wir haben dies jedoch absichtlich außerhalb des Skripts in einem Block platziert, damit es besser sichtbar ist. Hier sehen Sie, wie das fertige Diagramm aussieht.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Das endgültige Diagramm</p></figcaption></figure>

### Ergebnis

Hier sehen Sie die endgültige Dynamo-Geometrie.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Die resultierenden Dynamo-PolyCurves für die Einzugsgebietsgrenzen</p></figcaption></figure>

> :tada: Mission erfüllt!

## IronPython vs. CPython

Hier noch eine kurze Anmerkung. Je nachdem, welche Version von Civil 3D Sie verwenden, ist der Python-Block möglicherweise anders konfiguriert. In **Civil 3D 2020 und 2021** hat Dynamo das Werkzeug **IronPython** verwendet, um Daten zwischen .NET-Objekten und Python-Skripten zu verschieben. In **Civil 3D 2022** wurde in Dynamo jedoch der standardmäßige native Python-Interpreter (auch **CPython** genannt) anstelle von Python 3 verwendet. Zu den Vorteilen dieser Umstellung zählen der Zugriff auf beliebte moderne Bibliotheken und neue Plattformfunktionen, grundlegende Wartung und Sicherheits-Patches.

{% hint style="info" %}
 Weitere Informationen zu dieser Umstellung und zum Aktualisieren älterer Skripte finden Sie im [Dynamo-Blog](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Wenn Sie IronPython weiterhin verwenden möchten, müssen Sie lediglich das **DynamoIronPython2.7** \- Paket mithilfe des Dynamo Package Manager installieren. 
{% endhint %} 

[^1]: Vorgabemäßig wird die Dynamo-Geometriebibliothek nicht zur Python-Umgebung hinzugefügt. Unser Ziel mit diesem Skript ist die Ausgabe einer Liste von Dynamo-Punkten für die Einzugsgebietsgrenzen. Daher müssen wir diese Zeile hinzufügen, um die Punkte später erstellen zu können.

[^2]: Diese Zeile ruft die spezifische Klasse ab, die wir aus der Dynamo-Geometriebibliothek benötigen. Beachten Sie, dass wir hier `import Point as DynPoint` anstelle von `import *` angeben, da Letzteres zu Namenskonflikten führen würde.

[^3]: Hier benennen wir die Vorgabevariable `dataEnteringNode` in `objs` um, damit sie aussagekräftiger ist.

[^4]: Hier geben wir an, welcher Eingabeanschluss die gewünschten Daten enthält, und nicht die Vorgabe `IN`, die sich auf die gesamte Liste aller Eingaben bezieht.

[^5]: Dadurch wird eine leere Liste erstellt, zu der wir unsere Ausgabedaten später hinzufügen werden.

[^6]: Dies schützt vor einer möglichen leeren oder Null-Eingabe in das Skript.

[^7]: Anstatt das Skript zu unterbrechen, geben wir eine aussagekräftige Meldung aus, in der erläutert wird, warum das Skript nicht fortgesetzt werden kann.

[^8]: Im restlichen Skript wird davon ausgegangen, dass wir mit einer Liste von Objekten arbeiten können (im Gegensatz zu einem einzelnen Objekt). Aber für den Fall, dass es nur ein Objekt gibt (d. h. eine Zeichnung mit einem Einzugsgebiet), soll das Skript trotzdem ausgeführt werden können.

[^9]: Wenn die Eingabe keine Liste ist (d. h. ein einzelnes Objekt), wird einfach eine neue Liste erstellt, in der das Objekt das einzige Element ist.

[^10]: Initiiert eine Schleife für jedes Objekt in der Liste.

[^11]: "Abwickeln" des Dynamo-Objekts durch Abrufen seiner Objekt-ID.

[^12]: Abrufen des im Wrapper enthaltenen Objekts aus der AutoCAD-Datenbank. Beachten Sie, dass der OpenMode hier auf `ForRead` eingestellt ist, da wir nicht vorhaben, Objekte zu bearbeiten. Wir rufen einfach nur Daten ab.

[^13]: Es ist möglich, dass die eingegebene Objektliste eine Kombination aus Einzugsgebieten und anderen Elementen enthält, die keine Einzugsgebiete sind. Wir müssen prüfen, ob diese Situation vorliegt, und entsprechend damit umgehen (d. h. nur diese Schleifeniteration fortsetzen, wenn das Element tatsächlich ein Einzugsgebiet ist).

[^14]: Wenn das Skript bis hierhin gelaufen ist, wissen wir, dass das Objekt tatsächlich ein Einzugsgebiet ist. Wir fügen hier eine neue Variable hinzu, um die Benennung ordentlich und verständlich zu halten.

[^15]: Hier rufen wir die Einzugsgebiet-Grenzpunkte mit der entsprechenden Eigenschaft ab, die wir zuvor in den API-Dokumenten gesucht haben. Wie bereits erwähnt, erhalten wir dadurch ein `Point3dCollection`-Objekt, das im Wesentlichen eine Liste von AutoCAD-Punkten ist. Diese müssen in Dynamo-Punkte "konvertiert" werden, damit sie verwendet werden können.

[^16]: Erstellen Sie eine leere Liste, um die Grenzpunkte für dieses Einzugsgebiet zu speichern.

[^17]: Initiieren Sie eine Schleife für jedes `Point3d`-Objekt in der `Point3dCollection`.

[^18]: Erstellen Sie einen Dynamo-Punkt anhand der Koordinaten des AutoCAD-Punkts.

[^19]: Fügen Sie den Punkt zur Liste hinzu.

[^20]: Nachdem wir alle AutoCAD-Punkte in Dynamo-Punkte "konvertiert" haben, fügen wir die resultierende Liste zur Ausgabe hinzu. Danach macht die äußere Schleife mit dem nächsten Objekt in der Eingabeliste weiter.

[^21]: Heben Sie die Auskommentierung dieser Zeile auf, um die Transaktion zu übernehmen.

[^22]: Zu guter Letzt geben wir die Liste der Dynamo-Punkte aus.
