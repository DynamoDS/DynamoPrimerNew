# Benutzeroberfläche

### Überblick über die Benutzeroberfläche

Die Benutzeroberfläche (UI) für Dynamo ist in fünf Hauptbereiche unterteilt. Wir verschaffen uns hier kurz einen Überblick und erläutern den Arbeitsbereich und die Bibliothek in den folgenden Abschnitten näher.

![](<images/user interface - ui.jpg>)

> 1. Menüs
> 2. Werkzeugkasten
> 3. Bibliothek
> 4. Arbeitsbereich
> 5. Ausführungsleiste

### Menüs

![](<images/user interface - menu.jpg>)

Hier finden Sie Menüs für die grundlegenden Funktionen der Dynamo-Anwendung. Wie bei den meisten Windows-Programmen beziehen sich die ersten beiden Menüs auf die Verwaltung von Dateien, die Auswahl und die Bearbeitung von Inhalten. Die übrigen Menüs sind spezifisch für Dynamo.

#### Dynamo-Menüs

Allgemeine Informationen und Einstellungen finden Sie im Dropdown-Menü **Dynamo**.

![](<images/user interface - dynamo menu.jpg>)

> 1. Info: Hier sehen Sie, welche Version von Dynamo auf Ihrem Computer installiert ist.
> 2. Vereinbarung zur Erfassung von Benutzerdaten: Hier können Sie Ihre Benutzerdaten freigeben, um Dynamo zu verbessern.
> 3. Einstellungen: Enthält Einstellungen wie die Definition der Dezimalpunktgenauigkeit der Anwendung und die Renderqualität der Geometrie.
> 4. Dynamo beenden

#### Hilfe

Wenn Sie nicht weiterkommen, verwenden Sie das Menü **Hilfe**. Sie können über Ihren Internetbrowser auf eine der Referenz-Websites von Dynamo zugreifen.

![](<images/user interface - help menu.jpg>)

> 1. Erste Schritte: eine kurze Einführung in die Verwendung von Dynamo.
> 2. Interaktiver Leitfaden:
> 3. Beispiele: Beispieldateien als Referenz.
> 4. Dynamo-Wörterbuch: Ressource mit Dokumentation für alle Blöcke.
> 5. Dynamo-Website: Anzeigen des Dynamo-Projekts auf GitHub.
> 6. Dynamo Projekt-Wiki: Im Wiki erhalten Sie Entwicklungsinformationen mithilfe der Dynamo-API, unterstützenden Bibliotheken und Tools.
> 7. Startseite anzeigen: Kehren Sie von einem Dokument aus zur Dynamo-Startseite zurück.
> 8. Fehler melden: Melden Sie ein Problem auf GitHub.

### Werkzeugkasten

Der Werkzeugkasten von Dynamo enthält eine Reihe von Schaltflächen für den Schnellzugriff zum Arbeiten mit Dateien sowie die Befehle Rückgängig \[Ctrl + Z] und Wiederholen \[Ctrl + Y]. Ganz rechts befindet sich eine weitere Schaltfläche, über die Sie einen Snapshot des Arbeitsbereichs exportieren können. Dies ist für die Dokumentation und die gemeinsame Bearbeitung mit anderen äußerst nützlich.

* ![](<images/user interface - new file.jpg>) Neu – Erstellt eine neue DYN-Datei.
* ![](<images/user interface - open (1).jpg>) Öffnen – Öffnet eine vorhandene DYN-Datei (Arbeitsbereich) oder DYF-Datei (benutzerdefinierter Block).
* ![](<images/user interface - save.jpg>) Speichern/Speichern unter – Speichert die aktive DYN- oder DYF-Datei.
* ![](<images/user interface - undo.jpg>) Rückgängig – Letzte Aktion rückgängig machen.
* ![](<images/user interface - redo.jpg>) Wiederherstellen – Stellt die nächste Aktion wieder her.
* ![](<images/user interface - screenshot.jpg>) Arbeitsbereich als Bild exportieren – Exportiert den angezeigten Arbeitsbereich als PNG-Datei.

### Bibliothek

Die Dynamo-Bibliothek ist eine Sammlung funktionaler Bibliotheken, in der jede Bibliothek Blöcke enthält, die nach Kategorie gruppiert sind. Sie besteht aus grundlegenden Bibliotheken, die während der Vorgabeinstallation von Dynamo hinzugefügt werden. Während die Verwendung des Programms weiter vorgestellt wird, wird gezeigt, wie die Basisfunktionen um benutzerdefinierte Blöcke und zusätzliche Pakete erweitert werden können. Der Abschnitt [2-library.md](2-library.md "mention") enthält eine ausführlichere Anleitung zur Verwendung der Bibliothek.

![](<images/user interface - library.jpg>)

### Arbeitsbereich

Im Arbeitsbereich erstellen wir unsere visuellen Programme. Sie können auch die Vorschaueinstellung ändern, um die 3D-Geometrien hier anzuzeigen. Weitere Informationen finden Sie unter [1-workspace.md](1-workspace.md "mention").

![](<images/user interface - workspace.gif>)

### Ausführungsleiste

Führen Sie das Dynamo-Skript von hier aus. Klicken Sie auf das Dropdown-Symbol auf der Schaltfläche Ausführung, um zwischen den verschiedenen Modi zu wechseln.

![](<images/user interface - execution bar.gif>)

* Automatisch: Führt das Skript automatisch aus. Änderungen werden in Echtzeit aktualisiert.
* Manuell: Das Skript wird nur ausgeführt, wenn Sie auf die Schaltfläche Ausführen klicken. Nützlich, wenn Sie Änderungen an komplizierten und "schweren" Skripten vornehmen.
* Periodisch: Diese Option ist vorgabemäßig abgeblendet. Nur verfügbar, wenn der DateTime.Now-Block verwendet wird. Sie können festlegen, dass das Diagramm in einem bestimmten Intervall automatisch ausgeführt wird.

![](<images/user interface - execution bar DateTime node.jpg>)
