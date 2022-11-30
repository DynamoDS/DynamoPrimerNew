# Math

Zahlen stellen die einfachste Form von Daten dar und sind am einfachsten durch mathematische Operationen zu verknüpfen. Von einfachen Operatoren etwa zur Division über trigonometrische Funktionen bis hin zu komplexen Formeln: Mathematische Operationen eignen sich ausgezeichnet zur Analyse von Zahlenverhältnissen und -mustern.

### Arithmetische Operatoren

Operatoren sind Komponenten für algebraische Funktionen, die zwei Eingabewerte benötigen, um einen Ausgabewert zu erhalten (etwa bei der Addition, Subtraktion, Multiplikation, Division usw.). Sie finden die Operatoren unter Operators > Actions.

| Symbol                                                | Name (Syntax)     | Eingaben                     | Ausgaben      |
| --------------------------------------------------- | ----------------- | -------------------------- | ------------ |
| ![](../images/5-3/2/addition.jpg)       | Addieren (**+**)       | var[]...[], var[]...[] | var[]...[] |
| ![](../images/5-3/2/Subtraction.jpg)    | Subtrahieren (**-**)  | var[]...[], var[]...[] | var[]...[] |
| ![](../images/5-3/2/Multiplication.jpg) | Multiplizieren (*****) | var[]...[], var[]...[] | var[]...[] |
| ![](../images/5-3/2/Division.jpg)       | Dividieren (**/**)    | var[]...[], var[]...[] | var[]...[] |

## Übung: Die Goldene Spirale-Formel

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/5-3/2/Building Blocks of Programs - Math.dyn" %}

### Teil I: Parametrische Formel

Kombinieren Sie Operatoren und Variablen, um mithilfe von **Formeln** eine komplexere Beziehung zu bilden. Verwenden Sie die Schieberegler, um eine Formel zu erstellen, die mit Eingabeparametern gesteuert werden kann.

1\. Wir erstellen eine Zahlenfolge, die für die Angabe "t" in der parametrischen Gleichung steht. Wir benötigen daher eine Liste mit genügend Werten zum Definieren einer Spirale.

**Number Sequence**: Definieren Sie eine Zahlenfolge mithilfe von drei Eingaben: _start, amount_ und _step_.

![](../images/5-3/2/math-partI-01.jpg)

2\. Mit dem oben beschriebenen Schritt haben Sie eine Liste von Zahlen erstellt, die die parametrische Domäne definieren. Als Nächstes erstellen Sie eine Gruppe von Blöcken, die die Goldene Spirale-Gleichung darstellen.

Die Goldene Spirale ist durch die folgende Gleichungen definiert:

$$ x = r cos θ = a cos θ e^{bθ} $$

$$ y = r sin θ = a sin θe^{bθ} $$

Die folgende Abbildung zeigt die goldene Spirale in visueller Programmierung. Beachten Sie bei der Betrachtung dieser Blockgruppe die Entsprechungen zwischen dem visuellen Programm und der schriftlichen Gleichung.

![](../images/5-3/2/math-partI-02.jpg)

> a. **Number Slider**: Fügen Sie im Ansichtsbereich zwei Number Sliders ein. Diese Schieberegler steuern die Variablen _a_ und _b_ in der parametrischen Gleichung. Sie stehen für einstellbare Konstanten bzw. Parameter, die Sie anpassen können, um das gewünschte Ergebnis zu erhalten.
>
> b. **Multiplication (*)**: Der Block für die Multiplikation wird mit einem Sternchen dargestellt. Er kommt hier mehrmals zum Einsatz, um Variablen miteinander zu multiplizieren.
>
> c. **Math.RadiansToDegrees**: Die Werte für "_t_" müssen in Grad umgewandelt werden, damit sie in den trigonometrischen Funktionen verwendet werden können. Dynamo verwendet per Vorgabe Grad zur Auswertung dieser Funktionen.
>
> d. **Math.Pow**: Als Funktion von "_t_" und der Zahl "_e_" erstellt dieser Block die Fibonacci-Folge.
>
> e. **Math.Cos und Math.Sin**: Diese beiden trigonometrischen Funktionen differenzieren die x- und y-Koordinaten der einzelnen parametrischen Punkte.
>
> f. **Watch**: Als Ausgabe erhalten Sie zwei Listen mit Werten für die _x_\- und _y_-Koordinaten der Punkte, aus denen die Spirale erstellt wird.

### Teil II: Von der Formel zur Geometrie

Die Gruppe von Blöcken aus dem letzten Schritt funktioniert einwandfrei, erfordert jedoch erheblichen Aufwand. Einen effizienteren Arbeitsablauf finden Sie unter [DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md). Dort wird beschrieben, wie Sie eine Reihe von Dynamo-Ausdrücken in ein und demselben Block definieren können. In den nächsten Schritten zeichnen Sie mithilfe der parametrischen Gleichung die Fibonacci-Spirale.

**Point.ByCoordinates**: Verbinden Sie den oberen Multiplikationsblock mit der _x_-Eingabe und den unteren mit der _y_-Eingabe. Dadurch wird auf dem Bildschirm eine parametrische Spirale aus Punkten angezeigt.

![](../images/5-3/2/math-partII-01.gif)

**Polycurve.ByPoints**: Verbinden Sie **Point.ByCoordinates** aus dem vorigen Schritt mit _points_. Für _connectLastToFirst_ wird keine Eingabe benötigt, da Sie keine geschlossene Kurve erstellen. Dadurch wird eine durch die im vorigen Schritt erstellten Punkte verlaufende Spirale erstellt.

![](../images/5-3/2/math-partII-02.jpg)

Damit haben Sie die Fibonacci-Spirale erstellt. Dies entwickeln Sie in zwei weiteren Übungen weiter, die hier als "Nautilus" und "Sonnenblume" bezeichnet werden. Dabei handelt es sich um Abstraktionen aus Systemen, die in der Natur vorkommen und gute Beispiele für zwei verschiedene Verwendungsweisen der Fibonacci-Spirale darstellen.

### Teil III: Von der Spirale zur Nautilusmuschel

**Circle.ByCenterPointRadius**: Verwenden Sie hier einen Circle-Block mit denselben Eingaben wie im vorigen Schritt. Als Radius ist der Wert _1.0_ vorgegeben, d. h., Sie sehen sofort die ausgegebenen Kreise. Die zunehmende Entfernung der Punkte vom Ursprung ist sofort ersichtlich.

![](../images/5-3/2/math-partIII-01.jpg)

**Number Sequence**: Dies ist das Original-Array für "_t_". Die Verbindung mit dem Radiuswert **Circle.ByCenterPointRadius** bewirkt, dass die Mittelpunkte der Kreise sich nach wie vor vom Ursprung entfernen, wobei jedoch auch ihr Radius zunimmt. Sie erhalten eine recht originelle Fibonacci-Grafik.

Versuchen Sie, dies in 3D darzustellen!

![](../images/5-3/2/math-partIII-02.gif)

### Teil IV: Von der Nautilusmuschel zur Phyllotaxis

Muster: Nachdem Sie eine Nautilusmuschel aus Kreisen erstellt haben, betrachten Sie jetzt parametrische Raster. Durch einfaches Drehen der Fibonacci-Spirale erstellen Sie ein Fibonacci-Raster. Das Ergebnis ist anhand des [Wachstums vom Sonnenblumensamen](https://blogs.unimelb.edu.au/sciencecommunication/2018/09/02/this-flower-uses-maths-to-reproduce/) modelliert.

Beginnen Sie mit demselben Schritt wie in der vorigen Übung, d. h., indem Sie mithilfe des **Point.ByCoordinates**-Blocks ein spiralförmiges Array aus Punkten erstellen.

\![](../images/5-3/2/math-part IV-01.jpg)

Als Nächstes führen Sie diese kleinen Schritte aus, um eine Reihe von Spiralen mit verschiedenen Drehungen zu erstellen.

![](../images/5-3/2/math-partIV-02.jpg)

> a. **Geometry.Rotate**: Es stehen mehrere Optionen für **Geometry.Rotate** zur Verfügung. Achten Sie darauf, den Block mit den Eingaben _geometry_,_basePlane_ und _degrees_ zu wählen. Verbinden Sie **Point.ByCoordinates** mit der geometry-Eingabe. Klicken Sie mit der rechten Maustaste auf diesen Block, und vergewissern Sie sich, dass die Vergitterung auf Kreuzprodukt festgelegt ist.
>
> ![](../images/5-3/2/math-partIV-03crossproduct.jpg)
>
> b. **Plane.XY**: Verbinden Sie dies mit der _basePlane_-Eingabe. Das Zentrum der Drehung ist der Ursprung, d. h. derselbe Punkt wie die Basis der Spirale.
>
> c. **Number Range**: Sie benötigen mehrere Drehungen für die degree-Eingabe. Dies erreichen Sie schnell mit der Komponente für den **Zahlenbereich**. Verbinden Sie diese mit der _degrees_-Eingabe.
>
> d. **Number**: Fügen Sie im Ansichtsbereich drei Zahlenblöcke übereinander ein, um den Zahlenbereich zu definieren. Weisen Sie diesen von oben nach unten die Werte _0.0,360.0,_ und _120.0_ zu. Diese Werte steuern die Drehung der Spirale. Beachten Sie die Ergebnisse der Ausgabe aus dem **Number Range**-Block, nachdem Sie die drei Zahlenblöcke mit ihm verbunden haben.

Die Ausgabe nimmt eine gewisse Ähnlichkeit mit einem Wirbel an. Passen Sie jetzt einige der für **Number Range** verwendeten Parameter an und beobachten Sie, wie sich die Ergebnisse verändern.

Ändern Sie die Schrittgröße für den **Number Range**-Block von _120.0_ in _36.0_. Damit erhalten Sie mehr Drehungen und daher ein dichteres Raster.

![](../images/5-3/2/math-partIV-04.jpg)

Ändern Sie die Schrittgröße für den **Number Range**-Block von _36.0_ in _3.6_. Dadurch erhalten Sie ein wesentlich dichteres Raster und die Richtung der Spiralen ist nicht mehr erkennbar. Damit haben Sie ein Sonnenblumenmuster erstellt.

![](../images/5-3/2/math-partIV-05.jpg)
