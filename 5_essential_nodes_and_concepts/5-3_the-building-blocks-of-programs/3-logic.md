# Logik

**Logik**, genauer **Bedingungslogik** ermöglicht die Angabe einer Aktion oder einer Gruppe von Aktionen unter Verwendung einer Prüfung. Die Auswertung der Prüfung ergibt einen booleschen Wert für `True` oder `False`, der zur Steuerung des Programmablaufs verwendet werden kann.

### Boolesche Werte

Numerische Variablen können für eine Vielzahl unterschiedlicher Zahlen stehen. Boolesche Variablen hingegen können nur zwei Werte annehmen, die als True oder False, Ja oder Nein, 1 oder 0 wiedergegeben werden. Booleschen Operationen werden wegen ihrer begrenzten Möglichkeiten nur selten zum Durchführen von Berechnungen verwendet.

### Bedingungsanweisungen

Die "If"-Anweisung ist ein wichtiges Konzept in der Programmierung: "Wenn _dies_ zutrifft (True), dann geschieht _das_, andernfalls geschieht _etwas anderes_. Die aus der Anweisung resultierende Aktion wird durch einen booleschen Wert gesteuert. In Dynamo stehen mehrere Möglichkeiten zum Definieren von If-Anweisungen zur Verfügung:

| Symbol | Name (Syntax) | Eingaben | Ausgaben |
| ----------------------------------------------- | ------------------------- | ----------------- | ------- |
| ![](<../images/5-3/3/If.jpg>) | Wenn (**If**) | test, true, false | result |
| ![](../images/5-3/3/Formula.jpg) | Formel (**IF(x,y,z)**) | x, y, z | result |
| ![](<../images/5-3/3/Code Block.jpg>) | Codeblock (**(x?y:z);**) | x? y, z | result |

Die folgenden kurzen Beispiele zeigen die Funktionsweise dieser drei Blöcke in der If-Bedingungsanweisung.

In dieser Abbildung wurde im _Boolean_ die Option _True_, eingestellt, d. h., das Ergebnis ist die Zeichenfolge: _"this is the result if true"._ Die drei möglichen Blöcke, mit deren Hilfe die _If_-Anweisung erstellt werden kann, funktionieren hier auf dieselbe Weise.

![](<../images/5-3/3/logic - conditional statements 01 false.jpg>)

Auch in diesem Fall funktionieren die Blöcke auf dieselbe Weise. Wenn Sie den _Boolean_-Wert in _False_ ändern, wird als Ergebnis die Zahl _Pi_ ausgegeben, wie in der ursprünglichen _If_-Anweisung festgelegt.

![x](<../images/5-3/3/logic - conditional statements 02 true.jpg>)

## Übung: Logik und Geometrie

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-3/3/Building Blocks of Programs - Logic.dyn" %}

### Teil I: Filtern einer Liste

1. In diesem Beispiel teilen Sie eine Liste von Zahlen in eine Liste mit geraden und eine Liste ungeraden Zahlen auf.

![](<../images/5-3/3/logic - exercise part I-01.jpg>)

> a. **Number Range**: Fügt dem Ansichtsbereich einen Zahlenbereich hinzu.
>
> b. **Numbers**: Fügt dem im Ansichtsbereich drei Number-Blöcke hinzu. Legen Sie in diesen Blöcken die folgenden Werte fest: _0.0_ für _start_, _10.0_ für _end_ und _1.0_ für _step_.
>
> c. **Output**: Die Ausgabe zeigt eine Liste mit 11 Zahlen von 0-10.
>
> d. **Modulo (%)**: Verbindet **Number Range** mit _x_ und _2.0_ mit _y_. Mit dieser Funktion wird für jede Zahl aus der Liste der Rest bei Division durch 2 berechnet. Die Ausgabe für diese Liste ist eine Liste, die abwechselnd die Werte 0 und 1 enthält.
>
> e. **Equality Test (==)**: Fügt dem Ansichtsbereich einen Block für die Gleichheitsprüfung hinzu. Verbinden Sie die Ausgabe des _Modulus_-Blocks mit der _x_-Eingabe und _0.0_ mit der _y_-Eingabe.
>
> f. **Watch**: Die Gleichheitsprüfung gibt eine Liste aus, die abwechselnd die Werte true und false enthält. Mithilfe dieser Werte werden die Einträge aus der Zahlenliste eingeordnet. Dabei steht _0_ (bzw. _true_) für gerade und (_1_ bzw. _false_) für ungerade Zahlen.
>
> g. **List.FilterByBoolMask**: Dieser Block filtert die Werte anhand der eingegebenen Booleschen Operation in zwei Listen. Verbinden Sie den ursprünglichen _number range_ mit der _list_-Eingabe und die Ausgabe der _Gleichheitsprüfung_ mit der _mask_-Eingabe. Die _in_-Ausgabe enthält die true-Werte, die _out_-Ausgabe die false-Werte.
>
> h. **Watch**: Als Ergebnis erhalten wir eine Liste mit geraden und eine Liste mit ungeraden Zahlen. Damit haben Sie mithilfe logischer Operatoren Listen anhand eines Musters aufgeteilt.

### Teil II: Von der Logik zur Geometrie

In diesem Schritt wenden Sie die in der ersten Übung erstellte Logik auf einen Modellierungsvorgang an.

