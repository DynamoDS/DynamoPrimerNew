# Python-Blöcke

Wozu dient die Textprogrammierung in der visuellen Programmierumgebung von Dynamo? [Visuelle Programmierung](../../a\_appendix/visual-programming-and-dynamo.md) bietet viele Vorteile. Sie ermöglicht es, in einer intuitiven visuellen Oberfläche Programme zu entwickeln, ohne eine spezielle Syntax zu erlernen. Visuelle Programme können jedoch recht unübersichtlich werden und enthalten zuweilen nicht genügend Funktionalität. So stehen in Python beispielsweise wesentlich praktischere Methoden zum Schreiben von Bedingungsanweisungen (if/then) und für Schleifen zur Verfügung. Python ist ein leistungsstarkes Werkzeug, das das Funktionsspektrum von Dynamo erweitern und Ihnen die Möglichkeit geben kann, eine große Gruppe von Blöcken durch einige wenige präzise Codezeilen zu ersetzen.

**Visuelles Programm:**

![](<../images/8-3/1/python node - visual vs textual programming.jpg>)

**Textprogramm:**

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

### Der Python-Block

Python-Blöcke sind genau wie Codeblöcke eine Scripting-Oberfläche innerhalb einer Umgebung für die visuelle Programmierung. Der Python-Block befindet sich in der Bibliothek unter Skript > Editor > Python Script.

![](<../images/8-3/1/python node - the python node 01.jpg>)

Durch Doppelklicken auf den Block wird der Python-Skript-Editor geöffnet. (Sie können auch mit der rechten Maustaste auf den Block klicken und _Bearbeiten ..._ auswählen.) Oben auf dem Bildschirm befindet sich vorgegebener Text, der es Ihnen erleichtern soll, die benötigten Bibliotheken zu referenzieren. Eingaben werden in der IN-Reihe gespeichert. Werte werden durch Zuweisung zur OUT-Variablen an Dynamo zurückgegeben.

![](<../images/8-3/1/python node - the python node 02.jpg>)

In der Autodesk.DesignScript.Geometry-Bibliothek können Sie Punktnotation ähnlich wie in Codeblöcken verwenden. Weitere Informationen zur Dynamo-Syntax finden Sie unter [7-2\_design-script-syntax.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "mention") sowie im [DesignScript-Handbuch](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf). (Um dieses PDF-Dokument herunterzuladen, klicken Sie mit der rechten Maustaste auf den Link und wählen Link speichern unter... aus). Wenn Sie einen Geometrietyp, z. B. 'Point.' eingeben, wird eine Liste mit den Methoden zum Erstellen und Abfragen von Punkten angezeigt.

![](<../images/8-3/1/python node - the python node 03.jpg>)

> Zu den Methoden gehören Konstruktoren, wie _ByCoordinates_, Aktionen wie _Add_ und Abfragen wie _X_-, _Y_- und _Z_-Koordinaten.

## Übung: Erstellen eines benutzerdefinierten Blocks mit Python Script zum Erstellen von Mustern aus einem Volumenkörpermodul

### Teil I: Einrichten von Python Script

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

In diesem Beispiel schreiben Sie ein Python-Skript zum Erstellen von Mustern aus einem Körpermodul und wandeln das Skript in einen benutzerdefinierten Block um. Zuerst erstellen Sie das Körpermodul mithilfe von Dynamo-Blöcken.

![](<../images/8-3/1/python node - exercise pt I-01.jpg>)

> 1. **Rectangle.ByWidthLength**: Erstellen Sie ein Rechteck, das als Basis für den Körper verwendet wird.
> 2. **Surface.ByPatch**: Verbinden Sie das Rechteck mit der _closedCurve_-Eingabe, um die untere Oberfläche zu erstellen.

![](<../images/8-3/1/python node - exercise pt I-02.jpg>)

> 1. **Geometry.Translate**: Verbinden Sie das Rechteck mit der _geometry_-Eingabe, um es nach oben zu verschieben, wobei Sie mithilfe eines Codeblocks die allgemeine Dicke des Körpers festlegen.
> 2. **Polygon.Points**: Fragen Sie die Eckpunkte des verschobenen Rechtecks ab.
> 3. **Geometry.Translate**: Erstellen Sie mithilfe eines Codeblocks eine Liste mit vier Werten für die vier Punkte, wobei Sie eine Ecke des Körpers nach oben verschieben.
> 4. **Polygon.ByPoints**: Erstellen Sie das obere Polygon aus den verschobenen Punkten erneut.
> 5. **Surface.ByPatch**: Schließen Sie das Polygon, um die obere Oberfläche zu erstellen.

Damit haben Sie die obere und untere Oberfläche erstellt. Erstellen Sie jetzt durch eine Erhebung zwischen den beiden Profilen die Seiten des Körpers.

![](<../images/8-3/1/python node - exercise pt I-03.jpg>)

