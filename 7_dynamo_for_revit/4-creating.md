# Erstellen

Sie können in Dynamo Arrays von Revit-Elementen mit uneingeschränkter parametrischer Steuerung erstellen. Die Revit-Blöcke in Dynamo ermöglichen das Importieren von Elementen aus allgemeiner Geometrie in spezifische Kategorietypen (z. B. Wände und Geschossdecken). In diesem Abschnitt importieren Sie Elemente mit flexiblen Parametern für adaptive Bauteile.

![](images/4/creating-dynamonodes.jpg)

### Adaptive Bauteile

Ein adaptives Bauteil ist eine flexible Familienkategorie, die sich für generative Anwendungen eignet. Bei der Instanziierung können Sie ein komplexes geometrisches Element erstellen, das durch die Positionen adaptiver Punkte gesteuert wird.

Das Beispiel unten zeigt ein adaptives Bauteil mit drei Punkten im Familieneditor. Es dient zum Erstellen eines durch die Positionen der einzelnen adaptiven Punkte gesteuerten Fachwerkbinders. In der folgenden Übung erstellen Sie mithilfe dieses Bauteils eine Reihe von Fachwerkbindern an einer Fassade.

![](images/4/ac.jpg)

### Prinzipien der Interoperabilität

Adaptive Bauteile bieten ein gutes Beispiel für optimale Verfahren zur Interoperabilität. Sie können eine Reihe adaptiver Bauteile erstellen, indem Sie die zugrunde liegenden adaptiven Punkte definieren. Wenn Sie diese Daten in andere Programme übertragen, haben Sie die Möglichkeit, die Geometrie auf einfache Daten zu reduzieren. Dabei liegt eine ähnliche Logik zugrunde wie beim Importieren und Exportieren in Programmen wie Excel.

Angenommen, ein Berater für die Fassaden benötigt Angaben zur Position der Fachwerkelemente, möchte jedoch nicht die vollständig ausgearbeitete Geometrie analysieren. Zur Vorbereitung der Fertigung kann der Berater die Position der adaptiven Punkte ermitteln und daraus Geometrie für ein Programm wie Inventor neu generieren.

In der folgenden Übung richten Sie einen Arbeitsablauf ein, der Ihnen Zugriff auf alle diese Daten gibt und zugleich die Definition zum Erstellen von Revit-Elementen festlegt. In diesem Prozess führen Sie Konzeption, Dokumentation und Fertigung zu einem nahtlosen Arbeitsablauf zusammen. Damit erhalten Sie einen intelligenteren und effizienteren Prozess zur Interoperabilität.

### Mehrere Elemente und Listen

