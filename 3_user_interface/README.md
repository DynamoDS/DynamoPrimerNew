# Benutzeroberfläche

### Überblick über die Benutzeroberfläche

Die Benutzeroberfläche (UI) für Dynamo ist in fünf Hauptbereiche unterteilt. Wir verschaffen uns hier kurz einen Überblick und erläutern den Arbeitsbereich und die Bibliothek in den folgenden Abschnitten näher.

\![](<../.gitbook/assets/user interface - ui.png>)

> 1. Menüs
> 2. Werkzeugkasten
> 3. Bibliothek
> 4. Arbeitsbereich
> 5. Ausführungsleiste

### Menüs

![](../.gitbook/assets/userinterface-menu\(1\).jpg)

Hier finden Sie Menüs für die grundlegenden Funktionen der Dynamo-Anwendung. Wie bei den meisten Windows-Programmen beziehen sich die ersten beiden Menüs auf die Verwaltung von Dateien, die Auswahl und die Bearbeitung von Inhalten. Die übrigen Menüs sind spezifisch für Dynamo.

#### Dynamo-Menüs

Allgemeine Informationen und Einstellungen finden Sie im Dropdown-Menü **Dynamo**.

\![](<../.gitbook/assets/user interface - dynamo menu.jpg>)

> 1. Info: Hier sehen Sie, welche Version von Dynamo auf Ihrem Computer installiert ist.
> 2. Vereinbarung zur Erfassung von Benutzerdaten: Hier können Sie Ihre Benutzerdaten freigeben, um Dynamo zu verbessern.
> 3. Einstellungen: Enthält Einstellungen wie die Definition der Dezimalpunktgenauigkeit der Anwendung und die Renderqualität der Geometrie.
> 4. Dynamo beenden

#### Hilfe

Wenn Sie nicht weiterkommen, verwenden Sie das Menü **Hilfe**. Sie können über Ihren Internetbrowser auf eine der Referenz-Websites von Dynamo zugreifen.

![](../.gitbook/assets/help-menu.png)

> 1. Interaktive Leitfäden: Touren, die Sie Schritt für Schritt durch die verschiedenen Funktionen von Dynamo führen.
> 2. Beispiele: Beispieldateien als Referenz. Nur verfügbar in Host-Programmen wie Revit und Civil 3D.
> 3. Dynamo-Wörterbuch: Ressource mit Dokumentation für alle Blöcke.
> 4. Dynamo-Website: Eine Website mit Informationen zu Dynamo und Links zu Ressourcen wie Foren, Blogs usw.
> 5. Dynamo-Repository: Zeigen Sie Ihr Dynamo-Projekt auf GitHub an. 
> 6. Dynamo-Projekt-Wiki: Im Wiki erhalten Sie Entwicklungsinformationen mithilfe der Dynamo-API, unterstützenden Bibliotheken und Tools.
> 7. Startseite anzeigen: Kehren Sie von einem Dokument aus zur Dynamo-Startseite zurück.
> 8. Fehler melden: Melden Sie ein Problem auf GitHub.

### Werkzeugkasten

Der Werkzeugkasten von Dynamo enthält eine Reihe von Schaltflächen für den Schnellzugriff zum Arbeiten mit Dateien sowie die Befehle Rückgängig [Ctrl + Z] und Wiederholen [Ctrl + Y]. Ganz rechts befindet sich eine weitere Schaltfläche, über die Sie einen Snapshot des Arbeitsbereichs exportieren können. Dies ist für die Dokumentation und die gemeinsame Bearbeitung mit anderen äußerst nützlich.

* \![](<../.gitbook/assets/user interface - new file.jpg>) Neu – Erstellt eine neue DYN-Datei.
* \![](<../.gitbook/assets/user interface - open (1).png>) Öffnen – Öffnet eine vorhandene DYN-Datei (Arbeitsbereich) oder DYF-Datei (benutzerdefinierter Block).
* \![](<../.gitbook/assets/user interface - save.png>) Speichern/Speichern unter – Speichert die aktive DYN- oder DYF-Datei.
* \![](<../.gitbook/assets/user interface - undo.jpg>) Rückgängig – Macht die letzte Aktion rückgängig.
* \![](<../.gitbook/assets/user interface - redo.jpg>) Wiederherstellen – Stellt die nächste Aktion wieder her.
* \![](<../.gitbook/assets/user interface - screenshot.png>) Arbeitsbereich als Bild exportieren – Exportiert den angezeigten Arbeitsbereich als PNG-Datei.

### Bibliothek

Die Dynamo-Bibliothek ist eine Sammlung funktionaler Bibliotheken, in der jede Bibliothek Blöcke enthält, die nach Kategorie gruppiert sind. Sie besteht aus grundlegenden Bibliotheken, die während der Vorgabeinstallation von Dynamo hinzugefügt werden. Während die Verwendung des Programms weiter vorgestellt wird, wird gezeigt, wie die Basisfunktionen um benutzerdefinierte Blöcke und zusätzliche Pakete erweitert werden können. Der Abschnitt [2-library.md](2-library.md "mention") enthält eine ausführlichere Anleitung zur Verwendung der Bibliothek.

\![](<../.gitbook/assets/user interface - library (1).gif>)

### Arbeitsbereich

Im Arbeitsbereich erstellen wir unsere visuellen Programme. Sie können auch die Vorschaueinstellung ändern, um die 3D-Geometrien hier anzuzeigen. Weitere Informationen finden Sie unter [1-workspace.md](1-workspace.md "mention").

\![](<../.gitbook/assets/user interface - workspace (1).gif>)

### Ausführungsleiste

Führen Sie das Dynamo-Skript von hier aus. Klicken Sie auf das Dropdown-Symbol auf der Schaltfläche Ausführung, um zwischen den verschiedenen Modi zu wechseln.

\![](<../.gitbook/assets/user interface - execution bar.gif>)

* Automatisch: Führt das Skript automatisch aus. Änderungen werden in Echtzeit aktualisiert.
* Manuell: Das Skript wird nur ausgeführt, wenn Sie auf die Schaltfläche Ausführen klicken. Nützlich, wenn Sie Änderungen an einem komplizierten und "schweren" Skript vornehmen.
* Periodisch: Diese Option ist vorgabemäßig abgeblendet. Nur verfügbar, wenn der _DateTime.Now_-Block verwendet wird. Sie können festlegen, dass das Diagramm in einem bestimmten Intervall automatisch ausgeführt wird.

\![](<../.gitbook/assets/user interface - execution bar DateTime node.jpg>)
