# Pakete – Einführung

Ein Paket ist, kurz gesagt, eine Sammlung benutzerdefinierter Blöcke. Der Dynamo Package Manager ist ein Community-Portal, aus dem Sie beliebige Pakete herunterladen können, die online veröffentlicht wurden. Diese Toolsets werden von externen Anbietern entwickelt und stellen Erweiterungen der Hauptfunktionen von Dynamo dar. Sie stehen für alle Benutzer zur Verfügung und können durch einfaches Klicken auf eine Schaltfläche heruntergeladen werden.

![Package Manager-Site](../images/6-2/1/dpm.jpg)

Community-Engagement wie dieses ist die Grundlage des Erfolgs von Open Source-Projekten wie Dynamo. Dank der Arbeit dieser hochmotivierten externen Entwickler kann Dynamo für Arbeitsabläufe in zahlreichen verschiedenen Branchen genutzt werden. Aus diesem Grund hat das Dynamo-Team sich geschlossen bemüht, die Entwicklung und Veröffentlichung von Paketen zu vereinheitlichen. (Dies wird in den folgenden Abschnitten detaillierter beschrieben.)

### Installation eines Pakets

Die einfachste Methode zum Installieren eines Pakets ist die Verwendung des Werkzeugkastens Pakete in der Dynamo-Benutzeroberfläche. Diese Methode wird im Folgenden beschrieben. In diesem Beispiel installieren Sie ein häufig verwendetes Paket zum Erstellen viereckiger Felder in einem Raster.

Wechseln Sie in Dynamo zu _Pakete > Suchen nach Paket_.

![](../images/6-2/1/packageintroduction-installingapackage01.jpg)

Suchen Sie mithilfe der Suchleiste nach "quads from rectangular grid". Nach kurzer Zeit sollten alle Pakete, die dieser Suchabfrage entsprechen, angezeigt werden. Sie müssen in diesem Fall das erste Paket mit passendem Namen auswählen.

Klicken Sie auf Installieren, um dieses Paket zu Ihrer Bibliothek hinzuzufügen. Fertig!

![](../images/6-2/1/packageintroduction-installingapackage02.jpg)

In der Dynamo-Bibliothek wird jetzt eine weitere Gruppe namens buildz angezeigt. Dieser Name bezieht sich auf den Entwickler des Pakets und der benutzerdefinierte Block wird in dieser Gruppe abgelegt. Sie können ihn sofort verwenden.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

Verwenden Sie den **Codeblock**, um schnell ein rechteckiges Raster zu definieren, und geben Sie das Ergebnis als **Polygon.ByPoints**-Block und anschließend als **Surface.ByPatch**-Block aus, um die Liste der rechteckigen Elemente anzuzeigen, die Sie gerade erstellt haben.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### Paketordner wird installiert - DynamoUnfold

Das Paket im vorigen Beispiel enthält nur einen benutzerdefinierten Block. Pakete, die mehrere benutzerdefinierte Blöcke und die unterstützenden Datendateien enthalten, werden jedoch auf dieselbe Weise heruntergeladen. Dies wird hier an einem umfassenderen Paket demonstriert: Dynamo Unfold.

Beginnen Sie wie im Beispiel oben, indem Sie _Pakete > Suchen nach Paket_ wählen.

Suchen Sie in diesem Fall nach _DynamoUnfold_ – in einem Wort geschrieben und unter Berücksichtigung der Groß- und Kleinschreibung. Wenn die Pakete angezeigt werden, laden Sie sie herunter, indem Sie auf Installieren klicken, um Dynamo Unfold Ihrer Dynamo-Bibliothek hinzuzufügen.

![](../images/6-2/1/packageintroduction-installingpackagefolder01.jpg)

Die Dynamo-Bibliothek enthält jetzt die Gruppe _DynamoUnfold_ mit mehreren Kategorien und benutzerdefinierten Blöcken.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

Als Nächstes betrachten Sie die Dateistruktur des Pakets genauer. Wählen Sie zunächst Dynamo > Einstellungen.

![](../images/6-2/1/packageintroduction-installingpackagefolder03.jpg)

