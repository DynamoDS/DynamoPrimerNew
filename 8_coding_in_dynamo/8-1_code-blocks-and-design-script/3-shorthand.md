# Kurzschreibweisen

### Kurzschreibweisen

Für Codeblöcke stehen einige einfache Kurzschreibweisen zur Verfügung, die, einfach ausgedrückt, die Arbeit mit den Daten _erheblich_ erleichtern. Im Folgenden werden die Grundlagen genauer erläutert und beschrieben, wie die jeweilige Kurzschreibweise zum Erstellen und Abfragen von Daten verwendet werden kann.

| **Datentyp**          | **Dynamo-Standarddarstellung**                                      | **Codeblock-Entsprechung**                                    |
| ---------------------- | -------------------------------------------------------- | ------------------------------------------------------------- |
| Zahlen                | ![](../images/8-1/3/01node-numbers.jpg)       | ![](../images/8-1/3/01codeblock-numbers.jpg)       |
| Zeichenfolgen                | ![](../images/8-1/3/02node-string.jpg)        | ![](../images/8-1/3/02codeblock-string.jpg)         |
| Sequenzen              | ![](../images/8-1/3/03node-sequence.jpg)       | ![](../images/8-1/3/03codeblock-sequence.jpg)       |
| Bereiche                 | ![](../images/8-1/3/04node-range.jpg)          | ![](../images/8-1/3/04codeblock-range.jpg)         |
| Eintrag an Indexposition abrufen      | ![](../images/8-1/3/05node-listgetitem.jpg) | ![](../images/8-1/3/05codeblock-listgetitem.jpg) |
| Liste erstellen            | ![](../images/8-1/3/06node-listcreate.jpg)   | ![](../images/8-1/3/06codeblock-listcreate.jpg)   |
| Zeichenfolgen verketten    | ![](../images/8-1/3/07node-stringconcat.jpg) | ![](../images/8-1/3/07codeblock-stringconcat.jpg) |
| Bedingungsanweisungen | ![](../images/8-1/3/08node-conditional.jpg)   | ![](../images/8-1/3/08codeblock-conditional.jpg)   |

### Zusätzliche Syntax

|                                     |                           |                                                                                          |
| ----------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------- |
| **Block/Blöcke**                         | **Codeblock-Entsprechung** | **Anmerkung**                                                                                 |
| Beliebiger Operator (+, &&, >=, Not, usw.) | +, &&, >=, !, usw.        | Beachten Sie, dass "Not" (Nicht) durch "!" ersetzt wird, der Block jedoch zur Unterscheidung von "Fakultät" nach wie vor "Not" heißt. |
| Boolescher Wert True                        | true;                     | Anmerkung: Kleinbuchstaben                                                                          |
| Boolescher Wert False                       | false;                    | Anmerkung: Kleinbuchstaben                                                                          |

### Bereiche und Folgen

Die Methoden zum Definieren von Bereichen und Sequenzen können in einfachen Kurzschreibweisen ausgedrückt werden. Die folgende Abbildung bietet eine Anleitung zum Definieren einer Liste mit numerischen Daten mithilfe der Syntax ".." in Codeblöcken. Nachdem Sie sich mit dieser Notation vertraut gemacht haben, können Sie numerische Daten äußerst effizient erstellen:

![](../images/8-1/3/shorthand-rangesandsequences.jpg)

> 1. In diesem Beispiel wird ein Zahlenbereich durch einfache **Codeblock**-Syntax mit Angaben für `beginning..end..step-size;` ersetzt. In numerischer Darstellung erhalten wir die Werte: `0..10..1;`
> 2. Beachten Sie, dass die Syntax `0..10..1;` der Syntax `0..10;` entspricht. Die Schrittgröße 1 ist der Vorgabewert für die Kurzschreibweise. Mit `0..10;` erhalten wir daher eine Folge von 0 bis 10 mit der Schrittgröße 1.
> 3. Das Beispiel für die _Folge_ ist ähnlich, allerdings wird hier mithilfe eines #-Zeichens angegeben, dass die Liste nicht beim Wert 15 enden, sondern 15 Werte enthalten soll. In diesem Fall wird Folgendes definiert: `beginning..#ofSteps..step-size:`. Die tatsächliche Syntax für die Sequenz lautet: `0..#15..2`
> 4. Platzieren Sie das _#_-Zeichen aus dem vorigen Schritt jetzt im Bereich für die _Schrittgröße_ der Syntax. Damit haben Sie einen _Zahlenbereich_ vom _Anfang_ zum _Ende_ erstellt. Die Notation für die _Schrittgröße_ verteilt die angegebene Anzahl Werte gleichmäßig zwischen diesen beiden Angaben: `beginning..end..#ofSteps`

