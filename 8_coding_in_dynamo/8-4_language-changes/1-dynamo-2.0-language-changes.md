# Änderungen der Sprache

Der Abschnitt Änderungen der Sprache bietet einen Überblick über die Aktualisierungen und Änderungen an der Sprache in den einzelnen Dynamo-Versionen. Diese Änderungen können sich auf die Funktionalität, Leistung und Verwendung auswirken. Dieser Leitfaden hilft Benutzern zu verstehen, wann und warum sie diese Aktualisierungen übernehmen sollten.

## Änderungen der Sprache in Dynamo 2.0

1. Änderung der list@level-Syntax von "@-1" in "@L1"
* Neue Syntax für list@level, sodass list@L1 anstelle von list@-1 verwendet wird
* Grund: Benutzertests zeigen, dass die neue Anpassung der Code-Syntax an die Vorschau/Benutzeroberfläche besser verständlich ist.

2. Implementierung der Int- und Double-Typen in TS, sodass sie den Dynamo-Typen entsprechen

3. Unzulässigkeit überlasteter Funktionen, bei denen sich die Argumente nur durch die Kardinalität unterscheiden
* Alte Diagramme, bei denen Überlastungen verwendet werden, die entfernt wurden, sollten vorgabemäßig die Überlastungen mit höherem Rang verwenden.
* Grund: Beseitigung von Unklarheiten darüber, welche Funktion ausgeführt wird

4. Deaktivierung der Array-Hochstufung mit Replikationsanleitungen

5. Festlegen von Variablen in imperativen Blöcken als lokal für den imperativen Blockbereich
* Variablenwerte, die innerhalb imperativer Codeblöcke definiert sind, werden durch Änderungen in imperativen Blöcken, die diese referenzieren, nicht geändert.  

6. Festlegen von Variablen als unveränderlich, um die assoziative Aktualisierung in Codeblock-Block zu deaktivieren

7. Kompilierung aller Benutzeroberflächen-Blöcke zu statischen Methoden

8. Unterstützung von Rückgabeanweisungen ohne Zuweisung
* "=" wird weder in Funktionsdefinitionen noch in imperativem Code benötigt.

9. Migration von alten Methodennamen in CBNs
* Viele Blöcke wurden umbenannt, um die Lesbarkeit und Platzierung in der Benutzeroberfläche des Bibliotheks-Browsers zu verbessern.

10. Liste als Wörterbuchbereinigung

