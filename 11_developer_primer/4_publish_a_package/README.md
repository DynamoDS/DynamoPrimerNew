# Publizieren eines Pakets 

### Publizieren eines Pakets <a href="#publish-a-package" id="publish-a-package"></a>

Pakete sind eine einfache Möglichkeit, Blöcke zu speichern und gemeinsam mit der Dynamo-Community zu nutzen. Ein Paket kann alle möglichen Elemente enthalten, von benutzerdefinierten Blöcken, die im Dynamo-Arbeitsbereich erstellt wurden, bis hin zu von NodeModel abgeleiteten Blöcken. Pakete werden mit dem Package Manager publiziert und installiert. Zusätzlich zu dieser Seite enthält der [Primer](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) eine allgemeine Anleitung für Pakete.

#### Was ist ein Package Manager? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Der Dynamo Package Manager ist eine Softwareregistrierung (ähnlich wie npm), auf die Sie über Dynamo oder über einen Webbrowser zugreifen können. Der Package Manager umfasst das Installieren, Publizieren, Aktualisieren und Anzeigen von Paketen. Wie npm verwaltet er verschiedene Versionen von Paketen. Außerdem können Sie so die Abhängigkeiten Ihres Projekts verwalten.

Suchen Sie im Browser nach Paketen, und zeigen Sie Statistiken an: [https://dynamopackages.com/](https://dynamopackages.com).

* In Dynamo umfasst der Package Manager das Installieren, Publizieren und Aktualisieren von Paketen.

![Suchen nach Paketen](images/dynamopackagemanager.jpg)

> 1. Online-Suche nach Paketen: `Packages > Search for a Package...`
> 2. Anzeigen/Bearbeiten von installierten Paketen: `Packages > Manage Packages...`
> 3. Publizieren eines neuen Pakets: `Packages > Publish New Package...`

#### <a href="#publishing-a-package" id="publishing-a-package">Publizieren von Paketen</a>

Pakete werden in Dynamo über den Package Manager publiziert. Es wird empfohlen, lokal zu publizieren, das Paket zu testen und dann online zu publizieren, um das Paket gemeinsam mit der Community zu nutzen. Anhand der NodeModel-Fallstudie werden die erforderlichen Schritte zum Publizieren des RectangularGrid-Blocks als Paket lokal und dann online beschrieben.

Starten Sie Dynamo, und wählen Sie `Packages > Publish New Package...`, um das Fenster `Publish a Package` zu öffnen.

![Publizieren von Paketen](images/dyn-publish-package-add-files.jpg)

> 1. Wählen Sie `Add file...`, um nach Dateien zu suchen, die dem Paket hinzugefügt werden sollen.
> 2. Wählen Sie die beiden `.dll`-Dateien aus der NodeModel-Fallstudie aus.
> 3. Wählen Sie `Ok` aus.

Geben Sie dem Paket einen Namen, eine Beschreibung und eine Versionsbezeichnung, wenn die Dateien dem Paketinhalt hinzugefügt wurden. Beim Publizieren eines Pakets mit Dynamo wird automatisch eine `pkg.json`-Datei erstellt.

![Paketeinstellungen](images/dyn-publish-package.jpg)

> Ein Paket, das bereit für die Publizierung ist
>
> 1. Geben Sie die erforderlichen Informationen für Name, Beschreibung und Version ein.
> 2. Publizieren Sie das Paket, indem Sie auf Lokal publizieren klicken und den Dynamo-Paketordner `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages` auswählen, damit der Block in Core verfügbar ist. Publizieren Sie immer lokal, bis das Paket so weit ist, dass es freigegeben werden kann.

Nach dem Publizieren eines Pakets sind die Blöcke in der Dynamo-Bibliothek unter der Kategorie `CustomNodeModel` verfügbar.

![Paket in der Dynamo-Bibliothek](images/dyn-publish-package-library.jpg)

> 1. Das soeben in der Dynamo-Bibliothek erstellte Paket.

Wenn das Paket online publiziert werden kann, öffnen Sie den Package Manager, wählen Sie `Publish` und dann `Publish Online`.

![Publizieren eines Pakets im Package Manager](images/dyn-publish-package-directory.jpg)

> 1. Um zu sehen, wie Dynamo das Paket formatiert hat, klicken Sie auf die drei vertikalen Punkte rechts neben CustomNodeModel und wählen Stammverzeichnis anzeigen.
> 2. Wählen Sie im Fenster zum Publizieren von Dynamo-Paketen `Publish` und dann `Publish Online` aus.
> 3. Um ein Paket zu löschen, wählen Sie `Delete`.

#### Wie aktualisiere ich ein Paket? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

Das Aktualisieren eines Pakets ist ein ähnlicher Vorgang wie das Publizieren. Öffnen Sie den Package Manager, wählen Sie `Publish Version...` für das Paket aus, das aktualisiert werden muss, und geben Sie eine höhere Version ein.

![Publizieren einer Paketversion](images/dyn-publish-package-version.jpg)

> 1. Wählen Sie `Publish Version`, um ein vorhandenes Paket mit neuen Dateien im Stammverzeichnis zu aktualisieren, und wählen Sie dann, ob es lokal oder online publiziert werden soll.

#### Web-Client des Package Manager <a href="#package-manager-web-client" id="package-manager-web-client"></a>

Der Web-Client des Package Manager wird ausschließlich zum Suchen und Anzeigen von Paketdaten wie z. B. Versionierung und Download-Statistiken verwendet.

Der Web-Client des Package Manager ist über folgenden Link verfügbar: [https://dynamopackages.com/](https://dynamopackages.com)

![Web-Client des Package Manager](images/packagemanager-browser.jpg)