### Erweiterte Bereiche

Indem Sie erweiterte Bereiche erstellen, können Sie auf einfache Weise mit Listen von Listen arbeiten. In den Beispielen unten wird eine Variable aus der Darstellung der primären Liste isoliert und ein weiterer Bereich aus dieser Liste erstellt.

![](../images/8-1/3/shorthand-advancerange01.jpg)

> 1\. Vergleichen Sie die Notation mit und ohne #-Zeichen bei der Erstellung verschachtelter Bereiche. Dabei gilt dieselbe Logik wie bei einfachen Bereichen, die Angaben sind jedoch etwas komplexer.
>
> 2\. Sie können an beliebiger Stelle des primären Bereichs einen Unterbereich erstellen. Es ist auch möglich, zwei Unterbereiche zu verwenden.
>
> 3\. Mithilfe des Werts für end in einem Bereich erstellen Sie weitere Bereiche unterschiedlicher Länge.

Vergleichen Sie als Übung zu dieser Logik die beiden oben gezeigten Kurzschreibweisen und testen Sie, wie _Unterbereiche_ und das _#_-Zeichen sich auf das Ergebnis auswirken.

![](../images/8-1/3/shorthand-advancerange02.jpg)

### Erstellen von Listen und Abrufen von Einträgen aus Listen

Sie können Listen nicht nur mithilfe von Kurzschreibweisen, sondern auch ad hoc erstellen. Solche Listen können eine Vielfalt von Elementtypen enthalten und können abgefragt werden (da Listen ihrerseits Objekte sind). Kurz zusammengefasst: Mit einem Codeblock erstellen Sie Listen, und zum Abfragen der Listen verwenden Sie eckige Klammern:

![](../images/8-1/3/shorthand-list&getfromlist01.jpg)

> 1\. Sie können Listen schnell aus Zeichenfolgen erstellen und über die Indizes der Einträge abfragen.
>
> 2\. Sie können Listen mit Variablen erstellen und sie über die Kurzschreibweisen für Bereiche abfragen.

Verschachtelte Listen werden auf ähnliche Weise verwaltet. Beachten Sie dabei die Reihenfolge der Listen und verwenden Sie mehrere Paare eckiger Klammern:

![](../images/8-1/3/shorthand-list&getfromlist02.jpg)

> 1\. Definieren Sie eine Liste von Listen.
>
> 2\. Fragen Sie eine Liste mit einer Angabe in eckigen Klammern ab.
>
> 3\. Fragen Sie einen Eintrag mit zwei Angaben in eckigen Klammern ab.

## Übung: Sinusoberfläche

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/8-1/3/Obsolete-Nodes_Sine-Surface.dyn" %}

In dieser Übung wenden Sie Ihre Kenntnisse der Kurzschreibweise an und erstellen eine originelle gewölbte Oberfläche, die Sie mithilfe von Bereichen und Formeln definieren. Beachten Sie in dieser Übung, wie Codeblöcke und bestehende Dynamo-Blöcke zusammenwirken: Für umfangreiche Datenverarbeitungen kommen Codeblöcke zum Einsatz, durch die visuelle Darstellung der Dynamo-Blöcke ist die Definition leichter zu lesen.

Erstellen Sie zunächst eine Oberfläche, indem Sie die oben gezeigten Blöcke verbinden. Verwenden Sie zum Definieren der Breite und Länge keinen Number-Block, sondern doppelklicken Sie in den Ansichtsbereich und geben Sie den Wert `100;` in einen Codeblock ein.

![](../images/8-1/3/shorthand-exercise01.jpg)

![](../images/8-1/3/shorthand-exercise02.jpg)

> 1. Definieren Sie einen Bereich zwischen 0 und 1 mit 50 Unterteilungen, indem Sie `0..1..#50` in einen **Codeblock** eingeben.
> 2. Verbinden Sie den Bereich mit **Surface.PointAtParameter**. Dieser Block benötigt u- und v-Werte zwischen 0 und 1 für die gesamte Oberfläche. Sie müssen die Vergitterung in Kreuzprodukt ändern. Klicken Sie dazu mit der rechten Maustaste auf den **Surface.PointAtParameter**-Block.

In diesem Schritt verschieben Sie das Raster aus Punkten mithilfe der ersten Funktion in z-Richtung nach oben. Dieses Raster steuert die zu erstellende Oberfläche anhand der zugrunde liegenden Funktion. Fügen Sie neue Blöcke hinzu, wie in der folgenden Abbildung gezeigt.

![](../images/8-1/3/shorthand-exercise03.jpg)

