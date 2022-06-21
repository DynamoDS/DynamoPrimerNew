# n-dimensionale Listen

Sie können der Hierarchie weitere untergeordnete Ebenen hinzufügen. Datenstrukturen können Listen von Listen mit weit mehr als zwei Dimensionen umfassen. Da Listen in Dynamo als Objekte behandelt werden, können Sie Daten mit so vielen Dimensionen erstellen, wie es möglich ist.

Als Metapher für diese Funktionen eignen sich ineinander verschachtelte russische Matrjoschka-Puppen. Jede Liste kann ein Container betrachtet werden, der mehrere Elemente enthält. Jede Liste verfügt über eigene Eigenschaften und wird als eigenständiges Objekt angesehen.

![Dolls](../images/5-4/4/145493363\_fc9ff5164f\_o.jpg)

> Die verschachtelten Matrjoschka-Puppen (Foto: [Zeta](https://www.flickr.com/photos/beppezizzi/145493363)) sind eine Metapher für n-dimensionale Listen. Jede Ebene steht für eine Liste und jede Liste enthält Elemente. In Dynamo kann jeder Container seinerseits mehrere Container enthalten (die den Elementen der jeweiligen Liste entsprechen).

Die visuelle Veranschaulichung n-dimensionaler Listen ist schwierig. In diesem Kapitel finden Sie jedoch eine Reihe von Übungslektionen für die Arbeit mit Listen, die mehr als zwei Dimensionen enthalten.

### Mapping und Kombinationen

Mapping ist der wohl komplexeste Aspekt der Datenverwaltung in Dynamo. Bei der Arbeit mit komplexen Listenhierarchien kommt ihm besondere Bedeutung zu. In den folgenden Übungslektionen wird gezeigt, wann Mapping und Kombinationen für mehrdimensionale Daten eingesetzt werden sollten.

Eine vorbereitende Einführung zu **List.Map** und **List.Combine** finden Sie im vorigen Abschnitt. In der letzten Übungslektion werden diese Blöcke für eine komplexe Datenstruktur angewendet.

## Übungslektion: 2D-Listen – Grundlagen

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-4/4/n-Dimensional-Lists.zip" %}

Dies ist die erste von drei Übungslektionen zur Strukturierung importierter Geometrie. In den Teilen dieser Übungsreihe werden nach und nach komplexere Datenstrukturen verwendet.

![Exercise](<../images/5-4/4/n-dimensional lists - 2d lists basic 01.jpg>)

> 1. Sie beginnen mit der SAT-Datei aus dem Ordner mit den Übungsdateien. Diese Datei können Sie mithilfe des **File Path**-Blocks abrufen.
> 2. Mit **Geometry.ImportFromSAT** wird die Geometrie in Form zweier Oberflächen in die Dynamo-Vorschau importiert.

In dieser Übung wird der Einfachheit halber nur eine der Oberflächen verwendet.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 02.jpg>)

> 1. Wählen Sie den Index 1, um die obere Oberfläche auszuwählen. Verwenden Sie dazu den **List.GetItemAtIndex**-Block.
> 2. Deaktivieren Sie die Geometrievorschau für **Geometry.ImportFromSAT**.

Im nächsten Schritt unterteilen Sie die Oberfläche mithilfe eines Rasters aus Punkten.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 03.jpg>)

> 1\. Fügen Sie in einem **Codeblock** die beiden folgenden Codezeilen ein: `0..1..#10;` `0..1..#5;`
>
> 2\. Verbinden Sie in **Surface.PointAtParameter** die beiden Codeblock-Werte mit u und _v_. Ändern Sie die _Vergitterung_ dieses Knotens in _"Kreuzprodukt "_.
>
> 3\. In der Ausgabe und in der Dynamo-Vorschau wird die Datenstruktur angezeigt.

Als Nächstes verwenden Sie die Punkte aus dem letzten Schritt, um zehn Kurven entlang der Oberfläche zu erstellen.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 04.jpg>)

> 1. Um den Aufbau der Datenstruktur zu verdeutlichen, verbinden Sie einen **NurbsCurve.ByPoints**-Block mit der Ausgabe von **Surface.PointAtParameter**.
> 2. Sie können die Vorschau des **List.GetItemAtIndex**-Blocks für ein besser sichtbares Ergebnis deaktivieren.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 05.jpg>)

> 1. Mit einer einfachen **List.Transpose**-Operation vertauschen Sie die Spalten und Zeilen einer Liste von Listen.
> 2. Indem Sie die Ausgabe des **List.Transpose**-Blocks mit einem **NurbsCurve.ByPoints**-Blocks verbinden, erhalten Sie fünf Kurven, die horizontal auf der Oberfläche verlaufen.
> 3. Sie können die Vorschau des **NurbsCurve.ByPoints**-Blocks im vorherigen Schritt deaktivieren, um das gleiche Ergebnis im Bild zu erzielen.

## Übungslektion: 2D-Listen – Weiterführend

