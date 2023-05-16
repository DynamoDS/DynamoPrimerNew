# Příklady

Pokud hledáte příklady, jak vyvíjet pro aplikaci Dynamo, podívejte se na následující zdroje:

#### Vzorová úložiště <a href="#sample-repositories" id="sample-repositories"></a>

Tyto vzory jsou šablony aplikace Visual Studio, které můžete použít k zahájení vlastního projektu:

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**:** Šablona pro základní uzly ZeroTouch.
  * Vrácení více výstupů: [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Použití nativního objektu geometrie z aplikace Dynamo: [Kód](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89).
  * Příklad vlastnosti (uzel dotazu): [Kód](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** Šablony pro základní uzly NodeModel a přizpůsobení pohledu.
  * Základní šablona NodeModel: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * Definování atributů uzlů (názvy vstupů/výstupů, popisy, typy): [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * Vrácení uzlu null, pokud neexistují žádné vstupy: [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * Vytvoření volání funkce: [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * Základní šablona přizpůsobení pohledu NodeModel: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * Upozornění pro uživatelské rozhraní, že je třeba aktualizovat prvek: [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * Přizpůsobení uzlu NodeModel: [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * Definování atributů posuvníku: [Kód](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * Určení logiky interakce pro posuvník: [Kód](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**:** Šablony pro ZeroTouch, vlastní uživatelské rozhraní, testy a rozšíření pohledů.
  * [Vzory uživatelského rozhraní](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * Vytvoření základního vlastního uzlu uživatelského rozhraní: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * Vytvoření rozevírací nabídky: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [Testy](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * Systémové testy: [HelloDynamoSystemTesting.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * Testy ZeroTouch: [HelloDynamoZeroTouchTesting.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [Příklady uzlů ZeroTouch](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * Příklad uzlů ZeroTouch včetně uzlu implementujícího `IGraphicItem` k ovlivnění rendrování geometrie: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * Příklad uzlů ZeroTouch pro vybarvení geometrie pomocí `IRenderPackage`: [ColourExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [Příklady rozšíření pohledu](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): Implementace třídy IViewExtension, která po kliknutí na příslušnou položku MenuItem zobrazí nemodální okno.
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** Šablony pro pokročilý vývoj balíčku aplikace Dynamo pomocí uzlu NodeModel.
  * Základní vzory:
    * [Error](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiply](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * Ukázky geometrie:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * Vzory uživatelského rozhraní
    * [Button](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Slider](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [State](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynamoText**](https://github.com/DynamoDS/DynamoText)**:** Knihovna ZeroTouch pro vytváření textu v aplikaci Dynamo.

#### Případové studie <a href="#case-studies" id="case-studies"></a>

Vývojáři třetích stran obohatili platformu významnými a zajímavými příspěvky, z nichž mnohé jsou také open source. Následující projekty představují výjimečné příklady toho, co lze s aplikací Dynamo provádět.

**Ladybug** je knihovna jazyka Python, která slouží k načítání, analýze a úpravám souborů EnergyPlus Weather (epw).

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** je knihovna jazyka Python, která slouží k vytváření, spouštění a vizualizaci výsledků denního světla (RADIANCE) a energetické analýzy (EnergyPlus/OpenStudio).

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee** je modul plug-in pro interoperabilitu aplikací Excel a Dynamo (GPL).

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** je kolekce vlastních uzlů pro aktivity související s aplikací Revit a také pro další účely, jako je správa seznamů, matematické operace, řetězcové operace, geometrické operace (hlavně hraniční obdélníky, sítě, roviny, body, povrchy, UV a vektory) a obložení.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