----
Bekannte Probleme:
- Namensbereichskonflikte in imperativen Blöcken führen dazu, dass unerwartete Eingabeanschlüsse angezeigt werden. Weitere Informationen finden Sie unter [Github-Problem](https://github.com/DynamoDS/Dynamo/issues/8796). Um dies zu umgehen, definieren Sie die Funktion wie folgt außerhalb des imperativen Blocks:
```
pnt = Autodesk.Point.ByCoordinates;
lne = Autodesk.Line.ByStartPointEndPoint;

[Imperative]
{
    x = 1;
    start = pnt(0,0,0);
    end = pnt(x,x,x);
    line = lne(start,end);
    return = line;
};
```

## Erläuterungen zu den Änderungen der Sprache in Dynamo 2.0

Für die Version Dynamo 2.0 wurden eine Reihe von Verbesserungen an der Sprache vorgenommen. Der Hauptgrund dafür war die Vereinfachung der Sprache. Der Schwerpunkt liegt darauf, DesignScript überschaubarer und benutzerfreundlicher und somit leistungsstärker und flexibler zu gestalten, sodass die Verständlichkeit für den Endbenutzer verbessert wird.

Im Folgenden werden die Änderungen in Version 2.0 erläutert:
* Vereinfachte List@Level-Syntax
* Überlastete Methoden mit Parametern, die sich nur durch den Rang unterscheiden, sind unzulässig 
* Kompilierung aller Benutzeroberflächen-Blöcke zu statischen Methoden
* Deaktivierung der Listenhochstufung bei Verwendung mit Replikationsanleitungen/Vergitterung
* Variablen in assoziativen Blöcken sind unveränderlich, um die assoziative Aktualisierung zu verhindern
* Variablen in imperativen Blöcken gelten lokal für den imperativen Bereich
* Trennung von Listen und Wörterbüchern

## 1\. Vereinfachte list@level-Syntax 

Neue Syntax für list@level, sodass `list@L1` anstelle von `list@-1` ![](../images/8-4/1/lang2_1.png) verwendet wird


## 2\. Überlastete Funktionen mit Parametern, die sich nur durch den Rang unterscheiden, sind unzulässig
Überlastete Funktionen sind aus mehreren Gründen problematisch:
* Eine überlastete Funktion, die durch einen Benutzeroberflächen-Block im Diagramm angegeben wird, enthält möglicherweise nicht dieselbe Überlastung, die zur Laufzeit ausgeführt wird.
* Die Methodenauflösung ist teuer und funktioniert nicht gut für überlastete Funktionen.
* Das Replikationsverhalten für überlastete Funktionen ist schwer nachzuvollziehen.

Nehmen wir `BoundingBox.ByGeometry` als Beispiel (es gab zwei überlastete Funktionen in älteren Versionen von Dynamo, eine, die ein einzelnes Wertargument, und eine andere, die eine Liste von Geometrien als Argument akzeptierte):
```
BoundingBox BoundingBox.ByGeometry(geometry: Geometry) {...}
BoundingBox BoundingBox.ByGeometry(geometry: Geometry[]) {...}
```
Wenn der Benutzer den ersten Block im Ansichtsbereich abgelegt und eine Liste von Geometrien verbunden hat, erwartet er, dass die Replikation einsetzt, dies findet jedoch nie statt, da zur Laufzeit stattdessen die zweite Überlastung aufgerufen wird, wie hier gezeigt: ![](../images/8-4/1/lang2_2.png)
 
Aus diesem Grund sind in Version 2.0 überlastete Funktionen nicht zulässig, die sich nur in der Parameterkardinalität unterscheiden. Dies bedeutet, dass bei überlasteten Funktionen, die über die gleiche Anzahl und die gleichen Typen von Parametern verfügen, aber einen oder mehrere Parameter aufweisen, die sich nur im Rang unterscheiden, immer die zuerst definierte Überlastung verwendet wird, während der Rest vom Compiler verworfen wird. Der Hauptvorteil liegt in der Vereinfachung der Methodenauflösungslogik, da ein schneller Pfad zur Auswahl von Funktionskandidaten zur Verfügung steht.

In der Geometriebibliothek für Version 2.0 wurde die erste Überlastung im `BoundingBox.ByGeometry`-Beispiel als veraltet markiert und die zweite beibehalten. Wenn der Block also repliziert werden soll, d. h. im Kontext des ersten Blocks verwendet werden soll, muss er mit der kürzesten (oder längsten) Vergitterungsoption oder in einem Codeblock mit Replikationsanleitungen verwendet werden: 
```
BoundingBox.ByGeometry(geometry<1>);
```
In diesem Beispiel sehen wir, dass der Block mit höherem Rang sowohl in einem replizierten als auch in einem nicht replizierten Aufruf verwendet werden kann und daher immer einer Überlastung mit niedrigerem Rang vorgezogen wird. Als Faustregel wird daher **Blockautoren immer empfohlen, Überlastungen mit niedrigerem Rang zugunsten von Methoden mit höherem Rang unberücksichtigt zu lassen**, sodass der DesignScript-Compiler immer die Methode mit höherem Rang als die erste und einzige Methode aufruft, die gefunden werden kann.

### Beispiele:
Im folgenden Beispiel wurden zwei Überlastungen von Funktion `foo` definiert. In Version 1.x ist nicht eindeutig klar, welche Überlastung zur Laufzeit ausgeführt wird. Der Benutzer erwartet möglicherweise, dass die zweite Überlastung `foo(a:int, b:int)` ausgeführt wird. In diesem Fall wird erwartet, dass die Methode dreimal repliziert wird und den Wert `10` dreimal zurückgibt. In Wirklichkeit wird stattdessen ein einzelner Wert `10` zurückgegeben, da stattdessen die erste Überlastung mit dem Listenparameter aufgerufen wird.

### Die zweite Überlastung wird in Version 2.0 weggelassen:
In Version 2.0 wird immer die zuerst definierte Methode den anderen vorgezogen. Es gilt das Prinzip "Wer zuerst kommt, mahlt zuerst".

![](../images/8-4/1/lang2_3.png)

Für jeden der folgenden Fälle wird die zuerst definierte Überlastung verwendet. Beachten Sie, dass dies ausschließlich auf der Reihenfolge basiert, in der die Funktionen definiert werden, und nicht auf den Parameterrängen. Es wird jedoch empfohlen, Methoden mit höher eingestuften Parametern bei benutzerdefinierten und Zero-Touch-Blöcken zu bevorzugen.
```
1)
foo(a: int[], b: int); ✓
foo(a: int, b: int); ✕
```
```
2) 
foo(x: int, y: int); ✓
foo(x: int[], y: int[]); ✕
```
## 3\. Kompilierung aller Benutzeroberflächen-Blöcke zu statischen Methoden
In Dynamo 1.x wurden Benutzeroberflächen-Blöcke (Nicht-Codeblöcke) zu Instanzmethoden bzw. -eigenschaften kompiliert. So wurde z. B. der Block `Point.X` in `pt.X` und `Curve.PointAtParameter` in `curve.PointAtParameter(param)` kompiliert. Dieses Verhalten wies zwei Probleme auf:

__A. Die Funktion, die der Benutzeroberflächen-Block darstellte, war nicht immer dieselbe, die zur Laufzeit ausgeführt wurde.__

Ein typisches Beispiel ist der Block `Translate`. Es gibt mehrere `Translate`-Blöcke, die dieselbe Anzahl und die gleichen Typen von Argumenten verwenden, z. B. `Geometry.Translate`, `Mesh.Translate` und `FamilyInstance.Translate`. Aufgrund der Tatsache, dass Blöcke als Instanzmethoden kompiliert wurden, würde die Übergabe von `FamilyInstance` an einen `Geometry.Translate`-Block überraschenderweise immer noch funktionieren, da zur Laufzeit der Aufruf an die `Translate`-Instanzmethode in einer `FamilyInstance` weitergeleitet würde. Dies war offensichtlich irreführend für die Benutzer, da der Block nicht das tat, was erwartet wurde.

__B. Das zweite Problem war, dass Instanzmethoden nicht mit heterogenen Arrays funktionierten.__

Zur Laufzeit muss von der Ausführungs-Engine ermittelt werden, an welche Funktion weitergeleitet werden soll. Wenn es sich bei der Eingabe um eine Liste handelt, z. B. `list.Translate()`, gilt Folgendes: Da es teuer ist, die einzelnen Elemente in einer Liste durchzugehen und Methoden zum Typ zu suchen, würde die Methodenauflösungslogik einfach davon ausgehen, dass der Zieltyp mit dem Typ des ersten Elements identisch ist, und versuchen, die für diesen Typ definierte Methode `Translate()` zu suchen. Wenn also der erste Elementtyp nicht mit dem Zieltyp der Methode übereinstimmt (selbst wenn es sich um `null` oder eine leere Liste handelt), schlägt die gesamte Liste fehl, auch wenn andere übereinstimmende Typen in der Liste vorhanden sind. 

Wenn beispielsweise eine Listeneingabe mit den folgenden Typen `[Arc, Line]` an `Arc.CenterPoint` übergeben wird, enthält das Ergebnis wie erwartet einen Mittelpunkt für den Bogen und einen `null`-Wert für die Linie. Wenn die Reihenfolge jedoch umgekehrt wird, ist das gesamte Ergebnis null, da das erste Element die Methodenauflösungsprüfung nicht bestanden hat:
### Dynamo 1.x: Testet nur das erste Element der Eingabeliste in Bezug auf die Methodenauflösungsprüfung.
![](../images/8-4/1/lang2_4.png)
```
x = [arc, line];
y = x.CenterPoint; // y = [centerpoint, null] ✓
```
```
x = [line, arc];
y = x.CenterPoint; // y = null ✕
```
In Version 2.0 werden diese beiden Probleme behoben, indem Benutzeroberflächen-Blöcke als statische Eigenschaften und statische Methoden kompiliert werden. 

Bei statischen Methoden ist die Methodenauflösung zur Laufzeit einfacher, und alle Elemente in der Eingabeliste werden iteriert. Beispiel:

Die Semantik von `foo.Bar()` (Instanzmethode) muss auf den Typ `foo` prüfen und ebenso ermitteln, ob es sich um eine Liste handelt oder nicht, und sie dann mit möglichen Funktionen abgleichen. Das ist teuer. Auf der anderen Seite muss die Semantik von `Foo.Bar(foo)` (statische Methode) nur eine Funktion mit dem Parametertyp `foo` prüfen.

In Version 2.0 geschieht Folgendes:
* Ein Benutzeroberflächen-Eigenschaftsblock wird zu einem statischen Getter kompiliert: Die Engine generiert für jede Eigenschaft eine statische Version eines Getters. Beispiel: Ein `Point.X`-Block wird zu einem statischen Getter `Point.get_X(pt)` kompiliert. Beachten Sie, dass der statische Getter auch über seinen Alias aufgerufen werden kann: `Point.X(pt)` in einem Codeblock-Block.
* Ein Benutzeroberflächen-Methodenblock wird zur statischen Version kompiliert: Die Engine generiert eine entsprechende statische Methode für den Block. Der Block `Curve.PointAtParameter` wird beispielsweise nicht zu `curve.PointAtParameter(parameter)`, sondern zu `Curve.PointAtParameter(curve: Curve, parameter:double)` kompiliert. 

**Anmerkung:** Die Unterstützung von Instanzmethoden wurde mit dieser Änderung nicht entfernt, sodass vorhandene Instanzmethoden, die in CBNs wie `pt.X` und `curve.PointAtParameter(parameter)` in den obigen Beispielen verwendet werden, weiterhin funktionieren.

Dieses Beispiel funktionierte zuvor in Version 1.x, da das Diagramm zu `point.X;` kompiliert wurde und die `X`-Eigenschaft für das Punktobjekt gefunden wurde. Jetzt in Version 2.0 schlägt es fehl, da der kompilierte Code `Vector.X(point)` nur einen `Vector`-Typ erwartet:

![](../images/8-4/1/lang2_5.png)

### Vorteile:
**Kohärent/verständlich:** Statische Methoden beseitigen alle Unklarheiten darüber, welche Methode zur Laufzeit ausgeführt wird. Die Methode stimmt immer mit dem Benutzeroberflächen-Block überein, der in dem Diagramm verwendet wird, das der Benutzer aufrufen möchte.

**Kompatibel:** Es besteht eine bessere Korrelation zwischen dem Code und dem visuellen Programm.

**Anweisend:** Die Übergabe heterogener Listeneingaben an Blöcke führt jetzt zu Werten ungleich null für Typen, die vom Block akzeptiert werden, und zu Nullwerten für Typen, die den Block nicht implementieren. Die Ergebnisse sind besser vorhersagbar und geben einen besseren Hinweis darauf, welche Typen für den Block zulässig sind.

### Einschränkung: Nicht aufgelöste Mehrdeutigkeiten mit überlasteten Methoden

Da Dynamo Funktionsüberlastungen im Allgemeinen unterstützt, kann es dennoch zu Verwirrungen kommen, wenn eine weitere überlastete Funktion mit derselben Anzahl von Parametern vorhanden ist. Wenn Sie im folgenden Diagramm beispielsweise einen numerischen Wert mit der `direction`-Eingabe von `Curve.Extrude` und einen Vektor mit der `distance`-Eingabe von `Curve.Extrude` verbinden, funktionieren beide Blöcke weiterhin, was unerwartet ist. In diesem Fall kann die Engine zur Laufzeit keinen Unterschied erkennen, auch wenn die Blöcke zu statischen Methoden kompiliert werden, und wählt je nach Eingabetyp eine der beiden Methoden aus. ![](../images/8-4/1/lang2_6.png)
 
### Behobene Probleme:
Die Umstellung auf statische Methodensemantik brachte die folgenden Nebeneffekte mit sich, die hier als zugehörige Änderungen der Sprache in Version 2.0 erwähnenswert sind.

**1\. Verlust des polymorphen Verhaltens:**

Betrachten wir ein Beispiel aus `TSpline`-Blöcken in `ProtoGeometry` (beachten Sie, dass `TSplineTopology` Werte vom Basistyp `Topology` übernimmt): Der Block `Topology.Edges`, der zuvor zur Instanzmethode `object.Edges` kompiliert wurde, wird jetzt zur statischen Methode `Topology.Edges(object)` kompiliert. Der vorherige Aufruf würde nach einem Methoden-Dispatch über den Laufzeittyp des Objekts polymorph in die abgeleitete Klassenmethode `TsplineTopology.Edges` aufgelöst. 

![](../images/8-4/1/lang2_7.png)

Beim neuen statischen Verhalten wurde hingegen erzwungen, dass die Basisklassenmethode `Topology.Edges` aufgerufen wird. Dieser Block gab daher die `Edge`-Objekte der Basisklasse anstelle der abgeleiteten Klassenobjekte des Typs `TSplineEdge` zurück.
 
![](../images/8-4/1/lang2_8.png)

Dies war eine Regression, da nachgelagerte `TSpline`-Blöcke, die `TSplineEdges` erwarteten, fehlzuschlagen begannen. 

Das Problem wurde behoben, indem in der Methoden-Dispatch-Logik eine Laufzeitüberprüfung hinzugefügt wurde, um den Instanztyp anhand des Typs oder Untertyps des ersten Parameters der Methode zu überprüfen. Im Falle einer Eingabeliste haben wir den Methoden-Dispatch vereinfacht, sodass einfach nach dem Typ des ersten Elements gesucht wird. Somit war die endgültige Lösung ein Kompromiss zwischen einer teils statischen und teils dynamischen Methodensuche.

**Neues polymorphes Verhalten in Version 2.0:**

![](../images/8-4/1/lang2_9.png)

Da in diesem Fall das erste Element `a` ein `TSpline` ist, wird die abgeleitete Methode `TSplineTopology.Edges` zur Laufzeit aufgerufen. Als Ergebnis wird `null` für den `Topology`-Basistyp `b` zurückgegeben. 

Da im zweiten Fall der allgemeine `Topology`-Typ `b` das erste Element ist, wird die `Topology.Edges`-Basismethode aufgerufen. Da `Topology.Edges` auch den abgeleiteten `TSplineTopology`-Typ akzeptiert, gibt `a` als Eingabe `Edges` für beide Eingaben (`a` und `b`) zurück.

![](../images/8-4/1/lang2_10.png)
 
**2\. Regressionen von der Erstellung redundanter äußerer Listen**

Es gibt einen Hauptunterschied zwischen Instanzmethoden und statischen Methoden, wenn es um das Verhalten der Replikationsanleitung geht. Bei Instanzmethoden werden Eingaben mit einzelnen Werten mit Replikationsanleitungen nicht in Listen hochgestuft, während sie bei statischen Methoden hochgestuft werden.

Betrachten Sie das Beispiel des Blocks `Surface.PointAtParameter` mit Kreuzvergitterung und mit einer einzelnen surface-Eingabe sowie Arrays von `u`\- und `v`-Parameterwerten. Die Instanzmethode wird kompiliert zu:
```
surface<1>.PointAtParameter(u<1>, v<2>);
```
wodurch ein 2D-Array mit Punkten entsteht.
 
Die statische Methode wird kompiliert zu:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
wodurch eine 3D-Liste mit Punkten und einer redundanten äußersten Liste entsteht.

Dieser Nebeneffekt beim Kompilieren von Benutzeroberflächen-Blöcken zu statischen Methoden kann in solchen vorhandenen Anwendungsfällen möglicherweise zu Regressionen führen. Dieses Problem wurde behoben, indem die Hochstufung einzelner Werteingaben in eine Liste bei Verwendung mit Replikationsanleitungen/Vergitterung deaktiviert wurde (siehe nächster Punkt).
 
**4\. Deaktivierte Listenhochstufung mit Replikationsanleitungen/Vergitterung**

In Version 1.x gab es zwei Fälle, in denen einzelne Werte in Listen hochgestuft wurden:

* Wenn Eingaben mit niedrigerem Rang an Funktionen übergeben wurden, die Eingaben mit höherem Rang erwarteten
* Wenn Eingaben mit niedrigerem Rang an Funktionen übergeben wurden, die denselben Rang erwarteten, deren Eingabeargumente jedoch mit Replikationsanleitungen versehen waren oder Vergitterung verwendeten

In Version 2.0 wird der letztere Fall nicht mehr unterstützt, indem die Listenhochstufung in solchen Szenarien verhindert wird.

Im folgenden 1.x-Diagramm wurde durch eine Ebene der Replikationsanleitung für `y` bzw. `z` die Array-Hochstufung von Rang 1 für jeden Wert erzwungen, weshalb das Ergebnis den Rang 3 (jeweils 1 für `x`, `y` und `z`) aufwies. Stattdessen würde ein Benutzer erwarten, dass das Ergebnis Rang 1 aufweist, da es nicht offensichtlich ist, dass das Vorhandensein von Replikationsanleitungen für einzelne Werteingaben dem Ergebnis Ebenen hinzufügt.
```
x = 1..5;
y = 0;
z = 0;
p = Point.ByCoordinates(x<1>, y<2>, z<3>); // cross-lacing
```

### Dynamo 1.x: 3D-Liste mit Punkten

![](../images/8-4/1/lang2_11.png)

In Version 2.0 resultiert das Vorhandensein von Replikationsanleitungen für jedes der Einzelwertargumente `y` und `z` nicht in einer Hochstufung, was dazu führt, dass die Liste dieselbe Dimension wie die eingegebene 1D-Liste für `x` aufweist. 

### Dynamo 2.0: 1D-Liste mit Punkten

![](../images/8-4/1/lang2_12.png)

Die oben erwähnte Regression, die durch die statische Methodenkompilierung mit der Generierung redundanter äußerer Listen verursacht wurde, wurde durch diese Änderung der Sprache ebenfalls behoben.

Fahren wir mit dem obigen Beispiel fort: Wir haben gesehen, dass ein statischer Methodenaufruf wie
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>); 
```
in Dynamo 1.x eine 3D-Liste mit Punkten generiert hat. Dies geschah aufgrund der Hochstufung des ersten Einzelwertarguments surface in eine Liste bei Verwendung mit einer Replikationsanleitung.
 
### Dynamo 1.x: Listenhochstufung von Argumenten mit Replikationsanleitung

![](../images/8-4/1/lang2_13.png)

In Version 2.0 wurde die Hochstufung von Einzelwertargumenten in Listen bei Verwendung mit Replikationsanleitungen oder Vergitterung deaktiviert. Nun gibt der Aufruf von
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
einfach eine 2D-Liste zurück, da surface nicht hochgestuft wird.

### Dynamo 2.0: Listenhochstufung von Einzelwertargumenten mit Replikationsanleitung deaktiviert

![](../images/8-4/1/lang2_14.png)

Durch diese Änderung wird nun das Hinzufügen einer redundanten Listenebene verhindert, und außerdem wird die durch den Übergang zur statischen Methodenkompilierung verursachte Regression behoben.

### Vorteile:

**Lesbar:** Die Ergebnisse entsprechen den Erwartungen der Benutzer und sind einfacher zu verstehen.

**Kompatibel:** Benutzeroberflächen-Blöcke (mit Vergitterungsoption) und CBNs mit Replikationsanleitungen liefern kompatible Ergebnisse.

**Konsistent:** 
* Instanzmethoden und statische Methoden sind konsistent (Probleme mit statischer Methodensemantik behoben).
* Blöcke mit Eingaben und Vorgabeargumenten verhalten sich konsistent (siehe unten).

![](../images/8-4/1/lang2_15.png)

## 5\. Variablen in Codeblock-Blöcken sind unveränderlich, um eine assoziative Aktualisierung zu verhindern 

DesignScript unterstützte bisher zwei Programmierparadigmen: die assoziative und die imperative Programmierung. Assoziativer Code erstellt ein Abhängigkeitsdiagramm aus Programmanweisungen, wobei Variablen voneinander abhängig sind. Das Aktualisieren einer Variablen kann Aktualisierungen für alle anderen Variablen auslösen, die von dieser Variable abhängen. Dies bedeutet, dass die Sequenz der Ausführung von Anweisungen in einem assoziativen Block nicht auf ihrer Reihenfolge, sondern auf den Abhängigkeitsbeziehungen zwischen den Variablen basiert.

Im folgenden Beispiel lautet die Sequenz der Ausführung des Codes wie folgt: Zeilen 1 -> 2 -> 3 -> 2. Da `b` von `a` abhängig ist, springt die Ausführung bei einer Aktualisierung von `a` in Zeile 3 wieder zu Zeile 2, um `b` mit dem neuen Wert von `a` zu aktualisieren. 
```
1. a = 1; 
2. b = a * 2;
3. a = 2;
```
Wenn derselbe Code hingegen in einem imperativen Kontext ausgeführt wird, werden die Anweisungen in einem linearen Ablauf von oben nach unten ausgeführt. Imperative Codeblöcke eignen sich daher für die sequenzielle Ausführung von Code-Konstrukten wie Schleifen und If-else-Bedingungen.

### Mehrdeutigkeiten bei assoziativer Aktualisierung:

**1\. Variablen mit zyklischer Abhängigkeit:**

In bestimmten Fällen ist eine zyklische Abhängigkeit zwischen Variablen möglicherweise nicht so offensichtlich wie im folgenden Fall. In solchen Fällen, in denen der Compiler den Zyklus nicht statisch erkennen kann, kann dies zu einem unbegrenzten Laufzeitzyklus führen.
```
a = 1;
b = a;
a = b;
```
**2\. Variablen, die von sich selbst abhängig sind:**

Wenn eine Variable von sich selbst abhängig ist, sollte ihr Wert bei jeder Aktualisierung akkumuliert oder auf den ursprünglichen Wert zurückgesetzt werden?
```
a = 1;
b = 1;
b = b + a + 2; // b = 4
a = 4;         // b = 10 or b = 7?
```
Da der Würfel `b` in diesem Geometriebeispiel sowohl von sich selbst als auch vom Zylinder `a` abhängt, stellt sich die Frage, ob das Verschieben des Schiebereglers dazu führen soll, dass die Bohrung entlang des Blocks verschoben wird, oder dass bei jeder Aktualisierung der Position des Schiebereglers ein kumulativer Effekt erstellt wird, bei dem mehrere Löcher entlang des Pfads verteilt werden.

![](../images/8-4/1/lang2_16.gif)

**3\. Aktualisieren von Variableneigenschaften:**

```
1: def foo(x: A) { x.prop = ...; return x; }
2: a = A.A();
3: p = a.prop;
4: a1 = foo(a);  // will p update?
```

**4\. Aktualisieren von Funktionen:**

```
1: def foo(v: double) { return v * 2; }// define “foo”
2: x = foo(5);                         // first definition of “foo” called
3: def foo(v: int) { return v * 3; }   // overload of “foo” defined, will x update?
```
Im Laufe der Zeit haben wir festgestellt, dass sich die assoziative Aktualisierung in Codeblock-Blöcken in einem blockbasierten Datenflussdiagrammkontext nicht als sinnvoll erweist. Bevor eine visuelle Programmierumgebung verfügbar war, bestand die einzige Möglichkeit zum Prüfen der Optionen darin, die Werte einiger Variablen im Programm explizit zu ändern. Ein textbasiertes Programm verfügt über den vollständigen Verlauf der Aktualisierungen einer Variablen, während in einer visuellen Programmierumgebung nur der letzte Wert einer Variablen angezeigt wird. 

Wenn sie überhaupt von einigen Benutzern verwendet wurde, geschah dies höchstwahrscheinlich unwissentlich, was mehr Schaden als Nutzen angerichtet hat. Aus diesem Grund haben wir uns in Version 2.0 entschieden, die Assoziativität bei der Verwendung von Codeblock-Blöcken auszublenden, indem wir Variablen unveränderlich gemacht haben, während wir die assoziative Aktualisierung weiterhin nur als systemeigene Funktion der DS-Engine beibehalten. Dies ist eine weitere Änderung, mit der die Skripterstellung für Benutzer vereinfacht werden soll.

**Die assoziative Aktualisierung wird in CBNs deaktiviert, indem die Neudefinition von Variablen verhindert wird:** ![](../images/8-4/1/lang2_17.png)

**Listenindizierung in Codeblöcken weiterhin zulässig**

Eine Ausnahme wurde für die Listenindizierung gemacht, die in Version 2.0 mit Index-Operatorzuweisung weiterhin zulässig ist.

Im nächsten Beispiel sehen wir, dass die Liste `a` initialisiert wird, aber später mit einer Index-Operatorzuweisung überschrieben werden kann, und dass alle von `a` abhängigen Variablen assoziativ aktualisiert werden, wie aus dem Wert `c` hervorgeht. Die Blockvorschau zeigt außerdem die aktualisierten Werte von `a` nach der Neudefinition eines oder mehrerer der zugehörigen Elemente an.

![](../images/8-4/1/lang2_18.png)

## 6\. Variablen in imperativen Blöcken gelten für den imperativen Blockbereich lokal

Wir haben in Version 2.0 Änderungen an den imperativen Bereichsregeln vorgenommen, um komplizierte sprachübergreifende Aktualisierungsszenarien zu deaktivieren.

In Dynamo 1.x würde die Ausführungssequenz des folgenden Skripts aus den Zeilen 1 -> 2 -> 4 -> 6 -> 4 erfolgen, wobei eine Änderung vom äußeren zum inneren Sprachbereich weitergegeben wird. Da `y` im äußeren assoziativen Block aktualisiert wird und `x` im imperativen Block von `y` abhängig ist, wechselt die Steuerung vom äußeren assoziativen Programm zur imperativen Sprache in Zeile 4. 
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3;
```

