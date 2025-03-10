# Dynamo-Integration

Sie haben die Integrationsdokumentation für die visuelle Programmiersprache von Dynamo aufgerufen.

In diesem Handbuch werden verschiedene Aspekte zum Hosten von Dynamo in Ihrer Anwendung erläutert, damit Benutzer mithilfe visueller Programmierung mit Ihrer Anwendung interagieren können.

Inhalt:
* [Diese Einführung](#dynamo-integration): Allgemeiner Überblick über den Inhalt dieses Handbuchs und über Dynamo.
* [Benutzerdefinierter Dynamo-Einstiegspunkt](#dynamo-custom-entry-point): Anleitung zum Erstellen eines DynamoModel-Objekts und wo Sie beginnen sollten.
* [Elementbindung und Trace](#-element-binding-and-trace): Verwenden des Trace-Mechanismus von Dynamo zum Binden von Blöcken im Diagramm an deren Ergebnisse in Ihrem Host
* [Dynamo Revit-Auswahlblöcke](#-dynamo-revit-selection-nodes): Implementieren von Blöcken, mit denen Benutzer Objekte oder Daten aus Ihrem Host auswählen und als Eingaben an das Dynamo-Diagramm weitergeben können 
* [Überblick über die integrierten Dynamo-Pakete](#dynamo-built-in-packages-overview): Erläuterung der Dynamo-Standardbibliothek und Verwenden des zugrunde liegenden Mechanismus, um Pakete mit Ihrer Integration zu senden


##### Informationen zur Begriffsverwendung:
In diesen Dokumenten werden die Begriffe Dynamo-Skript, -Diagramm und -Programm synonym verwendet, um auf den Code zu verweisen, den Benutzer in Dynamo erstellen.

## Benutzerdefinierter Dynamo-Einstiegspunkt
#### Dynamo Revit als Beispiel 

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

Das `DynamoModel`-Objekt ist der Einstiegspunkt für eine Anwendung, die Dynamo hostet. Es stellt eine Dynamo-Anwendung dar. Das Modell ist das Stammobjekt der obersten Ebene, das Referenzen zu den anderen wichtigen Datenstrukturen und Objekten enthält, aus denen die Dynamo-Anwendung und die virtuelle DesignScript-Maschine bestehen. 

Ein Konfigurationsobjekt wird verwendet, um allgemeine Parameter für das `DynamoModel`-Objekt festzulegen, wenn es konstruiert wird. 

Die Beispiele in diesem Dokument stammen aus der DynamoRevit-Implementierung, einer Integration, bei der Revit ein `DynamoModel`-Objekt als Zusatzmodul hostet. (Plugin-Architektur für Revit.) Wenn dieses Zusatzmodul geladen wird, wird ein `DynamoModel`-Objekt gestartet, und es wird dem Benutzer mit einem `DynamoView`\- und `DynamoViewModel`-Objekt angezeigt. 

Dynamo ist ein C#-.NET-Projekt. Um es in Ihrer Anwendung verwenden zu können, müssen Sie .NET-Code hosten und ausführen können.

DynamoCore ist eine plattformübergreifende Berechnungs-Engine und Sammlung von Core-Modellen, die mit .NET oder Mono (zukünftig .NET Core) erstellt werden können. DynamoCoreWPF enthält jedoch nur die Windows-Benutzeroberflächenkomponenten von Dynamo und kann nicht auf anderen Plattformen kompiliert werden.

### Schritte zum Anpassen des Dynamo-Einstiegspunkts 

Um das `DynamoModel`-Objekt zu initialisieren, müssen Integratoren die folgenden Schritte an einer beliebigen Stelle im Host-Code ausführen.

### Laden Sie freigegebene Dynamo-DLL-Dateien vorab vom Host.  

Derzeit enthält die Liste in D4R nur `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.` Dies ist erforderlich, um Bibliotheksversionskonflikte zwischen Dynamo und Revit zu vermeiden. Beispiel: Wenn Konflikte bei `AvalonEdit` auftreten, kann die Funktion des Codeblocks völlig zerstört werden. Das Problem wurde in Dynamo 1.1.x unter https://github.com/DynamoDS/Dynamo/issues/7130 gemeldet und ist auch manuell reproduzierbar. Wenn Integratoren Bibliothekskonflikte zwischen der Host-Funktion und Dynamo festgestellt haben, sollten sie dies als ersten Schritt ausführen. Dies ist manchmal erforderlich, um zu verhindern, dass andere Plugins oder die Host-Anwendung selbst eine inkompatible Version von freigegebenen Abhängigkeiten laden bzw. lädt. Eine bessere Lösung besteht darin, den Versionskonflikt durch Ausrichten der Version zu lösen oder nach Möglichkeit eine .NET-Bindungsumleitung in der app.config-Datei des Hosts zu verwenden. 
 

### Laden von ASM 

#### Was sind ASM und LibG?

ASM ist die ADSK-Geometriebibliothek, auf der Dynamo aufbaut.

LibG ist ein benutzerfreundlicher .NET-Wrapper für den ASM-Geometrie-Kernel. LibG teilt sein Versionierungsschema mit ASM und verwendet die gleiche Haupt- und Nebenversionsnummer wie ASM, um anzugeben, dass es sich um den entsprechenden Wrapper einer bestimmten ASM-Version handelt. Wenn eine ASM-Version angegeben wird, sollte die entsprechende LibG-Version identisch sein. LibG sollte in den meisten Fällen mit allen Versionen von ASM einer bestimmten Hauptversion funktionieren. Beispiel: Mit LibG 223 sollte jede ASM 223-Version geladen werden können.


#### Dynamo Sandbox lädt ASM 

Dynamo Sandbox ist so konzipiert, dass mehrere ASM-Versionen verwendet werden können. Zu diesem Zweck werden mehrere LibG-Versionen gebündelt und mit dem Core ausgeliefert. Es gibt eine integrierte Funktion im Dynamo Shape Manager, um nach Autodesk-Produkten zu suchen, die im Lieferumfang von ASM enthalten sind, sodass Dynamo ASM aus diesen Produkten laden und Geometrieblöcke verwenden kann, ohne explizit in eine Host-Anwendung geladen zu werden. Die Produktliste in ihrer aktuellen Form lautet folgendermaßen: 
```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```
Dynamo durchsucht die Windows-Registrierung und ermittelt, ob die Autodesk-Produkte in dieser Liste auf dem Computer des Benutzers installiert sind. Sind diese Produkte installiert, sucht das Programm nach ASM-Binärdateien, ruft die Version ab und sucht in Dynamo nach einer entsprechenden LibG-Version.  

Aufgrund der ASM-Version wählt die folgende ShapeManager-API den entsprechenden LibG-Preloader-Speicherort zum Laden aus. Wenn es eine exakte Versionsübereinstimmung gibt, wird diese verwendet, andernfalls wird die nächstliegende LibG-Version darunter, jedoch mit derselben Hauptversion, geladen.  

Beispiel: Wenn Dynamo in einen Revit-Entwickler-Build integriert ist, in dem ein neuerer ASM-Build 225.3.0 vorhanden ist, versucht Dynamo, LibG 225.3.0 zu verwenden, falls vorhanden. Andernfalls wird versucht, die nächstliegende Hauptversion zu verwenden, die niedriger ist als die erste Wahl, z. B. 225.0.0. 

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)` 

#### Prozessinterne Dynamo-Integration lädt ASM vom Host 

Revit ist der erste Eintrag in der ASM-Produktsuchliste. Dies bedeutet, dass `DynamoSandbox.exe` vorgabemäßig zuerst versucht, ASM aus Revit zu laden. Sie sollten aber dennoch sicherstellen, dass die integrierte D4R-Arbeitssitzung ASM aus dem aktuellen Revit-Host lädt. Beispiel: Wenn der Benutzer sowohl R2018 als auch R2020 auf dem Computer installiert hat, sollte D4R beim Starten über R2020 ASM 225 aus R2020 anstelle von ASM 223 aus R2018 verwenden. Integratoren müssen ähnliche Aufrufe wie die folgenden implementieren, um das Laden ihrer angegebenen Version zu erzwingen. 

```
internal static Version PreloadAsmFromRevit() 

{ 

     var asmLocation = AppDomain.CurrentDomain.BaseDirectory; 
     Version libGVersion = findRevitASMVersion(asmLocation); 
     var dynCorePath = DynamoRevitApp.DynamoCorePath; 
     var preloaderLocation = DynamoShapeManager.Utilities.GetLibGPreloaderLocation(libGVersion, dynCorePath); 
     Version preLoadLibGVersion = PreloadLibGVersion(preloaderLocation); 
     DynamoShapeManager.Utilities.PreloadAsmFromPath(preloaderLocation, asmLocation); 
     return preLoadLibGVersion; 

} 
```

#### Dynamo lädt ASM aus einem benutzerdefinierten Pfad 

Seit Kurzem können `DynamoSandbox.exe` und `DynamoCLI.exe` auch eine bestimmte ASM-Version laden. Zum Überspringen des normalen Suchverhaltens in der Registrierung können Sie das Flag `�gp` verwenden, um zu erzwingen, dass Dynamo ASM aus einem bestimmten Pfad lädt. 

`DynamoSandbox.exe -gp �somePath/To/ASMDirectory/� `

  



### Erstellen eines StartConfiguration-Objekts 

StartupConfiguration wird als Parameter für die Initialisierung des DynamoModel-Objekts übergeben, was darauf hinweist, dass fast alle Definitionen zum Anpassen Ihrer Dynamo-Sitzungseinstellungen enthalten sind. Je nachdem, wie die folgenden Eigenschaften festgelegt sind, kann die Dynamo-Integration zwischen verschiedenen Integratoren variieren. Beispielsweise könnten verschiedene Integratoren unterschiedliche Python-Vorlagenpfade oder angezeigte Zahlenformate einstellen. 

Sie besteht aus folgenden Komponenten: 

* DynamoCorePath // Speicherort der ladenden DynamoCore-Binärdateien

* DynamoHostPath // Speicherort der Binärdateien der Dynamo-Integration

* GeometryFactoryPath // Speicherort der geladenen LibG-Binärdateien

* PathResolver // Objekt, das beim Auflösen verschiedener Dateien hilft

* PreloadLibraryPaths // Speicherort der Binärdateien der vorgeladenen Blöcke, z. B. DSOffice.dll 

* AdditionalNodeDirectories // Speicherort zusätzlicher Block-Binärdateien
 
* AdditionalResolutionPaths // Zusätzliche Assembly-Lösungspfade für andere Abhängigkeiten, die beim Laden von Bibliotheken erforderlich sein können 

* UserDataRootFolder // Benutzerdatenordner, z. B. `"AppData\Roaming\Dynamo\Dynamo Revit"` 

* CommonDataRootFolder // Vorgabeordner zum Speichern von benutzerdefinierten Definitionen, Beispielen usw.

* Context // Integrator-Hostname + Version `(Revit<BuildNum>)`

* SchedulerThread // Integrator-Scheduler-Thread, der `ISchedulerThread` implementiert. Für die meisten Integratoren ist dies der Haupt-Benutzeroberflächen-Thread oder der Thread, von dem aus sie auf ihre API zugreifen können.

* StartInTestMode // Angabe, ob die aktuelle Sitzung eine Testautomatisierungssitzung ist. Ändert eine Reihe von Dynamo-Verhaltensweisen. Verwenden Sie diese Option nur, wenn Sie Tests schreiben.

* AuthProvider // Implementierung von IAuthProvider des Integrators. Die RevitOxygenProvider-Implementierung befindet sich z. B. in der Datei Greg.dll. Wird für die PackageManager-Upload-Integration verwendet. 

### Voreinstellungen 

Der Pfad für die Vorgabeeinstellungen wird von `PathManager.PreferenceFilePath` verwaltet, z. B. `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`. Integratoren können entscheiden, ob sie auch eine benutzerdefinierte Datei mit den Voreinstellungen an einen Speicherort senden möchten, der mit dem Pfadmanager abgestimmt werden muss. Im Folgenden sind Voreinstellungseigenschaften aufgeführt, die serialisiert werden: 

* IsFirstRun // Gibt an, ob diese Version von Dynamo zum ersten Mal ausgeführt wird, z. B., um zu bestimmen, ob eine Meldung zur Aktivierung/Deaktivierung der allgemeinen Verfügbarkeit angezeigt werden muss. Wird auch verwendet, um zu ermitteln, ob die älteren Dynamo-Voreinstellungen beim Starten einer neuen Dynamo-Version migriert werden müssen, damit die Benutzer eine konsistente Erfahrung erhalten. 

* IsUsageReportingApproved // Gibt an, ob Nutzungsberichte genehmigt wurden. 

* IsAnalyticsReportingApproved // Gibt an, ob die Analyse-Berichterstellung genehmigt wurde. 

* LibraryWidth // Breite der linken Bibliotheksgruppe von Dynamo 

* ConsoleHeight // Höhe der Konsolenanzeige 

* ShowPreviewBubbles // Gibt an, ob Vorschaufenster angezeigt werden sollen. 

* ShowConnector // Gibt an, ob Connectors angezeigt werden. 

* ConnectorType // Gibt den Connector-Typ an: Bezier oder Polylinie. 

* BackgroundPreviews // Gibt den aktiven Status der angegebenen Hintergrundvorschau an. 

* RenderPrecision // Render-Genauigkeitsstufe. Ein niedrigerer Wert generiert Netze mit weniger Dreiecken. Mit einem höheren Wert wird eine glattere Geometrie in der Hintergrundvorschau erzeugt. 128 ist ein guter Mittelwert für die Vorschau von Geometrie.

* ShowEdges // Gibt an, ob Oberflächen- und Volumenkörperkanten gerendert werden. 

* ShowDetailedLayout // NICHT VERWENDET

* WindowX, WindowY // Letzte X-, Y-Koordinate des Dynamo-Fensters 

* WindowW, WindowH // Letzte Breite, Höhe des Dynamo-Fensters 

* UseHardwareAcceleration // Angabe, ob Dynamo die Hardwarebeschleunigung verwenden soll, wenn diese unterstützt wird 

* NumberFormat // Dezimalgenauigkeit, die zum Anzeigen von Zahlen im Vorschaufenster toString() verwendet wird 

* MaxNumRecentFiles // Maximale Anzahl der aktuellen Dateipfade, die gespeichert werden sollen 

* RecentFiles // Liste der zuletzt geöffneten Dateipfade. Wenn Sie hier Änderungen vornehmen, wirkt sich dies direkt auf die Liste der zuletzt geöffneten Dateien auf der Startseite von Dynamo aus. 

* BackupFiles // Liste von Sicherungsdateipfaden 

* CustomPackageFolders // Liste von Ordnern mit Zero-Touch-Binärdateien und Verzeichnispfaden, die nach Paketen und benutzerdefinierten Blöcken durchsucht werden.

* PackageDirectoriesToUninstall // Liste von Paketen, die vom Package Manager verwendet werden, um zu bestimmen, welche Pakete zum Löschen markiert sind. Diese Pfade werden nach Möglichkeit beim Start von Dynamo gelöscht.

* PythonTemplateFilePath // Pfad zur Python-Datei (.py), die beim Erstellen eines neuen PythonScript-Blocks als Startvorlage verwendet werden soll. Diese Option kann zum Einrichten einer benutzerdefinierten Python-Vorlage für Ihre Integration verwendet werden.

* BackupInterval // Gibt an, wie lange (in Millisekunden) das Diagramm automatisch gespeichert wird. 

* BackupFilesCount // Gibt an, wie viele Sicherungen erstellt werden. 

* PackageDownloadTouAccepted // Gibt an, ob der Benutzer die Nutzungsbedingungen für das Herunterladen von Paketen aus dem Package Manager akzeptiert hat. 

* OpenFileInManualExecutionMode // Gibt den Vorgabestatus des Kontrollkästchens Im manuellen Ausführungsmodus öffnen in OpenFileDialog an. 

* NamespacesToExcludeFromLibrary // Gibt an, welche Namensbereiche nicht in der Dynamo-Blockbibliothek angezeigt werden sollen (sofern vorhanden). Zeichenfolgenformat: "[Bibliotheksname]:[vollständig qualifizierter Namensbereich]". 

Beispiel für serialisierte Voreinstellungen: 

``` xml 
<PreferenceSettings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> 

<IsFirstRun>false</IsFirstRun> 

<IsUsageReportingApproved>false</IsUsageReportingApproved> 

<IsAnalyticsReportingApproved>false</IsAnalyticsReportingApproved> 

<LibraryWidth>204</LibraryWidth> 

<ConsoleHeight>0</ConsoleHeight> 

<ShowPreviewBubbles>true</ShowPreviewBubbles> 

<ShowConnector>true</ShowConnector> 

<ConnectorType>BEZIER</ConnectorType> 

<BackgroundPreviews> 

<BackgroundPreviewActiveState> 

<Name>IsBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

<BackgroundPreviewActiveState> 

<Name>IsRevitBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

</BackgroundPreviews> 

<IsBackgroundGridVisible>true</IsBackgroundGridVisible> 

<RenderPrecision>128</RenderPrecision> 

<ShowEdges>false</ShowEdges> 

<ShowDetailedLayout>true</ShowDetailedLayout> 

<WindowX>553</WindowX> 

<WindowY>199</WindowY> 

<WindowW>800</WindowW> 

<WindowH>676</WindowH> 

<UseHardwareAcceleration>true</UseHardwareAcceleration> 

<NumberFormat>f3</NumberFormat> 

<MaxNumRecentFiles>10</MaxNumRecentFiles> 

<RecentFiles> 

<string></string> 

</RecentFiles> 

<BackupFiles> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\backup\backup.DYN</string> 

</BackupFiles> 

<CustomPackageFolders> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\2.5</string> 

</CustomPackageFolders> 

<PackageDirectoriesToUninstall /> 

<PythonTemplateFilePath /> 

<BackupInterval>60000</BackupInterval> 

<BackupFilesCount>1</BackupFilesCount> 

<PackageDownloadTouAccepted>true</PackageDownloadTouAccepted> 

<OpenFileInManualExecutionMode>false</OpenFileInManualExecutionMode> 

<NamespacesToExcludeFromLibrary> 

<string>ProtoGeometry.dll:Autodesk.DesignScript.Geometry.TSpline</string> 

</NamespacesToExcludeFromLibrary> 

</PreferenceSettings> 
``` 
 

* Extensions // Liste der Erweiterungen, die IExtension implementieren. Wenn die Angabe null lautet, lädt Dynamo Erweiterungen aus dem Vorgabepfad (Ordner `extensions` im Ordner Dynamo). 

* IsHeadless // Gibt an, ob Dynamo ohne Benutzeroberfläche gestartet wird. Wirkt sich auf Analytics aus. 

* UpdateManager // Implementierung des UpdateManager durch den Integrator, siehe Beschreibung oben. 

* ProcessMode // Entspricht TaskProcessMode. Synchron, wenn im Testmodus, ansonsten Asynchron. Dies steuert das Verhalten des Schedulers. In Single-Thread-Umgebungen kann dies auch auf Synchron gesetzt werden.

Verwenden Sie zum Starten von `DynamoModel` das StartConfiguration-Zielobjekt.

Sobald StartConfig übergeben wurde, um `DynamoModel` zu starten, überwacht DynamoCore die eigentlichen Details, um sicherzustellen, dass die Dynamo-Sitzung korrekt mit den angegebenen Details initialisiert wird. Von den einzelnen Integratoren sollten nach der Initialisierung von `DynamoModel` einige dem Setup nachgelagerte Schritte ausgeführt werden, z. B. werden in D4R Ereignisse abonniert, um nach Revit-Host-Transaktionen oder Dokumentaktualisierungen, Python-Blockanpassungen usw. zu suchen. 

 ### Weiter geht es mit dem Teil zur visuellen Programmierung

Um `DynamoViewModel` und `DynamoView` zu initialisieren, müssen Sie zunächst ein `DynamoViewModel`-Objekt erstellen, was mit der statischen Methode `DynamoViewModel.Start` möglich ist. Siehe unten:

``` c#

    viewModel = DynamoViewModel.Start(
                    new DynamoViewModel.StartConfiguration()
                    {
                        CommandFilePath = commandFilePath,
                        DynamoModel = model,
                        Watch3DViewModel = 
                            HelixWatch3DViewModel.TryCreateHelixWatch3DViewModel(
                                null,
                                new Watch3DViewModelStartupParams(model), 
                                model.Logger),
                        ShowLogin = true
                    });
     
     var view = new DynamoView(viewModel);

```

`DynamoViewModel.StartConfiguration` verfügt über weit weniger Optionen als die Konfiguration des Modells. Die Optionen sind größtenteils selbsterklärend. `CommandFilePath` kann ignoriert werden, es sei denn, Sie schreiben einen Testfall.


Der Parameter `Watch3DViewModel` steuert, wie die Hintergrundvorschau und Watch3D-Blöcke 3D-Geometrie anzeigen. Sie können Ihre eigene Implementierung verwenden, wenn Sie die erforderlichen Schnittstellen implementieren.

Um `DynamoView` zu konstruieren, wird lediglich `DynamoViewModel` benötigt. Die Ansicht ist ein Fenstersteuerelement und kann mit WPF angezeigt werden.

 ### Beispiel für DynamoSandbox.exe:

 DynamoSandbox.exe ist eine Entwicklungsumgebung zum Testen und Verwenden von sowie Experimentieren mit DynamoCore. Dies ist ein gutes Beispiel, um zu sehen, wie `DynamoCore`- und `DynamoCoreWPF`-Komponenten geladen und eingerichtet werden. Sie können einen Teil des Einstiegspunkts [hier](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37) einsehen.

 ## Elementbindung und Trace

#### Überblick

*Trace* ist ein Mechanismus in Dynamo Core, mit dem Daten in die DYN-Datei (Dynamo-Datei) serialisiert werden können. Entscheidend ist, dass diese Daten den Callsites von Blöcken im Dynamo-Diagramm zugeordnet sind.

Wenn Sie ein Dynamo-Diagramm von einem Datenträger aus öffnen, werden die darin gespeicherten Trace-Daten erneut mit den Blöcken des Diagramms verknüpft.

#### Glossar:
* Trace-Mechanismus:
    
  * Implementiert die Elementbindung in Dynamo.
  * Der Trace-Mechanismus kann verwendet werden, um sicherzustellen, dass Objekte erneut an die von diesen erstellte Geometrie gebunden werden.
  * Der Callsite- und der Trace-Mechanismus sorgen für die Bereitstellung einer dauerhaften GUID, die der Block-Implementierer für die Neuverknüpfung verwenden kann.

* Callsite

  * Die ausführbare Datei enthält mehrere Callsites. Diese Callsites werden verwendet, um die Ausführung an die verschiedenen Stellen zu verteilen, von denen aus sie verteilt werden müssen:
    * C#-Bibliothek
    * Integrierte Methode
    * DesignScript-Funktion
    * Benutzerdefinierter Block (DS-Funktion)

* TraceSerializer
  * Serialisiert mit `ISerializable` und `[Serializable]` markierte Klassen in Trace.
  * Verarbeitet die Serialisierung und Deserialisierung von Daten in Trace.
  * TraceBinder steuert die Bindung deserialisierter Daten an einen Laufzeittyp (erstellt eine Instanz einer Realklasse).

#### Wie sieht das aus?
----

Trace-Daten werden in die DYN-Datei innerhalb einer Eigenschaft namens Bindings serialisiert. Dies ist ein Array von Callsite-IDs -> Daten. Eine Callsite ist die Stelle/Instanz, an der ein Block auf der virtuellen DesignScript-Maschine aufgerufen wird. Es sollte erwähnt werden, dass Blöcke in einem Dynamo-Diagramm mehrmals aufgerufen werden können und somit mehrere Callsites für eine einzelne Blockinstanz erstellt werden können.

```json
"Bindings": [
    {
      "NodeId": "1e83cc25-7de6-4a7c-a702-600b79aa194d",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_1e83cc25-7de6-4a7c-a702-600b79aa194d":  "Base64 Encoded Data"
      }
    },
    {
      "NodeId": "c69c7bec-d54b-4ead-aea8-a3f45bea9ab2",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_c69c7bec-d54b-4ead-aea8-a3f45bea9ab2": "Base64 Encoded Data"
      }
    }
  ],

 
```

 Es ist *NICHT* ratsam, sich auf das Format der serialisierten Base64-codierten Daten zu verlassen.


#### Welches Problem versuchen wir zu lösen?
----


Es gibt viele Gründe, warum beliebige Daten als Ergebnis einer Funktionsausführung gespeichert werden sollten. In diesem Fall wurde Trace jedoch entwickelt, um ein spezifisches Problem zu lösen, auf das Benutzer häufig stoßen, wenn sie Software-Programme erstellen und iterieren, die Elemente in Host-Anwendungen erstellen.

Das Problem haben wir mit `Element Binding` bezeichnet. Die Idee dahinter ist folgende:

Wenn ein Benutzer ein Dynamo-Diagramm entwickelt und ausführt, werden wahrscheinlich neue Elemente im Host-Anwendungsmodell generiert. In unserem Beispiel nehmen wir an, der Benutzer verwendet ein kleines Programm, mit dem 100 Türen in einem Architekturmodell generiert werden. Die Anzahl und Position dieser Türen wird durch das Programm gesteuert.

Wenn der Benutzer das Programm zum ersten Mal ausführt, werden diese 100 Türen generiert.

Wenn der Benutzer später eine Eingabe in seinem Programm ändert und es erneut ausführt, erstellt das Programm *(ohne Elementbindung)* 100 neue Türen, und die alten Türen sind zusammen mit den neuen Türen weiterhin im Modell vorhanden.

----

Da es sich bei Dynamo um eine Live-Programmierumgebung handelt, die über den Ausführungsmodus `"Automatic"` verfügt, in dem Änderungen am Diagramm eine neue Ausführung auslösen, kann dies schnell dazu führen, dass ein Modell mit den Ergebnissen vieler Programmausführungen überfüllt wird.


Wir haben festgestellt, dass dies in der Regel nicht den Erwartungen der Benutzer entspricht. Stattdessen werden bei aktivierter Elementbindung die vorherigen Ergebnisse einer Diagrammausführung bereinigt und gelöscht bzw. geändert. Welche Option (*gelöscht oder geändert*) verwendet wird, hängt von der Flexibilität der API Ihres Hosts ab. Bei aktivierter Elementbindung bleibt es auch nach der zweiten, dritten oder 50\. Ausführung des Dynamo-Programms des Benutzers bei 100 Türen im Modell.

Dazu ist mehr erforderlich als nur die Möglichkeit, Daten in die DYN-Datei zu serialisieren. Wie Sie unten sehen werden, gibt es in DynamoRevit Mechanismen, die auf der Trace-Funktion aufbauen und diese Arbeitsabläufe für Neubindungen unterstützen.

----

An dieser Stelle möchten wir den anderen wichtigen Anwendungsfall der Elementbindung für Hosts wie Revit erwähnen. Da Elemente, die bei aktivierter Elementbindung erstellt wurden, versuchen, die vorhandenen Element-IDs beizubehalten (Änderung vorhandener Elemente), ist die Logik, die in der Host-Anwendung auf Basis dieser Elemente erstellt wurde, nach der Ausführung eines Dynamo-Programms weiterhin vorhanden. Beispiel:

Kehren wir zu unserem Beispiel des Architekturmodells zurück.

Wir sehen uns zunächst ein Beispiel mit deaktivierter Elementbindung an. Diesmal verfügt der Benutzer über ein Programm, das nichttragende Wände generiert.

 Er führt sein Programm aus, und es werden einige Wände in der Host-Anwendung generiert. Anschließend verlässt er das Dynamo-Diagramm und verwendet die normalen Revit-Werkzeuge, um einige Fenster in diesen Wänden zu platzieren. Die Fenster sind als Teil des Revit-Modells an diese spezifischen Wände gebunden.

Der Benutzer kehrt zurück zu Dynamo und führt das Diagramm erneut aus. Wie im letzten Beispiel sind nun zwei Sätze von Wänden vorhanden. Beim ersten Satz wurden die Fenster hinzugefügt, bei den neuen Wänden jedoch nicht.

Wenn die Elementbindung aktiviert gewesen wäre, könnten wir die bereits geleistete Arbeit, die manuell ohne Dynamo in der Host-Anwendung durchgeführt wurde, weiterverwenden. Wenn z. B. beim zweiten Ausführen des Programms durch den Benutzer die Bindung aktiviert gewesen wäre, wären die Wände geändert, nicht gelöscht worden, und die nachfolgenden Änderungen in der Host-Anwendung wären erhalten geblieben. Das Modell würde Wände mit Fenstern, anstelle von zwei Sätzen von Wänden in verschiedenen Zuständen enthalten.

-----

![Wände erstellen](images/creates_walls.png)


#### Elementbindung und Trace im Vergleich
----

Trace ist ein Mechanismus in Dynamo Core. Er verwendet eine statische Variable von Callsites für Daten, um die Daten mit der Callsite einer Funktion im Diagramm zu verknüpfen, wie oben beschrieben.

Außerdem können Sie beliebige Daten in die DYN-Datei serialisieren, wenn Sie Zero-Touch-Blöcke in Dynamo schreiben. Dies ist im Allgemeinen nicht empfehlenswert, da dies bedeutet, dass der potenziell übertragbare Zero-Touch-Code nun von Dynamo Core abhängig ist.

Verlassen Sie sich nicht auf das serialisierte Format der Daten in der DYN-Datei, sondern verwenden Sie das Attribut und die Schnittstelle [Serializable].

ElementBinding hingegen baut auf den Trace-APIs auf und ist in der Dynamo-Integration *(DynamoRevit, Dynamo4Civil usw.)* implementiert.


#### Trace-APIs
Einige der untergeordneten Trace-APIs, die Sie kennen sollten, sind:

``` c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

Sie können deren Verwendung im folgenden Beispiel sehen:

Um mit den Trace-Daten zu interagieren, die Dynamo aus einer vorhandenen Datei geladen hat oder generiert, können Sie sich Folgendes ansehen:

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```
[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)


#### Beispiel für eine einfache Trace-Funktion aus einem Block
----
Ein Beispiel für einen Dynamo-Block, in dem Trace direkt verwendet wird, finden Sie hier im [DynamoSamples-Repository](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs).

Die Zusammenfassung der Klasse dort enthält den Kern dessen, worum es bei Trace geht:

```
  /*
     * After a graph update, Dynamo typically disposes of all
     * objects created during the graph update. But what if there are 
     * objects which are expensive to re-create, or which have other
     * associations in a host application? You wouldn't want those those objects
     * re-created on every graph update. For example, you might 
     * have an external database whose records contain data which needs
     * to be re-applied to an object when it is created in Dynamo.
     * In this example, we use a wrapper class, TraceExampleWrapper, to create 
     * TraceExampleItem objects which are stored in a static dictionary 
     * (they could be stored in a database as well). On subsequent graph updates, 
     * the objects will be retrieved from the data store using a trace id stored 
     * in the trace cache.
     */
```

In diesem Beispiel werden die Trace-APIs in DynamoCore direkt verwendet, um Daten zu speichern, wenn ein bestimmter Block ausgeführt wird. In diesem Fall übernimmt ein Wörterbuch die Rolle des Host-Anwendungsmodells, wie die Modelldatenbank von Revit.

Das grobe Setup lautet:

Eine statische Dienstprogramm-Klasse `TraceExampleWrapper` wird als Block in Dynamo importiert. Sie enthält eine einzige Methode `ByString`, mit der `TraceExampleItem` erstellt wird. Dies sind normale .NET-Objekte, die eine `description`-Eigenschaft enthalten.

Jedes `TraceExampleItem`-Objekt wird in Trace serialisiert und als `TraceableId` dargestellt. Dies ist nur eine Klasse, die ein `IntId`-Objekt enthält, das mit `[Serializeable]` markiert ist, damit es mit dem `SOAP`-Formatierer serialisiert werden kann. Weitere Informationen zum Serializable-Attribut finden Sie [hier](https://docs.microsoft.com/de-de/dotnet/api/system.serializableattribute?view=netframework-4.8).

Sie müssen außerdem die [hier](https://docs.microsoft.com/de-de/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8) definierte `ISerializable`-Schnittstelle implementieren.


``` c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

Diese Klasse wird für jedes `TraceExampleItem`-Objekt erstellt, das wir in Trace speichern möchten, serialisiert, Base64-codiert und auf dem Datenträger gespeichert, wenn das Diagramm gespeichert wird, sodass Bindungen neu zugeordnet werden können. Dies ist auch später möglich, wenn das Diagramm wieder zusätzlich zu einem vorhandenen Wörterbuch von Elementen geöffnet wird. Das wird in diesem Beispiel nicht gut funktionieren, da das Wörterbuch nicht wirklich dauerhaft ist, wie es bei einem Revit-Dokument der Fall ist.

Der letzte Teil der Gleichung ist schließlich das `TraceableObjectManager`-Objekt. Dieses ähnelt `ElementBinder` in `DynamoRevit`. Damit wird die Beziehung zwischen den im Dokumentmodell des Hosts vorhandenen Objekten und den in Trace in Dynamo gespeicherten Daten verwaltet.

Wenn ein Benutzer ein Diagramm mit dem Block `TraceExampleWrapper.ByString` zum ersten Mal ausführt, wird ein neues `TraceableId`-Objekt mit einer neuen ID erstellt, das `TraceExampleItem`-Objekt wird im Wörterbuch gespeichert, das dieser neuen ID zugeordnet ist, und wir speichern das `TraceableID`-Objekt in Trace.

Bei der nächsten Ausführung des Diagramms suchen wir in Trace die dort gespeicherte ID, anschließend das Objekt, das dieser ID zugeordnet ist, und geben dieses Objekt zurück. Anstatt ein neues Objekt zu erstellen, ändern wir das vorhandene.

Der Ablauf von zwei aufeinander folgenden Ausführungen des Diagramms, mit denen ein einzelnes `TraceExampleItem`-Objekt erstellt wird, sieht wie folgt aus:

![Erster Anruf](images/Trace-first-call.png)

![Zweiter Aufruf](images/Trace-second-call.png)

Die gleiche Idee wird im nächsten Beispiel anhand eines realistischeren Anwendungsfalls mit einem DynamoRevit-Block veranschaulicht.

#### Trace-Diagramm

![Trace-Schritte](images/trace_diagram.png) ![Trace-Ablauf](images/trace_alt_diagram.png)

#### ANMERKUNG:
In neueren Versionen von Dynamo TLS (Thread Local Storage, lokaler Thread-Speicher) werden statische Elemente verwendet.

#### Beispiel für die Implementierung der Elementbindung

-----
Sehen wir uns kurz an, wie ein Block aussieht, der Elementbindung verwendet, wenn er für DynamoRevit implementiert wird. Dies entspricht dem Typ eines Blocks, der oben in den Beispielen zur Wanderstellung verwendet wurde.


---


``` c#
    private void InitWall(Curve curve, Autodesk.Revit.DB.WallType wallType, Autodesk.Revit.DB.Level baseLevel, double height, double offset, bool flip, bool isStructural)
        {
            // This creates a new wall and deletes the old one
            TransactionManager.Instance.EnsureInTransaction(Document);

            //Phase 1 - Check to see if the object exists and should be rebound
            var wallElem =
                ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>(Document);

            bool successfullyUsedExistingWall = false;
            //There was a modelcurve, try and set sketch plane
            // if you can't, rebuild 
            if (wallElem != null && wallElem.Location is Autodesk.Revit.DB.LocationCurve)
            {
                var wallLocation = wallElem.Location as Autodesk.Revit.DB.LocationCurve;
                <SNIP>

                    if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;

                  <SNIP>
                
            }

            var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
            InternalSetWall(wall);

            TransactionManager.Instance.TransactionTaskDone();

            // delete the element stored in trace and add this new one
            ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
        }
```

Der obige Code zeigt einen Beispielkonstruktor für ein Wandelement. Dieser Konstruktor wird über einen Block in Dynamo wie den folgenden aufgerufen: `Wall.byParams`.

Die wichtigsten Phasen der Ausführung des Konstruktors im Zusammenhang mit der Elementbindung lauten:

1. Verwenden Sie `elementBinder`, um zu überprüfen, ob zuvor erstellte Objekte vorhanden sind, die in einer vergangenen Ausführung an diese Callsite gebunden wurden. `ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. Ist dies der Fall, versuchen Sie, diese Wand zu ändern, anstatt eine neue zu erstellen.

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. Andernfalls erstellen Sie eine neue Wand.
```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. Löschen Sie das alte Element, das wir gerade aus Trace abgerufen haben, und fügen Sie das neue Element hinzu, damit wir dieses Element in Zukunft nachschlagen können:
```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### Erläuterung

#### Wirkungsgrad
* Derzeit wird jedes serialisierte Trace-Objekt mit der SOAP-XML-Formatierung serialisiert. Diese ist recht ausführlich und dupliziert eine Menge Informationen. Anschließend werden die Daten zweimal Base64-codiert. Dies ist nicht effizient in Bezug auf Serialisierung oder Deserialisierung. In Zukunft kann dies verbessert werden, wenn das interne Format nicht darauf aufbaut. Es sei noch einmal gesagt: Verlassen Sie sich nicht auf das Format der serialisierten Daten im Ruhezustand.

#### Sollte ElementBinding vorgabemäßig aktiviert sein?
* Es gibt Anwendungsfälle, in denen die Elementbindung nicht erwünscht ist, beispielsweise, wenn ein erfahrener Dynamo-Benutzer ein Programm entwickelt, das mehrmals ausgeführt werden soll, um zufällige Elementgruppierungen zu generieren. Ziel des Programms ist es, bei jeder Ausführung zusätzliche Elemente zu erstellen. Dieser Anwendungsfall kann nur mit einer Umgehungslösung ausgeführt werden, mit der die Elementbindung aufgehoben wird. ElementBinding kann auf Integrationsebene deaktiviert werden. Dies sollte jedoch eine Kernfunktionalität von Dynamo sein. Es kann nicht klar bestimmt werden, wie detailliert diese Funktion sein sollte: Auf Blockebene? Auf Callsite-Ebene? Auf Basis der gesamten Dynamo-Sitzung? Auf Basis eines Arbeitsbereichs? usw.

## Dynamo Revit-Auswahlblöcke (was versteht man darunter?) 

Im Allgemeinen kann der Benutzer mit diesen Blöcken eine Teilmenge des aktiven Revit-Dokuments beschreiben, die er referenzieren möchte. Der Benutzer kann ein Revit-Element auf verschiedene Weise referenzieren (siehe unten), und die resultierende Ausgabe des Blocks kann ein Revit-Element-Wrapper (DynamoRevit-Wrapper) oder eine Dynamo-Geometrie (konvertiert aus Revit-Geometrie) sein. Der Unterschied zwischen diesen Ausgabetypen sollte im Kontext anderer Host-Integrationen berücksichtigt werden.

Im Allgemeinen **können Sie diese Blöcke gut als Funktion konzeptualisieren, die eine Element-ID akzeptiert und einen Zeiger auf dieses Element oder eine Geometrie, die dieses Element darstellt, zurückgibt**. 

In DynamoRevit sind mehrere `�Selection�`-Blöcke vorhanden. Sie können in mindestens zwei Gruppen unterteilt werden: 

![Revit-Auswahlblöcke](images/revitSelectionNodes.png)


 

1. Benutzeroberflächen-Auswahl durch Benutzer: 

    `DynamoRevit`-Beispielblöcke in dieser Kategorie sind`SelectModelElement` und `SelectElementFace`.

    Diese Blöcke ermöglichen es dem Benutzer, in den Kontext der Revit-Benutzeroberfläche zu wechseln und ein Element oder einen Elementsatz auszuwählen. Die IDs dieser Elemente werden erfasst, und es wird eine Konvertierungsfunktion ausgeführt. Entweder wird ein Wrapper erstellt oder Geometrie wird aus dem Element extrahiert und konvertiert. Die ausgeführte Konvertierung hängt vom Typ des Blocks ab, den der Benutzer auswählt. 

2. Dokumentabfrage: 

    Beispielblöcke in dieser Kategorie sind `AllElementsOfClass` und `AllElementsOfCategory`.

    Diese Blöcke ermöglichen es dem Benutzer, das gesamte Dokument nach einer Teilmenge von Elementen abzufragen. Die Blöcke geben in der Regel Wrapper zurück, die auf die zugrunde liegenden Revit-Elemente verweisen. Diese Wrapper sind ein wesentlicher Bestandteil der DynamoRevit-Benutzererfahrung, da sie erweiterte Funktionen wie die Elementbindung ermöglichen. Dynamo-Integratoren können so auswählen, welche Host-APIs den Benutzern als Blöcke zur Verfügung stehen.


### Dynamo Revit-Benutzerarbeitsabläufe: 

#### Beispielfälle 

1. 
    * Der Benutzer wählt eine Revit-Wand mit `SelectModelElement` aus. Ein Dynamo-Wand-Wrapper wird in das Diagramm zurückgegeben (sichtbar im Vorschaufenster des Blocks). 

    * Der Benutzer platziert den Block Element.Geometry und ordnet die `SelectModelElement`-Ausgabe diesem neuen Block zu, wobei die Geometrie der umschlossenen Wand extrahiert und mit der LibG-API in Dynamo-Geometrie konvertiert wird. 

    * Der Benutzer schaltet das Diagramm in den automatischen Ausführungsmodus um. 

    * Der Benutzer ändert die ursprüngliche Wand in Revit. 

    * Das Diagramm wird automatisch erneut ausgeführt, da das Revit-Dokument ein Ereignis ausgelöst hat, das signalisiert, dass einige Elemente aktualisiert wurden. Der Auswahlblock beobachtet dieses Ereignis und erkennt, dass die ID des ausgewählten Elements geändert wurde. 

### DynamoCivil-Benutzerarbeitsabläufe: 

Die Arbeitsabläufe in D4C ähneln der obigen Beschreibung für Revit. Hier sehen Sie zwei typische Sätze von Auswahlblöcken in D4C:  

![Civil 3D-Auswahlblöcke](images/civilSelectionNodes.png)



### Mängel: 

* Aufgrund des Dokumentänderungs-Updaters, den Auswahlblöcke in `DynamoRevit` implementieren, können Endlosschleifen einfach erstellt werden: Stellen Sie sich einen Block vor, der das Dokument auf alle Elemente hin beobachtet und dann an einer beliebigen Stelle hinter diesem Block neue Elemente erstellt. Wenn dieses Programm ausgeführt wird, löst es eine Schleife aus. `DynamoRevit` versucht, diese Fälle auf verschiedene Weise mithilfe von Transaktions-IDs zu erkennen, und vermeidet, dass das Dokument geändert wird, wenn sich die Eingaben für Elementkonstruktoren nicht geändert haben.

    Dies muss berücksichtigt werden, wenn die automatische Ausführung des Diagramms initiiert und ein ausgewähltes Element in der Host-Anwendung geändert wird. 

* Auswahlblöcke in `DynamoRevit` werden im `RevitUINodes.dll`-Projekt implementiert, das WPF referenziert. Dies muss kein Problem darstellen. Je nach Zielplattform ist es jedoch sinnvoll, dies zu beachten. 
 

### Datenablaufdiagramme

![Auswahlablauf](images/selectModelElement.png)

![Auswahlfluss2](images/selectElementFace.png)



### Technische Implementierung: (siehe Diagramme oben): 

Auswahlblöcke werden implementiert, indem sie Daten von den generischen `SelectionBase`-Typen `SelectionBase<TSelection, TResult> ` und einem minimalen Satz von Elementen übernehmen: 

* Implementierung einer `BuildOutputAST`-Methode. Diese Methode muss einen AST zurückgeben, der irgendwann in der Zukunft ausgeführt wird, wenn der Block ausgeführt werden soll. Im Falle von Auswahlblöcken sollten Elemente oder Geometrie aus den Element-IDs zurückgegeben werden. https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280 

* Die Implementierung von `BuildOutputAST` ist einer der schwierigsten Aspekte bei der Implementierung von `NodeModel`-/Benutzeroberflächen-Blöcken. Es empfiehlt sich, einer C#-Funktion so viel Logik wie möglich hinzuzufügen und einfach einen AST-Funktionsaufruf-Block in den AST einzubetten. Beachten Sie, dass `node` hier ein AST-Block in der abstrakten Syntaxstruktur und kein Block im Dynamo Diagramm ist. 

![Auswahlfluss2](images/selectionAST.png)


* Serialisierung -  

  * Da es sich hierbei um explizite abgeleitete `NodeModel`-Typen (nicht um ZeroTouch) handelt, muss auch ein [JsonConstructor] implementiert werden, der während der Deserialisierung des Blocks aus einer DYN-Datei verwendet wird. 

    Die Elementreferenzen vom Host sollten in der DYN-Datei gespeichert werden, sodass beim Öffnen eines Diagramms mit diesem Block durch einen Benutzer dessen Auswahl weiterhin festgelegt ist. NodeModel-Blöcke in Dynamo verwenden Json.NET zum Serialisieren. Alle öffentlichen Eigenschaften werden automatisch mit Json.NET serialisiert. Verwenden Sie das [JsonIgnore]-Attribut, um nur die erforderlichen Elemente zu serialisieren. 

* Dokumentabfrage-Blöcke sind etwas einfacher, da sie keine Referenz auf Element-IDs speichern müssen. Weitere Informationen zur Implementierung der `ElementQueryBase`-Klasse und abgeleiteter Klassen finden Sie weiter unten. Wenn sie ausgeführt werden, rufen diese Blöcke die Revit-API auf, fragen das zugrunde liegende Dokument nach Elementen ab und führen die zuvor erwähnte Konvertierung in Geometrie- oder Revit-Element-Wrapper durch. 

### Referenzen

#### DynamoCore-Basisklassen: 

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs )

* [NodeModel-Fallstudie – Angepasste Benutzeroberfläche](11\_developer\_primer/3\_developing\_for\_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Aktualisieren der Pakete und Dynamo-Bibliotheken für Dynamo 2.x](11\_developer\_primer/3\_developing\_for\_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Aktualisieren der Pakete und Dynamo-Bibliotheken für Dynamo 3.x](11\_developer\_primer/3\_developing\_for\_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)



#### DynamoRevit: 

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs )

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)


## Übersicht über integrierte Dynamo-Pakete

Mit dem Mechanismus für integrierte Pakete können Sie mehr Blockinhalte mit Dynamo Core bündeln, ohne den Core selbst zu erweitern. Dazu wird die von der Erweiterung `PackageLoader` und `PackageManager` implementierte Funktion zum Laden von Dynamo-Paketen genutzt.

In diesem Dokument werden die Begriffe Integrierte Pakete, Integrierte Dynamo-Pakete und Integrierte Pakete synonym verwendet.

### Sollte ich ein Paket als integriertes Paket senden?
* Das Paket muss über signierte binäre Einstiegspunkte verfügen, andernfalls wird es nicht geladen.
* Es sollten alle Anstrengungen unternommen werden, um fehlerhafte Änderungen in diesen Paketen zu vermeiden. Dies bedeutet, dass der Paketinhalt über automatische Tests verfügen sollte.
* Semantische Versionierung: Es ist wahrscheinlich eine gute Idee, das Paket mithilfe eines semantischen Versionierungsschemas zu versionieren und dies den Benutzern in der Paketbeschreibung oder in Dokumenten mitzuteilen.
* Automatisierte Tests: Siehe oben. Wenn ein Paket mithilfe des integrierten Paketmechanismus eingeschlossen wird, erscheint es für einen Benutzer als Teil des Produkts und sollte wie ein Produkt getestet werden.
* Ausgereifter Zustand: Symbole, Block-Dokumente, lokalisierte Inhalte
* Versenden Sie keine Pakete, die weder Sie noch Ihr Team verwalten können.
* Versenden Sie Pakete von Drittanbietern nicht auf diese Weise (siehe oben).

Grundsätzlich sollten Sie die vollständige Kontrolle über das Paket haben und in der Lage sein, das Paket zu reparieren, es auf dem neuesten Stand zu halten und es mit den neuesten Änderungen in Dynamo und Ihrem Produkt zu vergleichen. Sie müssen es auch signieren können.


### Integrierte Pakete und Host-integrationsspezifische Pakete im Vergleich

Wir beabsichtigen, die `Built-In Packages`-Funktion zu einer Kernfunktion zu machen, einem Satz von Paketen, auf den alle Benutzer Zugriff erhalten, auch wenn sie keinen Zugriff auf den Package Manager haben. Derzeit ist der zugrunde liegende Mechanismus zur Unterstützung dieser Funktion ein zusätzlicher vorgegebener Speicherort zum Laden von Paketen direkt im Dynamo Core-Verzeichnis, relativ zu DynamoCore.dll.

Mit einigen Einschränkungen kann dieser Speicherort von ADSK Dynamo-Clients und -Integratoren verwendet werden, um deren integrationsspezifische Pakete zu verteilen. *(Beispiel: Die Dynamo Formit-Integration erfordert ein benutzerdefiniertes Dynamo FormIt-Paket.)*

Da der zugrunde liegende Lademechanismus für Core- und Host-spezifische Pakete derselbe ist, muss sichergestellt werden, dass Pakete, die auf diese Weise verteilt werden, nicht zu Verwirrungen bei den Benutzern hinsichtlich `Built-In Packages`-Core-Paketen und integrationsspezifischen Paketen führen, die nur in einem einzigen Host-Produkt verfügbar sind. Um Verwirrungen bei den Benutzern zu vermeiden, sollten Host-spezifische Pakete im Austausch mit den Dynamo-Teams eingeführt werden.


### Lokalisierung von Paketen

Da die in der `Built-In Packages`-Funktion enthaltenen Pakete für mehr Kunden verfügbar sein werden und die Zusicherungen, die wir dafür geben, strenger sind (siehe oben), sollten sie lokalisiert werden.

Bei internen ADSK-Paketen, die für die `Built-In Packages`-Integration vorgesehen sind: Die aktuellen Einschränkungen, dass lokalisierte Inhalte nicht im Package Manager publiziert werden können, sind keine Hindernisse, da die Pakete nicht unbedingt im Package Manager publiziert werden müssen.

Mithilfe einer Umgehungslösung ist es möglich, Pakete mit Kultur-Unterverzeichnissen im Ordner /bin eines Pakets manuell zu erstellen (und sogar zu publizieren).

Erstellen Sie zunächst manuell die benötigten kulturspezifischen Unterverzeichnisse im Ordner `/bin` des Pakets. 

Wenn das Paket aus einem bestimmten Grund auch im Package Manager publiziert werden muss, müssen Sie zunächst eine Version des Pakets ohne diese Kultur-Unterverzeichnisse publizieren. Publizieren Sie dann eine neue Version des Pakets mithilfe der DynamoUI-`publish package version`-Funktion. Die neue in Dynamo hochgeladene Version sollte Ihre Ordner und Dateien unter `/bin`, die Sie manuell mit dem Windows-Datei-Explorer hinzugefügt haben, nicht löschen. Der Prozess zum Hochladen von Paketen in Dynamo wird aktualisiert, um die Anforderungen für lokalisierte Dateien in Zukunft zu erfüllen.

Diese Kultur-Unterverzeichnisse werden ohne Probleme von der .NET Runtime geladen, wenn sie sich im selben Verzeichnis wie die Block-/Erweiterungsbinärdateien befinden.

Weitere Informationen zu Ressourcen-Assemblys und RESX-Dateien finden Sie unter: [https://docs.microsoft.com/de-de/dotnet/framework/resources/creating-resource-files-for-desktop-apps](https://docs.microsoft.com/de-de/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

Sie werden wahrscheinlich die `.resx`-Dateien in Visual Studio erstellen und kompilieren. Für eine bestimmte Assembly-`xyz.dll` werden die resultierenden Ressourcen in eine neue Assembly-`xyz.resources.dll` kompiliert. Wie oben beschrieben, sind der Speicherort und der Name dieser Assembly wichtig.

Die generierte `xyz.resources.dll` sollte sich an folgendem Speicherort befinden: `package\bin\culture\xyz.resources.dll`.

Um auf die lokalisierten Zeichenfolgen in Ihrem Paket zuzugreifen, können Sie den ResourceManager verwenden. Noch einfacher ist es jedoch, auf `Properties.Resources.YourLocalizedResourceName` innerhalb der Assembly zu verweisen, für die Sie eine `.resx`-Datei hinzugefügt haben. Beispiele finden Sie unter 

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457) \- Beispiel für eine lokalisierte Fehlermeldung

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19) \- Beispiel für eine lokalisierte Dynamo-spezifische NodeDescription-Attributzeichenfolge

[https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs) \- weiteres Beispiel

### Layout der Blockbibliothek

Wenn Dynamo Blöcke aus einem Paket lädt, werden diese normalerweise im Abschnitt `Addons` der Blockbibliothek platziert. Um integrierte Paketblöcke besser in andere integrierte Inhalte zu integrieren, haben wir die Möglichkeit hinzugefügt, dass Autoren von integrierten Paketen eine partielle `layout specification`-Datei bereitstellen können, um die Platzierung der neuen Blöcke in der richtigen Kategorie der obersten Ebene im Abschnitt `default` der Bibliothek zu unterstützen.

Beispiel: Wenn die folgende JSON-Layout-Spezifikationsdatei im Pfad `package/extra/layoutspecs.json` gefunden wird, werden die von `path` angegebenen Blöcke in der Kategorie `Revit` im Abschnitt `default` platziert, welcher der Hauptabschnitt für integrierte Blöcke ist.

Beachten Sie, dass Blöcke, die aus einem integrierten Paket importiert wurden, das Präfix `bltinpkg://` aufweisen, wenn sie mit einem Pfad abgeglichen werden sollen, der in der Layout-Spezifikation enthalten ist.

```json
{
  "sections": [
    {
      "text": "default",
      "iconUrl": "",
      "elementType": "section",
      "showHeader": false,
      "include": [ ],
      "childElements": [
        {
          "text": "Revit",
          "iconUrl": "",
          "elementType": "category",
          "include": [],
          "childElements": [
            {
              "text": "some sub group name",
              "iconUrl": "",
              "elementType": "group",
              "include": [
                {
                  "path": "bltinpkg://namespace.namespace",
                  "inclusive": false
                }
              ],
              "childElements": []
            }
          ]
        }
      ]
    }
  ]
}
```

Komplexe Layoutänderungen wurden nicht ausreichend getestet und werden nicht umfassend unterstützt. Durch das Laden dieser speziellen Layout-Spezifikation wird beabsichtigt, einen ganzen Paket-Namensbereich unter einer bestimmten Host-Kategorie wie `Revit` oder `Formit` zu verschieben. 