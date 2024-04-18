# 패키지 소개 

Dynamo는 다양한 기능을 기본적으로 제공하고, Dynamo의 기능을 크게 확장할 수 있는 광범위한 패키지 라이브러리도 유지합니다. 패키지는 사용자 노드 또는 추가 기능의 모음입니다. Dynamo Package Manager는 커뮤니티가 온라인으로 게시된 패키지를 다운로드할 수 있는 포털입니다. 이러한 도구 세트는 Dynamo의 핵심 기능을 확장하고, 누구나 액세스할 수 있으며, 버튼 클릭으로 즉시 다운로드할 수 있도록 타사에서 개발되었습니다.

![Package Manager 사이트](../images/6-2/1/dpm.jpg)

Dynamo와 같은 오픈 소스 프로젝트에서는 이러한 유형의 커뮤니티 참여가 활성화되어 있습니다. 전용 타사 개발자가 있으므로 Dynamo는 다양한 산업 분야에 해당하는 워크플로우로 확장할 수 있습니다. 이러한 이유로 Dynamo 팀에서는 패키지 개발 및 게시를 간소화하기 위해 합심하여 노력했습니다. 이 내용은 다음 섹션에서 좀 더 자세히 다룰 예정입니다.

### 패키지 설치

패키지를 설치하는 가장 쉬운 방법은 Dynamo 인터페이스에서 패키지 메뉴 옵션을 사용하는 것입니다. 이 기능으로 이동한 후 바로 패키지를 설치하겠습니다. 이 빠른 예에서는 그리드에서 쿼드 패널을 작성할 때 자주 사용하는 패키지를 설치합니다.

Dynamo에서 _패키지 > Package Manager..._ 로 이동합니다.

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

검색 막대에서 "직사각형 그리드의 쿼드"를 검색해 보겠습니다. 그러면 이 검색 조회와 일치하는 모든 패키지가 표시됩니다. 일치하는 이름을 가진 첫 번째 패키지를 선택하려고 합니다.

설치를 클릭하여 이 패키지를 라이브러리에 추가한 다음 확인을 수락합니다. 완료되었습니다.

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

이제 Dynamo 라이브러리에 "buildz"라는 다른 그룹이 있는 것을 확인할 수 있습니다. 이 이름은 패키지의 개발자를 나타내며, 사용자 노드가 이 그룹에 배치됩니다. 이것을 바로 사용할 수 있습니다.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

**Code Block**을 사용하여 직사각형 그리드를 빠르게 정의하고 그 결과를 **Polygon.ByPoints** 노드로 출력한 다음, **Surface.ByPatch** 노드를 사용하여 방금 작성한 직사각형 패널 리스트를 봅니다.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### 패키지 폴더 설치 - DynamoUnfold

위의 예에서는 하나의 사용자 노드가 있는 패키지에 초점을 맞추고 있지만, 여러 개의 사용자 노드 및 지원 데이터 파일이 있는 패키지를 다운로드할 때도 동일한 프로세스를 사용합니다. 이제 보다 포괄적인 패키지인 Dynamo Unfold를 사용하면서 살펴보겠습니다.

위의 예에서와 같이 먼저 _패키지 > Package Manager.._ 를 선택합니다.

이번에는 한 단어 _"DynamoUnfold"_ 를 검색해 보겠습니다. 패키지가 표시되면 설치를 클릭하여 Dynamo Unfold를 Dynamo 라이브러리에 추가하여 다운로드합니다.

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

Dynamo 라이브러리에는 여러 카테고리와 사용자 노드가 있는 _DynamoUnfold_ 그룹이 있습니다.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

이제 패키지의 파일 구조를 살펴보겠습니다. 

1. 먼저 패키지 > Package Manager > 설치된 패키지로 이동합니다.
2. DynamoUnfold 옆의 옵션 메뉴 <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">를 선택합니다.
3. 그런 다음 루트 디렉토리 표시를 클릭하여 이 패키지의 루트 폴더를 엽니다.

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