> 1. **List.Create**: Verbinden Sie das Rechteck unten und das Polygon oben mit den index-Eingaben.
> 2. **Surface.ByLoft**: Erstellen Sie über eine Erhebung die Seiten des Körpers.
> 3. **List.Create**: Verbinden Sie die obere und untere sowie die seitlichen Oberflächen mit den index-Eingaben, um eine Liste der Oberflächen zu erhalten.
> 4. **Solid.ByJoinedSurfaces**: Verbinden Sie die Flächen, um das Körpermodul zu erstellen.

Damit haben Sie den Körper erstellt. Fügen Sie jetzt einen Block für das Python-Skript in den Arbeitsbereich ein.

![](<../images/8-3/1/python node - exercise pt I-04.jpg>)

> 1. Um dem Block weitere Eingaben hinzuzufügen, klicken Sie auf das +-Symbol im Block. Die Eingaben erhalten die Namen IN\[0], IN\[1] usw., um anzuzeigen, dass sie Einträge in einer Liste darstellen.

Sie beginnen, indem Sie die Ein- und Ausgaben definieren. Doppelklicken Sie auf den Block, um den Python-Editor zu öffnen. Halten Sie sich an den unten stehenden Code, um den Code im Editor zu ändern.

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

Dieser Code wird im weiteren Verlauf der Übung leichter verständlich. Als Nächstes müssen Sie überlegen, welche Informationen Sie zum Erstellen einer Reihe aus dem Körpermodul benötigen. Als Erstes müssen Sie die Maße des Körpers kennen, um die Entfernung für die Verschiebung zu ermitteln. Wegen eines Fehlers bei Begrenzungsrahmen müssen Sie diesen anhand der Kurvengeometrie der Kanten erstellen.

![](../images/8-3/1/python07.png)

> Werfen Sie einen Blick auf den Python-Block in Dynamo. Dieselbe Syntax wie in den Titeln der Blöcke in Dynamo wird auch hier verwendet. Sehen Sie sich den kommentierten Code unten an.

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

Da die Körpermodule sowohl verschoben als auch gedreht werden sollen, verwenden Sie hier die Geometry.Transform-Operation. Wenn Sie den Geometry.Transform-Block genauer betrachten, sehen Sie, dass für die Transformation des Körpers ein Quell- und ein Zielkoordinatensystem benötigt werden. Die Quelle ist das Koordinatensystem, das den Kontext für den Ausgangskörper bildet, während als Ziel ein eigenes Koordinatensystem für jedes Modul in der Reihe verwendet wird. Das bedeutet, dass Sie die x- und y-Werte in einer Schleife verarbeiten müssen, um das Koordinatensystem jedes Mal auf andere Weise zu transformieren.

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

Klicken Sie auf Ausführen, und speichern Sie dann den Code. Verbinden Sie den Python-Block wie folgt mit dem vorhandenen Skript.

![](<../images/8-3/1/python node - exercise pt I-07.jpg>)

> 1. Verbinden Sie die Ausgabe aus **Solid.ByJoinedSurfaces** als erste Eingabe für den Python-Block, und definieren Sie die anderen Eingaben mithilfe eines Codeblocks.
> 2. Erstellen Sie einen **Topology.Edges**-Block, und verwenden Sie die Ausgabe aus dem Python-Block als Eingabe.
> 3. Erstellen Sie abschließend einen **Edge.CurveGeometry**-Block, und verwenden Sie die Ausgabe von Topology.Edges als Eingabe.

Ändern Sie den Wert für die Ausgangszahl, um verschiedene Muster zu erstellen. Indem Sie die Parameter des Körpermoduls selbst ändern, erzielen Sie ebenfalls unterschiedliche Wirkungen.

![](../images/8-3/1/python10.png)

### Teil II: Umwandeln des Python Script-Blocks in einen benutzerdefinierten Block

Damit haben Sie ein nützliches Python-Skript erstellt. Speichern Sie dieses jetzt als benutzerdefinierten Block. Wählen Sie den Python Script-Block aus, klicken Sie mit der rechten Maustaste auf den Arbeitsbereich, und wählen Sie Benutzerdefinierten Block erstellen aus.

![](<../images/8-3/1/python node - exercise pt II-01.jpg>)

Weisen Sie einen Namen, eine Beschreibung und eine Kategorie zu.

![](<../images/8-3/1/python node - exercise pt II-02.jpg>)

Dadurch wird ein neuer Arbeitsbereich geöffnet, in dem Sie den benutzerdefinierten Block bearbeiten können.

![](<../images/8-3/1/python node - exercise pt II-03.jpg>)

> 1. **Eingabe-Blöcke**: Geben Sie den Eingaben aussagekräftigere Namen und fügen Sie Datentypen und Vorgabewerte hinzu.
> 2. **Output:** Ändert den Namen der Ausgabe.

Speichern Sie den Block als DYF-Datei. Nun sollten Sie sehen, dass der benutzerdefinierte Block die gerade vorgenommenen Änderungen widerspiegelt.

![](<../images/8-3/1/python node - exercise pt II-04.jpg>)
