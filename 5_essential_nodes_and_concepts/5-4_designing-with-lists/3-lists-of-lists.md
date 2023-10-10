# Listen von Listen

### Listen von Listen

In diesem Abschnitt kommt eine weitere Ebene zur Hierarchie hinzu. Für den Einstieg wurde ein Stapel Karten als Beispiel betrachtet. Angenommen, eine Schachtel enthält mehrere solche Stapel. In diesem Fall entspricht die Schachtel einer Liste von Stapeln und jeder Stapel einer Liste von Karten. Dies ist eine Liste von Listen. Als Analogie für diesen Abschnitt zeigt das Bild unten eine Liste von Münzstapeln, wobei jeder Stapel einer Liste von Münzen entspricht.

![Münzen](../images/5-4/3/coins-521245\_640.jpg)

> Foto von [Dori](https://commons.wikimedia.org/wiki/File:Stack\_of\_coins\_0214.jpg).

### Abfrage

Welche **Abfragen** sind in der Liste von Listen möglich? Dies ermöglicht den Zugriff auf vorhandene Eigenschaften.

* Anzahl von Münzarten? 2\.
* Wert der Münzarten? $0,01 und $0,25.
* Material der Vierteldollars? 75 % Kupfer und 25 % Nickel.
* Material der Cents? 97,5 % Zink und 2,5 % Kupfer.

### Aktion

Welche **Aktionen** können in der Liste von Listen durchgeführt werden? Diese Aktionen bewirken Änderungen in der Liste von Listen entsprechend der jeweiligen Operation.

* Wählen Sie einen bestimmten Stapel von Vierteldollars oder Cents aus.
* Wählen Sie einen bestimmten Vierteldollar oder Cent aus.
* Ordnen Sie den Stapel aus Vierteldollars und Cents neu.
* Mischen Sie die Stapel zusammen.

Auch in diesem Fall steht in Dynamo für jeden der oben genannten Vorgänge ein entsprechender Block zur Verfügung. Da allerdings keine realen Objekte, sondern abstrakte Daten verarbeitet werden, sind Regeln zum Navigieren in der Datenhierarchie erforderlich.

Die bei der Arbeit mit Listen von Listen verwendeten Daten sind komplex und umfassen mehrere Ebenen, was aber auch die Möglichkeit für äußerst wirkungsvolle parametrische Operationen eröffnet. Im Folgenden werden zunächst die Grundlagen im Einzelnen erläutert und in weiteren Lektionen einige weitere Operationen betrachtet.

## Übungslektion

### Hierarchie von oben nach unten

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/Top-Down-Hierarchy.dyn" %}

Das in diesem Abschnitt eingeführte Grundprinzip lautet: **Dynamo behandelt auch Listen als Objekte**. Diese Hierarchie von oben nach unten wurde im Hinblick auf objektorientierte Programmierung entwickelt. Anstatt Unterelemente mithilfe eines Befehls wie etwa **List.GetItemAtIndex** auszuwählen, wählt Dynamo den entsprechenden Index in der Hauptliste innerhalb der Datenstruktur. Dieses Element kann eine weitere Liste sein. Ein Beispielbild soll dies verdeutlichen:

![top-down](../images/5-4/3/listsoflists-topdownhierachy.jpg)

> 1. Im **Codeblock** wurden zwei Bereiche definiert: `0..2; 0..3;`
> 2. Diese Bereiche sind mit einem **Point.ByCoordinates**-Block unter Verwendung der Vergitterung _"Kreuzprodukt"_ verbunden. Dadurch wird ein Raster von Punkten erstellt und zugleich wird eine Liste von Listen als Ausgabe zurückgegeben.
> 3. Beachten Sie, dass im **Watch**-Block 3 Listen mit jeweils 4 Elementen angezeigt werden.
> 4. Bei Verwendung von **List.GetItemAtIndex** mit dem Index 0 wählt Dynamo die erste Liste sowie ihren gesamten Inhalt aus. In anderen Programmen wird eventuell das erste Element jeder Liste in der Datenstruktur ausgewählt, Dynamo verwendet jedoch eine von oben nach unten geordnete Hierarchie zur Verarbeitung der Daten.

### List.Flatten

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/Flatten.dyn" %}

Der Befehl Flatten entfernt alle Datenebenen aus einer Datenstruktur. Dies ist hilfreich, wenn Sie für Ihre Operationen keine Datenhierarchien benötigen, birgt jedoch auch Risiken, da Informationen entfernt werden. Das folgende Beispiel zeigt das Ergebnis nach der Vereinfachung einer Liste mit Daten.

![Übung](../images/5-4/3/listsoflists-flatten01.jpg)

> 1. Fügen Sie im **Codeblock** eine Zeile mit Code zum Definieren eines Bereichs ein: `-250..-150..#4;`
> 2. Der _Codeblock_ wird mit den _x_\- und _y_-Eingaben eines **Point.ByCoordinates**-Blocks verbunden, wobei die Vergitterung _"Kreuzprodukt"_ verwendet wird, um ein Raster aus Punkten zu erhalten.
> 3. Im **Watch**-Block wird angezeigt, dass eine Liste von Listen erstellt wurde.
> 4. Ein **PolyCurve.ByPoints**-Block referenziert die einzelnen Listen und erstellt die entsprechenden Polykurven. Die Dynamo-Vorschau zeigt vier Polykurven für die einzelnen Zeilen im Raster.

![Übung](../images/5-4/3/listsoflists-flatten02.jpg)

> 1. Durch Einfügen von _flatten_ vor dem Block für die Polykurven entsteht eine einzelne Liste, die sämtliche Punkte enthält. Der **PolyCurve.ByPoints**-Block referenziert diese Liste und erstellt nur eine Kurve. Da alle Punkte in derselben Liste enthalten sind, entsteht eine zickzackförmige Polykurve, die durch sämtliche Punkten aus der Liste verläuft.

Darüber hinaus stehen auch Optionen zum Vereinfachen isolierter Datenebenen zur Verfügung. Mithilfe des **List.Flatten**-Blocks können Sie festlegen, wie viele Datenebenen unterhalb der ersten Hierarchieebene vereinfacht werden sollen. Dies ist sehr hilfreich bei komplexen Datenstrukturen, die für Ihren Arbeitsablauf nicht unbedingt erforderlich sind. Eine weitere Möglichkeit besteht darin, den Flatten-Block als Funktion in **List.Map** einzusetzen. Weitere Informationen zu **List.Map** finden Sie weiter unten.

### Chop

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/Chop.dyn" %}

Bei der parametrischen Modellierung ist es zuweilen erforderlich, die Datenstruktur einer bestehenden Liste zu ändern. Hierfür stehen ebenfalls zahlreiche Blöcke zur Verfügung, wobei Chop die einfachste Version darstellt. Mit Chop können Sie eine Liste in Unterlisten mit der angegebenen Anzahl Elemente unterteilen.

Der Befehl Chop teilt Listen gemäß der angegebenen Listenlänge. In gewisser Weise stellt Chop das Gegenteil zu Flatten dar: Der Datenstruktur werden neue Ebenen hinzugefügt, anstatt dass vorhandene entfernt werden. Dieses Werkzeug ist hilfreich für geometrische Operationen wie im Beispiel unten gezeigt.

![Übung](../images/5-4/3/listsoflists-chop.jpg)

### List.Map

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/Map.dyn" %}

Mit **List.Map/Combine** wird eine angegebene Funktion auf die eingegebene Liste angewendet, allerdings geschieht dies eine Ebene tiefer in der Hierarchie. Kombinationen fungieren genau wie Maps. Allerdings sind für Kombinationen mehrere Eingaben entsprechend der Eingabe einer gegebenen Funktion möglich.

_Anmerkung: Diese Übung wurde mit einer früheren Version von Dynamo erstellt. Die Funktion_ **List.Map** _lässt sich großenteils durch die neu hinzugefügte Funktion_ **List@Level** _ersetzen. Weitere Informationen finden Sie _nachfolgend_ unter_ [_List@Level_](3-lists-of-lists.md#lists-of-lists).

Ziehen Sie als kurze Einführung den **List.Count**-Block aus dem vorigen Abschnitt heran.

Der **List.Count**-Block zählt alle Elemente in einer Liste. An diesem Beispiel wird die Funktionsweise von **List.Map** gezeigt.

![](../images/5-4/3/listsoflists-map01.jpg)

> 1.  Fügen Sie zwei Zeilen Code in den **Codeblock** ein: `-50..50..#Nx; -50..50..#Ny;`
>
>     Nach der Eingabe dieses Codes werden im Codeblock zwei Eingaben für Nx und Ny erstellt.
> 2. Definieren Sie mit zwei _Integer Slidern_ die Werte für _Nx_ und _Ny_, indem Sie die Schieberegler mit dem **Codeblock** verbinden.
> 3. Verbinden Sie jede Zeile des Codeblocks mit der entsprechenden _X_\- bzw. _Y_-Eingabe eines **Point.ByCoordinates**-Blocks. Klicken Sie mit der rechten Maustaste auf den Block und wählen Sie zuerst Vergitterung und dann _"Kreuzprodukt"_. Dadurch wird ein Raster aus Punkten erstellt. Da der Bereich zwischen -50 und 50 definiert wurde, umfasst er das vorgabemäßige Raster von Dynamo.
> 4. In einem _**Watch**_-Block werden die erstellten Punkte angezeigt. Beachten Sie die Datenstruktur. Es wurde eine Liste von Listen erstellt. Jede Liste entspricht einer Reihe von Punkten im Raster.

![Exercise](<../images/5-4/3/lists of lists - map 02.jpg>)

> 1. Verbinden Sie einen **List.Count**-Block mit der Ausgabe des Watch-Blocks aus dem vorigen Schritt.
> 2. Verbinden Sie einen **Watch**-Block mit der Ausgabe des **List.Count**-Blocks.

Der List.Count-Block gibt den Wert 5 aus. Dieser Wert ist gleich dem Wert der im Codeblock definierten Variablen Nx. Dies geschieht aus dem folgenden Grund:

* Im **Point.ByCoordinates**-Block wird die x-Eingabe als primäre Eingabe zum Erstellen von Listen verwendet. Für Nx = 5 und Ny = 3 ergibt sich daher eine Liste, die fünf Listen mit je 3 Elementen enthält.
* Da Listen in Dynamo als Objekte behandelt werden, wird der **List.Count**-Block auf die Hauptliste in der Hierarchie angewendet. Das Ergebnis ist der Wert 5, d. h. die Anzahl der Listen in der Hauptliste.

![Übung](../images/5-4/3/listsoflists-map03.jpg)

> 1. Mithilfe eines **List.Map**-Blocks bewegen Sie sich eine Ebene tiefer in die Hierarchie und führen dort eine _"Funktion"_ aus.
> 2. Beachten Sie, dass der **List.Count**-Block keine Eingabe hat. Er wird als Funktion eingesetzt, d. h., der **List.Count**-Block wird auf jede einzelne Liste eine Ebene unterhalb der Hauptliste in der Hierarchie angewendet. Die leere Eingabe von **List.Count** entspricht der Listeneingabe von **List.Map**.
> 3. Als Ergebnis von **List.Count** erhalten Sie jetzt eine Liste mit fünf Elementen, jeweils mit dem Wert 3. Dies entspricht der Länge jeder Unterliste.

### **List.Combine**

_Anmerkung: Diese Übung wurde mit einer früheren Version von Dynamo erstellt. Die Funktionalität von List.Combine lässt sich großenteils durch die neu hinzugefügte Funktion _**List@Level**_ ersetzen. Weitere Informationen finden Sie weiter _unten_ unter_ [_List@Level_](6-3\_lists-of-lists.md#listlevel).

In dieser Übung verwenden wir **List.Combine**, um zu zeigen, wie damit eine Funktion auf separate Listen von Objekten angewendet werden kann.

Beginnen Sie, indem Sie zwei Listen von Punkten einrichten.

![Übung](../images/5-4/3/listsoflists-combined01.jpg)

> 1. Verwenden Sie den **Sequence**-Block, um 10 Werte zu generieren, von denen jeder ein Inkrement von 10 Schritten aufweist.
> 2. Verbinden Sie das Ergebnis mit der x-Eingabe eines **Point.ByCoordinates**-Blocks. Dadurch wird eine Liste von Punkten in Dynamo erstellt.
> 3. Fügen Sie einen zweiten **Point.ByCoordinates**-Block zum Arbeitsbereich hinzu, verwenden Sie dieselbe **Sequence**-Ausgabe als x-Eingabe, verwenden Sie jedoch einen **Integer Slider** als y-Eingabe, und legen Sie den Wert auf 31 fest, sodass die 2 Punktsätze nicht überlappen. (Es kann ein beliebiger Wert sein, solange er nicht mit dem ersten Satz von Punkten überlappt).

Als Nächstes wenden Sie mit **List.Combine** eine Funktion auf Objekte in 2 separaten Listen an. In diesem Fall handelt es sich um eine einfache Funktion zum Zeichnen von Linien.

![Übung](../images/5-4/3/listsoflists-combined02.jpg)

> 1. Fügen Sie **List.Combine** zum Arbeitsbereich hinzu, und verbinden Sie die 2 Punktsätze als list0- und list1-Eingabe.
> 2. Verwenden Sie **Line.ByStartPointEndPoint** als Eingabefunktion für **List.Combine**.

Nach Abschluss werden die 2 Punktgruppen mithilfe einer **Line.ByStartPointEndPoint**-Funktion komprimiert/gekoppelt, sodass in Dynamo 10 Zeilen zurückgegeben werden.

{% hint style="info" %} In der Übung zu n-dimensionalen Listen finden Sie ein weiteres Beispiel für die Verwendung von List.Combine. {% endhint %}

### List@Level

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/Listatlevel.dyn" %}

Die Funktion **List@Level** kann als Alternative zu **List.Map** eingesetzt werden und ermöglicht es, die Listenebene, mit der Sie arbeiten möchten, direkt am Eingabeanschluss auszuwählen. Diese Funktion kann für jeden Eingabeanschluss eines Blocks angewendet werden und ermöglicht einen schnelleren und einfacheren Zugriff auf die Ebenen in Listen als andere Methoden. Geben Sie im Block einfach an, welche Ebene der Liste Sie als Eingabe verwenden möchten, und überlassen Sie die Ausführung dem Block.

In dieser Übung isolieren Sie mithilfe der Funktion **List@Level** eine bestimmte Datenebene.

![List@Level](../images/5-4/3/listsoflists-listatlevel01.jpg)

Sie beginnen mit einem einfachen 3D-Raster von Punkten.

> 1. Das Raster wurde mit Bereichen für X, Y und Z konstruiert, und es ist bekannt, dass drei Datenstufen vorhanden ist, jeweils eine Liste für X, Y und Z.
> 2. Diese Stufen entsprechen verschiedenen **Ebenen**. Die Ebenen werden im unteren Bereich des Vorschaufensters angezeigt. Die Spalten für die Listenebenen entsprechen den oben angegebenen Listendaten und erleichtern es, die Ebene zu finden, auf der Sie arbeiten möchten.
> 3. Die Ebenen sind in umgekehrter Reihenfolge geordnet, d. h., die unterste Datenebene ist immer "L1". Dadurch wird sichergestellt, dass Ihre Diagramme wie geplant funktionieren, auch wenn vorangehende Schritte geändert werden.

![List@Level](../images/5-4/3/listsoflists-listatlevel02.jpg)

> 1. Um die Funktion **List@Level** zu verwenden, klicken Sie auf ">". In diesem Menü werden zwei Kontrollkästchen angezeigt.
> 2. **Ebenen verwenden**: Aktiviert die Funktion **List@Level**. Nach dem Klicken auf diese Option können Sie durch die Eingabelistenebenen navigieren und die Ebene auswählen, die der Block verwenden soll. Über dieses Menü können Sie durch Navigieren nach oben oder unten rasch Optionen für verschiedene Ebenen testen.
> 3. _Listenstruktur beibehalten_: Ist dies aktiviert, haben Sie die Möglichkeit, die Ebenenstruktur der Eingabe beizubehalten. In manchen Fällen haben Sie eventuell Ihre Daten absichtlich in Unterlisten geordnet. Indem Sie diese Option aktivieren, können Sie Ihre Listenstruktur beibehalten, sodass keine Informationen verloren gehen.

In diesem einfachen 3D-Raster können Sie auf die Listenstruktur zugreifen und sie visualisieren, indem Sie durch die Listenebenen blättern. Mit jeder Kombination aus Listenebene und Index erhalten Sie eine andere Untergruppe von Punkten aus der ursprünglichen 3D-Punktmenge.

![](../images/5-4/3/listsoflists-listatlevel03.jpg)

> 1. Mit der Angabe "@L2" in DesignScript wählen Sie ausschließlich die Liste auf Ebene 2 aus. Die Liste auf Ebene 2 mit dem Index 0 enthält nur die Gruppe von Punkten mit dem ersten Y-Wert, d. h. nur das XZ-Raster.
> 2. Wenn Sie den Ebenenfilter in "L1" ändern, sind sämtliche Daten auf der ersten Listenebene sichtbar. Die Liste auf Ebene 1 mit dem Index 0 enthält alle 3D-Punkte in einer unstrukturierten Liste.
> 3. Mit "L3" werden nur die Punkte auf der dritten Listenebene angezeigt. Die Liste auf Ebene 3 mit dem Index 0 enthält nur die Gruppe von Punkten mit dem ersten Z-Wert, d. h. nur das XY-Raster.
> 4. Mit "L4" werden nur die Punkte auf der dritten Listenebene angezeigt. Die Liste auf Ebene 4 mit dem Index 0 enthält nur die Gruppe von Punkten mit dem ersten X-Wert, d. h. nur das YZ-Raster.

Obwohl dieses Beispiel auch mithilfe von **List.Map** erstellt werden kann, bietet **List@Level** eine wesentlich einfachere Interaktion und erleichtert dadurch den Zugriff auf die Daten des Blocks. Unten sehen Sie die Methoden **List.Map** und **List@Level** im Vergleich:

![](../images/5-4/3/listsoflists-listatlevel04.jpg)

> 1. Beide Methoden ermöglichen den Zugriff auf dieselben Punkte, mit **List@Level** ist jedoch der mühelose Wechsel zwischen Datenlayern innerhalb desselben Blocks möglich.
> 2. Für den Zugriff auf ein Punktraster mithilfe von **List.Map** benötigen Sie außer dem eigentlichen **List.Map**-Block einen **List.GetItemAtIndex**-Block. Für jeden Schritt auf eine tiefere Listenebene benötigen Sie einen weiteren **List.Map**-Block. Dies könnte je nach Komplexität Ihrer Listen bedeuten, dass Sie Ihrem Diagramm eine größere Anzahl von **List.Map**-Blöcken hinzufügen müssen, um die richtige Informationsebene zu erreichen.
> 3. In diesem Beispiel gibt der **List.GetItemAtIndex**-Block zusammen mit dem **List.Map**-Block dieselbe Punktgruppe mit derselben Listenstruktur zurück wie **List.GetItemAtIndex** mit der Auswahl "@L3".

### Transpose

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/Transpose.dyn" %}

Transpose gehört zu den Grundfunktionen bei der Arbeit mit Listen von Listen. Mit Transpose werden genau wie in Tabellenkalkulationsprogrammen die Spalten und Zeilen einer Datenstruktur vertauscht. Dies wird in einer einfachen Matrix unten gezeigt. Im darauf folgenden Abschnitt wird beschrieben, wie Sie mithilfe von Transpose geometrische Beziehungen erstellen können.

![Transpose](../images/5-4/3/transpose1.jpg)

Löschen Sie die **List.Count**-Blöcke aus der vorhergegangenen Übungslektion, sodass Sie mit Geometrie arbeiten und die Struktur der Daten sehen können.

![](../images/5-4/3/listsoflists-transpose01.jpg)

> 1. Verbinden Sie einen **PolyCurve.ByPoints**-Block mit der Ausgabe des Beobachtungsblocks für **Point.ByCoordinates**.
> 2. Die Ausgabe zeigt 5 Polykurven, die auch in der Dynamo-Vorschau angezeigt werden. Der Dynamo-Block sucht nach einer Liste von Punkten (bzw. in diesem Fall nach einer Liste von Listen von Punkten) und erstellt jeweils eine Polykurve. Dies bedeutet, dass jede Liste in eine Kurve in der Datenstruktur umgewandelt wurde.

![](../images/5-4/3/listsoflists-transpose02.jpg)

> 1. Ein **List.Transpose**-Block vertauscht sämtliche Elemente mit allen Listen in einer Liste von Listen. Dies wirkt kompliziert, folgt aber derselben Logik wie die Funktion Transponieren in Microsoft Excel: Spalten und Zeilen in einer Datenstruktur werden vertauscht.
> 2. Im abstrakten Ergebnis ist zu sehen, dass Transpose die Listenstruktur aus 5 Listen mit je 3 Elementen in 3 Listen mit je 5 Elementen umgewandelt hat.
> 3. Das geometrische Ergebnis aus **PolyCurve.ByPoints** zeigt 3 Polykurven, die rechtwinklig zu den Originalkurven liegen.

## Codeblock für Listenerstellung

In der Codeblock-Notation wird "[]" zum Definieren einer Liste verwendet. Mit diesem Verfahren können Sie Listen wesentlich schneller und flexibler erstellen als mithilfe des **List.Create**-Blocks. **Codeblöcke** werden ausführlich in [Codeblöcke und DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/) behandelt. Die folgende Abbildung zeigt, wie Sie eine Liste mit mehreren Ausdrücken in einem Codeblock definieren können.

![](../images/5-4/3/listsoflists-codeblockforlistcreation01.jpg)

#### Codeblock-Abfrage

Die **Codeblock**-Notation verwendet eckige Klammern ("[]") als schnelle und einfache Möglichkeit zur Auswahl bestimmter Elemente, die aus einer komplexen Datenstruktur abgerufen werden sollen. **Codeblöcke** werden im Kapitel [Codeblöcke und DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/) ausführlicher behandelt. Die folgende Abbildung zeigt, wie Sie mithilfe von Codeblöcken eine Liste mit mehreren Datentypen abfragen können.

![](../images/5-4/3/listsoflists-codeblockforlistcreation02.jpg)

## Übungslektion: Abfragen und Einfügen von Daten

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/3/ReplaceItems.dyn" %}

In dieser Übungslektion wird ein Teil der in der vorigen Lektion erstellten Logik zum Bearbeiten einer Oberfläche verwendet. Die Navigation in der Datenstruktur ist etwas komplexer, obwohl das angestrebte Ergebnis intuitiv wirkt. Die Oberfläche soll durch Verschieben eines Steuerpunkts strukturiert werden.

Beginnen Sie mit der oben gezeigten Folge von Blöcken. Dadurch wird eine einfache, das vorgegebene Dynamo-Raster umfassende Oberfläche erstellt.

![](../images/5-4/3/listoflists-exercisecbinsert\&query01.jpg)

> 1. Fügen Sie mithilfe eines **Codeblocks** diese beiden Codezeilen ein und verbinden Sie sie mit den _u_\- und _v_-Eingaben von **Surface.PointAtParameter**: `-50..50..#3;` `-50..50..#5;`
> 2. Achten Sie darauf, als Vergitterung von **Surface.PointAtParameter** die Option _Kreuzprodukt_ zu wählen.
> 3. Im **Watch**-Block wird eine Liste aus drei Listen mit je fünf Elementen angezeigt.

In diesem Schritt fragen Sie den Mittelpunkt des eben erstellten Rasters ab. Dazu wird die mittlere Zeile der mittleren Liste ausgewählt, was irgendwie Sinn ergibt, nicht wahr?

![](../images/5-4/3/listoflists-exercisecbinsert\&query02.jpg)

> 1. Sie können auch nacheinander auf die Elemente im Beobachtungsblock klicken, um sicherzustellen, dass dies der richtige Punkt ist.
> 2. Schreiben Sie im **Codeblock** eine einfache Codezeile zum Abfragen einer Liste von Listen:\
 `points[1][2];`
> 3. Verschieben Sie mithilfe von **Geometry.Translate** den ausgewählten Punkt in _Z_-Richtung um _20_ Einheiten nach oben.

![](../images/5-4/3/listoflists-exercisecbinsert\&query03.jpg)

> 1. Wählen Sie mithilfe eines **List.GetItemAtIndex**-Blocks auch die mittlere Reihe von Punkten aus. Anmerkung: Ähnlich wie im vorigen Schritt können Sie die Liste auch mithilfe eines **Codeblocks** abfragen, wobei Sie die Zeile `points[1];` eingeben.

Damit haben Sie den Mittelpunkt abgefragt und ihn nach oben verschoben. Als Nächstes müssen Sie den verschobenen Punkt in die ursprüngliche Datenstruktur einfügen.

![](../images/5-4/3/listoflists-exercisecbinsert\&query04.jpg)

> 1. Dazu ersetzen Sie zuerst das Listenelement, das Sie zuvor isoliert haben.
> 2. Mit **List.ReplaceItemAtIndex** ersetzen Sie das mittlere Element mit dem Index _"2"_ durch das Ersatzelement, das mit dem verschobenen Punkt (**Geometry.Translate**) verbunden ist.
> 3. Die Ausgabe zeigt, dass der verschobene Punkt in das mittlere Element der Liste eingegeben wurde.

Jetzt müssen Sie die geänderte Liste in die ursprüngliche Datenstruktur, d. h. die Liste von Listen, einfügen.

![](../images/5-4/3/listoflists-exercisecbinsert\&query05.jpg)

> 1. Ersetzen Sie nach demselben Prinzip die mittlere Liste mithilfe von **List.ReplaceItemAtIndex** durch die geänderte Liste.
> 2. Beachten Sie, dass in den **Codeblöck**_en_, die die Indizes für diese beiden Blöcke definieren, die Werte 1 und 2 angegeben wurde, was der ursprünglichen Abfrage aus dem **Codeblock** (_points[1][2]_) entspricht.
> 3. Wenn Sie die Liste mit _index 1_ auswählen, wird die Datenstruktur in der Dynamo-Vorschau hervorgehoben. Damit haben Sie den verschobenen Punkt in die ursprüngliche Datenstruktur aufgenommen.

Es gibt viele Möglichkeiten zum Erstellen einer Oberfläche aus dieser Punktgruppe. In diesem Fall erstellen Sie die Oberfläche durch Erheben und Verbinden von Kurven.

![](../images/5-4/3/listoflists-exercisecbinsert\&query06.jpg)

> 1. Erstellen Sie einen **NurbsCurve.ByPoints**-Block und verbinden Sie ihn mit der neuen Datenstruktur, um drei Nurbs-Kurven zu erstellen.

![](../images/5-4/3/listoflists-exercisecbinsert\&query07.jpg)

> 1. Verbinden Sie einen **Surface.ByLoft**-Block mit der Ausgabe von **NurbsCurve.ByPoints**. Damit haben Sie eine geänderte Oberfläche erstellt. Sie können den ursprünglichen _Z_-Wert der Geometrie ändern. Beobachten Sie, wie die Geometrie durch diese Verschiebung aktualisiert wird.
