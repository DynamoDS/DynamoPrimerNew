# Beispiele

Wenn Sie nach Beispielen für die Entwicklung für Dynamo suchen, sehen Sie sich die folgenden Ressourcen an:

#### Beispiel-Repositorys <a href="#sample-repositories" id="sample-repositories"></a>

Diese Beispiele sind Visual Studio-Vorlagen, mit denen Sie Ihr eigenes Projekt starten können:

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**:** Vorlage für ZeroTouch-Basisblöcke
  * Zurückgeben mehrerer Ausgaben: [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Verwenden eines nativen Geometrieobjekts aus Dynamo: [Code](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * Beispieleigenschaft (Abfrageblock): [Code](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** Vorlagen für NodeModel-Basisblöcke und Ansichtsanpassung
  * NodeModel-Basisvorlage: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * Definieren von Blockattributen (Eingabe-/Ausgabenamen, Beschreibungen, Typen): [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * Zurückgeben von Null-Blöcken, wenn keine Eingaben vorhanden sind: [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * Erstellen eines Funktionsaufrufs: [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * Basisvorlage zur NodeModel-Ansichtsanpassung: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * Benachrichtigen der Benutzeroberfläche, dass ein Element aktualisiert werden muss: [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * Anpassen von NodeModel: [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * Definieren von Schiebereglerattributen: [Code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * Bestimmen der Interaktionslogik für den Schieberegler: [Code](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**:** Vorlagen für ZeroTouch, angepasste Benutzeroberfläche, Tests und Ansichtserweiterungen
  * [Beispiele für die Benutzeroberfläche](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * Erstellen eines angepassten Benutzeroberflächen-Basisblocks: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * Erstellen eines Dropdown-Menüs: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [Tests](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * Systemtests: [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * ZeroTouch-Tests: [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [ZeroTouch-Beispiele](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * Zero-Touch-Beispielblöcke, einschließlich eines Blocks, der `IGraphicItem` implementiert, um das Geometrie-Rendering zu beeinflussen: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * Zero-Touch-Beispielblöcke zum Einfärben von Geometrie mit `IRenderPackage`: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [Beispiele für Ansichtserweiterungen](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): Eine IViewExtension-Implementierung, die ein modusunabhängiges Fenster anzeigt, wenn auf MenuItem geklickt wird
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** Vorlagen für die erweiterte Dynamo-Paketentwicklung mit NodeModel
  * Beispiele für Grundfunktionen:
    * [Fehler](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiplizieren](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Zeitüberschreitung](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * Geometrie-Beispiele:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * Beispiele für die Benutzeroberfläche:
    * [Schaltfläche](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Schieberegler](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [Status](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynamoText**](https://github.com/DynamoDS/DynamoText)**:** Eine Zero-Touch-Bibliothek zum Erstellen von Text in Dynamo.

#### Fallbeispiele <a href="#case-studies" id="case-studies"></a>

Drittentwickler haben wichtige und interessante Beiträge für die Plattform geleistet, von denen viele auch Open-Source-Beiträge sind. Die folgenden Projekte sind außergewöhnliche Beispiele für das, was mit Dynamo ausgeführt werden kann.

**Ladybug** ist eine Python-Bibliothek zum Laden, Analysieren und Ändern von EnergyPlus Weather-Dateien (epw).

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** ist eine Python-Bibliothek zum Erstellen, Ausführen und Visualisieren der Ergebnisse von Tageslichtanalysen (RADIANCE) und Energieanalysen (EnergyPlus/OpenStudio).

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee** ist ein Plugin für die Interoperabilität mit Excel und Dynamo (GPL).

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** ist eine Sammlung benutzerdefinierter Blöcke für Revit-bezogene Aktivitäten sowie für andere Zwecke, wie Listenverwaltung, mathematische Operationen, Zeichenfolgenoperationen, geometrische Operationen (hauptsächlich Begrenzungsrahmen, Netze, Ebenen, Punkte, Oberflächen, UVs und Vektoren) und die Unterteilung von Oberflächen.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