In der [ersten Übung](8-4\_creating.md#exercise) unten wird gezeigt, wie Dynamo Daten für die Erstellung von Revit-Elementen referenziert. Um mehrere adaptive Bauteile zu generieren, definieren Sie eine Liste von Listen, wobei jede Liste drei Punkte enthält, die für die Punkte im adaptiven Bauteil stehen. Beachten Sie dies, während die Datenstrukturen in Dynamo verwaltet werden.

![](images/4/creating-multipleelementsandlists01.jpg)

### DirectShape-Elemente

Mit DirectShape steht ein weiteres Verfahren zum Importieren parametrischer Dynamo-Geometrie in Revit zur Verfügung. Kurz zusammengefasst: Das DirectShape-Element und die dazugehörigen Klassen ermöglichen es, extern erstellte geometrische Formen in einem Revit-Dokument zu speichern. Zu dieser Geometrie können geschlossene Körper oder Netze gehören. DirectShape ist in erster Linie für den Import von Formen aus anderen Formaten wie z. B. IFC oder STEP vorgesehen, in denen nicht genügend Informationen zum Erstellen eines "echten" Revit-Elements zur Verfügung stehen. Die DirectShape-Funktionen eignen sich genau wie bei IFC- und STEP-Arbeitsabläufen auch zum Importieren mit Dynamo erstellter Geometrie als echte Elemente in Revit-Projekte.

Die [zweite Übung](8-4\_creating.md#exercise-directshape-elements) zeigt den Ablauf für den Import von Dynamo-Geometrie als DirectShape in ein Revit-Projekt. Mithilfe dieses Verfahrens können Sie die Kategorie, das Material und den Namen der importierten Geometrie zuweisen, wobei die parametrische Verknüpfung mit dem Dynamo-Diagramm erhalten bleibt.

## Übung: Erstellen von Elementen und Listen

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="datasets/4/Revit-Creating.zip" %}

Beginnen Sie mit der Beispieldatei für diesen Abschnitt (oder verwenden Sie weiterhin die Revit-Datei aus dem vorigen Abschnitt). Derselbe Revit-Körper wird angezeigt.

![](images/4/creating-exercise01.jpg)

> 1. Dies ist der Zustand beim Öffnen der Datei.
> 2. Dies ist das Fachwerksystem, das Sie mit Dynamo erstellt haben, wobei eine intelligente Verknüpfung mit dem Revit-Körper genutzt wird.

Sie haben bereits mit den Blöcken _Select Model Element_ und _Select Face_ gearbeitet. Hier verwenden Sie _Select Edge_ und arbeiten damit eine Ebene tiefer in der Geometriehierarchie. Stellen Sie für die Ausführung des Dynamo-Solver die Option _Automatisch_ ein, damit das Diagramm laufend gemäß den Änderungen in der Revit-Datei aktualisiert wird. Die Kante, die Sie auswählen, ist dynamisch mit der Topologie des Revit-Elements verbunden. Solange die Topologie* unverändert bleibt, bleibt die Verknüpfung zwischen Revit und Dynamo erhalten.

![](images/4/creating-exercise02.jpg)

> 1. Wählen Sie die oberste Kurve der verglasten Fassade aus. Diese erstreckt sich über die gesamte Länge des Gebäudes. Falls Sie bei der Auswahl der Kante Schwierigkeiten haben, können Sie in Revit den Mauszeiger auf die Kante setzen und die _Tabulatortaste_ drücken, bis die gewünschte Kante hervorgehoben wird.
> 2. Wählen Sie über zwei _Select Edge_-Blöcke die beiden Kanten für den Vorsprung in der Mitte der Fassade aus.
> 3. Wiederholen Sie dies für die Kanten am unteren Ende der Fassade in Revit.
> 4. In den _Watch_-Blöcken wird angezeigt, dass in Dynamo jetzt Linien vorhanden sind. Diese werden automatisch in Dynamo-Geometrie konvertiert, da die Kanten selbst keine Revit-Elemente sind. Diese Kurven sind die Referenzen, die zum Instanziieren der adaptiven Fachwerkbinder entlang der Fassade verwendet werden.

{% hint style="info" %} *Um eine konsistente Topologie zu erhalten, wird hier ein Modell verwendet, dem keine zusätzlichen Flächen oder Kanten hinzugefügt wurden. Sie können zwar mithilfe von Parametern seine Form ändern, seine Struktur bleibt jedoch einheitlich. {% endhint %}

Als Erstes müssen Sie die Kurven verbinden und zu einer Liste zusammenführen. Dadurch _gruppieren_ Sie die Kurven, um geometrische Operationen durchführen zu können.

![](images/4/creating-exercise03.jpg)

> 1. Erstellen Sie eine Liste für die beiden Kurven in der Mitte der Fassade.
> 2. Verbinden Sie die beiden Kurven zu einer Polykurve, indem Sie die _List.Create_-Komponente mit einem _Polycurve.ByJoinedCurves_-Block verbinden.
> 3. Erstellen Sie eine Liste für die beiden Kurven am unteren Rand der Fassade.
> 4. Verbinden Sie die beiden Kurven zu einer Polykurve, indem Sie die _List.Create_-Komponente mit einem _Polycurve.ByJoinedCurves_-Block verbinden.
> 5. Führen Sie schließlich die drei Hauptkurven (eine Linie und zwei Polykurven) zu einer Liste zusammen.

Danach verwenden Sie die oberste der Kurven, die eine über die gesamte Länge der Fassade verlaufende Linie ist. Sie erstellen entlang dieser Linie Ebenen, die die in der Liste zusammengefassten Kurven schneiden.

![](images/4/creating-exercise04.jpg)

> 1. Definieren Sie in einem _Code Block_ einen Bereich mit der Syntax: `0..1..#numberOfTrusses;`
> 2. Verbinden Sie einen *Integer Slider *mit der Eingabe des Codeblocks. Dessen Werte geben naheliegenderweise die Anzahl der Fachwerkbinder an. Beachten Sie, dass der Schieberegler die Anzahl der Einträge in einem von *0 *bis _1_ definierten Bereich steuert.
> 3. Verbinden Sie den _Code Block_ mit der _param_-Eingabe eines _Curve.PlaneAtParameter_-Blocks und verbinden Sie die obere Kante mit der _curve_-Eingabe. Damit erhalten Sie zehn Ebenen, die gleichmäßig über die Länge der Fassade verteilt sind.

Eine Ebene ist ein abstraktes Geometrieelement, das für einen unendlichen zweidimensionalen Raum steht. Ebenen eignen sich ausgezeichnet zum Erstellen von Konturen und Schnitten wie in diesem Schritt gezeigt.

![](images/4/creating-exercise05.jpg)

> 1. Verwenden Sie als Nächstes den _Geometry.Intersect_-Block (legen Sie die Vergitterungsoption auf Kreuzprodukt fest): Verbinden Sie den _Curve.PlaneAtParameter_-Block mit der _entity_-Eingabe des _Geometry.Intersect_-Blocks. Verbinden Sie den _List.Create_-Block mit der _geometry_-Eingabe. Im Dynamo-Ansichtsfenster werden daraufhin die Schnittpunkte der einzelnen Kurven mit den definierten Ebenen angezeigt.

Die Ausgabe ist eine Liste aus Listen von Listen. Sie enthält zu viele Listen für diesen Verwendungszweck. Sie müssen die Liste teilweise vereinfachen. Dazu müssen Sie die zweithöchste Ebene der Liste ansteuern und das Ergebnis vereinfachen. Verwenden Sie dazu die _List.Map_-Operation, die im Kapitel zu Listen in dieser Einführung beschrieben wird.

![](images/4/creating-exercise06.jpg)

> 1. Verbinden Sie den _Geometry.Intersect_-Block mit der list-Eingabe von _List.Map_.
> 2. Verbinden Sie einen _Flatten_-Block mit der f(x)-Eingabe von _List.Map_. Als Ergebnis erhalten Sie drei Listen, jeweils mit so vielen Einträgen, wie Fachwerkbinder erstellt werden sollen.
> 3. Sie müssen diese Daten ändern. Für die Instanziierung des Fachwerkbinders benötigen Sie dieselbe Anzahl adaptiver Punkte wie in der Familie definiert. Dieses adaptive Bauteil weist drei Punkte auf. Sie benötigen also anstelle von drei Listen mit je zehn Einträgen (numberOfTrusses) zehn Listen mit je drei Einträgen. Auf diese Weise können Sie 10 adaptive Bauteile erstellen.
> 4. Verbinden Sie den _List.Map_-Block mit einem _List.Transpose_-Block. Damit werden die gewünschten Daten ausgegeben.
> 5. Um sich zu vergewissern, dass Sie die richtigen Daten erhalten haben, fügen Sie im Ansichtsbereich einen _Polygon.ByPoints_-Block hinzu und überprüfen Sie das Ergebnis in der Dynamo-Vorschau.

Ordnen Sie jetzt die adaptiven Bauteile in einem Array an, wobei Sie dasselbe Verfahren verwenden wie beim Erstellen der Polygone.

![](images/4/creating-exercise07.jpg)

> 1. Fügen Sie im Ansichtsbereich einen _AdaptiveComponent.ByPoints_-Block hinzu und verbinden Sie den _List.Transpose_-Block mit der _points_-Eingabe.
> 2. Wählen Sie in einem _Family Types_-Block die _AdaptiveTruss_-Familie und verbinden Sie den Block mit der _FamilyType_-Eingabe des _AdaptiveComponent.ByPoints_-Blocks.

In Revit sind jetzt die zehn Fachwerkbinder gleichmäßig über die Länge der Fassade verteilt zu sehen.

Testen Sie das Diagramm: Erhöhen Sie den Wert für numberOfTrusses auf 30, indem Sie den Wert im Integer Slider ändern. Sie erhalten zahlreiche Fachwerkbinder: Dies mag nicht sonderlich realistisch wirken, es zeigt jedoch, dass die parametrische Verknüpfung funktionstüchtig ist. Legen Sie nach der Überprüfung numberOfTrusses auf 15 fest.

![](images/4/creating-exercise08.gif)

Führen Sie abschließend einen weiteren Test durch: Indem Sie in Revit den Körper auswählen und seine Exemplarparameter bearbeiten, können Sie die Form des Gebäudes ändern und beobachten, wie die Fachwerkbinder angepasst werden. Denken Sie daran, dass dieses Dynamo-Diagramm geöffnet sein muss, damit die Aktualisierung sichtbar ist. Sobald das Diagramm geschlossen wird, geht die Verknüpfung verloren.

![](images/4/creating-exercise09.jpg)

## Übung: DirectShape-Elemente

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="datasets/4/Revit-Creating-DirectShape.zip" %}

