# Entwickeln für Dynamo

Unabhängig vom Kenntnisstand ist die Dynamo-Plattform dafür konzipiert, dass alle Benutzer ihre Beiträge leisten können. Es gibt mehrere Entwicklungsoptionen, die auf unterschiedliche Fähigkeiten und Qualifikationen ausgerichtet sind und je nach Ziel alle ihre Stärken und Schwächen haben. Nachfolgend werden die verschiedenen Optionen und die Möglichkeiten zur Auswahl einer Option erläutert.

![Drei Entwicklungsumgebungen](images/developing-for-dynamo.png)

> Drei Entwicklungsumgebungen: Visual Studio, Python Editor und Code Block DesignScript

#### Welche Optionen stehen zur Auswahl? <a href="#what-are-my-options" id="what-are-my-options"></a>

Die Entwicklungsoptionen für Dynamo lassen sich primär in zwei Kategorien einteilen: _für_ Dynamo im Vergleich zu _in_ Dynamo. Sie können sich die beiden Kategorien folgendermaßen vorstellen: In Dynamo impliziert Inhalte, die mit der Dynamo-IDE zur Verwendung in Dynamo erstellt werden, und für Dynamo impliziert die Verwendung externer Werkzeuge zur Erstellung von Inhalten, die zur Verwendung in Dynamo importiert werden. Obwohl sich dieses Handbuch auf die Entwicklung _für_ Dynamo konzentriert, werden im Folgenden Ressourcen für alle Prozesse beschrieben.

#### Für Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

Diese Blöcke gestatten die größtmögliche Anpassung. Viele Pakete werden mit dieser Methode erstellt. Sie ist erforderlich, um zur Quelle von Dynamo beizutragen. Der Prozess ihrer Erstellung wird in diesem Handbuch behandelt.

* Zero-Touch-Blöcke
* Von NodeModel abgeleitete Blöcke
* Erweiterungen

> Der Primer enthält eine Anleitung zum [Importieren von Zero-Touch-Bibliotheken](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/5-zero-touch).

Für die folgende Beschreibung wird Visual Studio als Entwicklungsumgebung für Zero-Touch- und NodeModel-Blöcke verwendet.

![Visual Studio-Benutzeroberfläche](images/vs-devenv.jpg)

> Visual Studio-Benutzeroberfläche mit einem Projekt, das wir entwickeln werden

#### In Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

