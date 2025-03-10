# Erstellen eines Pakets in Visual Studio

Wenn Sie Assemblys entwickeln, die als Paket für Dynamo publiziert werden sollen, kann das Projekt so konfiguriert werden, dass alle erforderlichen Objekte gruppiert und in einer paketkompatiblen Verzeichnisstruktur abgelegt werden. Dadurch kann das Projekt schnell als Paket getestet werden und die Benutzererfahrung simuliert werden.

#### So erstellen Sie Pakete direkt im Paketordner <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Es gibt zwei Methoden zum Erstellen eines Pakets in Visual Studio:

* Hinzufügen von Postbuildereignissen über das Dialogfeld Projekteinstellungen, die XCopy- oder Python-Skripts zum Kopieren der erforderlichen Dateien verwenden
* Verwenden des Build-Ziels AfterBuild in der `.csproj`-Datei, um Aufgaben zum Kopieren von Dateien und Verzeichnissen zu erstellen

AfterBuild ist die bevorzugte Methode für diese Operationstypen (und die in diesem Handbuch beschriebenen), da sie nicht auf dem Kopieren von Dateien beruht, die möglicherweise nicht auf dem Build-Computer verfügbar sind.

#### Kopieren von Paketdateien mit der AfterBuild-Methode <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Richten Sie die Verzeichnisstruktur im Repository so ein, dass die Quelldateien von den Paketdateien getrennt sind. Platzieren Sie das Visual Studio-Projekt und alle zugehörigen Dateien in einem neuen `src`-Ordner, und arbeiten Sie dabei mit der Fallstudie CustomNodeModel. Sie speichern alle vom Projekt generierten Pakete in diesem Ordner. Die Ordnerstruktur sollte nun wie folgt aussehen:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Verschieben von Projektdateien](images/fe-proj-directory.jpg)

> 1. Verschieben Sie die Projektdateien in den neuen `src`-Ordner.

Nachdem sich die Quelldateien in einem separaten Ordner befinden, fügen Sie der Datei `CustomNodeModel.csproj` in Visual Studio ein `AfterBuild`-Ziel hinzu. Dadurch sollten die erforderlichen Dateien in einen neuen Paketordner kopiert werden. Öffnen Sie die Datei `CustomNodeModel.csproj` in einem Texteditor (wir haben [Atom](https://atom.io) verwendet), und platzieren Sie das Build-Ziel vor dem schließenden `</Project>`-Tag. Dieses AfterBuild-Ziel kopiert alle DLL-, PBD-, XML- und CONFIG-Dateien in einen neuen bin-Ordner und erstellt einen dyf-Ordner sowie zusätzliche Ordner.

```
  <Target Name="AfterBuild">
    <ItemGroup>
      <Dlls Include="$(OutDir)*.dll" />
      <Pdbs Include="$(OutDir)*.pdb" />
      <Xmls Include="$(OutDir)*.xml" />
      <Configs Include="$(OutDir)*.config" />
    </ItemGroup>
    <Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Pdbs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Xmls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Configs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\dyf" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
  </Target>
```

![Platzieren des AfterBuild-Ziels](images/atom-afterbuild.jpg)

> Wir müssen sicherstellen, dass das Ziel der Datei `CustomNodeModel.csproj` hinzugefügt wurde (nicht einer anderen Projektdatei) und dass das Projekt keine vorhandenen Postbuild-Einstellungen aufweist.
>
> 1. Platzieren Sie das AfterBuild-Ziel vor dem `</Project>`-End-Tag.

Im Abschnitt `<ItemGroup>` sind eine Reihe von Variablen definiert, die bestimmte Dateitypen darstellen. Die Variable `Dll` stellt beispielsweise alle Dateien im Ausgabeverzeichnis mit der Erweiterung `.dll` dar.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

Die Aufgabe `Copy` besteht darin, alle `.dll`-Dateien in ein Verzeichnis zu kopieren, insbesondere den Paketordner, in dem die Erstellung erfolgt.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Dynamo-Pakete verfügen in der Regel über einen `dyf`- und einen `extra`-Ordner für benutzerdefinierte Dynamo-Blöcke und andere Objekte, z. B. Bilder. Um diese Ordner zu erstellen, müssen wir eine `MakeDir`-Aufgabe verwenden. Diese Aufgabe erstellt einen Ordner, wenn er noch nicht vorhanden ist. Sie können diesem Ordner manuell Dateien hinzufügen.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

Wenn Sie das Projekt erstellen, sollte der Projektordner jetzt einen `packages`-Ordner neben dem zuvor erstellten `src`-Ordner enthalten. Im `packages`-Verzeichnis befindet sich ein Ordner, der alle für das Paket erforderlichen Elemente enthält. Außerdem müssen wir die Datei `pkg.json` in den Paketordner kopieren, damit Dynamo erkennt, dass das Paket geladen werden soll.

![Kopieren von Dateien](images/fe-proj-directory-package.jpg)

> 1. Der neue Paketordner, den das AfterBuild-Ziel erstellt hat.
> 2. Der vorhandene src-Ordner mit dem Projekt.
> 3. Die Ordner `dyf` und `extra`, die aus dem AfterBuild-Ziel erstellt wurden.
> 4. Kopieren Sie die Datei `pkg.json` manuell.

Jetzt können Sie das Paket mithilfe des Paket-Managers von Dynamo publizieren oder es direkt in das Paketverzeichnis von Dynamo kopieren: `<user>\AppData\Roaming\Dynamo\1.3\packages`.
