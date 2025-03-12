# サンプル

Dynamo の開発事例をお探しの場合は、次のリソースを参照してください。

#### サンプル リポジトリ <a href="#sample-repositories" id="sample-repositories"></a>

これらのサンプルは、ユーザ独自のプロジェクトを開始するために使用できる Visual Studio テンプレートです。

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**:** 基本的な ZeroTouch ノードのテンプレートです。
  * 複数の出力を返す: [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Dynamo のネイティブ ジオメトリ オブジェクトを使用する: [コード](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * サンプル プロパティ(Query ノード): [コード](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** 基本的な NodeModel ノードとビュー カスタマイズのテンプレートです。
  * 基本 NodeModel テンプレート: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * ノード属性を定義する(入力/出力名、説明、タイプ): [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * 入力がない場合に null ノードを返す: [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * 関数呼び出しを作成: [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * 基本 NodeModel ビュー カスタマイズ テンプレート: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs)、[HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs)、[Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml)、[Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * 要素を更新する必要があることを UI に警告する: [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * NodeModel をカスタマイズする: [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * スライダ属性を定義する: [コード](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * スライダの相互作用ロジックを決定する: [コード](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**:** ZeroTouch、カスタム UI、テスト、ビュー拡張機能のテンプレートです。
  * [UI サンプル](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * 基本的なカスタム UI ノードを作成する: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * ドロップダウン メニューを作成する: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [テスト](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * システムのテスト: [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * ZeroTouch テスト: [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [ZeroTouch のサンプル](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * ジオメトリ レンダリングに影響を与えるノード `IGraphicItem` を実装するノードを含む、ZeroTouch ノードのサンプル: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * `IRenderPackage` を使用してジオメトリを色付けする場合の ZeroTouch ノードのサンプル: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [ビュー拡張機能のサンプル](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): MenuItem をクリックしたときに、モードレス ウィンドウを表示する IViewExtension の実装です。
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** NodeModel を使用した高度な Dynamo パッケージ開発用テンプレートです。
  * 基本的なサンプル:
    * [Error](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiply](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * ジオメトリ サンプル:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * UI サンプル:
    * [Button](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Slider](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [State](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynaText**](https://github.com/DynamoDS/DynamoText)**:** Dynamo でテキストを作成するための ZeroTouch ライブラリです。

#### ケース スタディ <a href="#case-studies" id="case-studies"></a>

サードパーティの開発者は、プラットフォームの改良に多大な貢献をしており、その多くはオープン ソースとしても提供されています。次のプロジェクトは、Dynamoで実行できる機能の非常に優れたサンプルです。

**Ladybug** は、EnegyPlus 天候ファイル(epw)をロード、解析、および修正するための Python ライブラリです。

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** は、昼光(RADIANCE)とエネルギー解析(EnergyPlus/OpenStudio)の結果を作成、実行、および視覚化する Python ライブラリです。

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee**: Excel と Dynamo の相互運用性(GPL)のためのプラグインです。

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** は、Revit に関連するアクティビティに加えて、リスト管理、算術演算、文字列操作、ジオメトリ操作(主に境界ボックス、メッシュ、平面、点、サーフェス、UV、ベクトル)、パネル作成などの目的のためのカスタム ノードのコレクションです。

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
