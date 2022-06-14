# Attraktorpunkte

Attraktorpunkte eignen sich hervorragend zum Experimentieren mit geometrischen Mustern. Sie können verwendet werden, um schrittweise Änderungen an Objekten basierend auf ihrer Entfernung zu erstellen.

In diesem Arbeitsablauf lernen Sie Folgendes:

* Erstellen, Verwalten und Bearbeiten von Listen.
* Verschieben von Punkten in der 3D-Vorschau durch direkte Bearbeitung.
* Ändern des Ausführungsmodus.

![](../images/10-1/2/attractor1.gif)

## Definieren der Ziele

In dieser Übung erstellen Sie einen Kreis (_Ziel_), dessen Radius durch die Entfernung zu einem nahegelegenen Punkt (_Beziehung_) definiert wird.

![Handskizze des Kreises](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> Ein Punkt, der eine entfernungsabhängige Beziehung definiert, wird häufig als "Attraktor" bezeichnet. In diesem Beispiel wird die Entfernung zum Attraktorpunkt verwendet, um anzugeben, wie groß der Kreis sein soll.

## Nächste Schritte

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

Nachdem Sie die Ziele und Beziehungen skizziert haben, können Sie mit dem Erstellen des Diagramms beginnen. Sie benötigen die Blöcke, die die Reihenfolge der Aktionen darstellen, die von Dynamo ausgeführt werden. Wir fügen zunächst die folgenden Blöcke hinzu: **Number**, **Number Slider**, **Point.ByCoordinates**, **Geometry.DistanceTo, Circle.ByCenterPointRadius.**

![](<../images/10-1/2/attractor (2).png>)

> 1. Input > Basic > **Number**
> 2. Input > Basic > **Number Slider**
> 3. Geometry > Points > Point > **By Coordinates(x,y,z)**
> 4. Geometry > Modifiers> Geometry > **DistanceTo**
> 5. Geometry > Curves > Circle > **ByCenterPointRadius**

### Verbinden von Blöcken mit Drähten

Nachdem Sie mehrere Blöcke zum Arbeitsbereich hinzugefügt haben, müssen Sie die Anschlüsse der Blöcke mit Drähten verbinden. Diese Verbindungen definieren den Datenfluss.

![](<../images/10-1/2/attractor (3).png>)

> 1. **Number** zu **Point.ByCoordinates**
> 2. **Number Sliders** zu **Point.ByCoordinates**
> 3. **Point.ByCoordinates** (2) zu **DistanceTo**
> 4. **Point.ByCoordinates** und **DistanceTo** zu **Circle.ByCenterPointRadius**

### Programm ausführen

Nachdem Sie den Programmablauf definiert haben, müssen Sie Dynamo noch mitteilen, wie es ausgeführt werden soll. Wenn das Programm ausgeführt wird (entweder automatisch oder durch Klicken auf Ausführen im manuellen Modus), werden Daten über die Drähte übertragen und die Ergebnisse in der 3D-Vorschau angezeigt.

![](<../images/10-1/2/attractor (4).png>)

> 1. (Auf Ausführen klicken): Wenn sich die Ausführungsleiste im manuellen Modus befindet, muss auf Ausführen geklickt werden, um das Diagramm auszuführen.
> 2. Blockvorschau: Durch Verschieben des Mauszeigers auf das Feld in der rechten unteren Ecke eines Blocks wird ein Popup-Fenster mit den Ergebnissen angezeigt.
> 3. 3D-Vorschau: Wenn ein beliebiger der verfügbaren Blöcke Geometrie erzeugt, wird eine 3D-Vorschau angezeigt
> 4. Die Ausgabe von Geometrie im Erstellungsblock.

### Hinzufügen von **Codeblöcken**

Wenn das Programm ordnungsgemäß funktioniert, wird in der 3D-Vorschau ein Kreis angezeigt, der durch den Attraktorpunkt verläuft. Anschließend können Sie weitere Details oder Steuerelemente hinzufügen. Passen Sie die Eingabe für den Kreis-Block an, um den Einfluss auf den Radius zu kalibrieren. Fügen Sie einen weiteren **Number Slider**-Block zum Arbeitsbereich hinzu, und doppelklicken Sie auf einen leeren Bereich im Arbeitsbereich, um einen **Code Block**-Block hinzuzufügen. Bearbeiten Sie das Feld im Codeblock, indem Sie `X/Y` angeben.

![](<../images/10-1/2/attractor (5).png>)

> 1. **Code Block**
> 2. **DistanceTo** und **Number Slider** zu **Code Block**
> 3. **Code Block** zu **Circle.ByCenterPointRadius**

### Verwenden von Sequenzen

Einfach zu beginnen und dann die Komplexität zu erhöhen stellt eine effektive Möglichkeit dar, Programme schrittweise zu entwickeln. Wenn das Programm also für einen Kreis funktioniert, können Sie es auch für mehrere Kreise ausführen. Wenn Sie anstelle eines Mittelpunkts ein Raster an Punkten verwenden und diese Änderung in der resultierenden Datenstruktur umsetzen, werden von Ihrem Programm viele Kreise erzeugt, jeder mit einem eindeutigen Radiuswert, der durch die kalibrierte Entfernung zum Attraktorpunkt definiert wird.

![](<../images/10-1/2/attractor (6).png>)

> 1. Fügen Sie einen **Number Sequence**-Block hinzu und ersetzen Sie die Eingaben von **Point.ByCoordinates**: Klicken Sie mit der rechten Maustaste auf Point.ByCoordinates und wählen Sie Vergitterung > Kreuzprodukt.
> 2. Fügen Sie einen **Flatten**-Block nach Point.ByCoordinates hinzu. Zum vollständigen Abflachen einer Liste belassen Sie Vorgabeeinstellung von `amt` auf `-1`.
> 3. Die 3D-Vorschau wird mit einem Raster von Kreisen aktualisiert

### Mit Direktbearbeitung anpassen

In manchen Fällen ist die numerische Bearbeitung nicht der richtige Ansatz. Jetzt können Sie beim Navigieren in der Hintergrund-3D-Vorschau Punktgeometrie manuell drücken und ziehen. Sie können auch andere Geometrie steuern, die durch einen Punkt konstruiert wurde. **Sphere.ByCenterPointRadius** kann beispielsweise ebenfalls direkt bearbeitet werden. Sie können die Position eines Punkts aus einer Reihe von X-, Y- und Z-Werten mit **Point.ByCoordinates** steuern. Mit dem Direktbearbeitungsansatz sind Sie jedoch in der Lage, die Werte der Schieberegler zu aktualisieren, indem Sie den Punkt im Modus **Navigation in 3D-Vorschau** manuell verschieben. Dieser Ansatz bietet eine intuitivere Methode zum Steuern von mehreren diskreten Werten, die eine Punktposition identifizieren.

![](<../images/10-1/2/attractor (7).png>)

> 1. Um die **Direktbearbeitung** zu verwenden, wählen Sie die Gruppe mit dem zu verschiebenden Punkt aus – über dem ausgewählten Punkt werden Pfeile angezeigt.
> 2. Wechseln Sie in den Modus **Navigation in 3D-Vorschau**.

![](../images/10-1/2/attractor\(8\).png)

> 1. Wenn Sie den Cursor über den Punkt bewegen, werden die X -, Y - und Z-Achsen angezeigt.
> 2. Klicken Sie, und ziehen Sie den farbigen Pfeil, um die entsprechende Achse zu verschieben. Die **Number Slider**-Werte werden live mit dem manuell verschobenen Punkt aktualisiert.

![](<../images/10-1/2/attractor (1).png>)

> 1. Beachten Sie, dass vor der **Direktbearbeitung** nur ein Schieberegler an die **Point.ByCoordinates**-Komponente angeschlossen war. Wenn Sie den Punkt manuell in X-Richtung verschieben, erstellt Dynamo automatisch einen neuen **Number Slider** für die X-Eingabe.