Diese Prozesse befinden sich zwar im Arbeitsbereich für visuelle Programmierung und sind relativ einfach durchzuführen. Es handelt sich jedoch bei allen um realisierbare Optionen zur Anpassung von Dynamo. Der Primer behandelt diese Prozesse ausführlich und bietet im Kapitel [Vorgehensweisen zur Skripterstellung](http://dynamoprimer.com/de/12\_Best-Practice/12-1\_Scripting-Strategies.html) Tipps und Best Practices für die Skripterstellung.

*   Codeblöcke stellen DesignScript in der visuellen Programmierumgebung bereit und ermöglichen so flexible Text-Skript- und Block-Arbeitsabläufe. Eine Funktion in einem Codeblock kann von jedem beliebigen Element im Arbeitsbereich aufgerufen werden.

    > Laden Sie ein Codeblock-Beispiel herunter (klicken Sie mit der rechten Maustaste und dann auf Speichern unter), oder sehen Sie sich im [Primer](https://primer.dynamobim.org/07\_Code-Block/7-1\_what-is-a-code-block.html) eine detaillierte exemplarische Vorgehensweise an.
*   Benutzerdefinierte Blöcke sind Container für Sammlungen von Blöcken oder sogar ganzen Diagrammen. Sie stellen eine effektive Methode dar, um häufig verwendete Routinen zu sammeln und sie mit der Community zu teilen.

    > Laden Sie ein Beispiel für einen benutzerdefinierten Block herunter (klicken Sie mit der rechten Maustaste und dann auf Speichern unter), oder sehen Sie sich im [Primer](https://primer.dynamobim.org/10\_Custom-Nodes/10-1\_Introduction.html) eine detaillierte exemplarische Vorgehensweise an.
*   Python-Blöcke stellen eine Skripterstellungs-Schnittstelle im Arbeitsbereich für visuelle Programmierung dar, ähnlich wie Codeblöcke. Die Autodesk.DesignScript-Bibliotheken verwenden eine Punktnotation ähnlich der von DesignScript.

    > Laden Sie ein Beispiel für einen Python-Block herunter (klicken Sie mit der rechten Maustaste und dann auf Speichern unter), oder sehen Sie sich im [Primer](https://primer.dynamobim.org/10\_Custom-Nodes/10-4\_Python.html) eine detaillierte exemplarische Vorgehensweise an.

Die Entwicklung im Dynamo-Arbeitsbereich ist ein leistungsstarkes Werkzeug, um unmittelbar Feedback zu erhalten.

![Entwickeln im Dynamo-Arbeitsbereich mit dem Python-Block](images/python-example.jpg)

> Entwickeln im Dynamo-Arbeitsbereich mit dem Python-Block

#### Welche Vor- und Nachteile haben die einzelnen Optionen? <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Die Entwicklungsoptionen für Dynamo wurden speziell auf die Komplexität bei der Anpassung ausgelegt. Unabhängig davon, ob das Ziel darin besteht, ein rekursives Skript in Python zu schreiben oder eine vollständig angepasste Block-Benutzeroberfläche zu erstellen, gibt es Optionen zum Implementieren von Code, die nur das umfassen, was für den Einstieg erforderlich ist.

**Codeblöcke, der Python-Block und benutzerdefinierte Blöcke in Dynamo**

Mit diesen Optionen können Sie sehr einfach Code in der visuellen Programmierumgebung von Dynamo schreiben. Der Arbeitsbereich für visuelle Programmierung in Dynamo bietet Zugriff auf Python und DesignScript und umfasst die Möglichkeit, mehrere Blöcke in einem benutzerdefinierten Block zu enthalten.

![Codeblock, Python-Skript und benutzerdefinierter Block](images/Development-Icons.png)

Mit diesen Methoden haben wir folgende Möglichkeiten:

* Einstieg in das Schreiben mit Python oder DesignScript ohne oder mit geringem Einrichtungsaufwand
* Importieren von Python-Bibliotheken in Dynamo
* Gemeinsame Nutzung von Codeblöcken, Python-Blöcken und benutzerdefinierten Blöcken mit der Dynamo-Community im Rahmen eines Pakets

**Zero-Touch-Blöcke**

Unter Zero-Touch versteht man ein einfaches Verfahren zum Importieren von C#-Bibliotheken durch Zeigen und Klicken. Dynamo liest die öffentlichen Methoden einer `.dll`-Datei und konvertiert sie in Dynamo-Blöcke. Sie können Zero-Touch verwenden, um Ihre eigenen benutzerdefinierten Blöcke und Pakete zu entwickeln.

![Zero-Touch-Blöcke](images/ZTImport.png)

Mit dieser Methode haben wir folgende Möglichkeiten:

* Importieren von Bibliotheken, die nicht unbedingt für Dynamo entwickelt wurden, und automatisches Erstellen einer Suite mit neuen Blöcken, z. B. wie im [A-Forge-Beispiel](http://dynamoprimer.com/en/10\_Packages/10-5\_Zero-Touch.html) im Primer beschrieben.
* Schreiben von C#-Methoden und einfache Nutzung der Methoden als Blöcke in Dynamo
* Gemeinsame Nutzung einer C#-Bibliothek als Blöcke mit der Dynamo-Community in einem Paket

**Von NodeModel abgeleitete Blöcke**

Diese Blöcke gehen ein Schritt tiefer in die Struktur von Dynamo. Sie basieren auf der `NodeModel`-Klasse und sind in C# geschrieben. Diese Methode bietet die größte Flexibilität und Leistung. Die meisten Aspekte des Blocks müssen jedoch explizit definiert werden, und Funktionen müssen in einer separaten Assembly ausgeführt werden.

![Von NodeModel abgeleitete Blöcke](images/Development-Icons-NodeModel.png)

Mit dieser Methode haben wir folgende Möglichkeiten:

* Erstellen einer vollständig anpassbaren Block-Benutzeroberfläche mit Schiebereglern, Bildern, Farben usw. (z. B. ColorRange-Block)
* Zugreifen und Bearbeiten von Vorgängen im Dynamo-Ansichtsbereich
* Anpassen der Vergitterung
* Laden als Paket in Dynamo

#### Wissenswertes über Dynamo-Versionierung und API-Änderungen (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Da Dynamo regelmäßig aktualisiert wird, können Änderungen an Teilen der API vorgenommen werden, die von einem Paket verwendet wird. Die Nachverfolgung dieser Änderungen ist wichtig, um sicherzustellen, dass vorhandene Pakete weiterhin ordnungsgemäß funktionieren.

Änderungen an der API werden im [Dynamo-GitHub-Wiki](https://github.com/DynamoDS/Dynamo/wiki/API-Changes) nachverfolgt. Hier werden Änderungen an DynamoCore, Bibliotheken und Arbeitsbereichen behandelt.

![Dokument mit Dynamo-API-Änderungen](images/api-changes.jpg)

Ein Beispiel für eine bevorstehende, signifikante Änderung ist der Wechsel von XML zum JSON-Dateiformat in Version 2.0. Von NodeModel abgeleitete Blöcke benötigen jetzt einen [JSON-Konstruktor](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node), da sie sonst nicht in Dynamo 2.0 geöffnet werden können.

Die API-Dokumentation von Dynamo deckt derzeit die Kernfunktionen ab: [http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI).

![API-Dokumentation](images/api-docs.jpg)

#### Berechtigung zum Verteilen von Binärdateien in einem Paket <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Achten Sie auf DLL-Dateien, die in einem Paket enthalten sind, das in den Package Manager hochgeladen wird. Wenn der Autor des Pakets die DLL-Datei nicht erstellt hat, muss er über die Rechte zum Freigeben verfügen.

Wenn ein Paket Binärdateien enthält, müssen die Benutzer beim Herunterladen darüber informiert werden, dass das Paket Binärdateien enthält.
