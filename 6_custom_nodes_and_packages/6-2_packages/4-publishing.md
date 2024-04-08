# Veröffentlichen von Paketen

In den vorigen Abschnitten wurde gezeigt, wie das _MapToSurface_-Paket sich aus benutzerdefinierten Blöcken und Beispieldateien zusammensetzt. Aber wie veröffentlichen Sie ein Paket, das lokal entwickelt wurde? Diese Fallstudie zeigt, wie Sie ein Paket aus einer Gruppe von Dateien in einem lokalen Ordner publizieren können.

\![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

Es gibt mehrere Möglichkeiten zum Publizieren von Paketen. Im Folgenden wird der von uns empfohlene Prozess beschrieben: **Sie publizieren lokal, entwickeln lokal und publizieren schließlich online**. Sie beginnen mit einem Ordner, der sämtliche Dateien im Paket enthält.

### Deinstallieren eines Pakets

Bevor Sie mit der Veröffentlichung des MapToSurface-Pakets beginnen, deinstallieren Sie das Paket aus der vorigen Lektion, falls Sie es installiert haben. Dadurch vermeiden Sie, mit identischen Paketen zu arbeiten.

Beginnen Sie, indem Sie zu Pakete > Package Manager > Registerkarte Installierte Pakete navigieren. Klicken Sie neben MapToSurface auf das Menü mit den drei Punkten und dann auf Löschen.

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

Starten Sie dann Dynamo erneut. Wenn Sie beim erneuten Öffnen das Fenster _Pakete verwalten_ überprüfen, darf _MapToSurface_ dort nicht mehr vorhanden sein. Jetzt können Sie den Vorgang von Anfang an durchführen.

### Lokale Veröffentlichung von Paketen

{% hint style="warning" %} Sie können benutzerdefinierte Blöcke und Pakete aus Dynamo Sandbox in Version 2.17 und höher publizieren, sofern diese keine Abhängigkeiten zur Host-API aufweisen. In älteren Versionen ist das Publizieren von benutzerdefinierten Blöcken und Paketen nur in Dynamo for Revit und Dynamo for Civil 3D aktiviert. {% endhint %}

> Laden Sie die Beispieldatei herunter, indem Sie auf den folgenden Link klicken.
>
> Eine vollständige Liste der Beispieldateien finden Sie im Anhang.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Sie übermitteln Ihr Paket zum ersten Mal und alle Beispieldateien und benutzerdefinierten Blöcke befinden sich im selben Ordner. Nachdem dieser Ordner vorbereitet ist, können Sie jetzt auf Dynamo Package Manager hochladen.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Der Ordner enthält fünf benutzerdefinierte Blöcke (.dyf).
> 2. Er enthält außerdem fünf Beispieldateien (.dyn) und eine importierte Vektordatei (.svg). Diese Dateien dienen als einführende Übungen, die dem Benutzer die Arbeit mit den benutzerdefinierten Blöcken erläutern sollen.

Klicken Sie in Dynamo zunächst auf _Pakete > Package Manager > Registerkarte Neues Paket publizieren_.

Füllen Sie auf der Registerkarte _Paket publizieren_ die entsprechenden Felder auf der linken Seite des Fensters aus.

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

Als Nächstes fügen wir Paketdateien hinzu. Sie können Dateien einzeln oder ganze Ordner mit Dateien hinzufügen, indem Sie Verzeichnis hinzufügen (1) auswählen. Um Dateien hinzuzufügen, die keine DYF-Dateien sind, ändern Sie den Dateityp im Browser-Fenster in **Alle Dateien(**_._**)**. Beachten Sie, dass sämtliche Dateien ohne Unterscheidung zwischen benutzerdefinierten Blöcken (.dyf) und Beispieldateien (.dyn) hinzugefügt werden. Dynamo ordnet diese Objekte beim Publizieren des Pakets in Kategorien ein.

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

Sobald Sie den Ordner MapToSurface ausgewählt haben, zeigt Package Manager den Inhalt des Ordners an. Wenn Sie Ihr eigenes Paket mit einer komplexen Ordnerstruktur hochladen und nicht möchten, dass Dynamo Änderungen an der Ordnerstruktur vornimmt, können Sie die Option Ordnerstruktur beibehalten aktivieren. Diese Option ist für erfahrene Benutzer gedacht. Wenn Ihr Paket nicht absichtlich auf eine bestimmte Weise konfiguriert ist, sollten Sie diese Option deaktiviert lassen und Dynamo erlauben, die Dateien bei Bedarf zu organisieren. Klicken Sie auf Weiter, um fortzufahren.

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

Hier können Sie eine Vorschau der Organisation Ihrer Paketdateien in Dynamo vor dem Publizieren anzeigen. Klicken Sie zum Fortfahren auf Fertig stellen.

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

Publizieren Sie das Paket, indem Sie auf Lokal publizieren (1) klicken. Achten Sie darauf, auf _Lokal publizieren_ und **nicht** auf _Online publizieren_ zu klicken, um zu vermeiden, dass Duplikate im Package Manager erstellt werden.

Nach dem Publizieren werden die benutzerdefinierten Blöcke in der Gruppe DynamoPrimer oder in Ihrer Dynamo-Bibliothek angezeigt.

\![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Sehen wir jetzt im Stammverzeichnis nach, wie Dynamo das eben erstellte Paket formatiert hat. Navigieren Sie dazu zur Registerkarte Installierte Pakete, klicken Sie neben MapToSurface auf das Menü mit den drei Punkten, und wählen Sie Stammverzeichnis anzeigen aus.

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

Das Stammverzeichnis befindet sich am lokalen Speicherort des Pakets (da Sie es lokal veröffentlicht haben). Dynamo greift derzeit zum Lesen benutzerdefinierter Blöcke auf diesen Ordner zu. Aus diesem Grund müssen Sie das Verzeichnis lokal an einem dauerhaften Speicherort ablegen (d. h. nicht auf Ihrem Desktop). Der Ordner mit dem Dynamo-Paket ist wie folgt gegliedert.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. Im Ordner _bin_ befinden sich DLL-Dateien, die mit C#- oder Zero Touch-Bibliotheken erstellt wurden. Dieses Paket enthält keine solchen Dateien; dieser Ordner ist also in diesem Beispiel leer.
> 2. Im Ordner _dyf_ befinden sich die benutzerdefinierten Blöcke. Wenn Sie ihn öffnen, werden alle benutzerdefinierten Blöcke (DYF-Dateien) für das Paket angezeigt.
> 3. Im Ordner extra befinden sich alle zusätzlichen Dateien. Dies sind wahrscheinlich Dynamo-Dateien (.dyn) oder sonstige erforderliche Zusatzdateien (.svg, .xls, .jpeg, .sat usw.).
> 4. Die Datei pkg ist eine einfache Textdatei, die die Einstellungen des Pakets definiert. Diese Datei wird in Dynamo automatisch erstellt, Sie können Sie jedoch bearbeiten, wenn Sie detaillierte Einstellungen benötigen.

### Online-Veröffentlichung von Paketen

{% hint style="warning" %} Anmerkung: Führen Sie diesen Schritt bitte nicht aus, es sei denn, Sie möchten tatsächlich ein eigenes Paket veröffentlichen. {% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. Wenn Sie zum Publizieren bereit sind, wählen Sie im Fenster Pakete > Package Manager > Installierte Pakete die Schaltfläche rechts neben dem Paket, das Sie publizieren möchten, und dann Publizieren aus.
2. Um ein bereits veröffentlichtes Paket zu aktualisieren, wählen Sie Version veröffentlichen. Dynamo aktualisiert dann Ihr Paket online mit den neuen Dateien im Stammverzeichnis des Pakets. Dieser einfache Schritt genügt.

### Version veröffentlichen

Wenn Sie die Dateien im Stammverzeichnis des publizierten Pakets aktualisieren, können Sie über _Version veröffentlichen..._ auf der Registerkarte _Meine Pakete_ auch eine neue Version des Pakets publizieren. Auf diese Weise können Sie nahtlos erforderliche Aktualisierungen Ihrer Inhalte vornehmen und sie für die Community bereitstellen. Die Funktion _Version veröffentlichen_ kann nur verwendet werden, wenn Sie der Verwalter des Pakets sind.