> 1. Verwenden Sie anstelle eines Formelblocks einen **Codeblock** mit der folgenden Zeile: `(0..Math.Sin(x*360)..#50)*5;`. Dadurch definieren Sie, kurz zusammengefasst, einen Bereich, der eine Formel enthält. Diese Formel ist die Sinusfunktion. Da für die Sinusfunktion in Dynamo Werte in Grad eingegeben werden müssen, multiplizieren Sie die x-Werte (d. h. den eingegebenen Bereich zwischen 0 und 1) mit 360, um eine vollständige Sinuskurve zu erhalten. Als Nächstes wird dieselbe Anzahl Unterteilungen als Rastersteuerpunkte für die einzelnen Reihen benötigt. Definieren Sie daher 50 Unterteilungen mit #50. Der Multiplikator 5 schließlich verstärkt die Verschiebung, sodass deren Wirkung in der Dynamo-Vorschau deutlich zu sehen ist.

![](../images/8-1/3/shorthand-exercise04.jpg)

> 1. Der zuvor verwendete **Codeblock** erfüllte seine Funktion, war jedoch nicht vollständig parametrisch. Die Parameter sollen dynamisch gesteuert werden. Ersetzen Sie daher die Zeile aus dem vorherigen Schritt durch `(0..Math.Sin(x*360*cycles)..#List.Count(x))*amp;`. Dadurch erhalten Sie die Möglichkeit, diese Werte anhand Ihrer Eingaben zu definieren.

Indem Sie die Werte der Schieberegler (zwischen 0 und 10) ändern, erhalten Sie interessante Ergebnisse.

![](../images/8-1/3/shorthand-exercise05.gif)

![](../images/8-1/3/shorthand-exercise06.jpg)

> 1. Indem Sie Transpose auf den Zahlenbereich anwenden, kehren Sie die Richtung der Wellen um: `transposeList = List.Transpose(sineList);`

![](../images/8-1/3/shorthand-exercise07.jpg)

> 1. Durch Addieren von sineList und tranposeList erhalten Sie eine verzerrte Hülle: `eggShellList = sineList+transposeList;`

Ändern Sie die unten angegebenen Schiebereglerwerte, um den Algorithmus etwas ausgeglichener zu gestalten.

![](../images/8-1/3/shorthand-exercise08.jpg)

In dieser letzten Übung fragen wir isolierte Teile der Daten mithilfe eines Codeblocks ab. Fügen Sie den oben gezeigten Codeblock zwischen dem **Geometry.Translate**\- und dem **NurbsSurface.ByPoints**-Block ein, um die Oberfläche aus einem bestimmten Bereich von Punkten neu zu erstellen. Der Codeblock enthält die folgende Textzeile: `sineStrips[0..15..1];`. Dadurch werden die ersten 16 (von 50) Punktreihen ausgewählt. Wenn die Oberfläche neu erstellt wird, ist zu erkennen, dass ein isolierter Teil des Punktrasters generiert wurde.

![](../images/8-1/3/shorthand-exercise09.jpg)

![](../images/8-1/3/shorthand-exercise10.jpg)

> 1. Im letzten Schritt erweitern Sie den **Codeblock** um parametrische Funktionen, indem Sie einen Schieberegler mit dem Bereich von 0 bis 1 zur Steuerung der Abfrage verwenden. Dies geschieht mit der folgenden Codezeile: `sineStrips[0..((List.Count(sineStrips)-1)*u)];`. Dies mag etwas verwirrend wirken. Diese Codezeile ermöglicht jedoch eine schnelle Skalierung der Länge der Liste über einen Multiplikator zwischen 0 und 1.

Mithilfe des Werts `0.53` im Schieberegler erstellen Sie eine Oberfläche, die sich knapp über die Mitte des Rasters hinaus erstreckt.

![](../images/8-1/3/shorthand-exercise11.jpg)

Mit dem Wert `1` im Schieberegler wird erwartungsgemäß eine Oberfläche aus dem gesamten Punktraster erstellt.

![](../images/8-1/3/shorthand-exercise12.jpg)

Im visuellen Diagramm können Sie die einzelnen Codeblöcke markieren und ihre Funktionen sehen.

![](../images/8-1/3/shorthand-exercise13.jpg)

> 1\. Der erste **Codeblock** ersetzt den **Number**-Block.
>
> 2\. Der zweite **Codeblock** ersetzt den **Number Range**-Block.
>
> 3\. Der dritte **Codeblock** ersetzt den **Formula**-Block (sowie **List.Transpose**, **List.Count** und **Number Range**).
>
> 4\. Der vierte **Codeblock** ruft eine Liste von Listen ab und ersetzt den **List.GetItemAtIndex**-Block.
