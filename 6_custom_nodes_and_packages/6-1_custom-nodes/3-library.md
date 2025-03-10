# In der Bibliothek publizieren

Sie haben einen benutzerdefinierten Block erstellt und ihn auf einen bestimmten Prozess im Dynamo-Diagramm angewendet. Da dieser Block sehr nützlich ist, möchten Sie ihn in die Dynamo-Bibliothek aufnehmen, damit er in anderen Diagrammen referenziert werden kann. Dazu müssen Sie den Block lokal veröffentlichen. Sie gehen dabei auf ähnliche Weise vor wie beim Veröffentlichen von Paketen, das im nächsten Kapitel im Detail behandelt wird.

Indem Sie den Block lokal veröffentlichen, stellen Sie ihn in Ihrer Dynamo-Bibliothek bereit und können darauf zugreifen, wenn Sie eine neue Sitzung öffnen. Wenn ein Block nicht publiziert wird, muss er für ein Dynamo-Diagramm, das diesen benutzerdefinierten Block referenziert, in dessen Ordner enthalten sein (oder über _Datei > Bibliothek importieren_ in Dynamo importiert werden).

{% hint style="warning" %} Sie können benutzerdefinierte Blöcke und Pakete aus Dynamo Sandbox in Version 2.17 und höher publizieren, sofern diese keine Abhängigkeiten zur Host-API aufweisen. In älteren Versionen ist das Publizieren von benutzerdefinierten Blöcken und Paketen nur in Dynamo for Revit und Dynamo for Civil 3D aktiviert. {% endhint %}

## Übung: Lokales Veröffentlichen eines benutzerdefinierten Blocks

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

Verwenden Sie weiterhin den benutzerdefinierten Block, den Sie im vorigen Abschnitt erstellt haben. Nachdem Sie den benutzerdefinierten PointsToSurface-Block geöffnet haben, wird das Diagramm im Editor für benutzerdefinierte Blöcke von Dynamo angezeigt. Sie können einen benutzerdefinierten Block auch durch Doppelklicken im Diagrammeditor von Dynamo öffnen.

![](../images/6-1/3/publishcustomnodelocally01.jpg)

Um einen benutzerdefinierten Block lokal zu veröffentlichen, klicken Sie mit der rechten Maustaste in den Ansichtsbereich und wählen Sie _Diesen benutzerdefinierten Block veröffentlichen_.

![](../images/6-1/3/publishcustomnodeexercise-02.jpg)

Geben Sie wie in der Abbildung oben gezeigt die nötigen Informationen ein und wählen Sie _Lokal publizieren_. Beachten Sie, dass das Feld Gruppe den Haupteintrag angibt, der über das Dynamo-Menü aufgerufen wird.

<figure><img src="../../.gitbook/assets/publish_a_package.png" alt=""><figcaption></figcaption></figure>

Wählen Sie einen Ordner, in dem alle benutzerdefinierten Blöcke gespeichert werden sollen, die Sie lokal veröffentlichen werden. Dynamo prüft diesen Ordner jedes Mal beim Laden der Anwendung. Achten Sie daher darauf, dass der Ordner sich an einem dauerhaften Speicherort befindet. Navigieren Sie zu diesem Ordner und wählen Sie _Ordner auswählen_. Damit haben Sie den Dynamo-Block lokal publiziert. Er steht jetzt jedes Mal, wenn Sie das Programm laden, in der Dynamo-Bibliothek zur Verfügung.

![](../images/6-1/3/publishcustomnodeexercise-04.jpg)

Um den Speicherort des benutzerdefinierten Blocks zu überprüfen, wechseln Sie zu _Dynamo > Voreinstellungen > Paketeinstellungen > Pfade für Blöcke und Pakete_.

<figure><img src="../../.gitbook/assets/settings.png" alt="" width="520"><figcaption></figcaption></figure>

In diesem Fenster wird eine Liste von Pfaden angezeigt.

<figure><img src="../../.gitbook/assets/package-locations.png" alt=""><figcaption></figcaption></figure>

> 1. _Documents\\DynamoCustomNodes..._ gibt den Speicherort der von Ihnen lokal veröffentlichten benutzerdefinierten Blöcke an.
> 2. _AppData\\Roaming\\Dynamo..._ bezieht sich auf den vorgegebenen Speicherort der online installierten Dynamo-Pakete.
> 3. Es ist sinnvoll, den Pfad des lokalen Ordners in der Liste nach unten zu verschieben (indem Sie auf den nach unten zeigenden Pfeil links neben dem Pfadnamen klicken). Der zuoberst stehende Ordner ist die Vorgabe für die Installation von Paketen. Indem Sie den vorgegebenen Installationspfad für Dynamo-Pakete als Vorgabe beibehalten, stellen Sie daher sicher, dass Online-Pakete und Ihre lokal veröffentlichten Blöcke separat abgelegt werden.

Hier wurde die Reihenfolge der Pfadnamen vertauscht, damit Pakete unter dem Vorgabepfad von Dynamo installiert werden.

<figure><img src="../../.gitbook/assets/updated-package-locations.png" alt=""><figcaption></figcaption></figure>

Wenn Sie zu diesem lokalen Ordner navigieren, finden Sie den ursprünglichen benutzerdefinierten Block im Ordner _dyf_. Dies ist die Erweiterung von Dateien für benutzerdefinierte Dynamo-Blöcke. Sie können die Datei in diesem Ordner bearbeiten. Der Block wird dann in der Benutzeroberfläche aktualisiert. Sie können auch weitere Blöcke im Ordner _DynamoCustomNode_ hinzufügen. Diese werden beim Neustart von Dynamo Ihrer Bibliothek hinzugefügt.

![](../images/6-1/3/publishcustomnodeexercise-08.jpg)

Wenn Sie Dynamo jetzt laden, wird der Block PointsToSurface jedes Mal in der Gruppe DynamoPrimer Ihrer Dynamo-Bibliothek angezeigt.

![](../images/6-1/3/publishcustomnodeexercise-09.jpg)
