# Lichtraumprofil

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

Die Entwicklung kinematischer Profile für die Lichtraumvalidierung ist ein wichtiger Bestandteil der Schienenkonstruktion. Dynamo kann verwendet werden, um Volumenkörper für die Profile zu erstellen, anstatt komplexe 3D-Profilkörper-Querschnittsbestandteile erstellen und verwalten zu müssen.

## Ziel

> :dart: Verwenden Sie einen Block für einen Fahrzeug-Längsschnitt, um 3D-Volumenkörper mit Lichtraumprofil entlang eines 3D-Profilkörpers zu erstellen.

## Wichtige Konzepte

> * Arbeiten mit 3D-Profilkörper-Elementkanten
> * Transformieren von Geometrie zwischen Koordinatensystemen
> * Erstellen von Volumenkörpern durch Anhebung
> * Steuern des Blockverhaltens mit Vergitterungseinstellungen

## Kompatibilität der Versionen

{% hint style="success" %} Dieses Diagramm wird in **Civil 3D 2020** und höher ausgeführt. {% endhint %}

## Datensatz

Laden Sie zunächst die folgenden Beispieldateien herunter, und öffnen Sie dann die DWG-Datei und das Dynamo-Diagramm.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## Lösung

Hier sehen Sie einen Überblick über die Logik in diesem Diagramm.

> 1. Elementkanten aus der angegebenen 3D-Profilkörper-Basislinie abrufen
> 2. Koordinatensysteme entlang der 3D-Profilkörper-Elementkante mit dem gewünschten Abstand erstellen
> 3. Blockgeometrie des Längsschnitts in die Koordinatensysteme umwandeln
> 4. Volumenkörper zwischen den Längsschnitten anheben
> 5. Volumenkörper in Civil 3D erstellen

Los gehts!

### Abrufen von 3D-Profilkörperdaten

Unser erster Schritt ist das Abrufen von 3D-Profilkörperdaten. Wir wählen das 3D-Profilkörpermodell nach Name aus, rufen eine bestimmte Basislinie innerhalb des 3D-Profilkörpers ab und erhalten dann eine Elementkante innerhalb der Basislinie anhand ihres Punktcodes.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>Auswählen von 3D-Profilkörpern, Basislinien und Elementkanten</p></figcaption></figure>

### Erstellen von Koordinatensystemen

Als Nächstes generieren wir **Koordinatensysteme** entlang der Elementkanten eines 3D-Profilkörpers zwischen einer bestimmten Anfangs- und Endstation. Diese Koordinatensysteme werden verwendet, um die Blockgeometrie des Fahrzeug-Längsschnitts am 3D-Profilkörper auszurichten.

{% hint style="info" %} Wenn Koordinatensysteme neu für Sie sind, finden Sie im Abschnitt [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") weitere Informationen. {% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>Abrufen von Koordinatensystemen entlang der 3D-Profilkörper-Elementkanten</p></figcaption></figure>

> 1. Beachten Sie das kleine **XXX** in der unteren rechten Ecke des Blocks. Dies bedeutet, dass die Vergitterungseinstellungen des Blocks auf _Kreuzprodukt_ festgelegt sind. Dies ist erforderlich, um die Koordinatensysteme an den gleichen Stationswerten für beide Elementkanten zu generieren.

{% hint style="info" %} Wenn die Blockvergitterung neu für Sie ist, finden Sie im Abschnitt [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention") weitere Informationen. {% endhint %}

### Transformieren von Blockgeometrie

Jetzt müssen wir eine Anordnung der Fahrzeug-Längsschnitte entlang der Elementkanten erstellen. Dafür transformieren wir die Geometrie mithilfe des **Geometry.Transform**-Blocks aus der Blockdefinition des Fahrzeug-Längsschnitts. Das Visualisieren dieses Konzepts ist schwierig. Bevor wir uns die Blöcke ansehen, sehen wir uns eine Grafik an, die zeigt, was passieren wird.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>Eine Visualisierung der Geometrietransformation zwischen Koordinatensystemen</p></figcaption></figure>

Im Prinzip wird also die Dynamo-Geometrie aus einer _einzelnen_ Blockdefinition während der Erstellung einer Anordnung entlang der Elementkante verschoben/gedreht. Ziemlich cool! Hier sehen Sie die Blocksequenz.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. Hiermit wird die Blockdefinition aus dem Dokument abgerufen.
> 2. Diese Blöcke übernehmen die Dynamo-Geometrie der Objekte innerhalb des Blocks.
> 3. Diese Blöcke definieren im Wesentlichen das Koordinatensystem, _aus_ dem die Geometrie transformiert wird.
> 4. Und schließlich führt dieser Block die eigentliche Transformation der Geometrie durch.
> 5. Beachten Sie die _längste_ Vergitterung in diesem Block.

Und so sieht dies in Dynamo aus.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>Die Blockgeometrie des Fahrzeug-Längsschnitts nach der Transformation</p></figcaption></figure>

### Erstellen von Volumenkörpern

Gute Neuigkeiten! Die harte Arbeit ist getan. Jetzt müssen wir nur noch Volumenkörper zwischen den Längsschnitten generieren. Dies lässt sich einfach mithilfe des **Solid.ByLoft**-Blocks bewerkstelligen.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

Hier sehen Sie das Ergebnis. Beachten Sie, dass es sich um Dynamo-Volumenkörper handelt, die in Civil 3D noch erstellt werden müssen.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>Die Dynamo-Volumenkörper nach dem Anheben</p></figcaption></figure>

### Ausgeben von Volumenkörpern in Civil 3D

Der letzte Schritt besteht darin, die generierten Volumenkörper im Modellbereich auszugeben. Wir ordnen ihnen auch eine Farbe zu, damit sie sehr leicht zu erkennen sind.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Ausgeben der Volumenkörper in Civil 3D</p></figcaption></figure>

### Ergebnis

Hier sehen Sie ein Beispiel für die Ausführung des Diagramms mit **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Ausführen des Diagramms mit Dynamo Player und Anzeigen der Ergebnisse in Civil 3D</p></figcaption></figure>

{% hint style="info" %} Wenn Dynamo Player neu für Sie ist, finden Sie im Abschnitt [dynamo-player.md](../../dynamo-player.md "mention") weitere Informationen. {% endhint %}

> :tada: Mission erfüllt!

## Ideen

Im Folgenden finden Sie einige Anregungen, wie Sie die Funktionen dieses Diagramms erweitern können.

{% hint style="info" %} Fügen Sie eine Funktion hinzu, mit der Sie **verschiedene Stationsbereiche** separat für jede Spur verwenden können. {% endhint %}

{% hint style="info" %} **Teilen Sie die Volumenkörper** in kleinere Segmente, die einzeln auf Kollisionen analysiert werden können. {% endhint %}

{% hint style="info" %} Überprüfen Sie, ob sich die Profilvolumenkörper ** mit Objekten überschneiden**, und färben Sie kollidierende Objekte ein. {% endhint %}