Öffnen Sie als Erstes die Beispieldatei für diese Lektion: ARCH-DirectShape-BaseFile.rvt.

![](images/4/creating-exerciseII-01.jpg)

> 1. Die 3D-Ansicht zeigt den Gebäudekörper aus der letzten Lektion.
> 2. Entlang der Kante des Foyers verläuft eine Referenzkurve, die Sie in Dynamo referenzieren werden.
> 3. Entlang der gegenüberliegenden Kante des Foyers verläuft eine zweite Referenzkurve, die ebenfalls in Dynamo referenziert werden soll.

![](images/4/creating-exerciseII-02.jpg)

> 1. Zum Referenzieren der Geometrie in Dynamo verwenden Sie _Select Model Element_ für die einzelnen Elemente in Revit. Wählen Sie den Körper in Revit aus und importieren Sie die Geometrie mithilfe von _Element.Faces_ in Dynamo. Dadurch wird der Körper in der Dynamo-Vorschau angezeigt.
> 2. Importieren Sie eine der Referenzkurven mithilfe von _Select Model Element_ und _CurveElement.Curve_ in Dynamo.
> 3. Importieren Sie die andere Referenzkurve mithilfe von _Select Model Element_ und _CurveElement.Curve_ in Dynamo.

![](images/4/creating-exerciseII-03.jpg)

