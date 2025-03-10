# Przykłady

Jeśli szukasz przykładów ułatwiających programowanie rozwiązań dla dodatku Dynamo, zapoznaj się z poniższymi zasobami:

#### Repozytoria przykładowe <a href="#sample-repositories" id="sample-repositories"></a>

Te przykładowe szablony programu Visual Studio umożliwiają rozpoczęcie własnego projektu:

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**:** szablon podstawowych węzłów ZeroTouch.
  * Zwracanie wielu pozycji danych wyjściowych: [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Używanie natywnego obiektu geometrii z dodatku Dynamo: [kod](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * Przykładowa właściwość (węzeł zapytania): [kod](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** szablony podstawowych węzłów NodeModel i dostosowywanie widoku.
  * Podstawowy szablon NodeModel: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * Definiowanie atrybutów węzłów (nazw danych wejściowych/wyjściowych, opisów, typów): [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * Zwracanie węzła null, jeśli nie ma danych wejściowych: [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * Tworzenie wywołania funkcji: [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * Podstawowy szablon dostosowywania widoku NodeModel: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * Wysyłanie do interfejsu użytkownika alertów o tyn, że element wymaga aktualizacji: [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * Dostosowywanie klasy NodeModel: [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * Definiowanie atrybutów suwaka: [kod](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * Określanie logiki interakcji dla suwaka: [kod](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**:** szablony dla rozwiązań ZeroTouch, niestandardowy interfejs użytkownika, testy i rozszerzenia widoku.
  * [Przykłady interfejsu użytkownika](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * Tworzenie podstawowego niestandardowego węzła interfejsu użytkownika: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * Tworzenie menu rozwijanego: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [Testy](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * Testy systemu: [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * Testy ZeroTouch: [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [Przykłady ZeroTouch](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * Przykładowe węzły ZeroTouch, w tym węzły, w których zaimplementowano interfejs `IGraphicItem` w celu wpływania na renderowanie geometrii: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * Przykładowe węzły ZeroTouch do kolorowania geometrii z użyciem interfejsu `IRenderPackage`: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [Przykładowe rozszerzenia widoku](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): implementacja interfejsu IViewExtension powodująca wyświetlenie okna niemodalnego po kliknięciu jej elementu MenuItem.
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** szablony do zaawansowanego opracowywania pakietów dodatku Dynamo za pomocą klasy NodeModel.
  * Przykłady podstawowe:
    * [Error](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiply](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * Przykłady geometrii:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * Przykłady interfejsu użytkownika:
    * [Button](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Slider](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [State](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynaText**](https://github.com/DynamoDS/DynamoText)**:** biblioteka ZeroTouch umożliwiająca tworzenie tekstu w dodatku Dynamo.

#### Analizy przypadków <a href="#case-studies" id="case-studies"></a>

Programiści zewnętrzni znacząco i bardzo pozytywnie przyczynili się do rozwoju tej platformy, a wiele z ich prac jest dostępnych na licencji open source. Poniższe projekty to wyjątkowe przykłady możliwości pracy z dodatkiem Dynamo.

**Ladybug** to biblioteka języka Python umożliwiająca wczytywanie, analizowanie i modyfikowanie plików meteorologicznych EnergyPlus (epw).

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** to biblioteka języka Python umożliwiająca tworzenie, uruchamianie i wizualizowanie wyników analizy światła dziennego (RADIANCE) i analizy energetycznej (EnergyPlus/OpenStudio).

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee** to wtyczka umożliwiająca współdziałanie programów Excel i Dynamo (GPL).

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** to kolekcja węzłów niestandardowych do obsługi czynności związanych z programem Revit oraz do innych celów, takich jak zarządzanie listami, operacje matematyczne, operacje na ciągach, operacje geometryczne (dotyczące głównie ramek ograniczających, siatek, płaszczyzn, punktów, powierzchni, UV i wektorów) oraz panelowanie.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
