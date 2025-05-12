# Forma에서 Dynamo Player 설정


Forma에서는 Dynamo를 두 가지 옵션인 클라우드 기반 DaaS(Dynamo as a Service) 또는 Desktop Dynamo로 사용할 수 있습니다. 수행하려는 작업에 따라 각 옵션이 제공하는 이점이 다르므로 시작하기 전에 본인의 요구 사항에 가장 적합한 옵션이 무엇인지 고려해 보십시오. 하지만 두 옵션을 언제든지 전환할 수 있다는 점에 유념하십시오.

**Dynamo as a Service와 Dynamo Desktop 비교**

<table><thead><tr><th>Dynamo as a Service</th><th>Desktop</th><th data-hidden></th></tr></thead><tbody><tr><td>간편한 설정</td><td>다단계 설치 프로세스</td><td></td></tr><tr><td>Dynamo를 설치하거나 열어 둘 필요가 없음</td><td>Dynamo를 열어 두어야 함</td><td></td></tr><tr><td>Dynamo에 이미 열려 있는 그래프는 인식하지 않음</td><td>버튼 클릭 한 번으로 Dynamo에 열려 있는 그래프를 Player에서 열 수 있음</td><td></td></tr><tr><td>Python을 사용할 수 없음</td><td>Python을 사용할 수 있음</td><td></td></tr><tr><td>승인된 패키지만 사용할 수 있음</td><td>모든 패키지 사용 가능</td><td></td></tr></tbody></table>

이 페이지에서는 두 옵션을 설정하는 방법을 설명합니다.

### Dynamo as a Service 설정

Forma 베타의 Dynamo는 현재 얼리 액세스 오픈 베타로 제공되고 있으며, 이는 기능 및 사용자 인터페이스가 자주 변경될 수 있음을 의미합니다.

먼저, Forma에 Dynamo Player를 설치하겠습니다.

1. Forma 사이트에서 왼쪽 사이드바의 **Extensions**으로 이동하고 **Add extension**을 클릭합니다. 그러면 Autodesk App Store가 열립니다.
2. Dynamo를 검색하고 Dynamo Player 베타를 추가합니다. 고지 사항을 읽고 **Agree**를 클릭합니다.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. 이제 Dynamo Player를 확장 프로그램으로 사용할 수 있습니다. 클릭하여 엽니다.
4. 이제 Dynamo Player를 사용할 준비가 되었습니다.

### Dynamo Desktop 설정

Dynamo Desktop을 사용하려면 독립 실행형 샌드박스이든 Revit 또는 Civil 3D에 연결된 버전이든 Dynamo가 필요합니다. 또한 DynamoFormaBeta 패키지도 필요합니다.

#### Revit

아래의 지침에 따라 Revit 및 DynamoFormaBeta 패키지에서 Dynamo를 설정합니다.

1. Revit 2024.1 이상이 설치되어 있는지 확인합니다.
2. 관리 > Dynamo로 이동하여 Revit에서 Dynamo 엽니다.
3. Dynamo에서 DynamoFormaBeta 패키지를 설치합니다. 패키지 > Package Manager로 이동한 후 DynamoFormaBeta를 검색합니다.
   1. Revit 2024가 설치되어 있는 경우 2.x용 DynamoFormaBeta 패키지를 설치합니다.
   2. Revit 2025가 설치되어 있는 경우 DynamoFormaBeta 패키지를 설치합니다.

#### Civil 3D

아래의 지침에 따라 Civil 3D 및 DynamoFormaBeta 패키지에서 Dynamo를 설정합니다.

1. Civil 3D 2024.1 이상이 설치되어 있는지 확인합니다.
2. 관리 > Dynamo로 이동하여 Civil 3D에서 Dynamo를 엽니다.
3. Dynamo에서 DynamoFormaBeta 패키지를 설치합니다. 패키지 > Package Manager로 이동한 후 DynamoFormaBeta를 검색합니다.
   1. Civil 3D 2024가 설치되어 있는 경우 2.x용 DynamoFormaBeta 패키지를 설치합니다.
   2. Civil 3D 2025가 설치되어 있는 경우 DynamoFormaBeta 패키지를 설치합니다.

#### Dynamo Sandbox

아래의 지침에 따라 Dynamo Sandbox 및 DynamoFormaBeta 패키지를 설치합니다.

1. [Dynamo 빌드](https://dynamobuilds.com/)에서 Dynamo 2.18.0 이상을 다운로드합니다. 최상의 환경을 위해 맨 위에 나열된 가장 안정적인 최신 버전을 선택합니다.
   1. Daily 버전은 개발 버전으로, 불완전하거나 진행 중인 기능을 포함하고 있을 수 있습니다.
2. [7zip](https://7-zip.org/)을 사용하여 선택한 폴더에 Dynamo의 압축을 풉니다.
3. Dynamo 설치 폴더에서 DynamoSandbox.exe를 실행합니다.
4. Dynamo에서 DynamoFormaBeta 패키지를 설치합니다. 패키지 > Package Manager로 이동한 후 DynamoFormaBeta를 검색합니다.
   1. Dynamo 2.x가 설치되어 있는 경우 2.x용 DynamoFormaBeta 패키지를 설치합니다.
   2. Dynamo 3.x가 설치되어 있는 경우 DynamoFormaBeta 패키지를 설치합니다.

Dynamo를 설치하고 나면 Forma에서 사용할 수 있습니다. Forma에서 Dynamo의 데스크톱 옵션을 실행할 때 Dynamo Player 확장 프로그램을 사용하려면 Dynamo가 열려 있어야 합니다.

#### Forma에서 Dynamo Desktop에 액세스

먼저, Forma에 Dynamo Player를 설치하겠습니다.

1. Forma 사이트에서 왼쪽 사이드바의 **Extensions**으로 이동하고 **Add extension**을 클릭합니다. 그러면 Autodesk App Store가 열립니다.
2. Dynamo를 검색하고 Dynamo Player 베타를 추가합니다. 고지 사항을 읽고 **Agree**를 클릭합니다.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. 이제 Dynamo Player를 확장 프로그램으로 사용할 수 있습니다. 클릭하여 엽니다.
4. 상단 근처에서 Desktop을 클릭하여 Dynamo Desktop에 액세스합니다.

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. 이제 Dynamo Player를 사용할 준비가 되었습니다. Dynamo에서 이미 그래프를 열어 둔 경우 **Connected graph** 아래의 Open을 클릭하기만 하면 Player에서 해당 그래프를 볼 수 있습니다.
