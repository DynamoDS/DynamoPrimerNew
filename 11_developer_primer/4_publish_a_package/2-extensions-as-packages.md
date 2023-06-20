# Erweiterungen als Pakete 

### Erweiterungen als Pakete <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### Überblick <a href="#overview" id="overview"></a>

Dynamo-Erweiterungen können wie normale Dynamo-Blockbibliotheken im Package Manager bereitgestellt werden. Wenn ein installiertes Paket eine Ansichtserweiterung enthält, wird die Erweiterung zur Laufzeit geladen, wenn Dynamo geladen wird. Sie können in der Dynamo-Konsole überprüfen, ob die Erweiterung ordnungsgemäß geladen wurde.

### Paketstruktur <a href="#package-structure" id="package-structure"></a>

Die Struktur eines Erweiterungspakets ist dieselbe wie die eines normalen Pakets und enthält Folgendes:

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

Wenn Sie die Erweiterung bereits erstellt haben, verfügen Sie (mindestens) über eine .NET-Assembly und eine Manifestdatei. Die Assembly sollte eine Klasse enthalten, die `IViewExtension` oder `IExtension` implementiert. Die XML-Manifestdatei teilt Dynamo mit, welche Klasse instanziiert werden soll, um die Erweiterung zu starten. Damit der Package Manager die Erweiterung ordnungsgemäß findet, muss die Manifestdatei genau dem Assembly-Speicherort und dem -Namen entsprechen.

Platzieren Sie alle Assembly-Dateien im Ordner `bin` und die Manifestdatei im Ordner `extra`. In diesem Ordner können auch weitere Objekte platziert werden.

Beispiel für .XML-Manifestdatei:

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### Hochladen <a href="#uploading" id="uploading"></a>

Sobald Sie einen Ordner mit den oben aufgeführten Unterverzeichnissen erstellt haben, können Sie mit dem Übertragen (Hochladen) in den Package Manager beginnen. Beachten Sie, dass Sie derzeit keine Pakete aus Dynamo Sandbox publizieren können. Dies bedeutet, dass Sie Dynamo Revit verwenden müssen. Navigieren Sie in Dynamo Revit zu Pakete => Neues Paket publizieren. Dadurch wird der Benutzer aufgefordert, sich bei seinem Konto bei Autodesk Account anzumelden, mit dem er das Paket verknüpfen möchte.

An dieser Stelle sollten Sie sich im normalen Fenster zum Publizieren des Pakets befinden, in dem Sie alle erforderlichen Felder für Ihr Paket/Ihre Erweiterung ausfüllen. Es gibt einen **sehr wichtigen** zusätzlichen Schritt, bei dem Sie sicherstellen müssen, dass keine der Assembly-Dateien als Blockbibliothek markiert ist. Klicken Sie dazu mit der rechten Maustaste auf die importierten Dateien (den oben erstellten Paketordner). Ein Kontextmenü wird angezeigt, in dem Sie diese Option aktivieren (oder deaktivieren) können. Alle Erweiterungs-Assemblys sollten deaktiviert werden.

![Publizieren eines Pakets](images/ViewExtension_Search.png)

Vor dem öffentlichen Publizieren sollten Sie immer lokal publizieren, um sicherzustellen, dass alles wie erwartet funktioniert. Nachdem Sie sich dahingehend vergewissert haben, können Sie durch Auswahl von Publizieren die Publizierung starten.

### Abrufen <a href="#pulling" id="pulling"></a>

Um zu überprüfen, ob das Paket erfolgreich hochgeladen wurde, können Sie es anhand des im Publizierungsschritt angegebenen Namens und der Schlüsselwörter suchen. Beachten Sie zum Schluss noch, dass die Erweiterungen einen Neustart von Dynamo erfordern, bevor sie funktionieren. In der Regel erfordern diese Erweiterungen Parameter, die beim Starten von Dynamo angegeben werden.

![Suchen nach Paketen](images/ViewExtension_Search.jpg)
