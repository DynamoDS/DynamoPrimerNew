# 자주 묻는 질문과 대답(FAQ)

## Dynamo 빌드 활용 방법

### 일일 빌드와 안정적인 빌드 비교
Autodesk Dynamo 팀은 일반적으로 커밋될 때마다 일일 빌드를 릴리즈하고 시스템 테스트 및 릴리즈 주기를 거친 이후 안정적인 릴리즈 빌드를 릴리즈하여 빠른 속도로 반복 개발을 유지합니다. 저희 팀은 사용자가 디스크에서 DynamoCore가 추출되는 위치를 로컬로 제어할 수 있도록 일일 빌드와 안정적인 빌드를 다시 시작하려고 합니다. 따라서 다른 ADSK 제품이 Dynamo의 영향을 받지 않고 사용자는 자신 있게 DynamoCore를 사용할 수 있습니다. .nupkg, .zip 파일 또는 사용자가 설치 경로 또는 기타 옵션을 선택할 수 있는 전용 설치 프로그램을 포함하여 이를 위한 몇 가지 적합한 후보가 있습니다. 

사용자가 가장 간단한 방법으로 최신 코드를 사용할 수 있도록 한다는 목표를 고려하여, 저희는 DynamoCore 바이너리와 Dynamo Sandbox가 포함된 .zip 파일을 제공하기로 결정했습니다. 이 파일은 Revit 없이 사용할 수 있습니다. 단, 몇 가지 제약 조건이 있습니다.

### Dynamo Zip 빌드
#### 정의 및 출처
DynamoCoreRuntime zip 빌드는 자동화된 빌드 중에 만들어지는 DynamoCore 바이너리의 스냅샷입니다. 

추출된 폴더에서 DynamoSandbox.exe를 시작하여 최소한의 설정으로 Dynamo를 사용할 수 있어야 합니다.


#### 필수 구성요소

| Dynamo 버전  |Microsoft Visual C++  | DirectX  |   |   |   |   |
|---|---|---|---|---|---|---|
|  2.0 - 2.6 |  2015 재배포 가능 요소  | 10  |   |   |   |   |
| 2.7  | 2019 재배포 가능 요소  | 11/12(Windows 10에 포함)  |   |   |   |   |
| >=2.8  | 2019 재배포 가능 요소  | 11/12(Windows 10에 포함)  |   |   |   |   |
##### Dynamo Github 리포지토리에서도 공개적으로 사용할 수 있는 Microsoft DirectX([여기](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX))

##### 패키지의 압축을 푸는 데 사용되는 7zip([여기](https://www.7-zip.org/download.html))


##### Microsoft Visual C++ 2015-2024 재배포 가능 요소(x64) [링크](https://aka.ms/vs/17/release/vc_redist.x64.exe)

##### 선택적 구성요소
형상 라이브러리(Revit, Civil 3D, Advanced Steel 등과 같은 특정 Autodesk 모델링 도구에서만 사용할 수 있음)

### 문제 해결
빌드의 압축을 풀고 DynamoSandbox.exe를 전혀 시작할 수 없는 경우 [7zip](https://www.7-zip.org/download.html)을 사용하여 빌드의 압축을 푸십시오. 컴퓨터에 대한 권한이 있는 경우 .zip 아카이브를 추출하기 *전에* 수동으로 차단을 해제할 수도 있습니다.

![](images/a-7/dynamo-builds-1.png)


필요한 구성요소가 누락되면 Dynamo를 사용하는 데 문제가 발생하고 UI의 특정 부분이 로드되지 않을 수 있습니다.

다음 스크린샷을 예로 들어보면 GPU가 없는 초기 상태의 Windows 10 VM에서 빌드의 압축을 풀면 이 컴퓨터에는 필수 구성 요소가 둘 다 없습니다. 이러한 내용이 Dynamo 콘솔에 표시됩니다.

![](images/a-7/dynamo-builds-2.png)

##### DirectX 설치
Microsoft 지침에 따라 DirectX가 이미 설치되어 있는지 확인하십시오. 설치되어 있는지 않으면 Dynamo Github 리포지토리([여기](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX))에서 DXSETUP.exe를 열 수 있습니다. 아래 대화상자가 표시되면 다음을 눌러 기본 위치에 DirectX를 설치합니다.

![](images/a-7/dynamo-builds-3.png)

##### Microsoft Visual C++ 2015-2024 재배포 가능 요소(x64) 설치
[여기](https://aka.ms/vs/17/release/vc_redist.x64.exe)에서 최신 버전을 다운로드하십시오. 그러면 브라우저 다운로드 위치에서 vc_redist.x64.exe라는 설치 프로그램을 실행할 수 있습니다. 아래 대화상자가 표시되면 설치를 클릭하여 이 구성요소를 기본 위치에 설치합니다.

![](images/a-7/dynamo-builds-4.png)


위의 링크에서 필수 구성요소를 둘 다 설치한 후 DynamoSandbox.exe를 다시 실행하면 다음과 같은 결과가 나타납니다.

![](images/a-7/dynamo-builds-5.png)

##### 3D 그래픽 누락 

처음으로 Sandbox를 실행할 때 그래픽 문제가 발생할 수 있는데 다음 표준 그래픽 문제 FAQ를 따를 수 있습니다.

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

일반적으로 DynamoSandbox.exe를 실행할 때 그래픽 카드에 고성능 GPU 모드를 강제로 사용해야 할 수 있습니다.

_NVIDIA 제어판의 예:_

![](images/a-7/dynamo-builds-6.png)

##### WebView2 Runtime 설치
현재, 다음 Dynamo 모듈은 WebView2 구성요소인 문서 탐색기, 안내 둘러보기 및 라이브러리를 사용합니다. 따라서 Dynamo의 이 부분이 웹 컨텐츠를 올바르게 표시하도록 하려면 WebView2 Evergreen Runtime 설치 프로그램을 설치해야 합니다. 그 전에 이 컴퓨터에 이미 설치되어 있는지 또는 설치해야 하는지 확인해야 합니다.

WebView2 Runtime을 설치할 수 있는 링크: [https://developer.microsoft.com/ko-kr/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section)

![](images/a-7/dynamo-builds-7.png)

Evergreen Bootstrapper 또는 Evergreen Standalone Installer 둘 중 하나만 설치하면 됩니다. Evergreen Bootstrapper는 1.50MB 설치 프로그램을 다운로드하고 Evergreen Standalone Installer는 130MB 설치 프로그램을 다운로드합니다.

Runtime이 설치되면 Dynamo의 다음 구성요소가 제대로 작동해야 합니다.

![](images/a-7/dynamo-builds-8.png)


##### Dynamo Excel 노드 문제
진단은 이 [문서](https://knowledge.autodesk.com/support/revit-products/troubleshooting/caas/sfdcarticles/sfdcarticles/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html)를 참조하십시오.

### Dynamo 빌드 위치
안정적인 릴리즈

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

일일 빌드 및 안정적인 릴리즈

[https://dynamobuilds.com/](https://dynamobuilds.com/)

