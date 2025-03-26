# Häufig gestellte Fragen 

## So verwenden Sie Dynamo-Builds

### Tägliche Builds und stabile Builds im Vergleich
Das Dynamo-Team bei Autodesk arbeitet von jeher mit einem hohen Iterationstempo, indem es sowohl tägliche Builds durch Commit als auch stabile Release-Builds nach Systemtest- und Freigabezyklus veröffentlicht. Unser Team würde die täglichen und stabilen Builds gerne neu starten, damit die Benutzer steuern können, wo DynamoCore lokal auf ihrem Datenträger extrahiert wird. So können sie es sicher verwenden, ohne dass es sich auf Dynamo für andere Autodesk-Produkte auswirkt. Einige Kandidaten eignen sich besonders gut für diesen Zweck, darunter NUPKG-Dateien, ZIP-Dateien oder ein spezielles Installationsprogramm, bei dem Benutzer den Installationspfad oder andere Optionen auswählen können. 

Da wir den Benutzern unseren neuesten Code so einfach wie möglich zur Verfügung stellen möchten, haben wir uns entschieden, eine ZIP-Datei mit den DynamoCore-Binärdateien und Dynamo Sandbox bereitzustellen, die ohne Revit (mit einigen Einschränkungen) verwendet werden kann.

### Dynamo-ZIP-Builds
#### Definition und Quelle
Der DynamoCoreRuntime-ZIP-Build ist ein Snapshot der DynamoCore-Binärdateien, der während der automatisierten Builds erstellt wird. 

Sie sollten DynamoSandbox.exe im extrahierten Ordner starten können, um Dynamo mit minimalem Setup-Aufwand zu verwenden.


#### Erforderliche Komponenten

| Dynamo-Version  |Microsoft Visual C++  | DirectX  |   |   |   |   |
|---|---|---|---|---|---|---|
|  2.0 bis 2.6 |  2015 Redistributable  | 10  |   |   |   |   |
| 2.7  | 2019 Redistributable  | 11/12 (in Windows 10 enthalten)  |   |   |   |   |
| >= 2.8  | 2019 Redistributable  | 11/12 (in Windows 10 enthalten)  |   |   |   |   |
##### Microsoft DirectX, das [hier](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX) in unserem Dynamo-GitHub-Repository auch öffentlich verfügbar ist

##### 7zip zum Entpacken des Pakets [hier](https://www.7-zip.org/download.html)


##### [Link](https://aka.ms/vs/17/release/vc_redist.x64.exe) zu Microsoft Visual C++ 2015-2024 Redistributable (x64)

##### Optionale Komponenten
Geometriebibliothek (nur für bestimmte Autodesk-Modellierungswerkzeuge wie Revit, Civil 3D, Advanced Steel usw. verfügbar)

### Fehlerbehebung
Wenn Sie den Build entpackt haben und DynamoSandbox.exe gar nicht starten konnten, stellen Sie sicher, dass Sie den Build mit [7zip](https://www.7-zip.org/download.html) entpacken. Sie können die Blockierung des ZIP-Archivs *vor* dem Extrahieren auch manuell aufheben, sofern Sie über entsprechende Berechtigungen auf Ihrem Computer verfügen.

![](images/a-7/dynamo-builds-1.png)


Wenn eine der erforderlichen Komponenten fehlt, können Probleme bei der Verwendung von Dynamo auftreten, und bestimmte Teile der Benutzeroberfläche können möglicherweise nicht geladen werden.

Am Beispiel des folgenden Screenshots sehen wir, dass beim Entpacken des Builds auf einer fehlerfreien virtuellen Windows 10-Maschine ohne GPU auf dem Computer beide erforderlichen Komponenten fehlen. Dies wird in der Dynamo-Konsole angezeigt.

![](images/a-7/dynamo-builds-2.png)

##### Installieren von DirectX
Folgen Sie den hier aufgeführten Microsoft-Anweisungen, um zu prüfen, ob DirectX bereits installiert ist. Wenn nicht, können Sie DXSETUP.exe [hier](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX) in unserem Dynamo-GitHub-Repository öffnen. Wenn das Dialogfeld unten angezeigt wird, klicken Sie auf Next (Weiter), um DirectX am Vorgabespeicherort zu installieren.

![](images/a-7/dynamo-builds-3.png)

##### Installation von Microsoft Visual C++ 2015-2024 Redistributable (x64)
Laden Sie [hier](https://aka.ms/vs/17/release/vc_redist.x64.exe) die neueste Version herunter. Dann sollten Sie das Installationsprogramm mit dem Namen vc_redist.x64.exe im Download-Verzeichnis Ihres Browsers ausführen können. Wenn das unten stehende Dialogfeld angezeigt wird, klicken Sie auf Install (Installieren), um diese Komponente am vorgegebenen Speicherort abzulegen.

![](images/a-7/dynamo-builds-4.png)


Starten Sie DynamoSandbox.exe nach der Installation der beiden erforderlichen Komponenten über den obigen Link neu. Das folgende Ergebnis sollte angezeigt werden:

![](images/a-7/dynamo-builds-5.png)

##### Fehlende 3D-Grafik. 

Es kann auch vorkommen, dass beim ersten Ausführen von Sandbox Grafikprobleme auftreten. Sehen Sie sich in diesem Fall die häufig gestellten Fragen zu Standard-Grafikproblemen hier an:

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

Grundsätzlich müssen Sie wahrscheinlich den GPU-Hochleistungsmodus für Ihre Grafikkarte erzwingen, wenn Sie DynamoSandbox.exe verwenden.

_Beispiel für Nvidia-Systemsteuerung:_

![](images/a-7/dynamo-builds-6.png)

##### Installation von WebView2 Runtime
Derzeit verwenden die folgenden Dynamo-Module die WebView2-Komponente: Dokumentationsbrowser, geführte Touren und Bibliothek. Um sicherzustellen, dass der Web-Inhalt in diesen Teilen von Dynamo korrekt angezeigt wird, müssen wir das Installationsprogramm für WebView2 Evergreen Runtime installieren. (Sie müssen prüfen, ob die Anwendung bereits auf dem Computer installiert ist oder installiert werden muss.)

Dies ist der Link zum Installieren von WebView2 Runtime: [https://developer.microsoft.com/de-de/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/de-de/microsoft-edge/webview2/#download-section)

![](images/a-7/dynamo-builds-7.png)

Eines der Installationsprogramme Evergreen Bootstrapper oder Evergreen Standalone Installer sollte installiert werden. Das erste Programm lädt ein 1.50 MB großes und das zweite ein 130 MB großes Installationsprogramm herunter.

Nach der Installation von Runtime sollten die nächsten Komponenten von Dynamo ordnungsgemäß funktionieren:

![](images/a-7/dynamo-builds-8.png)


##### Probleme mit Dynamo-Excel-Blöcken
Weitere Informationen zur Diagnose finden Sie in diesem [Artikel](https://knowledge.autodesk.com/support/revit-products/troubleshooting/caas/sfdcarticles/sfdcarticles/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html).

### Speicherort der Dynamo-Builds
Stabile Versionen

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

Tägliche Builds und stabile Versionen

[https://dynamobuilds.com/](https://dynamobuilds.com/)

