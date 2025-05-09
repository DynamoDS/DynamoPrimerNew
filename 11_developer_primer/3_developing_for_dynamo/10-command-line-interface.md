# Dynamo-Befehlszeilenschnittstelle

`-o, -O, --OpenFilePath`: Weisen Sie Dynamo an, eine Befehlsdatei zu öffnen und die enthaltenen Befehle unter diesem Pfad auszuführen. Diese Option wird nur unterstützt, wenn die Ausführung über DynamoSandbox erfolgt.  

`-c, -C, --CommandFilePath`: Weisen Sie Dynamo an, eine Befehlsdatei zu öffnen und die enthaltenen Befehle unter diesem Pfad auszuführen. Diese Option wird nur unterstützt, wenn die Ausführung über DynamoSandbox erfolgt.  

`-v, -V, --Verbose`: Weisen Sie Dynamo an, alle vom Programm durchgeführten Auswertungen in einer XML-Datei im angegebenen Pfad auszugeben.  

`-g, -G, --Geometry`: Weisen Sie Dynamo an, die Geometrie aller Auswertungen in einer JSON-Datei unter diesem Pfad auszugeben.  

`-h, -H, --help`: Rufen Sie die Hilfe ab.  

`-i, -I, --Import`: Weisen Sie Dynamo an, eine Assembly als Blockbibliothek zu importieren. Dieses Argument sollte ein Dateipfad zu einer einzelnen `.dll` sein. Wenn Sie mehrere `.dlls` importieren möchten, listen Sie diese durch ein Leerzeichen getrennt auf: `-i 'assembly1.dll' 'assembly2.dll'`.  

`--GeometryPath`: Relativer oder absoluter Pfad zu einem Verzeichnis, das ASM-Dateien enthält. Wenn bereitgestellt, wird auf der Festplatte nicht nach der ASM-Datei gesucht, sondern diese wird direkt aus diesem Pfad geladen.  

`-k, -K, --KeepAlive`: Im Keepalive-Modus können Sie den Dynamo-Prozess so lange ausführen, bis er durch eine geladene Erweiterung beendet wird.  

`--HostName`: Identifizieren Sie die mit dem Host verknüpfte Dynamo-Variante.  

`-s, -S, --SessionId`: Identifizieren Sie die Sitzungs-ID von Dynamo-Host-Analysen.  

`-p, -P, --ParentId`: Identifizieren Sie die übergeordnete ID von Dynamo-Host-Analysen.  

`-x, -X, --ConvertFile`: Wird dies in Kombination mit dem Flag `-O` verwendet, wird eine `.dyn`-Datei aus dem angegebenen Pfad geöffnet und in `.json` konvertiert. Die Datei hat die Erweiterung `.json` und befindet sich im selben Verzeichnis wie die ursprüngliche Datei.  

`-n, -N, --NoConsole`: Verlassen Sie sich nicht auf das Konsolenfenster, um im Keepalive-Modus mit der CLI zu interagieren.  

`-u, -U, --UserData`: Geben Sie den Benutzerdatenordner an, der von PathResolver mit der CLI verwendet werden soll.  

`--CommonData`: Geben Sie den gemeinsamen Datenordner an, der von PathResolver mit der CLI verwendet werden soll.  

`--DisableAnalytics`: Deaktiviert Analysen in Dynamo für die Dauer des Prozesses.  

`--CERLocation`: Geben Sie das Werkzeug für den Absturzfehlerbericht an, das sich auf der Festplatte befindet.  

`--ServiceMode`: Geben Sie den Startvorgang für den Servicemodus an.  



#### Warum? 
 Aus unterschiedlichen Gründen kann es erforderlich sein, Dynamo über die Befehlszeile zu steuern. Dazu zählen die folgenden: 
 
 * Automatisieren vieler Dynamo-Läufe
 * Testen von Dynamo-Diagrammen (bei Verwendung von DynamoSandbox siehe auch unter -c)
 * Ausführen einer Sequenz von Dynamo-Diagrammen in einer bestimmten Reihenfolge
 * Schreiben von Stapeldateien für mehrere Befehlszeilenausführungen
 * Schreiben eines weiteren Programms, um die Ausführung von Dynamo-Diagrammen zu steuern und zu automatisieren sowie die Ergebnisse dieser Berechnungen für verschiedene Aufgaben zu verwenden

#### Was?
Die Befehlszeilenschnittstelle (DynamoCLI) ist eine Ergänzung zu DynamoSandbox. Es handelt sich um ein DOS/TERMINAL-Befehlszeilen-Dienstprogramm, mit dem Dynamo komfortabel über Befehlszeilenargumente ausgeführt werden kann. In der ersten Implementierung kann das Programm nicht eigenständig ausgeführt werden, sondern muss in dem Ordner ausgeführt werden, in dem sich die Dynamo-Binärdateien befinden, da es von denselben DLL-Kerndateien wie Sandbox abhängt. Es funktioniert nicht mit anderen Builds von Dynamo.

Es gibt vier Möglichkeiten zum Ausführen der CLI: über eine DOS-Eingabeaufforderung, über DOS-Stapeldateien und als Windows-Desktop-Verknüpfung, deren Pfad so geändert wird, dass er die angegebenen Befehlszeilen-Flags enthält. Die Spezifikation für die DOS-Datei kann vollständig qualifiziert oder relativ sein, und zugeordnete Laufwerke und URL-Syntax werden ebenfalls unterstützt. Sie kann auch mit Mono erstellt und unter Linux oder Mac vom Terminal aus ausgeführt werden.

