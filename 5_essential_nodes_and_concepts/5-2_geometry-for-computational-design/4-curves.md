# Kurven

## Kurven in Dynamo

### Was ist eine Kurve?

[Kurven](4-curves.md#deep-dive-into...) sind der erste geometrische Datentyp, den wir abgedeckt haben, die einen vertrauteren Satz an beschreibenden Eigenschaften für die Form aufweisen: Wie kurvig oder gerade? Wie lange oder kurz? Und denken Sie daran, dass Punkte weiterhin unsere Bausteine für die Definition von allem darstellen, von einer Linie zu einem Spline und zu allen Kurvenarten dazwischen.

![Kurventypen](../images/5-2/4/CurveTypes.jpg)

> 1. Linie
> 2. Polylinie
> 3. Bogen
> 4. Kreis
> 5. Ellipse
> 6. NURBS-Kurve
> 7. Polykurve

### Linie

Eine [Linie](4-curves.md#lines) besteht aus einer Reihe von Punkten, jede Linie verfügt über mindestens zwei Punkte. Eine der häufigsten Methoden zum Erstellen von Linien in Dynamo ist die Verwendung von `Line.ByStartPointEndPoint` ![](images/5-2/4/Linebystartpointendpoint.jpg), um eine Linie in Dynamo zu erstellen.

![](<../images/5-2/4/curves - line by start point end point (1).jpg>)

### NURBS-Kurve

[NURBS](4-curves.md#nurbs-+-polycurves) ist ein Modell, das zur exakten Darstellung von Kurven und Oberflächen verwendet wird. Eine Sinuskurve in Dynamo mit zwei unterschiedlichen Methoden, um NURBS-Kurven zu erstellen und die Ergebnisse zu vergleichen.

![](../images/5-2/4/curves-NurbsCurves.jpg)

> 1. _NurbsCurve.ByControlPoints_ verwendet die Liste der Punkte als Steuerpunkte.
> 2. _NurbsCurve.ByPoints_ zeichnet eine Kurve durch die Liste der Punkte.

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-2/4/Geometry for Computational Design - Curves.dyn" %}

## Vertiefung...

### Kurven

Der Begriff **Kurve** ist im Allgemeinen ein Oberbegriff für all die unterschiedlichen Typen von gekrümmten (auch geraden) Formen. Kurve heißt deshalb auch die übergeordnete Kategorisierung für all diese Formen – Linien, Kreise, Splines usw. Technisch betrachtet beschreibt eine Kurve jeden möglichen Punkt, der durch die Eingabe von "t" in eine Sammlung von Funktionen gefunden werden kann, von einfachen Funktionen wie `x = -1.26*t, y = t` bis hin zu Funktionen mit Calculi. Unabhängig davon, mit welcher Art von Kurve Sie arbeiten, handelt es sich bei dem **Parameter** mit dem Namen "t" um eine Eigenschaft, die bewertet werden kann. Darüber hinaus haben alle Kurven unabhängig vom Aussehen der Form einen Startpunkt und einen Endpunkt, die zufälligerweise mit dem Mindest- und dem Höchstwert von t zusammenfallen, die zum Erzeugen der Kurve verwendet wurden. Dies hilft Ihnen auch dabei, ihre Direktionalität zu verstehen.

![Kurvenparameter](../images/5-2/4/CurveParameter.jpg)

> Es ist wichtig zu beachten, dass Dynamo davon ausgeht, dass die Domäne der Werte "t" für eine Kurve 0.0 bis 1.0 beträgt.

Alle Kurven besitzen auch eine Reihe von Eigenschaften oder Merkmalen, die verwendet werden können, um sie zu beschreiben oder zu analysieren. Wenn der Abstand zwischen dem Start- und dem Endpunkt null beträgt, ist die Kurve "geschlossen". Außerdem weist jede Kurve eine Reihe von Steuerpunkten auf. Wenn sich diese Punkte alle in derselben Ebene befinden, ist die Kurve "planar". Einige Eigenschaften gelten für die Kurve als Ganzes, während andere nur für bestimmte Punkte auf der Kurve gelten. Die Ebenheit ist beispielsweise eine globale Eigenschaft, während ein tangentialer Vektor an einem bestimmten Wert t eine lokale Eigenschaft ist.

### Linien

**Linien** sind die einfachste Form von Kurven. Sie sehen möglicherweise nicht gekrümmt aus, sind jedoch tatsächlich Kurven – nur ohne Krümmung. Es gibt mehrere Möglichkeiten zum Erstellen von Linien, die intuitivste davon zwischen den Punkten A und B. Die Form der Linie AB erstreckt sich zwischen den beiden Punkten, mathematisch betrachtet geht sie jedoch in beide Richtungen unendlich weiter.

![Linie](../images/5-2/4/Line.jpg)

Wenn Sie zwei Linien miteinander verbinden, erhalten Sie eine **Polylinie**. Im Folgenden ist auf einfache Weise dargestellt, was ein Steuerpunkt ist. Durch das Bearbeiten der Position eines dieser Punkte ändert sich die Form der Polylinie. Wenn die Polylinie geschlossen ist, erhalten Sie ein Polygon. Wenn die Kantenlängen des Polygons gleich sind, wird von einem regelmäßigen Polygon gesprochen.

![Polylinie und Polygon](../images/5-2/4/Polyline.jpg)

### Bogen, Kreise, Ellipsenbogen und Ellipsen

Um mehr Komplexität zu den parametrischen Funktionen hinzuzufügen, die eine Form definieren, können Sie mit einem weiteren Schritt aus einer Linie einen **Bogen**, **Kreis**, **Ellipsenbogen** oder eine **Ellipse** erstellen, indem Sie einen oder zwei Radien beschreiben. Dabei besteht der Unterschied zwischen der Bogenversion sowie der Kreis- oder Ellipsenversion darin, ob die Form offen oder geschlossen ist.

![Bogen und Kreise](../images/5-2/4/Arcs+Circles.jpg)

### NURBS + Polykurven

**NURBS** (nicht-uniforme rationale B-Splines) sind mathematische Darstellungen, mit denen jede beliebige Form von einfachen zweidimensionalen Linien, Kreisen, Bogen oder Rechtecken bis hin zu den komplexesten dreidimensionalen organischen Freiformkurven präzise modelliert werden kann. Aufgrund ihrer Flexibilität (relativ wenige Steuerpunkte, aber eine glatte Interpolation basierend auf Gradeinstellungen) und Genauigkeit (durch eine robuste Mathematik) können NURBS-Modelle in jedem beliebigen Prozess von der Illustration über die Animation bis hin zur Fertigung verwendet werden.

![NURBS-Kurve](../images/5-2/4/NURBScurve.jpg)

**Grad**: Der Grad einer Kurve bestimmt den Einflussbereich, den die Steuerpunkte auf eine Kurve haben. Dabei gilt: je höher der Grad, desto größer der Bereich. Der Grad ist eine positive ganze Zahl. Er lautet normalerweise 1, 2, 3 oder 5, es kann sich dabei aber um jede beliebige ganze Zahl handeln. NURBS-Linien und -Polylinien weisen normalerweise den Grad 1 und die meisten Freiformkurven den Grad 3 oder 5 auf.

**Steuerpunkte**: Steuerpunkte sind eine Liste mit mindestens einem Grad und einem Punkt. Eine der einfachsten Möglichkeiten zum Ändern der Form einer NURBS-Kurve besteht im Verschieben der Steuerpunkte.

**Gewichtung**: Steuerpunkten ist eine Zahl zugewiesen, die als Gewichtung bezeichnet wird. Gewichtungen werden in der Regel als positive Zahlen angegeben. Wenn die Steuerpunkte einer Kurve alle dieselbe Gewichtung aufweisen (normalerweise 1), wird die Kurve als nichtrational, andernfalls als rational bezeichnet. Die meisten NURBS-Kurven sind nichtrational.

**Blöcke**: Blöcke sind eine Liste mit (Grad+N-1) Zahlen, wobei N der Anzahl an Steuerpunkten entspricht. Die Blöcke werden zusammen mit den Gewichtungen verwendet, um den Einfluss der Steuerpunkte auf die resultierende Kurve zu steuern. Eine Verwendung von Blöcken besteht darin, Knicke an bestimmten Punkten in der Kurve zu erzeugen.

![NURBS-Kurvengrad](../images/5-2/4/NURBScurve\_Degree.jpg)

> 1. Grad = 1
> 2. Grad = 2
> 3. Grad = 3

{% hint style="info" %} Beachten Sie, dass je höher der Gradwert ist, desto mehr Steuerpunkte werden verwendet, um die resultierende Kurve zu interpolieren. {% endhint %}