Die Ausführungssequenz in diesem nächsten Beispiel würde folgendermaßen lauten: Zeilen 1 -> 2 -> 4 -> 2, wobei die Änderung vom inneren Sprachbereich zum äußeren weitergegeben würde.
```
1: x = 1;
2: y = x * 2;
3: [Imperative] {
4:     x = 3;
5: }
```
Die oben genannten Szenarien beziehen sich auf die sprachübergreifende Aktualisierung, die ebenso wie assoziative Aktualisierungen in Codeblock-Blöcken nicht sehr nützlich sind. Um komplexe sprachübergreifende Aktualisierungsszenarien zu deaktivieren, haben wir Variablen im imperativen Bereich als lokal festgelegt. 

Im folgenden Beispiel in Dynamo 2.0
```
x = 1;
y = x * 2;
i = [Imperative] {
     x = 3;
     return x;
}
```
* ist der im imperativen Block definierte Wert `x` jetzt lokal für den imperativen Bereich.
* bleiben die Werte für `x` und `y` im äußeren Bereich bei `1` bzw. `2`.

Jede lokale Variable innerhalb eines imperativen Blocks muss zurückgegeben werden, wenn auf ihren Wert in einem äußeren Bereich zugegriffen werden soll.

Beispiel:
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3; // x = 1, y = 3
```
* `y` wird lokal in den imperativen Bereich kopiert.  
* Der Wert von `x`, lokal für den imperativem Bereich, lautet `4`.
* Das Aktualisieren des Werts von `y` im äußeren Bereich führt aufgrund der sprachübergreifenden Aktualisierung weiterhin zu einer Aktualisierung von `x`, ist jedoch in Codeblöcken in Version 2.0 aufgrund der Unveränderlichkeit von Variablen deaktiviert.
* Der Wert von `x` und `y` im äußeren assoziativen Bereich bleibt `1` bzw. `2`.

## 7\. Listen und Wörterbücher

In Dynamo 1.x wurden Listen und Wörterbücher durch einen einzigen, einheitlichen Container dargestellt, der sowohl durch einen ganzzahligen Index als auch durch einen nicht ganzzahligen Schlüssel indiziert werden konnte. In der folgenden Tabelle werden die Trennung zwischen Listen und Wörterbüchern in Version 2.0 sowie die Regeln des neuen Wörterbuchdatentyps zusammengefasst:

|                               |    1.x                      |    2.0                                   |
| :---------------------------- | --------------------------- | ---------------------------------------- |
| **Listeninitialisierung**       | `a = {1, 2, 3};`            | `a = [1, 2, 3];`                         |
| **Leere Liste**                | `a = {};`                   | `a = [];`                                |
| **Wörterbuch-Initialisierung** | **Kann dynamisch an dasselbe Wörterbuch angehängt werden:** | **Kann nur neue Wörterbücher erstellen:** |
|                           | `a = {};`                   | `a = {“foo” : 1, “bar” : 2};`            |
|                           | `a[“foo”] = 1;`             | `b = {“foo” : 1, “bar” : 2, “baz” : 3};` |
|                           | `a[“bar”] = 2;`             | `a = {};` // Erstellt ein leeres Wörterbuch. |
|                           | `a[“baz”] = 3;`             |                                          |
| **Wörterbuch-Indizierung**   | **Schlüssel-Indizierung**            | **Die Indizierungssyntax bleibt unverändert.**     |
|                           | `b = a[“bar”];`             | `b = a[“bar”];`                          |
| **Wörterbuch-Schlüssel**       | **Jeder Schlüsseltyp war zulässig.**  | **Nur Zeichenfolgenschlüssel sind zulässig.**           |
|                           | `a = {};`                   | `a  = {“false” : 23, “point” : 12};`     |
|                           | `a[false] = 23;`            |                                          |
|                           | `a[point] = 12;`            |                                          |

### Neue `[]`-Listensyntax
Die Syntax der Listeninitialisierung wurde in Version 2.0 von geschweiften Klammern `{}` in eckige Klammern `[]` geändert. Alle 1.x-Skripte werden automatisch auf die neue Syntax migriert, wenn sie in Version 2.0 geöffnet werden. 

**Anmerkung zu vorgegebenen Argumentattributen in Zero-Touch-Blöcken:**

Beachten Sie jedoch, dass die automatische Migration mit der alten Syntax, die in vorgegebenen Argumentattributen verwendet wird, nicht funktioniert. Blockautoren müssten ihre Zero-Touch-Methodendefinitionen bei Bedarf manuell aktualisieren, um die neue Syntax in vorgegebenen Argumentattributen des Typs `DefaultArgumentAttribute` zu verwenden.

**Anmerkung zur Indizierung:**

Das neue Indizierungsverhalten hat sich in bestimmten Fällen geändert. Beim Indizieren in eine Liste/ein Wörterbuch mit einer beliebigen Liste von Indizes/Schlüsseln mithilfe des Operators `[]` wird jetzt die Listenstruktur der Eingabeliste mit Indizes/Schlüsseln beibehalten. Bisher wurde immer eine 1D-Liste mit Werten zurückgegeben:
```
Given:
a = {“foo” : 1, “bar” : 2};