In diesem Schritt erhöhen Sie die Komplexität. Angenommen, an den Kurven aus der vorigen Übungslektion soll eine Operation durchgeführt werden. Vielleicht sollen die Kurven auf eine andere Oberfläche bezogen und eine Erhebung zwischen ihnen erstellt werden. Die hierfür erforderliche Datenstruktur erfordert mehr Aufwand, wobei jedoch dieselbe Logik zugrunde liegt.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 01.jpg>)

> 1. Beginnen Sie mit demselben Schritt wie in der vorherigen Übung, indem Sie die obere der beiden Oberflächen der importierten Geometrie mithilfe des **List.GetItemAtIndex**-Blocks isolieren.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 02.jpg>)

> 1. Versetzen Sie die Oberfläche mithilfe von **Surface.Offset** um den Wert _10_.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 03.jpg>)

> 1. Definieren Sie auf dieselbe Weise wie in der vorigen Übung einen _Codeblock_ mit den folgenden beiden Codezeilen: `0..1..#10;` `0..1..#5;`
> 2. Verbinden Sie diese Ausgaben mit zwei **Surface.PointAtParameter**-Blöcken, jeweils mit der _Vergitterung_ _"Kreuzprodukt"_. Verbinden Sie einen dieser Blöcke mit der ursprünglichen und den anderen mit der versetzten Oberfläche.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 04.jpg>)

> 1. Deaktivieren Sie die Vorschau dieser Oberflächen.
> 2. Verbinden Sie wie in der vorigen Übungslektion die Ausgaben mit zwei **NurbsCurve.ByPoints**-Blöcken. Das Ergebnis zeigt Kurven, die zwei Oberflächen entsprechen.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 05.jpg>)

> 1. Mithilfe von **List.Create** können Sie die beiden Kurvengruppen in einer Liste von Listen kombinieren.
> 2. Die Ausgabe zeigt zwei Listen mit je zehn Elementen für die beiden Gruppen verbundener Nurbs-Kurven.
> 3. Mithilfe von **Surface.ByLoft** können Sie diese Datenstruktur visuell verdeutlichen. Mithilfe dieses Blocks verbinden Sie sämtliche Kurven in jeder der beiden Unterlisten durch eine Erhebung.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 06.jpg>)

> 1. Deaktivieren Sie die Vorschau des **Surface.ByLoft**-Blocks aus dem vorherigen Schritt.
> 2. Mithilfe von **List.Transpose** werden die Spalten und Zeilen vertauscht. Dadurch werden zwei Listen mit je zehn Kurven in zehn Listen mit je zwei Kurven umgewandelt. Damit haben Sie für jede Nurbs-Kurve eine Beziehung zu ihrer Nachbarkurve auf der anderen Oberfläche erstellt.
> 3. Mit **Surface.ByLoft** erhalten Sie eine gerippte Struktur.

Als Nächstes werden wir einen alternativen Prozess vorstellen, um dieses Ergebnis zu erzielen.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 07.jpg>)

> 1. Bevor wir beginnen, deaktivieren Sie die Vorschau für **Surface.ByLoft** aus dem vorherigen Schritt, um Verwirrung zu vermeiden.
> 2. Als Alternative zu **List.Transpose** können Sie **List.Combine** verwenden. Damit wird ein _"Kombinator"_ für die beiden Unterlisten ausgeführt.
> 3. In diesem Fall wird **List.Create** als _"Kombinator"_ verwendet, um eine Liste der Elemente innerhalb der Unterlisten zu erstellen.
> 4. Mithilfe des **Surface.ByLoft**-Blocks erhalten Sie dieselben Oberflächen wie im vorigen Schritt. Transpose ist in diesem Falle einfacher zu verwenden, bei komplexeren Datenstrukturen erweist sich **List.Combine** jedoch als zuverlässiger.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 08.jpg>)

> 1. Sie können in einem der vorigen Schritte die Richtung der Kurven in der gerippten Struktur ändern, indem Sie **List.Transpose** vor der Verbindung zu **NurbsCurve.ByPoints** einfügen. Dadurch werden Spalten und Zeilen vertauscht und Sie erhalten 5 horizontale Rippen.

## Übungslektion: 3D-Listen

In diesem Abschnitt gehen Sie einen Schritt weiter. Sie arbeiten in dieser Übungslektion mit beiden importierten Oberflächen und erstellen eine komplexe Datenhierarchie. Dabei soll dieselbe Operation nach wie vor unter Verwendung derselben zugrunde liegenden Logik durchgeführt werden.

Beginnen Sie mit der Datei aus der vorherigen Übungslektion.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 01.jpg>)

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 02.jpg>)

> 1. Verwenden Sie wie in der vorigen Übungslektion den **Surface.Offset**-Block, um einen Versatz mit dem Wert _10_ zu erstellen.
> 2. Die Ausgabe zeigt, dass Sie mithilfe des Versatzblocks zwei Oberflächen erstellt haben.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 03.jpg>)

> 1. Definieren Sie auf dieselbe Weise wie in der vorigen Übung einen **Codeblock** mit den folgenden beiden Codezeilen: `0..1..#20;` `0..1..#20;`
> 2. Verbinden Sie diese Ausgaben mit zwei **Surface.PointAtParameter**-Blöcken, jeweils mit der Vergitterung _"Kreuzprodukt"_. Verbinden Sie einen dieser Blöcke mit den ursprünglichen und den anderen mit den versetzten Oberflächen.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 04.jpg>)