Das Dienstprogramm unterstützt Dynamo-Pakete, Sie können jedoch keine benutzerdefinierten Blöcke (dyf), sondern nur eigenständige Diagramme (dyn) laden.

In vorläufigen Tests unterstützt das CLI-Dienstprogramm lokalisierte Versionen von Windows, und Sie können filespec-Argumente mit ASCII-Großbuchstaben angeben.

Auf die CLI kann über die Anwendung DynamoCLI.exe zugegriffen werden. Diese Anwendung ermöglicht es Benutzern oder einer anderen Anwendung, mit dem Dynamo-Auswertungsmodell zu interagieren, indem DynamoCLI.exe mit einer Befehlszeichenfolge aufgerufen wird. Dies kann in etwa wie folgt aussehen:
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
Mit diesem Befehl wird Dynamo angewiesen, die angegebene Datei unter *"C:\\BeliebigeDynamoDatei.Dyn"* zu öffnen, ohne eine Benutzeroberfläche anzuzeigen, und sie dann auszuführen. Dynamo wird beendet, wenn die Ausführung des Diagramms abgeschlossen ist. 

**Neu in Version 2.1**: Anwendung DynamoWPFCLI.exe. Diese Anwendung unterstützt all das, was auch die Anwendung DynamoCLI.exe unterstützt, weist aber zusätzlich die Option Geometrie (-g) auf. Die Anwendung DynamoWPFCLI.exe ist nur für Windows verfügbar.

#### Wichtige Anmerkungen

* Die bevorzugte Methode für die Interaktion mit DynamoCLI erfolgt über eine Schnittstelle für Eingabeaufforderungen.
* Derzeit müssen Sie DynamoCLI vom Installationsverzeichnis im Ordner [Dynamo-Version] aus ausführen. Die CLI benötigt Zugriff auf dieselben DLL-Dateien wie Dynamo und sollte daher nicht verschoben werden.
* Sie sollten Diagramme ausführen können, die derzeit in Dynamo geöffnet sind. Dies kann jedoch zu unbeabsichtigten Nebeneffekten führen.
* Alle Dateipfade sind vollständig DOS-konform; relative und vollständig qualifizierte Pfade sollten daher funktionieren. Stellen Sie jedoch sicher, dass Sie Ihre Pfade in Anführungszeichen setzen: *"C:Pfad\\zur\\Datei.dyn"*. 
* DynamoCLI ist eine neue Funktion, die sich derzeit noch stark im Wandel befindet: Die **CLI lädt derzeit nur einen Teilsatz** der Standardbibliotheken von Dynamo. Beachten Sie dies, wenn ein Diagramm nicht ordnungsgemäß ausgeführt wird. Diese Bibliotheken werden [hier](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) angegeben. 
* Derzeit wird keine Standardausgabe bereitgestellt. Wenn keine Fehler gefunden werden, wird die CLI nach der Ausführung einfach beendet.
 
#### wie

`-o`: Sie können Dynamo mit Verweis auf eine DYN-Datei im Headless-Modus öffnen, in dem das Diagramm ausgeführt wird.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v` kann verwendet werden, wenn Dynamo im Headless-Modus ausgeführt wird (wenn Sie `-o` zum Öffnen eines Datei verwendet haben). Dieses Flag wiederholt alle Blöcke im Diagramm und gibt ihre Ausgabewerte in einer einfachen XML-Datei aus. Da das Flag `--ServiceMode` Dynamo dazu zwingen kann, mehrere Diagrammauswertungen auszuführen, enthält die Ausgabedatei Werte für jede auftretende Auswertung.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
Die XML-Ausgabedatei hat folgende Form:
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` kann verwendet werden, wenn Dynamo im Headless-Modus ausgeführt wird (wenn Sie `-o` zum Öffnen einer Datei verwendet haben). Dieses Flag generiert das Diagramm und gibt dann die resultierende Geometrie in einer JSON-Datei aus. 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
Die JSON-Geometriedatei hat folgende Form:

 Noch offen – In Bearbeitung

`-h`: Verwenden Sie diese Option, um eine Liste der möglichen Optionen abzurufen.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

Das Flag -i kann mehrmals verwendet werden, um mehrere Assemblys zu importieren, die zum Ausführen des zu öffnenden Diagramms erforderlich sind.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

Das Flag -l kann verwendet werden, um Dynamo unter einer anderen Gebietsschema-Einstellung auszuführen. In der Regel wirkt sich die Gebietsschema-Einstellung jedoch nicht auf die Diagrammergebnisse aus.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

Das Flag --GeometryPath kann verwendet werden, um DynamoSandbox oder die CLI auf einen bestimmten Satz von ASM-Binärdateien zu verweisen. Verwenden Sie es wie

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

oder

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

Mit dem Flag -k können Sie den Dynamo-Prozess so lange ausführen, bis er durch eine geladene Erweiterung beendet wird.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

Das Flag --HostName kann verwendet werden, um die mit dem Host verknüpfte Dynamo-Variante zu identifizieren.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

oder

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

Das Flag -s kann verwendet werden, um die Sitzungs-ID von Dynamo-Host-Analysen zu ermitteln.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

Das Flag -p kann verwendet werden, um die übergeordnete ID von Dynamo-Host-Analysen zu ermitteln.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