1.x:
b = a[{“foo”, {“bar”}}];
returns {1, 2}

2.0:
b = a[[“foo”, [“bar”]]];
returns [1, [2]];
```

### Syntax zur Initialisierung des Wörterbuchs:

`{}` (Syntax mit geschweiften Klammern) für die Wörterbuchinitialisierung kann nur im Schlüssel-Wert-Paarformat 
```
dict = {<key> : <value>, …}; 
```
verwendet werden, bei dem nur eine Zeichenfolge für `<key>` zulässig ist und mehrere Schlüssel-Wert-Paare durch Kommas getrennt werden.

![](../images/8-4/1/lang2_19.png)

Die `Dictionary.ByKeysValues`-Zero-Touch-Methode kann als vielseitigere Methode zur Initialisierung eines Wörterbuchs verwendet werden, indem eine Liste von Schlüsseln bzw. Werten übergeben wird und alle Vorteile der Verwendung von Zero-Touch-Methoden wie Replikationsanleitungen usw. enthalten sind.

![](../images/8-4/1/lang2_20.png)

### Warum haben wir keine beliebigen Ausdrücke für die Syntax der Wörterbuchinitialisierung verwendet?

Wir haben mit der Idee, beliebige Ausdrücke für Schlüssel in der Wörterbuch-Schlüssel-Wert-Initialisierungssyntax zu verwenden, experimentiert, haben aber festgestellt, dass dies zu verwirrenden Ergebnissen führen kann, insbesondere wenn eine Syntax wie `{keys : vals}` (`keys` und `vals` repräsentieren beide Listen) mit anderen Sprachfunktionen von DesignScript wie der Replikation kollidierte und andere Ergebnisse aus dem Zero-Touch-Initialisierungsblock erzeugte. 

Es könnte beispielsweise andere Fälle wie diese Anweisung geben, bei denen es schwierig wäre, das erwartete Verhalten zu definieren:
```
dict = {["foo", "bar"] : "baz" };
```
Das weitere Hinzufügen von Replikationsanleitungs-Syntax usw., nicht nur von Kennungen, würde der Idee der Einfachheit in der Sprache widersprechen. 

Wir _könnten_ Wörterbuchschlüssel in Zukunft erweitern, um beliebige Ausdrücke zu unterstützen, wir müssen jedoch auch sicherstellen, dass die Interaktion mit anderen Sprachfunktionen konsistent und verständlich ist. Wir müssen daher zwischen einer größeren Komplexität und einer etwas geringeren Leistung des Systems abwägen, das dafür aber leichter verständlich ist. Außerdem gibt es mit der `Dictionary.ByKeysValues(keyList, valueList)`-Methode immer eine alternative Möglichkeit, ohne großen Aufwand zu betreiben.

### Interaktion mit Zero-Touch-Blöcken:

__1\. Zero-Touch-Block, der ein .NET-Wörterbuch zurückgibt, wird als Dynamo-Wörterbuch zurückgegeben.__

**Betrachten Sie die folgende C#-Zero-Touch-Methode, die ein IDictionary zurückgibt:** ![](../images/8-4/1/lang2_21.png)

**Der entsprechende Rückgabewert des ZT-Blocks wird als Dynamo-Wörterbuch arrangiert:** ![](../images/8-4/1/lang2_22.png)

__2\. Blöcke mit mehreren Rückgaben werden in der Vorschau als Wörterbücher angezeigt.__

**Zero-Touch-Block, der IDictionary mit einem Attribut mit Mehrfachrückgabe zurückgibt, gibt ein Dynamo-Wörterbuch zurück:** ![](../images/8-4/1/lang2_23.png)

![](../images/8-4/1/lang2_24.png)

__3\. Das Dynamo-Wörterbuch kann als Eingabe für einen Zero-Touch-Block übergeben werden, der das .NET-Wörterbuch akzeptiert.__

**ZT-Methode mit IDictionary-Parameter:** ![](../images/8-4/1/lang2_25.png)

**ZT-Block akzeptiert Dynamo-Wörterbuch als Eingabe:** ![](../images/8-4/1/lang2_26.png)

### Wörterbuchvorschau in Blöcken mit mehreren Rückgaben

Wörterbücher sind ungeordnete Schlüssel-Wert-Paare. In Übereinstimmung mit dieser Idee ist es daher nicht garantiert, dass die Vorschau der Schlüssel-Wert-Paare von Blöcken, die Wörterbücher zurückgeben, in der Reihenfolge der Rückgabewerte der Blöcke sortiert werden. 

Wir haben jedoch eine Ausnahme für Blöcke mit Mehrfachrückgabe hinzugefügt, für die `MultiReturnAttribute`-Werte definiert sind. Im folgenden Beispiel ist der Block `DateTime.Components` ein Block mit Mehrfachrückgabe, und in der Blockvorschau werden die Schlüssel-Wert-Paare in derselben Reihenfolge wie bei den Ausgabeanschlüssen im Block angezeigt, was auch der Reihenfolge entspricht, in der die Ausgaben basierend auf den `MultiReturnAttribute`-Werten in der Blockdefinition angegeben werden.

Beachten Sie außerdem, dass die Vorschau für Codeblöcke, anders als beim Benutzeroberflächen-Block, nicht angeordnet ist, da die Informationen zum Ausgabeanschluss (in Form eines Mehrfachrückgabe-Attributs) für den Codeblock-Block nicht vorhanden sind: ![](../images/8-4/1/lang2_27.png)