> 1. Wenn Sie das Beispieldiagramm verkleinern und nach rechts schwenken, sehen Sie eine große Gruppe von Blöcken. Diese stehen für die geometrischen Operationen, mit denen die in der Dynamo-Vorschau gezeigte Gitterkonstruktion für das Dach generiert wird. Diese Blöcke wurden mithilfe der Funktion _Block zu Code_ erstellt, die im [Abschnitt zu Codeblöcken](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md#Node) dieser Einführung beschrieben wird.
> 2. Diese Konstruktion wird im Wesentlichen durch drei Parameter gesteuert: Diagonal Shift, Camber und Radius.

Vergrößern Sie die Darstellung der Parameter für dieses Diagramm. Indem Sie mit diesen experimentieren, erhalten Sie unterschiedliche Geometrie.

![](images/4/creating-exerciseII-04.jpg)

![](images/4/creating-exerciseII-05.jpg)

> 1. Fügen Sie im Ansichtsbereich einen _DirectShape.ByGeometry_-Block hinzu. Dieser Block weist die vier Eingaben _geometry_**,** _category_**,** _material_ und _name_ auf.
> 2. Die Geometrie ist der Körper, der mithilfe des Diagrammteils für die Geometrieerstellung erstellt wird.
> 3. Die category-Eingabe wird mithilfe der Dropdown-Liste im _Categories_-Block gewählt. Verwenden Sie hier "Tragwerk".
> 4. Die material-Eingabe wird über das Array von Blöcken darüber ausgewählt. In diesem Fall kann allerdings auch einfach die Vorgabe definiert werden.

Nachdem Sie Dynamo ausgeführt haben, befindet sich die importierte Geometrie im Revit-Projekt auf dem Dach. Dabei handelt es sich nicht um ein allgemeines Modell, sondern ein Tragwerkselement. Die parametrische Verknüpfung mit Dynamo bleibt erhalten.

![](images/4/creating-exerciseII-06.jpg)