> 1. Verbinden Sie wie in der vorigen Übungslektion die Ausgaben mit zwei **NurbsCurve.ByPoints**-Blöcken.
> 2. Die Ausgabe der **NurbsCurve.ByPoints**-Blöcke ist eine Liste aus zwei Listen mit komplexerer Struktur als bei denjenigen in der vorigen Übungslektion. Die Daten werden durch die zugrunde liegende Oberfläche kategorisiert: Damit haben Sie der Datenstruktur eine weitere Ebene hinzugefügt.
> 3. Im **Surface.PointAtParameter**-Block ist eine komplexere Struktur zu erkennen: Sie haben eine Liste aus Listen von Listen erstellt.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 05.jpg>)

> 1. Bevor wir fortfahren, deaktivieren Sie die Vorschau der vorhandenen Oberflächen.
> 2. Führen Sie mithilfe des **List.Create**-Blocks die Nurbs-Kurven zu ein und derselben Datenstruktur zusammen, wobei eine Liste aus Listen von Listen entsteht.
> 3. Durch Verbinden eines **Surface.ByLoft**-Blocks erhalten Sie eine Version der ursprünglichen Oberflächen, die in ihrer eigenen Liste wie aus der ursprünglichen Datenstruktur erstellt erhalten bleiben.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 06.jpg>)

> 1. In der vorigen Übung konnten Sie mithilfe von **List.Transpose** eine gerippte Struktur erstellen. Dies ist hier nicht möglich. Die Funktion Transpose ist für zweidimensionale Listen vorgesehen. Hier liegt eine dreidimensionale Liste vor, die ein "Vertauschen von Spalten und Zeilen" nicht ohne Weiteres zulässt. Da Listen Objekte sind, können mit **List.Transpose** zwar die Listen mit den Unterlisten vertauscht werden, die Nurbs-Kurven, die sich eine Ebene tiefer in der Hierarchie befinden, bleiben dabei jedoch unverändert.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 07.jpg>)

> 1. In diesem Fall ist **List.Combine** besser geeignet. Für komplexere Datenstrukturen sollten **List.Map**- und **List.Combine**-Blöcke zum Einsatz kommen.
> 2. Mit **List.Create** als _"Kombinator"_ erhalten Sie eine Datenstruktur, die in diesem Fall besser verwendbar ist.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 08.jpg>)

> 1. Die Datenstruktur muss auf der nächsttieferen Ebene der Hierarchie nach wie vor vertauscht werden. Verwenden Sie hierfür **List.Map**. Dies funktioniert auf dieselbe Weise wie **List.Combine**, wobei jedoch nur eine Liste anstelle mehrerer eingegeben wird.
> 2. Als Funktion für **List.Map** wird **List.Transpose** eingegeben, wodurch die Spalten und Zeilen der Unterlisten innerhalb der Hauptliste vertauscht werden.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 09.jpg>)

> 1. Schließlich können Sie die Nurbs-Kurven in der vorgesehenen Datenhierarchie durch eine Erhebung miteinander verbinden und erhalten eine gerippte Struktur.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 10.jpg>)

> 1. Fügen Sie der Geometrie mithilfe eines **Surface.Thicken**-Blocks mit den angezeigten Eingabeeinstellungen Tiefe hinzu.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 11.jpg>)

> 1. Es ist vorteilhaft, eine Oberfläche zur Unterstützung dieser Struktur hinzuzufügen. Fügen Sie daher einen weiteren **Surface.ByLoft**-Block hinzu und verwenden Sie die erste Ausgabe von **NurbsCurve.ByPoints** aus einem früheren Schritt als Eingabe.
> 2. Da die Vorschau unübersichtlich wird, deaktivieren Sie die Vorschau für diese Blöcke, indem Sie mit der rechten Maustaste auf jeden Block klicken und die Option Vorschau deaktivieren, um das Ergebnis besser sehen zu können.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 12.jpg>)

> 1. Mit der Verstärkung dieser ausgewählten Oberflächen ist die Untergliederung vollständig.

Das Ergebnis ist vielleicht kein sonderlich bequemer Schaukelstuhl, aber er enthält erhebliche Datenmengen.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 13.jpg>)

Im letzten Schritt kehren Sie die Richtung der Rippen um. Bei diesem Vorgang wird in ähnlicher Weise wie in der vorigen Übungslektion die Funktion Transpose verwendet.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 14.jpg>)

> 1. Da in der Hierarchie eine weitere Ebene vorhanden ist, müssen Sie **List.Map** zusammen mit **List.Transpose** als Funktion verwenden, um die Richtung der Nurbs-Kurven zu ändern.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 15.jpg>)

> 1. Es ist eventuell sinnvoll, die Anzahl der Rippen zu erhöhen. Ändern Sie daher den **Codeblock** wie folgt: `0..1..#20;` `0..1..#30;`

Während die erste Version des Schaukelstuhls eher elegant wirkte, punktet das zweite Modell durch robuste, sportliche Qualitäten.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 16.jpg>)
