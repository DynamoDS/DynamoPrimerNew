# Punkte

## Points in Dynamo

### Was ist ein Punkt?

Ein [Punkt](5-3\_points.md#point-as-coordinates) wird lediglich durch einen oder mehrere Werte, die sogenannten Koordinaten, definiert. Die Anzahl der Koordinatenwerte, die zum Definieren des Punkts benötigt werden, ist vom Koordinatensystem oder Kontext abhängig, in dem er sich befindet.

### 2D-/3D-Punkt

In Dynamo werden größtenteils Punkte verwendet, die sich im dreidimensionalen Weltkoordinatensystem befinden und drei Koordinaten [x,y,z] aufweisen (3D-Punkt in Dynamo).

![](../images/5-2/3/points-3dpointindynamo.jpg)

Ein 2D-Punkt in Dynamo hat zwei Koordinaten: [x,y].

![](../images/5-2/3/points-2dpointindynamo.jpg)

### Punkt auf Kurven und Flächen

Die Parameter für Kurven und Flächen sind kontinuierlich und erstrecken sich über die Kante der angegebenen Geometrie hinaus. Da die Formen, die den Parameterraum definieren, sich im dreidimensionalen Weltkoordinatensystem befinden, können parametrische Koordinaten jederzeit in Weltkoordinaten konvertiert werden. Der Punkt [0.2, 0.5] auf der Oberfläche entspricht beispielsweise dem Punkt [1.8, 2.0, 4.1] in Weltkoordinaten.

![](../images/5-2/3/points-xyzvscoordsysvsuv.jpg)

> 1. Punkt in angenommenen Weltkoordinaten (xyz)
> 2. Punkt relativ zu einem angegebenen Koordinatensystem (zylindrisch)
> 3. Punkt in UV-Koordinaten auf einer Oberfläche

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-2/3/Geometry for Computational Design - Points.dyn" %}

## Vertiefung...

Wenn Geometrie gewissermaßen die Sprache für ein Modell ist, sind Punkte das Alphabet. Punkte sind die Grundlage für die Erstellung aller anderen Geometrie: Sie benötigen mindestens zwei Punkte, um eine Kurve zu erstellen, mindestens drei Punkte für ein Polygon oder eine Netzfläche usw. Indem Sie die Position, Anordnung und Beziehung zwischen Punkten angeben (z. B. mithilfe einer Sinusfunktion), können Sie Geometrie höherer Ordnung definieren, die etwa als Kreise oder Kurven zu erkennen ist.

![Punkt zu Kurve](../images/5-2/3/PointsAsBuildingBlocks-1.jpg)

> 1. Ein Kreis, der die Funktionen `x=r*cos(t)` und `y=r*sin(t)` verwendet
> 2. Eine Sinuskurve, die die Funktionen `x=(t)` und `y=r*sin(t)` verwendet

### Punkt als Koordinaten

Punkte können auch in zweidimensionalen Koordinatensystemen vorhanden sein. Für unterschiedliche Räume bestehen unterschiedliche Notationskonventionen: So wird etwa bei einer Ebene [X,Y], bei einer Oberfläche jedoch [U,V] verwendet.

![Punkt als Koordinaten](../images/5-2/3/Coordinates.jpg)

> 1. Punkt in euklidischen Koordinatensystem: [x,y,z]
> 2. Punkt in einem Koordinatensystem mit Kurvenparameter: [t]
> 3. Punkt in Koordinatensystem mit Oberflächenparametern: [U,V]