Öffnen Sie über das Popup-Menü Voreinstellungen den Package Manager, und wählen Sie neben DynamoUnfold das vertikale Punktemenü ![](../images/6-2/1/packageintroduction-verticaldotsmenu.jpg) und dann Stammverzeichnis anzeigen aus, um den Stammordner für dieses Paket zu öffnen.

![](../images/6-2/1/packageintroduction-installingpackagefolder04.jpg)

Dadurch gelangen Sie zum Stammverzeichnis des Pakets. Hier sind drei Ordner und eine Datei vorhanden.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. Im Ordner _bin_ werden DLL-Dateien gespeichert. Dieses Dynamo-Paket wurde mit Zero-Touch entwickelt, d. h., die benutzerdefinierten Blöcke wurden in diesem Ordner abgelegt.
> 2. Im Ordner _dyf_ befinden sich die benutzerdefinierten Blöcke. Da dieses Paket nicht mithilfe benutzerdefinierter Dynamo-Blöcke entwickelt wurde, ist dieser Ordner in diesem Paket leer.
> 3. Der Ordner extra enthält alle zusätzlichen Dateien, darunter Ihre Beispieldateien.
> 4. Die Datei pkg ist eine einfache Textdatei, die die Einstellungen des Pakets definiert. Sie können diese Datei für den Augenblick ignorieren.

Wenn Sie den Ordner extra öffnen, sehen Sie eine Reihe von Beispieldateien, die mit der Installation heruntergeladen wurden. Beispieldateien stehen nicht in allen Paketen zur Verfügung. Falls sie jedoch vorhanden sind, finden Sie sie in diesem Ordner.

Öffnen Sie SphereUnfold.

![](../images/6-2/1/rd2.jpg)

Nachdem Sie die Datei geöffnet und im Solver auf Ausführen geklickt haben, erhalten Sie das Netz einer Kugel! Beispieldateien wie diese erleichtern den Einstieg in die Arbeit mit einem neuen Dynamo-Paket.

![](<../images/6-2/5/packageintroduction-installingpackagefolder07 (1).jpg>)

### Dynamo Package Manager

Sie können auch online im [Dynamo Package Manager](http://dynamopackages.com) nach Dynamo-Paketen suchen. Dies ist ein sehr effizientes Verfahren für die Suche nach Paketen, da diese im Repository nach Anzahl der Downloads und Beliebtheit sortiert werden. Sie finden auf diese Weise auch mühelos Informationen zu kürzlich erfolgten Aktualisierungen der Pakete: Manche Dynamo-Pakete unterliegen einer Versionskontrolle und es bestehen Abhängigkeiten zu Dynamo-Builds.

Durch Klicken auf _Quads from Rectangular Grid_ im Dynamo Package Manager zeigen Sie die Beschreibungen, Versionen, den Entwickler sowie eventuelle Abhängigkeiten an.

![](../images/6-2/1/dpm2.jpg)

Sie können die Paketdateien auch über den Dynamo Package Manager herunterladen, der direkte Download in Dynamo ist jedoch ein nahtloserer Ablauf.

### Wo werden die Paketdateien lokal gespeichert?

Wenn Sie Dateien aus dem Dynamo Package Manager herunterladen oder sehen möchten, wo alle Paketdateien gespeichert sind, klicken Sie auf Dynamo > Package Manager > Pfade für Blöcke und Pakete. Hier finden Sie das aktuelle Stammordnerverzeichnis.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

Pakete werden vorgabemäßig unter einem Speicherort ähnlich dem folgenden installiert: _C:/Benutzer/[Benutzername]/AppData/Roaming/Dynamo/[Dynamo-Version]_.

### Weitere Schritte mit Paketen

Die Dynamo-Community wächst laufend und entwickelt sich dabei weiter. Besuchen Sie Dynamo Package Manager von Zeit zu Zeit, um über neue inspirierenden Entwicklungen auf dem Laufenden zu bleiben. In den folgenden Abschnitten werden Pakete eingehender behandelt, wobei sowohl auf die Perspektive des Endbenutzers eingegangen als auch die Entwicklung eigener Dynamo-Pakete behandelt wird.
