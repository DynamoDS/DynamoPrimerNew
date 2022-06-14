# Entwickeln von Paketen

In Dynamo steht eine Reihe von Verfahren zum Entwickeln von Paketen zur Verfügung, die Sie für Ihre eigenen Zwecke verwenden oder für die Dynamo-Community bereitstellen können. In der folgenden Fallstudie wird ein bestehendes Paket zerlegt, um den Aufbau von Paketen zu demonstrieren. Dabei werden Lektionen aus dem vorigen Kapitel zugrunde gelegt: Eine Gruppe benutzerdefinierter Blöcke zum Zuordnen von Geometrie aus einer Dynamo-Oberfläche zu einer anderen mithilfe von UV-Koordinaten wird bereitgestellt.

## MapToSurface-Paket

Sie arbeiten mit einem Beispielpaket, das die UV-Zuordnung von Punkten aus einer Oberfläche zu einer anderen zeigt. Sie haben die Grundfunktionen dieses Werkzeugs bereits im Abschnitt [Erstellen eines benutzerdefinierten Blocks](../10\_custom-nodes/10-2\_creating.md) in diesem Leitfaden erstellt. In den folgenden Dateien wird gezeigt, wie Sie die UV-Zuordnung weiterentwickeln und eine Gruppe von Werkzeugen für eine Bibliothek erstellen können, die anschließend veröffentlicht werden kann.

In der unten gezeigten Abbildung wird ein Punkt aus einer Oberfläche mithilfe von UV-Koordinaten einer anderen zugeordnet. Dem Paket liegt dasselbe Prinzip zugrunde, es wird jedoch komplexere Geometrie verwendet.

![](../images/6-2/3/uvMap.jpg)

### Installieren des Pakets

Im vorigen Kapitel wurden in Dynamo verfügbare Methoden vorgestellt, mit denen eine Oberfläche mithilfe in der xy-Ebene definierter Kurven in Felder unterteilt werden kann. In dieser Fallstudie wird dies auf mehrdimensionale Geometrie erweitert. Dabei installieren Sie das fertige Paket und es wird gezeigt, wie dieses entwickelt wurde. Im nächsten Abschnitt wird gezeigt, wie dieses Paket veröffentlicht wurde.

Klicken Sie in Dynamo auf \_Pakete > Suchen nach Paket... und s\_uchen Sie nach dem Paket "MapToSurface" (alles ein Wort). Klicken Sie auf Installieren, um den Download zu starten und das Paket zu Ihrer Bibliothek hinzuzufügen.

![](<../images/6-2/3/develop package - install package 01.jpg>)

Nach der Installation sollten die benutzerdefinierten Blöcke im Abschnitt Add-Ons > Dynamo Primer verfügbar sein.

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Nachdem Sie das Paket installiert haben, analysieren Sie dieses jetzt.

### Benutzerdefinierte Blöcke

Für das Paket, das Sie hier erstellen, werden fünf benutzerdefinierte Blöcke verwendet. Diese Blöcke sind hier als Referenz bereits definiert. Im Folgenden analysieren Sie die Funktionen der einzelnen Blöcke. Einige der benutzerdefinierten Blöcke wurden ihrerseits unter Verwendung benutzerdefinierter Blöcke erstellt. Das Layout der Diagramme soll anderen Benutzern das Verständnis erleichtern.

Dieses Paket ist mit fünf benutzerdefinierten Blöcken recht einfach konstruiert. In den nachfolgenden Schritten wird der Aufbau der einzelnen Blöcke kurz erläutert.

