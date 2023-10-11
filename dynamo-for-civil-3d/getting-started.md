# Erste Schritte

Nachdem Sie sich einen Gesamtüberblick verschafft haben, können Sie jetzt direkt mit der Erstellung Ihres ersten Dynamo-Diagramms in Civil 3D beginnen.

{% hint style="info" %}

 Dies ist ein einfaches Beispiel zur Veranschaulichung der grundlegenden Dynamo-Funktionen. Es wird empfohlen, die Schritte in einem neuen, leeren Civil 3D-Dokument zu durchlaufen. 

{% endhint %}

## Öffnen von Dynamo

Als Erstes müssen Sie ein leeres Dokument in Civil 3D öffnen. Navigieren Sie in der Civil 3D-Multifunktionsleiste zur Registerkarte **Verwalten**, und suchen Sie die Gruppe **Visuelle Programmierung**.

![](<../.gitbook/assets/image (7).png>)

Klicken Sie auf die Schaltfläche **Dynamo**. Dadurch wird Dynamo in einem separaten Fenster gestartet.

{% hint style="info" %}

 **Was ist der Unterschied zwischen Dynamo und Dynamo Player?**

Dynamo wird zum Erstellen und Ausführen von Diagrammen verwendet. Dynamo Player bietet eine einfache Möglichkeit zum Ausführen von Diagrammen, ohne sie in Dynamo öffnen zu müssen.

Gehen Sie zum Abschnitt [dynamo-player.md](dynamo-player.md "mention"), wenn sie ihn ausprobieren möchten. 

{% endhint %}

## Starten eines neuen Diagramms

Wenn Dynamo geöffnet ist, wird der Startbildschirm angezeigt. Klicken Sie auf **Neu**, um einen leeren Arbeitsbereich zu öffnen.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Dynamo-Startbildschirm</p></figcaption></figure>

{% hint style="info" %}

 **Wie sieht es mit Beispielen aus?**

Dynamo for Civil 3D enthält einige vordefinierte Diagramme, die Ihnen Ideen zur Verwendung von Dynamo liefern können. Wir empfehlen, dass Sie sich diese sowie die hier in der Einführung erwähnten [sample-workflows](sample-workflows/ "mention") ansehen. 

{% endhint %}

## Hinzufügen von Blöcken

Sie sollten jetzt einen leeren Arbeitsbereich sehen. Erleben Sie Dynamo in Aktion! Hier ist unser Ziel:

>  :dart: **Erstellen Sie ein Dynamo-Diagramm, das Text in den Modellbereich einfügt.**

Ganz einfach, nicht wahr? Aber bevor wir beginnen, müssen wir einige Grundlagen klären.

Die wichtigsten Bausteine eines Dynamo-Diagramms werden **Blöcke** genannt. Ein Block ist wie ein kleiner Computer: Sie geben Daten ein, der Computer bearbeitet die Daten und gibt ein Ergebnis aus. Dynamo for Civil 3D verfügt über eine **Bibliothek** von Blöcken, die Sie mit **Drähten** verbinden können, um ein **Diagramm** zu erstellen, das größere und bessere Aufgaben bewältigen kann als ein einzelner Block.

{% hint style="info" %}

 **Was ist, wenn ich noch nie mit Dynamo gearbeitet habe?**

Einiges davon ist möglicherweise ziemlich neu für Sie. Aber keine Sorge. Diese Abschnitte helfen Ihnen.

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") 

{% endhint %}

Erstellen wir nun unser Diagramm. Hier sehen Sie eine Liste aller benötigten Blöcke.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

Sie können diese Blöcke finden, indem Sie ihre Namen in die Suchleiste in der Bibliothek eingeben, oder indem Sie mit der rechten Maustaste auf eine beliebige Stelle im Ansichtsbereich klicken und dort suchen.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>Blöcke können aus der Bibliothek oder durch Klicken mit der rechten Maustaste in den Ansichtsbereich platziert werden.</p></figcaption></figure>

{% hint style="info" %}

 **Wie kann ich feststellen, welche Blöcke ich verwenden sollte und wo ich sie finde?**

Blöcke in der Bibliothek werden in logische Kategorien eingeteilt, abhängig davon, welche Funktion sie haben. Eine ausführlichere Beschreibung finden Sie im Abschnitt [node-library.md](node-library.md "mention"). 

{% endhint %}

Hier sehen Sie, wie das endgültige Diagramm aussehen sollte.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>Das fertige Diagramm</p></figcaption></figure>

Fassen wir zusammen, was wir hier erreicht haben:

> 1. Wir haben das Dokument ausgewählt, in dem wir arbeiten möchten. In diesem (und in vielen anderen Fällen) möchten wir in Civil 3D im aktiven Dokument arbeiten.
> 2. Wir haben den Zielblock definiert, in dem das Textobjekt erstellt werden soll (in diesem Fall der Modellbereich).
> 3. Wir haben mithilfe eines _String_-Blocks angegeben, auf welchem Layer der Text platziert werden soll.
> 4. Wir haben einen Punkt mithilfe des _Point.ByCoordinates_-Blocks erstellt, um die Position zu definieren, an der der Text platziert werden soll.
> 5. Wir haben die X- und Y-Koordinaten des Texteinfügepunkts mithilfe von zwei _Number Slider_-Blöcken definiert.
> 6. Wir haben den Inhalt des Text-Objekts mithilfe eines anderen _String_-Blocks definiert.
> 7. Schließlich haben wir das Text-Objekt erstellt.

Sehen wir uns nun die Ergebnisse unseres brandneuen Diagramms an.

## Anzeigen von Ergebnissen

Vergewissern Sie sich in Civil 3D, dass die Registerkarte **Modell** ausgewählt ist. Das neue von Dynamo erstellte Text-Objekt sollte angezeigt werden.

{% hint style="info" %}

 Wenn Sie den Text nicht sehen können, müssen Sie möglicherweise den Befehl ZOOM -> GRENZEN ausführen, um zur richtigen Stelle zu zoomen. 

{% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

Ausgezeichnet. Jetzt nehmen wir einige Änderungen am Text vor.

Ändern Sie im Dynamo-Diagramm einige der Eingabewerte, z. B. die Textzeichenfolge, die Koordinaten des Einfügepunkts usw. Der Text sollte automatisch in Civil 3D aktualisiert werden. Beachten Sie auch, dass beim Trennen eines der Eingabeanschlüsse der Text entfernt wird. Wenn Sie den Anschluss wieder verbinden, wird der Text erneut erstellt. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>Das fertige Diagramm in Aktion</p></figcaption></figure>

</div>

{% hint style="info" %}

 **Warum fügt Dynamo nicht bei jeder Ausführung des Diagramms ein neues Textobjekt ein?**

Vorgabemäßig "merkt sich" Dynamo die erstellten Objekte. Wenn Sie die Blockeingabewerte ändern, werden die Objekte in Civil 3D aktualisiert, anstatt neue Objekte zu erstellen. Weitere Informationen zu diesem Verhalten finden Sie im Abschnitt [object-binding.md](advanced-topics/object-binding.md "mention"). 

{% endhint %}

> :tada: Mission erfüllt!

## Nächste Schritte

In diesem Beispiel wird nur oberflächlich erläutert, was Sie mit Dynamo for Civil 3D tun können. Lesen Sie weiter, um mehr zu erfahren!
