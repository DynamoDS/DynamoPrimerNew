# Dynamo 통합

Dynamo 시각적 프로그래밍 언어에 대한 통합 문서입니다.

이 안내서에서는 사용자가 시각적 프로그래밍을 사용하여 응용프로그램과 상호 작용할 수 있도록 응용프로그램에서 Dynamo를 호스트하는 방법을 다각도로 설명합니다.

목차:
* [이 소개](#dynamo-integration): 이 안내서에서 다루는 내용과 Dynamo란 무엇인지 개략적으로 설명합니다.
* [Dynamo 사용자 진입점](#dynamo-custom-entry-point): DynamoModel을 작성하는 방법과 시작 위치를 안내합니다.
* [요소 바인딩 및 추적](#-element-binding-and-trace): Dynamo의 추적 메커니즘을 사용하여 그래프의 노드를 호스트의 결과에 바인딩하는 방법을 설명합니다.
* [Dynamo Revit 선택 노드 ](#-dynamo-revit-selection-nodes): 사용자가 호스트에서 객체 또는 데이터를 선택하여 Dynamo 그래프에 입력으로 전달할 수 있는 노드를 구현하는 방법을 안내합니다. 
* [Dynamo 기본 제공 패키지 개요](#dynamo-built-in-packages-overview): Dynamo 표준 라이브러리란 무엇이며 기본 메커니즘을 사용하여 통합을 포함한 패키지를 제공하는 방법을 설명합니다.


##### 용어 설명:
이러한 문서에서 Dynamo 스크립트, 그래프, 프로그램이라는 용어는 모두 사용자가 Dynamo에서 작성하는 코드를 지칭합니다.

## Dynamo 사용자 진입점
#### Dynamo Revit 예시 

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

`DynamoModel`은 Dynamo를 호스팅하는 응용프로그램의 진입점으로, Dynamo 응용프로그램을 나타냅니다. 이 모델은 Dynamo 응용프로그램 및 DesignScript 가상 컴퓨터를 구성하는 다른 중요한 데이터 구조 및 객체에 대한 참조를 포함하는 최상위 루트 객체입니다. 

구성 객체는 구성 시 `DynamoModel`에 대한 공통 매개변수를 설정하는 데 사용됩니다. 

이 문서에서 제시하는 예시는 DynamoRevit 구현에서 가져온 것으로, 이 구현은 Revit이 `DynamoModel`을 애드인으로 호스팅하는 통합입니다. (Revit용 플러그인 아키텍처). 이 애드인이 로드되면 `DynamoModel`을 시작한 다음 `DynamoView` 및 `DynamoViewModel`과 함께 사용자에게 표시합니다. 

Dynamo는 c# .net 프로젝트이므로, 응용프로그램의 프로세스에서 사용하려면 .net 코드를 호스팅하고 실행할 수 있어야 합니다.

DynamoCore는 교차 플랫폼 컴퓨팅 엔진이자 코어 모델 모음으로, .net 또는 mono(향후 .net core)를 사용하여 빌드할 수 있습니다. 그러나 DynamoCoreWPF는 Dynamo의 Windows 전용 UI 구성요소를 포함하며 다른 플랫폼에서는 컴파일되지 않습니다.

### Dynamo 진입점을 사용자화하는 단계 

`DynamoModel`을 초기화하려면 통합 개발자가 호스트 코드의 임의 위치에서 다음 단계를 수행해야 합니다.

### 호스트에서 공유 Dynamo Dll 미리 로드  

현재 D4R의 리스트에는 Dynamo와 Revit 간의 라이브러리 버전 충돌을 방지하기 위해 `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.`만 포함되어 있습니다. 예: `AvalonEdit`에서 충돌이 발생하면 코드 블록의 기능이 완전히 중단될 수 있습니다. 이 문제는 Dynamo 1.1.x에서 발생하여 https://github.com/DynamoDS/Dynamo/issues/7130에서 보고되었으며 수동으로 재현할 수도 있습니다. 호스트 기능과 Dynamo 간에 라이브러리 충돌이 발생하면 통합 개발자는 첫 번째 단계로 공유 Dynamo Dll을 미리 로드하는 것이 좋습니다. 이 단계는 다른 플러그인이나 호스트 응용프로그램 자체가 호환되지 않는 버전을 공유 종속성으로 로드하지 않도록 방지하는 데 필요할 수 있습니다. 더 나은 해결 방법은 버전을 일치시켜 버전 충돌을 해결하거나 가능한 경우 호스트의 app.config에서 .net 바인딩 리디렉션을 사용하는 것입니다. 
 

### ASM 로드 

#### ASM 및 LibG란

ASM은 Dynamo의 기반으로 구축된 ADSK 형상 라이브러리입니다.

LibG는 ASM 형상 커널을 감싸는 .Net 사용자에게 친숙한 래퍼입니다. LibG는 해당 버전 관리 체계를 ASM과 공유합니다. 즉, ASM과 같은 주 버전 및 부 버전 번호를 사용하여 특정 ASM 버전에 해당하는 래퍼임을 나타냅니다. ASM 버전이 제공되면 해당 LibG 버전이 같아야 합니다. 대부분의 경우 LibG는 특정 주 버전의 모든 ASM 버전에서 작동해야 합니다. 예를 들어 LibG 223은 모든 ASM 223 버전을 로드할 수 있어야 합니다.


#### ASM을 로드하는 Dynamo Sandbox 

Dynamo Sandbox는 여러 ASM 버전에서 작업할 수 있도록 설계되었으며, 이를 위해 여러 LibG 버전이 번들로 제공되고 코어와 함께 제공됩니다. Dynamo Shape Manager에는 ASM과 함께 제공되는 Autodesk 제품을 검색하는 기능이 내장되어 있어 Dynamo는 이러한 제품에서 ASM을 로드할 수 있으며, 호스트 응용프로그램에 명시적으로 로드하지 않고도 형상 노드가 작동하도록 할 수 있습니다. 현재 제공되는 제품 리스트는 다음과 같습니다. 
```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```
Dynamo는 Windows 레지스트리를 검색하여 이 리스트에 있는 Autodesk 제품이 사용자 컴퓨터에 설치되어 있는지 확인합니다. 이러한 Autodesk 제품 중 일부가 설치되어 있으면 ASM 바이너리를 검색하여 버전을 가져온 후 Dynamo에서 해당하는 LibG 버전을 찾습니다.  

이 ASM 버전을 고려해 다음 ShapeManager API는 로드하려는 해당 LibG 프리로더 위치를 선택합니다. 정확히 일치하는 버전이 있으면 해당 버전이 사용되고 그렇지 않으면 아래와 같이 주 버전이 동일한 가장 최근 버전의 LibG가 로드됩니다.  

예: 최신 ASM 빌드 225.3.0이 포함된 Revit 개발 빌드와 Dynamo가 통합되었는데 LibG 225.3.0이 있으면 Dynamo는 이 버전을 사용합니다. LibG 225.3.0이 없으면 이 버전보다 낮은 가장 최신 주 버전(예: 225.0.0)을 사용합니다. 

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)` 

#### Dynamo, 통합 중 호스트에서 ASM 로드 

Revit은 ASM 제품 검색 리스트의 첫 번째 항목입니다. 즉, 기본적으로 `DynamoSandbox.exe`는 가장 먼저 Revit에서 ASM을 로드하려고 합니다. 그러나, 저희는 통합된 D4R 작업 세션이 현재 Revit 호스트에서 ASM을 로드하도록 하고 싶습니다. 예를 들어, 사용자 컴퓨터에 R2018과 R2020이 둘 다 설치되어 있는 경우 R2020에서 D4R을 시작하면 D4R은 R2018에서 ASM 223을 사용하는 대신 R2020에서 ASM 225를 사용해야 합니다. 통합 개발자는 지정된 버전을 로드하도록 다음과 유사한 호출을 구현해야 합니다. 

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

#### Dynamo, 사용자화된 경로에서 ASM 로드 

최근에 `DynamoSandbox.exe` 및 `DynamoCLI.exe`에 특정 ASM 버전을 로드하는 기능을 추가했습니다. 일반 레지스트리 검색 동작을 건너뛰려면 `�gp` 플래그를 사용하여 Dynamo가 특정 경로에서 ASM 로드하도록 할 수 있습니다. 

`DynamoSandbox.exe -gp �somePath/To/ASMDirectory/� `

  



### StartConfiguration 작성 

StartupConfiguration은 DynamoModel을 초기화하기 위한 매개변수로 전달하는 데 사용되며, 이는 Dynamo 세션 설정을 사용자화하려는 방법에 대한 거의 모든 정의가 포함되어 있음을 나타냅니다. 다음 특성이 설정되는 방식에 따라 Dynamo 통합 방식이 통합 개발자마다 다를 수 있습니다. 예를 들어, 각 통합 개발자는 표시되는 숫자 형식 또는 Python 템플릿 경로를 다르게 설정할 수 있습니다. 

구성요소는 다음과 같습니다. 

* DynamoCorePath // 로드 중인 DynamoCore 바이너리의 위치

* DynamoHostPath // Dynamo 통합 바이너리의 위치

* GeometryFactoryPath // 로드된 LibG 바이너리의 위치

* PathResolver //다양한 파일을 확인하는 데 도움이 되는 객체

* PreloadLibraryPaths // 미리 로드된 노드 바이너리(예 : DSOffice.dll)의 위치 

* AdditionalNodeDirectories // 추가 노드 바이너리의 위치
 
* AdditionalResolutionPaths // 라이브러리 로드 중 필요할 수 있는 다른 종속성에 대한 추가 조립품 확인 경로 

* UserDataRootFolder // 사용자 데이터 폴더, 예: `"AppData\Roaming\Dynamo\Dynamo Revit"` 

* CommonDataRootFolder // 사용자 정의, 샘플 등을 저장하기 위한 기본 폴더

* Context // 통합 개발자 호스트 이름 + 버전 `(Revit<BuildNum>)`

* SchedulerThread // `ISchedulerThread`를 구현하는 통합 개발자 스케줄러 스레드로, 대부분의 통합 개발자에게는 기본 UI 스레드이거나 API에 액세스할 수 있는 모든 스레드입니다.

* StartInTestMode // 현재 세션이 테스트 자동화 세션인지 여부(많은 Dynamo 동작을 수정하므로 테스트를 작성하지 않는 한 사용하지 않음)

* AuthProvider // 통합 개발자의 IAuthProvider 구현(예: RevitOxygenProvider 구현은 Greg.dll에 있으며 packageManager 업로드 통합에 사용됨) 

### 기본 설정 

기본 설정 경로는 `PathManager.PreferenceFilePath`(예: `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`)에서 관리됩니다. 통합 개발자는 사용자화된 기본 설정 파일도 해당 위치에 제공할지를 결정할 수 있으며, 이 위치는 경로 관리자와 일치해야 합니다. 직렬화된 기본 설정 속성은 다음과 같습니다. 

* IsFirstRun // 이 Dynamo 버전을 처음 실행하는지를 나타냄(예: GA 동의 여부 메시지를 표시해야 할지 결정하는 데 사용됨). 또한 사용자가 일관된 환경을 사용할 수 있도록 새 Dynamo 버전을 시작할 때 기존 Dynamo 기본 설정을 마이그레이션해야 하는지를 결정하는 데 사용됩니다 

* IsUsageReportingApproved // 사용량 보고가 승인되었는지를 나타냄 

* IsAnalyticsReportingApproved // 분석 보고가 승인되었는지를 나타냄 

* LibraryWidth // Dynamo 왼쪽 라이브러리 패널의 폭 

* ConsoleHeight // 콘솔 디스플레이의 높이 

* ShowPreviewBubbles // 미리보기 풍선을 표시해야 하는지를 나타냄 

* ShowConnector // 커넥터의 표시 여부를 나타냄 

* ConnectorType //커넥터 유형(베지어 또는 폴리선)을 나타냄 

* BackgroundPreviews // 지정된 배경 미리보기의 활성 상태를 나타냄 

* RenderPrecision // 렌더링 정밀도 수준(낮을수록 더 적은 수의 삼각형을 사용하여 메쉬가 생성됨) 삼각형 수가 많을수록 배경 미리보기에서 더 부드러운 형상이 생성됩니다. 128은 형상을 빠르게 미리 보는 데 적합한 값입니다.

* ShowEdges // 표면 및 솔리드 모서리를 렌더링할지를 나타냄 

* ShowDetailedLayout // 사용되지 않음

* WindowX, WindowY // Dynamo 창의 마지막 X, Y 좌표 

* WindowW, WindowH // Dynamo 창의 마지막 폭, 높이 

* UseHardwareAcceleration // 지원되는 경우 Dynamo에서 하드웨어 가속을 사용해야 함 

* NumberFormat // 미리보기 풍선 toString()에 숫자를 표시하는 데 사용되는 소수점 자릿수 

* MaxNumRecentFiles // 저장할 최근 파일 경로의 최대 수 

* RecentFiles // 최근에 연 파일 경로 리스트. 이 리스트를 수정하면 Dynamo 시작 페이지의 최근 파일 리스트에 직접 영향을 줍니다. 

* BackupFiles // 백업 파일 경로 리스트 

* CustomPackageFolders // 패키지 및 사용자 노드가 있는지 스캔할 Zero-Touch 바이너리 및 디렉터리 경로가 포함된 폴더 리스트

* PackageDirectoriesToUninstall // Package Manager가 삭제 표시할 패키지를 결정하는 데 사용되는 패키지 리스트. 이러한 경로는 가능한 경우 Dynamo 시작 중에 삭제됩니다.

* PythonTemplateFilePath // 새 PythonScript 노드를 작성할 때 시작 템플릿으로 사용할 Python(.py) 파일의 경로. 이 경로는 통합을 위한 사용자 Python 템플릿 설정하는 데 사용할 수 있습니다.

* BackupInterval // 그래프가 자동으로 저장되는 기간(밀리초)을 나타냄 

* BackupFilesCount // 만들 백업 수를 나타냄 

* PackageDownloadTouAccepted // 사용자가 패키지 관리자에서 패키지를 다운로드하기 위한 이용 약관에 동의했는지를 나타냄 

* OpenFileInManualExecutionMode // OpenFileDialog의 "수동 모드에서 열기" 확인란의 기본 상태를 나타냄 

* NamespacesToExcludeFromLibrary // Dynamo 노드 라이브러리에 표시하 수 없는 네임스페이스(있는 경우)를 나타냄 문자열 형식: "[라이브러리 이름]:[정규화된 네임스페이스]" 

직렬화된 기본 설정의 예는 다음과 같습니다. 

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
 

* Extensions // IExtension을 구현하는 확장 리스트. null인 경우 Dynamo가 기본 경로(Dynamo 폴더 아래 `extensions` 폴더)에서 확장을 로드합니다. 

* IsHeadless // Dynamo가 UI 없이 실행되는지를 나타냄. 분석에 영향을 줍니다. 

* UpdateManager // 통합 개발자의 UpdateManager 구현, 위의 설명 참조 

* ProcessMode // TaskProcessMode와 동일하며 테스트 모드인 경우 동기, 그렇지 않으면 비동기임. 이 속성은 스케줄러의 동작을 제어합니다. 단일 스레드 환경에서는 이 속성을 동기로 설정할 수도 있습니다.

대상 StartConfiguration을 사용하여 `DynamoModel` 시작

StartConfig가 전달되어 `DynamoModel`이 실행되면 DynamoCore는 실제 세부 사항을 관리하여 Dynamo 세션이 지정된 상세 정보로 올바르게 초기화되었는지 확인합니다. `DynamoModel`이 초기화된 후에는 개별 통합 개발자가 몇 가지 후속 설정 단계를 수행해야 합니다. 예를 들어 D4R에서 Revit 호스트 트랜잭션 또는 문서 업데이트, Python 노드 사용자 지정 등을 확인하기 위해 이벤트를 구독해야 합니다. 

 ### '시각적 프로그래밍' 살펴보기

`DynamoViewModel` 및 `DynamoView`를 초기화하려면 먼저 `DynamoViewModel`을 생성해야 합니다. 이 작업은 `DynamoViewModel.Start` 정적 메서드를 사용하여 수행할 수 있습니다. 아래 내용을 참조하십시오.

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

`DynamoViewModel.StartConfiguration`에는 모델 구성보다 훨씬 적은 수의 옵션이 있습니다. 이들은 대부분 따로 설명이 필요 없습니다. 테스트 사례를 작성하지 않는 한 `CommandFilePath`는 무시할 수 있습니다.


`Watch3DViewModel` 매개변수는 배경 미리보기 및 watch3d 노드에서 3D 형상을 표시하는 방법을 제어합니다. 필요한 인터페이스를 구현하는 경우 자체 구현을 사용할 수 있습니다.

`DynamoView`를 구성하려면 `DynamoViewModel`만 있으면 됩니다. 이 뷰는 창 컨트롤이며 WPF를 사용하여 표시할 수 있습니다.

 ### DynamoSandbox.exe의 예:

 DynamoSandbox.exe는 DynamoCore를 테스트하고, 사용하고, 실험하기 위한 개발 환경입니다. `DynamoCore` 및 `DynamoCoreWPF` 구성요소가 로드되고 설정되는 방식을 확인할 수 있는 좋은 예입니다. [여기](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37)에서 몇 가지 시작 지점을 살펴볼 수 있습니다.

 ## 요소 바인딩 및 추적

#### 개요

*추적*은 Dynamo Core의 메커니즘으로, 데이터를 .dyn(Dynamo 파일)으로 직렬화할 수 있습니다. 중요한 것은 이 데이터가 Dynamo 그래프 내에서 노드의 Callsite에 연결되어 있다는 점입니다.

디스크에서 Dynamo 그래프를 열면 여기에 저장된 추적 데이터가 그래프의 노드와 다시 연결됩니다.

#### 용어집:
* 추적 메커니즘:
    
  * Dynamo에서 요소 바인딩 구현
  * 추적 메커니즘을 사용하여 객체가 작성된 형상에 다시 바인딩되도록 할 수 있습니다
  * Callsite 및 추적 메커니즘은 노드 구현자가 다시 연결하는 데 사용할 수 있는 영구 GUID를 제공합니다

* Callsite

  * 실행 파일에는 여러 개의 Callsite가 포함되어 있습니다. 이러한 Callsite는 다음에서 디스패치해야 하는 여러 위치로 실행을 디스패치하는 데 사용됩니다.
    * C# 라이브러리
    * 기본 제공 메서드
    * DesignScript 함수
    * 사용자 노드(DS 함수)

* TraceSerializer
  * `ISerializable` 및 `[Serializable]` 표시 클래스를 추적으로 직렬화합니다.
  * 데이터의 직렬화 및 역직렬화를 추적으로 처리합니다.
  * TraceBinder는 역직렬화된 데이터를 런타임 형식에 바인딩하는 것을 제어합니다. (실제 클래스의 인스턴스 작성)

#### 추적은 어떤 형태일까요?
----

추적 데이터는 .dyn 파일의 Bindings라는 특성 내로 직렬화됩니다. 이는 callsites-ids -> 데이터의 배열입니다. Callsite는 designscript 가상 컴퓨터 내에서 노드가 호출되는 특정한 위치/인스턴스입니다. Dynamo 그래프의 노드는 여러 번 호출될 수 있으므로 단일 노드 인스턴스에 대해 Callsite가 여러 개 작성될 수 있습니다.

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

 직렬화된 base64 인코딩 데이터의 형식은 사용하지 *않는* 것이 좋습니다.


#### 추적을 통해 해결하려는 문제
----


함수 실행의 결과로 임의의 데이터를 저장하려는 데에는 여러 가지 이유가 있지만, 이 경우 추적은 사용자가 호스트 애플리케이션에서 요소를 작성하는 소프트웨어 프로그램을 빌드하고 반복할 때 자주 발생하는 특정 문제를 해결하기 위해 개발되었습니다.

문제는 `Element Binding`이라고 하는 특성이며, 문제에 관한 설명은 다음과 같습니다.

사용자가 Dynamo 그래프를 개발하고 실행할 때 호스트 응용프로그램 모델에서 새 요소를 생성할 수 있습니다. 이 예에서는 사용자에게 건축 모델에서 문을 100개 생성하는 소규모 프로그램이 있다고 가정해 보겠습니다. 이러한 문의 수와 위치는 프로그램으로 제어됩니다.

사용자가 프로그램을 처음 실행하면 이 프로그램은 문 100개를 생성합니다.

나중에 사용자가 프로그램에 대한 입력을 수정하고 다시 실행하면 프로그램이 *(요소 바인딩 없이)* 새 문 100개를 작성하는데 이전 문은 새 문과 함께 모델에 계속 존재합니다.

----

Dynamo는 라이브 프로그래밍 환경이며 그래프를 변경하면 새로운 실행이 트리거되는 `"Automatic"` 실행 모드가 특징이기 때문에 프로그램 실행 횟수가 많으면 모델이 빠르게 복잡해질 수 있습니다.


이는 일반적으로 사용자가 기대하는 상황이 아닙니다. 대신 요소 바인딩이 활성화된 상태에서는 그래프 실행의 이전 결과가 정리되고 삭제되거나 수정됩니다. 호스트 API의 유연성에 따라 *삭제 또는 수정*됩니다. 요소 바인딩을 사용하면 사용자가 Dynamo 프로그램을 두 번째, 세 번째 또는 50번째 실행한 이후에도 모델에는 문이 100개만 있습니다.

이를 위해서는 데이터를 .dyn 파일로 직렬화하는 기능 그 이상이 필요합니다. 또한 아래의 설명처럼 DynamoRevit에는 리바인딩 워크플로우를 지원하기 위해 추적 기능을 기반으로 구축된 메커니즘이 있습니다.

----

이 시점에서 Revit과 같은 호스트에 대한 요소 바인딩의 또 다른 중요한 사용 사례를 소개하겠습니다. 요소 바인딩을 사용하는 상태에서 작성된 요소는 기존 요소 ID를 유지하려고 하기 때문에(기존 요소 수정) 호스트 응용프로그램에서 이러한 요소를 바탕으로 빌드된 논리는 Dynamo 프로그램이 실행된 후에도 계속 존재합니다. 예는 다음과 같습니다.

건축 모델의 예로 돌아가 보겠습니다.

요소 바인딩을 사용하지 않는 상태에서 먼저 예제를 실행해 보겠습니다. 이번에는 몇 가지 건축 벽을 생성하는 프로그램이 있습니다.

 사용자가 프로그램을 실행하고 호스트 응용프로그램에서 벽을 생성합니다. 그런 다음 Dynamo 그래프를 종료하고 일반 Revit 도구를 사용하여 해당 벽에 몇 개의 창을 배치합니다. 창은 Revit 모델의 일부로 이러한 특정 벽에 바인딩됩니다.

사용자가 Dynamo 백업을 시작하고 그래프를 다시 실행합니다. 이제 마지막 예에서처럼 벽 세트가 두 개 있습니다. 첫 번째 세트에는 창이 추가되었지만 새로운 벽에는 추가되지 않았습니다.

요소 바인딩을 사용하는 상태였다면 Dynamo 없이 호스트 응용프로그램에서 수동으로 수행된 기존 작업을 유지할 수 있습니다. 예를 들어 사용자가 프로그램을 두 번째로 실행했을 때 바인딩을 사용하는 상태였다면 벽이 삭제되는 것이 아니라 수정되고 호스트 응용프로그램의 다운스트림 변경 사항이 유지되었을 것입니다. 모델에는 다양한 상태의 두 가지 벽 세트 대신 창이 있는 벽이 포함되었을 것입니다.

-----

![벽 작성](images/creates_walls.png)


#### 추적과 요소 바인딩 비교
----

추적은 Dynamo Core의 메커니즘으로, 위에서 설명한 대로 데이터에 대한 Callsite의 정적 변수를 사용하여 그래프에서 함수의 Callsite에 데이터를 매핑합니다.

또한 Zero Touch Dynamo 노드를 작성할 때 임의의 데이터를 .dyn 파일로 직렬화할 수 있습니다. 이는 잠재적으로 전송 가능한 Zero Touch 코드가 이제 Dynamo Core에 종속됨을 의미하므로 일반적으로는 권장되지 않습니다.

.dyn 파일의 직렬화된 데이터 형식을 사용하는 대신 [Serializable] 속성 및 인터페이스를 사용하십시오.

반면 ElementBinding은 추적 API를 기반으로 빌드되며 Dynamo 통합*(DynamoRevit, Dynamo4Civil 등)에서 구현됩니다.*


#### 추적 API
알아야 할 몇 가지 낮은 수준의 추적 API는 다음과 같습니다.

``` c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

아래 예에서는 이러한 추적 API가 사용된 것을 확인할 수 있습니다.

Dynamo가 기존 파일에서 로드했거나 생성 중인 추적 데이터와 상호 작용하기 위해 다음 내용을 살펴볼 수 있습니다.

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```
[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)


#### 노드에서 단순 추적의 예
----
추적을 직접 사용하는 Dynamo 노드의 예는 [DynamoSamples 리포지토리](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs)에 나와 있습니다.

클래스의 요약에는 추적이 무엇인지에 대한 요점이 잘 설명되어 있습니다.

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

이 예에서는 DynamoCore의 추적 API를 직접 사용하여 특정 노드가 실행될 때마다 일부 데이터를 저장합니다. 이 경우 사전은 Revit의 모델 데이터베이스처럼 호스트 응용프로그램 모델의 역할을 수행합니다.

대략적인 설정은 다음과 같습니다.

정적 util 클래스 `TraceExampleWrapper`를 Dynamo에 노드로 가져옵니다. 이 클래스에는 `TraceExampleItem`을 작성하는 단일 메서드 `ByString`이 포함되어 있습니다. 이들은 `description` 속성을 포함하는 일반적인 .net 객체입니다.

각 `TraceExampleItem`은 `TraceableId`로 표현되는 추적으로 직렬화되며, `SOAP` 포맷터로 직렬화할 수 있도록 `[Serializeable]`이 표시된 `IntId`를 포함하는 클래스입니다. [직렬화 가능 속성에 대한 자세한 내용은 여기](https://docs.microsoft.com/en-us/dotnet/api/system.serializableattribute?view=netframework-4.8)를 참조하십시오.

[여기](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8)에서 정의된 `ISerializable` 인터페이스도 구현해야 합니다.


``` c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

이 클래스는 추적에 저장하려는 각 `TraceExampleItem`에 대해 생성되며, 그래프가 저장될 때 직렬화되고 base64 인코딩되어 디스크에 저장됩니다. 이는 기존 요소 사전을 바탕으로 그래프가 다시 열릴 때도 나중에 바인딩이 재연결될 수 있도록 하기 위함입니다. 사전은 Revit 문서처럼 영구적이지 않으므로 이 예에서는 제대로 작동하지 않습니다.

마지막으로 방정식의 마지막 부분은 `TraceableObjectManager`로, `DynamoRevit`의 `ElementBinder`와 유사합니다. 이 부분은 호스트의 문서 모델에 있는 객체와 Dynamo 추적에 저장한 데이터 간의 관계를 관리합니다.

사용자가 `TraceExampleWrapper.ByString` 노드가 포함된 그래프를 처음 실행하면 새 ID를 사용하여 새 `TraceableId`가 작성되고 `TraceExampleItem`이 새 ID에 매핑된 사전에 저장됩니다. 그리고 `TraceableID`를 추적에 저장합니다.

다음에 그래프를 실행할 때 추적을 살펴보고 거기에 저장한 ID를 찾은 다음 해당 ID에 매핑된 객체를 찾아 객체를 반환합니다! 완전히 새로운 객체를 작성하는 대신 여기서는 기존 객체를 수정합니다.

단일 `TraceExampleItem`을 작성하는 그래프를 두 번 연속으로 실행하는 흐름은 다음과 같습니다.

![첫 번째 호출](images/Trace-first-call.png)

![두 번째 호출](images/Trace-second-call.png)

다음 예에서는 같은 개념을 더 실제적인 DynamoRevit 노드 사용 사례를 사용하여 보여줍니다.

#### 추적 다이어그램

![추적 단계](images/trace_diagram.png) ![추적 흐름](images/trace_alt_diagram.png)

#### 참고:
최신 버전의 Dynamo에서는 TLS(스레드 로컬 저장소) 사용이 정적 멤버 사용으로 대체되었습니다.

#### 요소 바인딩 구현 예시

-----
DynamoRevit용으로 구현될 때 요소 바인딩을 사용하는 노드가 어떤 모습인지 빠르게 살펴보겠습니다. 이는 위에서 주어진 벽 작성 예시에서 사용되는 노드 유형과 유사합니다.


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

위의 코드는 벽 요소의 샘플 생성자를 보여줍니다. Dynamo에서 이 생성자는 `Wall.byParams`처럼 노드에서 호출됩니다.

요소 바인딩과 관련이 있을 때 생성자 실행의 중요한 단계는 다음과 같습니다.

1. `elementBinder`를 사용하여 이전에 작성한 객체 중 지난 실행에서 이 Callsite에 바인딩된 객체가 있는지 확인합니다. `ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. 있으면 새 벽을 작성하는 대신 해당 벽을 수정합니다.

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. 없는 경우 새 벽을 작성합니다.
```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. 추적에서 방금 검색한 이전 요소를 삭제하고 새 요소를 추가합니다. 그래야 나중에 이 요소를 조회할 수 있습니다.
```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### 토론

#### 효율성
* 현재 직렬화된 각각의 추적 객체는 SOAP xml 형식을 사용하여 직렬화되는데, 이는 매우 장황하고 많은 정보를 복제합니다. 그런 다음 데이터가 base64로 두 번 인코딩되는데, 이는 직렬화 또는 역직렬화 측면에서 비효율적입니다. 이 문제는 나중에 이러한 객체를 바탕으로 내부 형식을 빌드하지 않으면 개선될 수 있습니다. 다시 정리하면, 미사용 상태의 직렬화된 데이터 형식을 사용하지 마십시오.

#### ElementBinding을 기본적으로 설정해야 합니까?
* 요소 바인딩이 적합하지 않은 사용 사례가 있습니다. 고급 Dynamo 사용자가 무작위 그룹화 요소를 생성하기 위해 여러 번 실행해야 하는 프로그램을 개발 중이라고 가정해 보겠습니다. 이 프로그램의 목적은 프로그램이 실행될 때마다 추가 요소를 작성하는 것입니다. 이 사용 사례는 요소 바인딩 작동을 방지하는 해결 방법을 사용해야 구현할 수 있습니다. 통합 레벨에서 elementBinding을 사용하지 않도록 설정할 수 있지만 elementBinding은 Dynamo의 핵심 기능으로 제공하는 것이 적합합니다. 이 기능을 노드 레벨, Callsite 레벨, 전체 Dynamo 세션 아니면 작업 영역 등으로 얼마나 세분화해야 하는지는 명확하지 않으며 상황에 따라 판단해야 합니다.

## Dynamo Revit 선택 노드란 

일반적으로 이러한 노드를 통해 사용자는 참조하려는 활성 Revit 문서의 하위 세트를 설명할 수 있습니다. 사용자가 (아래에서 설명하는) Revit 요소를 참조하는 방법에는 여러 가지가 있으며, 노드의 결과 출력은 Revit 요소 래퍼(DynamoRevit 래퍼)이거나 (Revit 형상에서 변환된) 몇몇 Dynamo 형상일 수 있습니다. 다른 호스트 통합 컨텍스트에서 이러한 출력 유형 간의 차이점을 고려하면 도움이 됩니다.

개략적으로 **이러한 노드는 요소 ID가 입력되면 해당 요소를 가리키거나 해당 요소를 나타내는 일부 형상을 가리키는 포인터를 반환하는 함수로 개념화하는 것이 좋습니다. ** 

DynamoRevit에는 `�Selection�` 노드가 여러 개 있으며 이러한 노드는 최소 두 개의 그룹으로 나눌 수 있습니다. 

![Revit 선택 노드](images/revitSelectionNodes.png)


 

1. 사용자 UI 선택: 

    이 카테고리에서 `DynamoRevit` 노드의 예는 `SelectModelElement`, `SelectElementFace`입니다.

    이러한 노드를 사용하면 사용자는 Revit UI 컨텍스트로 전환하고 요소 또는 요소 세트를 선택할 수 있습니다. 그러면 선택된 요소의 ID가 캡처되고 일부 변환 함수가 실행되어 래퍼가 작성되거나 형상이 요소에서 추출 및 변환됩니다. 실행되는 변환은 사용자가 선택하는 노드 유형에 따라 달라집니다. 

2. 문서 조회: 

    이 카테고리에서 노드의 예는 `AllElementsOfClass`, `AllElementsOfCategory`입니다.

    이러한 노드를 통해 사용자는 전체 문서에서 요소의 하위 세트를 조회할 수 있습니다. 이러한 노드는 일반적으로 기본 Revit 요소를 가리키는 래퍼를 반환합니다. 이러한 래퍼는 DynamoRevit 환경에 필수적이며, 요소 바인딩과 같은 고급 기능을 작동하게 하고 Dynamo 통합 개발자가 사용자에게 노드로 노출되는 호스트 API를 선택할 수 있게 합니다.


### Dynamo Revit 사용자 워크플로우: 

#### 샘플 사례 

1. 
    * 사용자가 `SelectModelElement`를 사용하여 Revit 벽을 선택합니다. Dynamo 벽 래퍼가 그래프로 반환됩니다(노드의 미리보기 풍선에 표시됨). 

    * 사용자가 Element.Geometry 노드를 배치하고 이 새 노드에 `SelectModelElement` 출력을 연결합니다. 마무리된 벽의 형상이 추출되고 LibG API를 사용하여 Dynamo 형상으로 변환됩니다. 

    * 사용자가 그래프를 자동 실행 모드로 전환합니다. 

    * 사용자가 Revit에서 원래 벽을 수정합니다. 

    * Revit 문서에서 이벤트가 발생하여 일부 요소가 업데이트되었다고 알리면 그래프가 자동으로 다시 실행됩니다. 선택 노드는 이 이벤트를 감지하고 선택한 요소의 ID가 수정되었는지 확인합니다. 

### DynamoCivil 사용자 워크플로우: 

D4C의 워크플로우는 위에서 Revit에 대해 설명한 내용과 매우 유사하며 D4C에서 일반적인 두 가지 선택 노드 세트는 다음과 같습니다.  

![Civil 3D 선택 노드](images/civilSelectionNodes.png)



### 문제: 

* `DynamoRevit`의 선택 노드가 구현하는 문서 수정 업데이터로 인해 무한 루프를 쉽게 빌드할 수 있습니다. 문서의 모든 요소를 감지하는 노드가 있고 이 노드의 다운스트림 어딘가에서 새 요소를 작성한다고 상상해 보십시오. 이 프로그램을 실행하면 루프가 트리거됩니다. `DynamoRevit`은 트랜잭션 ID를 사용하여 다양한 방법으로 이러한 경우를 포착하려고 시도하며 요소 생성자에 대한 입력이 변경되지 않은 경우 문서를 수정하지 않습니다.

    선택한 요소가 호스트 응용프로그램에서 수정될 때 그래프의 자동 실행이 시작되면 이러한 점을 고려해야 합니다! 

* `DynamoRevit`의 선택 노드는 WPF를 참조하는 `RevitUINodes.dll` 프로젝트에서 구현됩니다. 이는 문제가 되지 않을 수 있지만 대상 플랫폼에 따라 알아두는 것이 좋습니다. 
 

### 데이터 흐름 다이어그램

![선택 흐름](images/selectModelElement.png)

![선택 흐름2](images/selectElementFace.png)



### 기술 구현: (위 다이어그램 참조): 

선택 노드는 일반 `SelectionBase` 유형(`SelectionBase<TSelection, TResult> ` 및 최소 멤버 세트)에서 상속하여 구현됩니다. 

* `BuildOutputAST` 메서드 구현. 이 메서드는 노드가 실행되는 미래의 어느 시점에 실행될 AST를 반환해야 합니다. 선택 노드의 경우 요소 ID로부터 요소 또는 형상을 반환해야 합니다. https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280 

* `BuildOutputAST` 구현은 `NodeModel`/UI 노드를 구현하는 데 있어 가장 어려운 부분 중 하나입니다. C# 함수에 가능한 한 많은 논리를 포함하고 AST 함수 호출 노드를 AST에 포함하는 것이 가장 좋습니다. 여기에서 `node`는 Dynamo 그래프의 노드가 아니라 추상 구문 트리의 AST 노드입니다. 

![선택 흐름2](images/selectionAST.png)


* 직렬화 -  

  * 명시적인 `NodeModel` 파생 유형(ZeroTouch가 아님)이므로 .dyn 파일에서 노드를 역직렬화하는 동안 사용할 [JsonConstructor]도 구현해야 합니다. 

    호스트의 요소 참조를 .dyn 파일에 저장해야 사용자가 이 노드가 포함된 그래프를 열 때 사용자가 선택한 요소가 그대로 유지됩니다. Dynamo의 NodeModel 노드는 json.net을 사용하여 직렬화하며, 모든 공용 속성은 Json.net을 사용하여 자동으로 직렬화됩니다. [JsonIgnore] 특성을 사용하면 필요한 항목만 직렬화할 수 있습니다. 

* 문서 조회 노드는 요소 ID에 대한 참조를 저장할 필요가 없으므로 좀 더 단순합니다. `ElementQueryBase` 클래스 및 파생 클래스 구현은 아래 내용을 참조하십시오. 이러한 노드를 실행할 때 Revit API를 호출하고 기본 문서에서 요소를 조회한 다음 앞서 언급한 대로 형상 또는 Revit 요소 래퍼로 변환합니다. 

### 참조 문헌:

#### DynamoCore 기준 클래스: 

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs )

* [NodeModel 사례 연구 - 사용자 지정 UI](11\_developer\_primer/3\_developing\_for\_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Dynamo 2.x용 패키지 및 Dynamo 라이브러리 업데이트하기](11\_developer\_primer/3\_developing\_for\_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Dynamo 3.x용 패키지 및 Dynamo 라이브러리 업데이트하기](11\_developer\_primer/3\_developing\_for\_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)



#### DynamoRevit: 

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs )

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)


## Dynamo 내장 패키지 개요

내장 패키지 메커니즘은 `PackageLoader` 및 `PackageManager` 확장에 의해 구현된 Dynamo 패키지 로드 기능을 활용하여 코어 자체를 확장하지 않아도 더 많은 노드 컨텐츠를 Dynamo Core와 번들로 묶기 위한 노력의 일환입니다.

이 문서에서는 내장 패키지와 Dynamo 내장 패키지라는 용어는 같은 의미입니다.

### 패키지를 내장 패키지로 제공해야 합니까?
* 패키지에는 서명된 이진 진입점이 있어야 하며 그렇지 않으면 로드되지 않습니다.
* 이러한 패키지에서는 호환성이 손상되는 변경을 방지해야 합니다. 즉, 패키지 컨텐츠에 자동화된 테스트가 있어야 합니다.
* 의미론적 버전 관리. 의미론적 버전 관리 체계를 사용하여 패키지의 버전을 관리하여 패키지 설명 또는 문서에서 사용자에게 그 의미를 전달하는 좋은 방법입니다.
* 자동화된 테스트! 위의 내용을 참조하십시오. 패키지가 내장 패키지 메커니즘을 사용하여 포함되는 경우 사용자에게는 제품의 일부로 보이며 제품처럼 테스트해야 합니다.
* 높은 수준의 완성도: 아이콘, 노드 문서, 현지화된 컨텐츠
* 자신 또는 팀에서 유지 관리할 수 없는 패키지는 제공하지 마십시오.
* 타사 패키지는 내장 패키지 메커니즘으로 제공하지 마십시오(위 내용 참조).

기본적으로 패키지에 대한 전체 제어 권한이 있어야 하며 패키지를 수정하고 최신 상태로 유지하고 Dynamo 및 제품의 최신 변경 사항에 대해 테스트할 수 있는 기능이 있어야 합니다. 서명하는 기능도 필요합니다.


### 내장 패키지와 호스트 통합 관련 패키지 비교

저희는 `Built-In Packages`를 핵심 기능, 즉 Package Manager에 대한 액세스 권한이 없더라도 모든 사용자가 액세스할 수 있는 패키지 세트로 만들려고 합니다. 현재 이 기능을 지원하는 기본 메커니즘은 DynamoCore.dll을 기준으로 Dynamo Core 디렉토리에 바로 패키지를 위한 추가적인 기본 로드 위치를 두는 것입니다.

몇 가지 구속조건이 있기는 하지만 이 위치는 ADSK Dynamo 클라이언트 및 통합 개발자가 통합별 패키지를 배포하는 데 사용할 수 있습니다. *(예를 들어, Dynamo Formit 통합을 위해서는 사용자 Dynamo Formit 패키지가 필요합니다.)*

코어 및 호스트 전용 패키지에 대한 기본 로드 메커니즘이 동일하므로 이러한 방식으로 배포된 패키지를 보고 사용자가 코어 `Built-In Packages` 패키지와 단일 호스트 제품에서만 사용할 수 있는 통합 관련 패키지를 혼동하지 않도록 해야 합니다. 사용자 혼동을 피하기 위해 Dynamo 팀과 논의하여 호스트별 패키지를 도입하는 것이 좋습니다.


### 패키지 현지화

`Built-In Packages`에 포함된 패키지는 더 많은 고객에게 제공될 것이고 이러한 패키지에 대한 더 엄격한 보장이 적용될 것이므로(위 내용 참조) 패키지를 현지화해야 합니다.

`Built-In Packages`에 포함하려는 내부 ADSK 패키지의 경우 패키지를 Package Manager에 게시할 필요가 없으므로 현지화된 컨텐츠를 Package Manager에 게시할 수 없다는 현재 제한 사항은 문제가 되지 않습니다.

해결 방법을 따르면 패키지의 /bin 폴더에 있는 문화 하위 디렉터리를 사용하여 패키지를 수동으로 작성하고 게시할 수도 있습니다.

먼저 패키지의 `/bin` 폴더 아래에 필요한 문화별 하위 디렉터리를 수동으로 만듭니다. 

어떤 이유로든 패키지를 Package Manager에도 게시해야 하는 경우 먼저 이러한 문화 하위 디렉토리가 누락된 패키지 버전을 게시한 다음 DynamoUI `publish package version`을 사용하여 새 패키지 버전을 게시해야 합니다. Dynamo에서 새 버전을 업로드하면 Windows 파일 탐색기를 사용하여 `/bin` 아래에 수동으로 추가한 폴더 및 파일을 삭제해서는 안 됩니다. Dynamo의 패키지 업로드 프로세스는 향후 현지화된 파일에 대한 요구사항을 처리하도록 업데이트될 예정입니다.

이러한 문화 하위 디렉터리는 노드/확장 이진 파일과 동일한 디렉터리에 있는 경우 .net 런타임에서 문제 없이 로드됩니다.

리소스 어셈블리 및 .resx 파일에 대한 자세한 내용은 [https://docs.microsoft.com/ko-kr/dotnet/framework/resources/creating-resource-files-for-desktop-apps](https://docs.microsoft.com/en-us/dotnet/framework/resources/creating-resource-files-for-desktop-apps)를 참조하십시오.

Visual Studio를 사용하여 `.resx` 파일을 작성하고 컴파일할 수 있습니다. 지정된 조립품 `xyz.dll`의 경우 결과 리소스가 새 조립품 `xyz.resources.dll`로 컴파일되는데, 위에서 설명한 대로 이 조립품의 위치와 이름이 중요합니다.

생성된 `xyz.resources.dll`의 위치는 `package\bin\culture\xyz.resources.dll`이어야 합니다.

패키지의 현지화된 문자열에 액세스하기 위해 ResourceManager를 사용할 수 있지만 더 간단하게 `.resx` 파일을 추가한 조립품 내에서 `Properties.Resources.YourLocalizedResourceName`을 참조할 수 있어야 합니다. 예시는 다음 문서를 참조하십시오. 

현지화된 오류 메시지의 예시: [https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457)

현지화된 Dynamo 관련 NodeDescription 특성 문자열의 예시: [https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19)입니다.

다른 예시: [https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs).

### 노드 라이브러리 배치

일반적으로 Dynamo는 패키지에서 노드를 로드할 때 노드 라이브러리의 `Addons` 섹션에 노드를 배치합니다. 내장 패키지 노드를 다른 내장 컨텐츠와 효과적으로 통합하기 위해 내장 패키지 작성자가 부분 `layout specification` 파일을 제공하여 `default` 라이브러리 섹션의 올바른 최상위 카테고리에 새 노드를 배치할 수 있는 기능을 추가했습니다.

예를 들어 다음 배치 사양 json 파일이 경로 `package/extra/layoutspecs.json`에 있으면 이 파일은 `path`로 지정된 노드를 기본 내장 노드 섹션인 `default` 섹션의 `Revit` 카테고리에 배치합니다.

레이아웃 사양에 포함된 경로와 일치하는 것으로 간주될 때 내장 패키지에서 가져온 노드에는 접두사 `bltinpkg://`가 붙습니다.

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

복잡한 레이아웃 수정은 꼼꼼하게 테스트되거나 잘 지원되지 않습니다. 이와 같이 특별한 레이아웃 사양 로드의 의도는 전체 패키지 네임 스페이스를 `Revit` 또는 `Formit`과 같은 특정 호스트 카테고리 아래로 이동하는 것입니다. 