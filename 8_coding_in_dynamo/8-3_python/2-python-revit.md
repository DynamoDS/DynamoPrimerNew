# Python und Revit

### Python und Revit

Nachdem im vorigen Abschnitt die Verwendung von Python-Skripts in Dynamo gezeigt wurde, erhalten Sie hier eine Einführung zur Einbindung von Revit-Bibliotheken in die Skriptumgebung. Rufen Sie sich kurz ins Gedächtnis zurück, dass wir Python Standard und unsere Dynamo-Core-Blöcke mithilfe der ersten vier Zeilen im folgenden Codeabschnitt importiert haben. Um die Blöcke, Elemente und die Dokumentenverwaltung von Revit zu importieren, sind lediglich einige weitere Zeilen erforderlich:

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

Dadurch erhalten Sie Zugriff auf die Revit-API und können benutzerdefinierte Skripte für beliebige Revit-Aufgaben erstellen. Die Kombination der visuellen Programmierung mit der Skripterstellung in der Revit-API bringt erhebliche Verbesserungen für die Zusammenarbeit und die Entwicklung von Werkzeugen mit sich. So könnten beispielsweise ein BIM-Manager und ein Schemaplanentwickler gemeinsam am selben Diagramm arbeiten. Bei dieser Zusammenarbeit können sie sowohl den Entwurf als auch die Realisierung des Modells verbessern.

![](<../../.gitbook/assets/python & revit - 01.jpg>)

### Plattformspezifische APIs

