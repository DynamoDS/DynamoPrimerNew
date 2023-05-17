# Примеры

Примеры разработки для Dynamo можно найти в следующих ресурсах.

#### Репозиторий примеров <a href="#sample-repositories" id="sample-repositories"></a>

Следующие примеры представляют собой шаблоны Visual Studio, которые можно использовать для создания собственного проекта.

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**:** шаблон для базовых узлов ZeroTouch.
  * Возвращение нескольких выходных данных: [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Использование исходного геометрического объекта из Dynamo: [код](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * Пример свойства (узел запроса): [код](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** шаблоны для базовых узлов NodeModel и персонализации вида.
  * Базовый шаблон NodeModel: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * Определение атрибутов узла (имена, описание и типы входных и выходных данных): [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * Возвращение пустого узла, если нет входных данных: [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * Создание вызова функции: [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * Шаблон персонализации базового вида NodeModel: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * Предупреждение о необходимости обновления элемента в пользовательском интерфейсе: [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * Персонализация NodeModel: [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * Определение атрибутов ползунка: [код](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * Определение логики взаимодействия для ползунка: [код](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**: **шаблоны для ZeroTouch, персонализированного пользовательского интерфейса, тестов и расширений видов.
  * [Примеры API](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * Создание базового узла персонализированного пользовательского интерфейса: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs).
    * Создание раскрывающегося меню: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs).
  * [Тесты](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * Системные тесты: [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * Тесты ZeroTouch: [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [Примеры ZeroTouch](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * Пример узлов ZeroTouch, включая реализацию `IGraphicItem` с целью повлиять на визуализацию геометрии: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs).
    * Пример узлов ZeroTouch для раскрашивания геометрии с помощью `IRenderPackage`: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [Примеры расширений видов](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): реализация IViewExtension, которая отображает немодальное окно при нажатии на элемент MenuItem.
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** шаблоны для расширенной разработки пакетов Dynamo с помощью NodeModel.
  * Важные примеры:
    * [Ошибка](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Умножение](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * Примеры геометрии:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * Примеры пользовательского интерфейса:
    * [Кнопка](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Регулятор](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [Состояние](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynaText**](https://github.com/DynamoDS/DynamoText)**:** библиотека ZeroTouch для создания текста в Dynamo.

#### Примеры <a href="#case-studies" id="case-studies"></a>

Сторонние разработчики внесли значительный и ценный вклад в платформу, и часто они предоставляют свои решения с открытым исходным кодом. Следующие проекты являются отличными примерами того, что можно сделать с помощью Dynamo.

**Ladybug** — это библиотека Python для загрузки, анализа и изменения файлов EneregyPlus Weather (epw).

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** — это библиотека Python для создания, запуска и визуализации результатов расчета естественного освещения (RADIANCE) и энергопотребления (EnergyPlus/OpenStudio).

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee** — подключаемый модуль для совместимости Excel и Dynamo (GPL).

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** — это набор пользовательских узлов для операций, связанных с Revit, а также для других целей, например управления списками, выполнения математических и строковых операций, выполнения геометрических операций (преимущественно ограничивающие рамки, сетки, плоскости, точки, поверхности, UV и векторы) и разбивки на панели.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
