# 예제

Dynamo를 개발하는 방법에 대한 예제를 보려면 아래 리소스를 검토하십시오.

#### 샘플 리포지토리 <a href="#sample-repositories" id="sample-repositories"></a>

이러한 샘플은 자체 프로젝트를 시작하는 데 사용할 수 있는 Visual Studio 템플릿입니다.

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)**: **기본 ZeroTouch 노드에 대한 템플릿입니다.
  * 여러 출력 반환: [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Dynamo의 기본 형상 객체 사용: [코드](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * 예제 특성(조회 노드): [코드](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)**:** 기본 NodeModel 노드 및 뷰 사용자 지정을 위한 템플릿입니다.
  * 기본 NodeModel 템플릿: [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * 노드 속성 정의(입력/출력 이름, 설명, 유형): [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * 입력이 없는 경우 null 노드 반환: [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * 함수 호출 생성: [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * 기본 NodeModel 뷰 사용자 지정 템플릿: [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * 요소를 업데이트해야 함을 UI에 알림: [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * NodeModel 사용자 지정: [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * 슬라이더 속성 정의: [코드](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * 슬라이더에 대한 상호 작용 논리 확인: [코드](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)**:** ZeroTouch, 사용자 지정 UI, 테스트 및 뷰 확장을 위한 템플릿입니다.
  * [UI 샘플](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * 기본 사용자 지정 UI 노드 생성: [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * 드롭다운 메뉴 생성: [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [테스트](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * 시스템 테스트: [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * ZeroTouch 테스트: [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [ZeroTouch 예제](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples):
    * 형상 렌더링에 영향을 주는 `IGraphicItem`을 구현하는 노드를 포함한 ZeroTouch 노드의 예제: [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * `IRenderPackage`를 사용하여 형상에 색상을 지정하는 ZeroTouch 노드의 예제: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [뷰 확장 예제](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension): MenuItem을 클릭할 때 모델리스 창을 표시하는 IViewExtension 구현
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)**:** NodeModel을 사용한 고급 Dynamo 패키지 개발을 위한 템플릿입니다.
  * 필수 샘플:
    * [Error](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiply](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Timeout](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * 형상 샘플:
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * UI 샘플:
    * [Button](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Slider](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [State](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynaText**](https://github.com/DynamoDS/DynamoText)**:** Dynamo에서 텍스트를 생성하기 위한 ZeroTouch 라이브러리입니다.

#### 사례 연구 <a href="#case-studies" id="case-studies"></a>

타사 개발자들은 플랫폼에 중요하고 놀라운 기여를 해왔으며, 그 중 대부분은 오픈 소스이기도 합니다. 다음 프로젝트는 Dynamo를 사용하여 수행할 수 있는 작업을 보여주는 훌륭한 예입니다.

**Lyblue**는 EnergyPlus Weather 파일(epw)을 로드, 분석 및 수정하는 Python 라이브러리입니다.

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee**는 일광(RADIANCE) 및 에너지 해석(EnergyPlus/OpenStudio)의 결과를 생성, 실행 및 시각화하는 Python 라이브러리입니다.

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee**는 Excel 및 Dynamo 상호 운용성(GPL)을 위한 플러그인입니다.

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork**는 Revit 관련 작업뿐만 아니라 리스트 관리, 수학적 연산, 문자열 연산, 기하학적 연산(주로 경계 상자, 메쉬, 평면, 점, 표면, UV 및 벡터) 및 패널링과 같은 기타 목적을 위한 사용자 지정 노드의 모음입니다.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