Mit dem Dynamo-Projekt ist beabsichtigt, die Möglichkeiten der Plattformimplementierung zu erweitern. Da Dynamo nach und nach weitere Programme in seine Palette aufnimmt, erhalten Benutzer Zugriff auf plattformspezifische APIs aus der Python-Skriptumgebung. In diesem Abschnitt wird ein Fallbeispiel für Revit behandelt. Zukünftig sollen jedoch weitere Kapitel mit umfassenden Lernprogrammen zur Skripterstellung für andere Plattformen bereitgestellt werden. Darüber hinaus stehen jetzt zahlreiche [IronPython](http://ironpython.net)-Bibliotheken zur Verfügung, die Sie in Dynamo importieren können.

Die folgenden Beispiele zeigen Möglichkeiten zur Implementierung Revit-spezifischer Vorgänge aus Dynamo mit Python. Eine genauere Beschreibung der Beziehung zwischen Python einerseits und Dynamo und Revit andererseits finden Sie auf der [Wiki-Seite zu Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Python-0.6.3-to-0.7.x-Migration). Eine andere nützliche Ressource für Python und Revit ist das [Revit Python Shell](https://github.com/architecture-building-systems/revitpythonshell)-Projekt.

## Übung 1

> Erstellen Sie ein neues Revit-Projekt.
>
> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/8-2/2/Revit-Doc.dyn" %}

In diesen Übungen lernen Sie die grundlegenden Python-Skripts in Dynamo für Revit kennen. Dabei liegt das Hauptaugenmerk auf der Verarbeitung von Revit-Dateien und -Elementen sowie der Kommunikation zwischen Revit und Dynamo.

Dies ist eine einfache Methode zum Abrufen von _doc_, _uiapp_ und _app_ für die mit der Dynamo-Sitzung verknüpfte Revit-Datei. Programmierern, die zuvor bereits in der Revit-API gearbeitet haben, fallen eventuell die Einträge in der Liste des Watch-Blocks auf. Falls diese Einträge ungewohnt wirken, besteht kein Grund zur Beunruhigung. In den weiteren Übungen werden andere Beispiele verwendet.

Der folgende Code zeigt, wie Sie die Revit-Dienste importieren und die Dokumentdaten in Dynamo abrufen können.

![](<../images/8-3/2/python & revit - exercise 01 - 01.jpg>)

Werfen Sie einen Blick auf den Python-Block in Dynamo. Sie finden den Code auch unten:

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

## Übung 2

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/8-2/2/Revit-ReferenceCurve.dyn" %}

In dieser Übung erstellen Sie mithilfe des Python-Blocks von Dynamo eine einfache Modellkurve in Revit.

Beginnen Sie, indem Sie in Revit eine neue Entwurfskörperfamilie erstellen.

![](<../images/8-3/2/python & revit - exercise 02 - 01.jpg>)

Öffnen Sie den Ordner _Conceptual Mass_, und verwenden Sie die Vorlagendatei _Metric Mass.rft_.

![](<../images/8-3/2/python & revit - exercise 02 - 02.jpg>)

Verwenden Sie in Revit den Tastaturbefehl **`un`**, um die Einstellungen für Projekteinheiten aufzurufen, und ändern Sie die Längeneinheit in Meter.

![](<../images/8-3/2/python & revit - exercise 02 - 03.jpg>)

Starten Sie Dynamo und erstellen Sie die Gruppe von Blöcken in der Abbildung unten. Sie erstellen zunächst mithilfe von Dynamo-Blöcken zwei Referenzpunkte in Revit.

![](<../images/8-3/2/python & revit - exercise 02 - 04.jpg>)

> 1. Erstellen Sie einen **Codeblock** mit dem Wert `"0;"`.
> 2. Verbinden Sie diesen Wert mit den x-, y- und z-Eingaben eines **ReferencePoint.ByCoordinates**-Blocks.
> 3. Erstellen Sie drei Schieberegler mit dem Bereich zwischen -100 und 100 und der Schrittgröße 1.
> 4. Verbinden Sie die Schieberegler jeweils mit einem **ReferencePoint.ByCoordinates**-Block.
> 5. Fügen Sie einen **Python**-Block im Arbeitsbereich hinzu, klicken Sie auf die Schaltfläche +, um eine weitere Eingabe hinzuzufügen, und verbinden Sie die beiden Referenzpunkte mit den Eingaben. Öffnen Sie den **Python**-Block.

Werfen Sie einen Blick auf den Python-Block in Dynamo. Den vollständigen Code finden Sie unten.

![](<../images/8-3/2/python & revit - exercise 02 - 05.jpg>)

> 1. **System.Array:** Für Revit wird eine **Systemreihe** (anstelle einer Python-Liste) benötigt. Hierfür genügt eine weitere Codezeile. Indem Sie sorgfältig auf die Argumenttypen achten, erleichtern Sie jedoch die Python-Programmierung in Revit.

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

Sie haben in Dynamo zwei Referenzpunkte erstellt und diese mithilfe von Python mit einer Linie verbunden. In der nächsten Übung führen Sie dies weiter.

![](<../images/8-3/2/python & revit - exercise 02 - 06.jpg>)

## Übung 3

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/8-2/2/Revit-StructuralFraming.zip" %}

Diese Übung ist relativ einfach, macht jedoch die Verbindung von Daten und Geometrie zwischen Revit und Dynamo – in beiden Richtungen – deutlich. Öffnen Sie zuerst Revit-StructuralFraming.rvt. Starten Sie anschließend Dynamo und öffnen Sie die Datei Revit-StructuralFraming.dyn.

![](<../../.gitbook/assets/python & revit - exercise 03 - 01.jpg>)

Diese Datei ist denkbar einfach. Sie umfasst zwei auf Ebene 1 und Ebene 2 gezeichnete Referenzkurven. Diese Kurven sollen in Dynamo übernommen werden, wobei eine Direktverknüpfung bestehen bleibt.

Diese Datei enthält eine Gruppe von Blöcken, die mit den fünf Eingaben eines Python-Blocks verbunden sind.

![](<../images/8-3/2/python & revit - exercise 03 - 02.jpg>)

> 1. **Select Model Element**-Blöcke: Klicken Sie jeweils auf die Schaltfläche Auswählen und wählen Sie die zugehörige Kurv in Revit aus.
> 2. **Code Block:** Geben Sie die Syntax `0..1..#x;`_,_ ein und verbinden Sie einen Integer Slider mit Werten zwischen 0 und 20 mit der _x_-Eingabe. Dadurch wird die Anzahl der Träger gesteuert, die zwischen den beiden Kurven gezeichnet werden sollen.
> 3. **Structural Framing Types:** Wählen Sie hier den vorgegebenen W12x26-Träger aus der Dropdown-Liste.
> 4. **Levels:** Wählen Sie "Level 1".

Dieser Code in Python ist etwas komplexer, aus den darin enthaltenen Kommentaren geht jedoch hervor, wie der Prozess abläuft.

![](<../images/8-3/2/python & revit - exercise 03 - 03.jpg>)

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

In Revit wird eine Reihe von Trägern zwischen den beiden Kurven als Tragwerkselemente angezeigt. Anmerkung: Dies ist kein realistisches Beispiel. Die Tragwerkselemente sind nur als Beispiele für aus Dynamo erstellte native Revit-Exemplare.

In Dynamo werden die Ergebnisse ebenfalls angezeigt. Die Träger im **Watch3D**-Block verweisen auf die aus den Revit-Elementen abgefragte Geometrie.

![](<../images/8-3/2/python & revit - exercise 03 - 05.jpg>)

Dabei werden Daten aus der Revit-Umgebung für die Dynamo-Umgebung konvertiert. Zusammenfassung: Der Prozess läuft wie folgt ab:

1. Wählen Sie das Revit-Element aus.
2. Konvertieren Sie das Revit-Element in eine Dynamo-Kurve.
3. Unterteilen Sie die Dynamo-Kurve in eine Folge von Dynamo-Punkten.
4. Erstellen Sie Dynamo-Linien mithilfe der Dynamo-Punkte zwischen den beiden Kurven.
5. Erstellen Sie Revit-Träger durch Referenzieren der Dynamo-Linien.
6. Geben Sie Dynamo-Oberflächen durch Abfragen der Geometrie der Revit-Träger aus.

Dies mag etwas umständlich erscheinen. Das Skript erleichtert den Vorgang jedoch erheblich: Sie müssen jetzt lediglich die Kurve in Revit bearbeiten und den Solver erneut ausführen. (Es ist möglich, dass Sie dabei die bisherigen Träger löschen müssen.) _Dies liegt daran, dass wir die Träger in Python platzieren und dadurch die Zuordnung der OOTB-Blöcke auflösen._

Werden die Referenzkurven in Revit aktualisiert, erhalten Sie eine neue Reihe von Trägern.

![](<../images/8-3/2/python & revit - ex 03 - 06.gif>)
