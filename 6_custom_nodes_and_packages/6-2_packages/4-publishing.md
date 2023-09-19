# Veröffentlichen von Paketen

In den vorigen Abschnitten wurde gezeigt, wie das _MapToSurface_-Paket sich aus benutzerdefinierten Blöcken und Beispieldateien zusammensetzt. Aber wie veröffentlichen Sie ein Paket, das lokal entwickelt wurde? Diese Fallstudie zeigt, wie Sie ein Paket aus einer Gruppe von Dateien in einem lokalen Ordner publizieren können.

\![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

Es gibt mehrere Möglichkeiten zum Publizieren von Paketen. Im Folgenden wird der von uns empfohlene Prozess beschrieben: **Sie veröffentlichen lokal, entwickeln lokal und veröffentlichen schließlich online**. Sie beginnen mit einem Ordner, der sämtliche Dateien im Paket enthält.

### Deinstallieren eines Pakets

Bevor Sie mit der Veröffentlichung des MapToSurface-Pakets beginnen, deinstallieren Sie das Paket aus der vorigen Lektion, falls Sie es installiert haben. Dadurch vermeiden Sie, mit identischen Paketen zu arbeiten.

Wechseln Sie zunächst zu Dynamo > Einstellungen > Package Manager, und klicken Sie neben MapToSurface auf das Menü mit den Punkten > Löschen.

![](../images/6-2/4/publishapackage-deletepackage.jpg)

Starten Sie dann Dynamo erneut. Wenn Sie beim erneuten Öffnen das Fenster _Pakete verwalten_ überprüfen, darf _MapToSurface_ dort nicht mehr vorhanden sein. Jetzt können Sie den Vorgang von Anfang an durchführen.

### Lokale Veröffentlichung von Paketen

{% hint style="warning" %} Das Publizieren von Dynamo-Paketen ist nur in Dynamo for Revit und Dynamo for Civil 3D aktiviert. Dynamo Sandbox verfügt nicht über Veröffentlichungsfunktionen. {% endhint %}

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Sie übermitteln Ihr Paket zum ersten Mal und alle Beispieldateien und benutzerdefinierten Blöcke befinden sich im selben Ordner. Nachdem dieser Ordner vorbereitet ist, können Sie jetzt auf Dynamo Package Manager hochladen.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Der Ordner enthält fünf benutzerdefinierte Blöcke (.dyf).
> 2. Er enthält außerdem fünf Beispieldateien (.dyn) und eine importierte Vektordatei (.svg). Diese Dateien dienen als einführende Übungen, die dem Benutzer die Arbeit mit den benutzerdefinierten Blöcken erläutern sollen.

Klicken Sie in Dynamo zunächst auf _Pakete > Neues Paket veröffentlichen_.

![](../images/6-2/4/publishapackage-publishlocally02.jpg)

In Fenster _Dynamo-Paket veröffentlichen_ ist das relevante Formular im linken Bereich bereits ausgefüllt.

![](../images/6-2/4/publishapackage-publishlocally03.jpg)

> 1. Durch Klicken auf _Datei hinzufügen_ wurden außerdem die Dateien aus der Ordnerstruktur rechts auf dem Bildschirm hinzugefügt. (Um andere Dateien als DYF-Dateien hinzuzufügen, müssen Sie den Dateityp im Browserfenster in **Alle Dateien (**_**.**_**)** ändern. Beachten Sie, dass sämtliche Dateien ohne Unterscheidung zwischen benutzerdefinierten Blöcken (.dyf) und Beispieldateien (.dyn) hinzugefügt wurden. Dynamo ordnet diese Objekte beim Veröffentlichen des Pakets in Kategorien ein.
> 2. Im Feld Gruppe wird definiert, in welcher Gruppe die benutzerdefinierten Blöcke in der Benutzeroberfläche von Dynamo abgelegt werden.
> 3. Veröffentlichen Sie das Paket, indem Sie auf Lokal publizieren klicken. Achten Sie darauf, auf _Lokal publizieren_ und **nicht** auf _Online publizieren_ zu klicken: Es sollen keine Duplikate im Package Manager erstellt werden.

Nach dem Publizieren werden die benutzerdefinierten Blöcke in der Gruppe DynamoPrimer oder in Ihrer Dynamo-Bibliothek angezeigt.

\![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Sehen wir jetzt im Stammverzeichnis nach, wie Dynamo das eben erstellte Paket formatiert hat. Klicken Sie dazu auf Dynamo > Einstellungen > Package Manager, und klicken Sie neben MapToSurface das Menü mit den Punkten > Stammverzeichnis anzeigen.

![](../images/6-2/4/publishapackage-publishlocally05.jpg)

Das Stammverzeichnis befindet sich am lokalen Speicherort des Pakets (da Sie es lokal veröffentlicht haben). Dynamo greift derzeit zum Lesen benutzerdefinierter Blöcke auf diesen Ordner zu. Aus diesem Grund müssen Sie das Verzeichnis lokal an einem dauerhaften Speicherort ablegen (d. h. nicht auf Ihrem Desktop). Der Ordner mit dem Dynamo-Paket ist wie folgt gegliedert.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. Im Ordner _bin_ befinden sich DLL-Dateien, die mit C#- oder Zero Touch-Bibliotheken erstellt wurden. Dieses Paket enthält keine solchen Dateien; dieser Ordner ist also in diesem Beispiel leer.
> 2. Im Ordner _dyf_ befinden sich die benutzerdefinierten Blöcke. Wenn Sie ihn öffnen, werden alle benutzerdefinierten Blöcke (DYF-Dateien) für das Paket angezeigt.
> 3. Im Ordner extra befinden sich alle zusätzlichen Dateien. Dies sind wahrscheinlich Dynamo-Dateien (.dyn) oder sonstige erforderliche Zusatzdateien (.svg, .xls, .jpeg, .sat usw.).
> 4. Die Datei pkg ist eine einfache Textdatei, die die Einstellungen des Pakets definiert. Diese Datei wird in Dynamo automatisch erstellt, Sie können Sie jedoch bearbeiten, wenn Sie detaillierte Einstellungen benötigen.

### Online-Veröffentlichung von Paketen

{% hint style="warning" %} Anmerkung: Führen Sie diesen Schritt bitte nicht aus, es sei denn, Sie möchten tatsächlich ein eigenes Paket veröffentlichen. {% endhint %}

![](../images/6-2/4/publishapackage-publishonline01.jpg)

> 1. Wenn das Paket zur Veröffentlichung bereit ist, wählen Sie unter Einstellungen > Package Manager die Schaltfläche rechts von MapToSurface und wählen _Veröffentlichen_.
> 2. Um ein bereits veröffentlichtes Paket zu aktualisieren, wählen Sie Version veröffentlichen. Dynamo aktualisiert dann Ihr Paket online mit den neuen Dateien im Stammverzeichnis des Pakets. Dieser einfache Schritt genügt.

### Version veröffentlichen

Wenn Sie die Dateien im Stammverzeichnis des veröffentlichten Pakets aktualisieren, können Sie über _Version veröffentlichen_ im Fenster _Pakete verwalten_ eine neue Version des Pakets veröffentlichen. Auf diese Weise können Sie nahtlos erforderliche Aktualisierungen Ihrer Inhalte vornehmen und sie für die Community bereitstellen. Die Funktion _Version veröffentlichen_ kann nur verwendet werden, wenn Sie der Verwalter des Pakets sind.
