# 範例

如果您要尋找如何開發 Dynamo 的範例，請查看以下資源：

#### 範例儲存庫 <a href="#sample-repositories" id="sample-repositories"></a>

這些範例是 Visual Studio 樣板，您可以用來開始您自己的專案：

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**：**基本 ZeroTouch 節點的樣板。
  * 傳回多個輸出：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * 使用 Dynamo 的原生幾何圖形物件：[程式碼](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * 範例性質 (查詢節點)：[程式碼](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**：**基本 NodeModel 節點和視圖自訂的樣板。
  * 基本 NodeModel 樣板：[HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * 定義節點屬性 (輸入/輸出名稱、描述、類型)：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * 如果沒有輸入，則傳回空節點：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * 建立函數呼叫：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * 基本 NodeModel 視圖自訂樣板：[HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs)、[HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs)、[Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml)、[Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * 警示使用者介面：某個元素需要更新：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * 自訂 NodeModel：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * 定義滑棒屬性：[程式碼](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * 決定滑棒的互動邏輯：[程式碼](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**：**ZeroTouch、自訂使用者介面、測試和視圖延伸的樣板。
  * [使用者介面範例](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * 建立基本的自訂使用者介面節點：[CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * 建立下拉式功能表：[DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [測試](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * 系統測試：[HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * ZeroTouch 測試：[HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [ZeroTouch 範例](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples)：
    * 範例 ZeroTouch 節點，包括實作 `IGraphicItem` 影響幾何圖形彩現的節點：[BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * 使用 `IRenderPackage` 為幾何圖形著色的範例 ZeroTouch 節點：[ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [視圖延伸範例](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension)：實作 IViewExtension，在按一下無模式視窗的 MenuItem 時，會顯示視窗。
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**：**使用 NodeModel 進行進階 Dynamo 套件開發的樣板。
  * 基本範例：
    * [Error](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiply](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * 幾何圖形範例：
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * 使用者介面範例：
    * [Button](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [滑棒](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [State](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynaText**](https://github.com/DynamoDS/DynamoText)**：**用於在 Dynamo 中建立文字的 ZeroTouch 資源庫。

#### 案例研究 <a href="#case-studies" id="case-studies"></a>

第三方開發人員為平台做出了巨大且振奮人心的貢獻，其中許多也是開放原始碼。下列專案是使用 Dynamo 可以執行的特殊範例。

**Ladybug** 是一個載入、分析和修改 EneregyPlus Weather 檔案 (epw) 的 Python 資源庫。

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** 是一個建立、執行和視覺化日照 (RADIANCE) 和能源分析 (EnergyPlus/OpenStudio) 結果的 Python 資源庫。

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee** 是一個讓 Excel 和 Dynamo 具有互通性 (GPL) 的外掛程式。

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork**是一個 Revit 相關動作以及諸如以下各種其他用途的自訂節點集合，例如清單管理、數學運算、字串運算、幾何運算 (主要是邊界框、網格、平面、點、曲面、UV 和向量) 和面板化。

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