이렇게 하면 패키지의 루트 디렉토리로 이동됩니다. 3개의 폴더와 1개의 파일이 있는지 확인합니다.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. _bin_ 폴더에는 .dll 파일이 있습니다. 이 Dynamo 패키지는 Zero-Touch를 사용하여 개발되었으므로 사용자 노드가 이 폴더에 저장됩니다.
> 2. _dyf_ 폴더에는 사용자 노드가 있습니다. 이 패키지는 Dynamo 사용자 노드를 사용하여 개발되지 않았으므로 이 패키지의 경우 이 폴더는 비어 있습니다.
> 3. extra 폴더에는 예시 파일을 비롯한 모든 추가 파일이 저장됩니다.
> 4. pkg 파일은 패키지 설정을 정의하는 기본 텍스트 파일입니다. 지금은 이를 무시해도 됩니다.

"extra" 폴더를 열면 설치 중에 다운로드된 여러 가지 예시 파일이 있습니다. 모든 패키지에 예시 파일이 있는 것은 아니지만, 패키지에 속하는 예시가 있는 경우 여기서 찾을 수 있습니다.

"SphereUnfold"를 열어 보겠습니다.

![](../images/6-2/1/rd2.jpg)

파일을 열고 솔버에서 "실행"을 누르면 펼쳐진 구가 표시됩니다. 이와 같은 예제 파일은 새 Dynamo 패키지를 사용하는 방법을 배우는 데 유용합니다.

\![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### 패키지 정보 찾아보기 및 보기

Package Manager에서 패키지 검색 탭의 정렬 및 필터링 옵션을 사용하여 패키지를 찾을 수 있습니다. 호스트 프로그램, 상태(신규, 더 이상 사용되지 않음 또는 아직 사용 가능) 및 패키지의 종속성 유무에 따라 사용할 수 있는 여러 필터가 있습니다.

패키지를 정렬하여 평가 점수가 높거나 가장 많이 다운로드된 패키지를 식별하거나 최신 업데이트가 있는 패키지를 찾을 수 있습니다. 

상세 정보 보기를 클릭하여 각 패키지에 대한 자세한 내용에 액세스할 수도 있습니다. 이렇게 하면 Package Manager의 측면 패널이 열리며, 여기에서 버전 관리 및 종속성, 웹사이트 또는 리포지토리 URL, 라이센스 정보 등과 같은 정보를 찾을 수 있습니다.

### Dynamo Package Manager 웹사이트

Dynamo 패키지를 찾는 또 다른 방법은 [Dynamo Package Manager](http://dynamopackages.com) 웹사이트를 탐색하는 것입니다. 여기에서 패키지에 대한 통계를 찾고 순위표를 작성할 수 있습니다. 패키지 파일은 Dynamo Package Manager에서 다운로드할 수도 있지만, 보다 원활한 프로세스는 Dynamo에서 바로 다운로드하는 것입니다.

![](../images/6-2/1/dpm2.jpg)

### 패키지 파일이 로컬로 저장되는 위치

패키지 파일이 저장되는 위치를 확인하려면 상단 탐색에서 Dynamo > 기본 설정 > 패키지 설정 > 노드 및 패키지 파일 위치를 클릭합니다. 여기에서 현재 루트 폴더 디렉토리를 찾을 수 있습니다.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

기본적으로 패키지는 _C:/Users/[사용자 이름]/AppData/Roaming/Dynamo/[Dynamo 버전]_ 폴더 경로와 비슷한 위치에 설치됩니다.

### 패키지에 대해 좀 더 자세히 알아보기

Dynamo 커뮤니티는 끊임없이 발전하고 진화하고 있습니다. 때때로 Dynamo Package Manager를 탐색하면 흥미로운 새 개발 항목을 찾아낼 수 있습니다. 다음 섹션에서는 최종 사용자 관점부터 고유한 Dynamo 패키지 작성자의 관점까지 고려하며 패키지를 좀 더 자세히 살펴보겠습니다.