![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

#### **PointsToSurface**

Dieser recht einfache benutzerdefinierte Block bildet die Grundlage für alle anderen Zuordnungsblöcke. Dieser Block ordnet, einfach ausgedrückt, einen Punkt mit UV-Koordinaten auf einer Quelloberfläche der Position mit den entsprechenden UV-Koordinaten auf der Zieloberfläche zu. Punkte sind das einfachste Geometrieelement, aus dem komplexere Geometrie erstellt wird. Aus diesem Grund können Sie mithilfe dieser Logik 2D- und sogar 3D-Geometrie von einer Oberfläche einer anderen zuweisen.

![](<../images/6-2/3/develop package -pointToSurface.jpg>)

#### **PolygonsToSurface**

Die Logik zum Erweitern der Punktzuordnung von 1D- auf 2D-Geometrie wird hier auf einfache Weise anhand von Polygonen gezeigt. Beachten Sie, dass der _PointsToSurface_-Block in diesem Block verschachtelt ist. Dadurch können Sie Punkte jedes Polygons der Oberfläche zuordnen und dann das Polygon aus den zugeordneten Punkten rekonstruieren. Durch Verwendung der geeigneten Datenstruktur (eine Liste aus Listen von Punkten) bleiben die Polygone nach ihrer Umwandlung in eine Punktsammlung als separate Polygone erhalten.

![](<../images/6-2/3/develop package -polygonsToSurface.jpg>)

#### **NurbsCrvtoSurface**

Hier kommt dieselbe Logik zum Einsatz wie beim _PolygonsToSurface_-Block. Dabei werden allerdings keine Punkte für Polygone, sondern Steuerpunkte für Nurbs-Kurven zugeordnet.

![](<../images/6-2/3/develop package -nurbsCrvtoSurface.jpg>)

**OffsetPointsToSurface**

Dieser Block ist etwas komplexer, wobei jedoch ein einfaches Prinzip zugrunde liegt: Mit diesem Block werden genau wie beim _PointsToSurface_-Block Punkte aus einer Oberfläche einer anderen zugeordnet. Dabei werden jedoch auch Punkte berücksichtigt, die nicht auf der ursprünglichen Quelloberfläche liegen. Für diese Punkte wird ihr Abstand zum nächstgelegenen UV-Parameter abgerufen und dieser Abstand wird der Normalen an der entsprechenden UV-Koordinatenposition der Zieloberfläche zugeordnet. Bei der Bearbeitung der Beispieldateien wird dies leichter verständlich.

![](<../images/6-2/3/develop package -OffsetPointsToSurface.jpg>)

#### **SampleSrf**

Mit diesem einfachen Block wird eine parametrische Oberfläche erstellt, die aus dem Quellraster einer gewellten Oberfläche in den Beispieldateien zugeordnet werden kann.

![](<../images/6-2/3/develop package -sampleSrf.jpg>)

### Beispieldateien

Die Beispieldateien befinden sich im Stammordner des Pakets. Klicken Sie auf Dynamo > Einstellungen > Package Manager.

Klicken Sie neben MapToSurface auf das Menü mit den drei Punkten > Stammverzeichnis anzeigen.

![](<../images/6-2/3/develop package - example files 01.jpg>)

Navigieren Sie zum Ordner _extra_, in dem sich alle Dateien des Pakets befinden, die keine benutzerdefinierten Blöcke sind. Hier werden Beispieldateien (sofern vorhanden) für Dynamo-Pakete abgelegt. Mit den im Folgenden gezeigten Abbildungen werden die in den einzelnen Beispieldateien gezeigten Konzepte erläutert.

#### **01-PanelingWithPolygons**

Dieses Beispiel zeigt die Unterteilung einer Oberfläche anhand eines Rasters aus Rechtecken mithilfe von _PointsToSurface_. Dieser Vorgang ist Ihnen wahrscheinlich vertraut, da ein ähnlicher Arbeitsablauf im [vorigen Kapitel](../10\_custom-nodes/10-2\_creating.md) gezeigt wurde.

![](<../images/6-2/3/develop package -sample file 01.jpg>)

#### **02-PanelingWithPolygons-II**

In dieser Übungsdatei wird eine Einrichtung zum Zuordnen von Kreisen (bzw. Polygonen, die Kreise repräsentieren) von einer Oberfläche zu einer anderen gezeigt. Hierfür wird der _PolygonsToSurface_-Block verwendet.

![](<../images/6-2/3/develop package -sample file 02.jpg>)

#### **03-NurbsCrvsAndSurface**

Diese Beispieldatei zeigt einen komplexeren Vorgang unter Verwendung des NurbsCrvToSurface-Blocks. Die Zieloberfläche wird um eine angegebene Strecke versetzt und die Nurbs-Kurve wird sowohl der ursprünglichen Zieloberfläche als auch der versetzten Oberfläche zugeordnet. Die beiden zugeordneten Kurven werden anschließend durch eine Erhebung verbunden, sodass eine Oberfläche entsteht, die anschließend verdickt wird. Der resultierende Körper weist eine gewellte Form auf, die den Normalen der Zieloberfläche folgt.

![](<../images/6-2/3/develop package -sample file 03.jpg>)

#### **04-PleatedPolysurface-OffsetPoints**

In dieser Beispieldatei wird die Zuordnung einer gefalteten Polysurface von einer Quell- zu einer Zieloberfläche gezeigt. Die Quell- und Zieloberfläche sind eine rechteckige, das ganze Raster umfassende Oberfläche und ein Rotationskörper.

![](<../images/6-2/3/develop package -sample file 04a.jpg>)

Zuordnung der Quell-PolySurface von der Quell- zur Zieloberfläche.

![](<../images/6-2/3/develop package -sample file 04b.jpg>)

#### **05-SVG-Import**

Mithilfe der benutzerdefinierten Blöcke können Kurven unterschiedlicher Typen zugeordnet werden. In dieser letzten Datei wird daher eine aus Illustrator exportierte SVG-Datei referenziert und die importierten Kurven werden einer Zieloberfläche zugeordnet.

![](<../images/6-2/3/develop package -sample file 05a.jpg>)

Durch Analyse der Syntax einer SVG-Datei werden die Kurven aus dem XML-Format in Dynamo-Polykurven konvertiert.

![](<../images/6-2/3/develop package -sample file 05b.jpg>)

Die importierten Kurven werden einer Zieloberfläche zugeordnet. Dieser Vorgang ermöglicht es, eine Unterteilung explizit (durch Zeigen und Klicken) in Illustrator zu entwerfen, und sie anschließend in Dynamo zu importieren und auf eine Zieloberfläche anzuwenden.

![](<../images/6-2/3/develop package -sample file 05c.jpg>)