2\. Beginnen Sie mit den Blöcken aus der letzten Übung. Die einzigen Ausnahmen (zusätzlich zum Ändern des Formats) sind:

![](<../images/5-3/3/logic - exercise part II-01.jpg>)

> a. Verwenden Sie einen **Sequence**-Block mit diesen Eingabewerten.
>
> b. Die in-Listeneingabe für **List.FilterByBoolMask** wurde entfernt. Diese Blöcke werden momentan nicht benötigt, kommen aber weiter unten in dieser Übung zum Einsatz.

3\. Erstellen Sie zunächst eine separate Gruppe von Diagrammen, wie in der Abbildung oben gezeigt. Diese Gruppe von Blöcken stellt eine parametrische Gleichung zum Definieren einer Sinuskurve dar. Einige Hinweise:

![](<../images/5-3/3/logic - exercise part II-02.jpg>)

> a. Der erste **Number Slider**-Block repräsentiert die Frequenz der Welle. Er sollte einen Mindestwert von 1, einen Höchstwert von 4 und eine Schrittweite von 0.01 haben.
>
> b. Der erste **Number Slider**-Block repräsentiert die Amplitude der Welle. Er sollte einen Mindestwert von 0, einen Höchstwert von 1 und eine Schrittweite von 0.01 haben.
>
> c. **PolyCurve.ByPoints**: Indem Sie das oben gezeigte Blockdiagramm kopieren, erhalten im Ansichtsfenster der Dynamo-Vorschau eine Sinuskurve.

Für die Eingaben gilt hier folgende Regel: Verwenden Sie Zahlenblöcke für statische und Schieberegler für veränderliche Eigenschaften. Der anfangs definierte ursprüngliche Zahlenbereich soll erhalten bleiben. Für die Sinuskurve, die hier erstellt werden soll, wird jedoch mehr Flexibilität benötigt. Mithilfe dieser Schieberegler können Sie die Frequenz und Amplitude der Kurve ändern.

![](<../images/5-3/3/logic - exercise part II-03.gif>)

4\. Die Schritte dieser Definition werden hier nicht nacheinander beschrieben. Hier wird zunächst das Endergebnis gezeigt, um eine Vorstellung der fertigen Geometrie zu vermitteln. Die ersten beiden Schritte wurden separat durchgeführt und müssen jetzt zusammengeführt werden. Die Position der reißverschlussähnlichen Bauteile soll durch die zugrunde liegende Sinuskurve gesteuert werden, wobei mithilfe der True/False-Logik abwechselnd große und kleine Quader eingefügt werden.

![](<../images/5-3/3/logic - exercise part II-04.jpg>)

> a. **Math.RemapRange**: Erstellen Sie aus der in Schritt 02 erstellten Zahlenfolge eine neue Zahlenfolge, indem Sie den Bereich neu zuordnen. In Schritt 01 wurden Zahlen von 0 – 100 festgelegt. Diese Zahlen liegen zwischen 0 und 1, wie mithilfe der Eingaben _newMin_ und _newMax_ festgelegt.

5\. Erstellen Sie einen **Curve.PointAtParameter**-Block, und verbinden Sie dann die **Math.RemapRange**-Ausgabe aus Schritt 04 als _param_-Eingabe.

![](<../images/5-3/3/logic - exercise part II-05.jpg>)

Mithilfe dieses Schritts erstellen Sie Punkte entlang der Kurve. Die Zahlen mussten dem Bereich 0 bis 1 neu  zugeordnet werden, da als Eingabe für _param_ Werte in diesem Bereich verlangt werden. Der Wert _0_ steht für den Startpunkt, der Wert _1_ für die Endpunkte. Die Auswertung aller dazwischen liegenden Zahlen ergibt Werte im Bereich _\[0,1]_.

6\. Verbinden Sie die Ausgabe von **Curve.PointAtParameter** mit **List.FilterByBoolMask**, um die Liste der ungeraden und geraden Indizes zu trennen.

![](<../images/5-3/3/logic - exercise part II-06.jpg>)

> a. **List.FilterByBoolMask**: Verbindet **Curve.PointAtParameter** aus dem vorigen Schritt mit der _list_-Eingabe.
>
> b. **Watch**: Die Watch-Blöcke für _in_ und _out_ zeigen, dass zwei Listen für gerade bzw. ungerade Indizes erstellt wurden. Diese Punkte sind auf dieselbe Weise entlang der Kurve angeordnet, wie im folgenden Schritt gezeigt.

7\. Als Nächstes verwenden Sie das Ausgabeergebnis von **List.FilterByBoolMask** in Schritt 05, um Geometrien mit Größen entsprechend ihrer Indizes zu generieren.

**Cuboid.ByLengths**: Stellen Sie die in der Abbildung oben gezeigten Verbindungen wieder her, um eine reißverschlussähnliche Struktur entlang der Kurve zu erhalten. Sie erstellen hier einfache Quader, deren Größe jeweils durch ihren auf der Kurve liegenden Mittelpunkt definiert wird. Die Logik der Aufteilung in gerade und ungerade Werte wird damit im Modell deutlich.

![](<../images/5-3/3/logic - exercise part II-07.jpg>)

> a. Liste der Quader mit geraden Indizes.
>
> b. Liste der Quader mit ungeraden Indizes.

Geschafft! Sie haben soeben einen Prozess zum Definieren der Geometriebemaßungen entsprechend der in dieser Übung dargestellten logischen Operation programmiert.
