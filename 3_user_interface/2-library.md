# 라이브러리

라이브러리에는 설치와 함께 제공되는 10개의 기본 카테고리 노드뿐만 아니라 추가로 로드된 사용자 노드 또는 패키지까지, 로드된 모든 노드가 포함되어 있습니다. 라이브러리의 노드는 라이브러리, 카테고리 및 하위 카테고리(해당되는 경우) 내에서 계층적으로 구성되어 있습니다.

![](images/3-2/library-libraryUI.jpg)

* 기본 노드: 기본 설치와 함께 제공됩니다.
* 사용자 노드: 자주 사용하는 루틴 또는 특수 그래프를 사용자 노드로 저장합니다. 또한 커뮤니티와 사용자 노드를 공유할 수도 있습니다.
* 패키지 관리자의 노드: 게시된 사용자 노드의 모음입니다.

이제 [노드 카테고리의 계층](3-3\_dynamo\_libraries.md#library-hierarchy-for-categories)을 살펴보고, [라이브러리에서 빠르게 검색](3-3\_dynamo\_libraries.md#quick-search-in-library)할 수 있는 방법과 그중에서 [자주 사용하는 노드](3-3\_dynamo\_libraries.md#frequently-used-nodes)에 대해 알아보겠습니다.

### 카테고리의 라이브러리 계층

이러한 카테고리를 훑어보면 작업공간에 추가할 수 있는 작업의 계층을 파악하고 이전에 사용해 본 적이 없는 새 노드를 빠르게 확인할 수 있습니다.

메뉴를 통해 각 카테고리와 하위 카테고리를 확장하여 라이브러리를 검색합니다.

{% hint style="info" %} 형상은 가장 많은 노드 수를 포함하고 있으므로 탐색을 시작하기에 유용한 메뉴입니다. {% endhint %}

![](images/3-2/library-modifiedandresizelibrarycategories.jpg)

> 1. 라이브러리
> 2. 카테고리
> 3. 하위카테고리
> 4. 노드

노드가 데이터를 **작성(Create)** 하는지, **작업(Action)** 을 실행하는지 또는 데이터를 **조회(Query)** 하는지에 따라 노드를 동일한 하위 카테고리로 추가로 분류합니다.

* ![](images/3-2/userinterface-create.jpg) **Create**: 처음부터 새로 형상을 작성하거나 구성합니다. 예: 원.
* ![](images/3-2/userinterface-action.jpg) **Action**: 항목에 대해 작업을 수행합니다. 예: 원 축척.
* ![](images/3-2/userinterface-query.jpg) **Query**: 이미 존재하는 항목의 특성을 가져옵니다. 예: 원의 반지름 가져오기.

노드 위에 마우스를 놓으면 해당 노드의 이름 및 아이콘 외에 자세한 정보가 표시됩니다. 이를 통해 노드에서 수행하는 작업, 입력에 필요한 항목, 출력으로 제공되는 항목을 쉽게 파악할 수 있습니다.

![](images/3-2/userinterface-nodedescription.jpg)

> 1. 설명 - 노드에 대한 일반 언어 설명
> 2. 아이콘 - 더 큰 버전의 라이브러리 메뉴 아이콘
> 3. 입력 - 이름, 데이터 유형 및 데이터 구조
> 4. 출력 - 데이터 유형 및 구조

### 라이브러리에서 빠른 검색

작업공간에 추가할 노드에 대해 상대적 특수성을 알고 있는 경우 **검색** 필드에 입력하여 일치하는 모든 노드를 조회합니다.

추가하려는 노드를 클릭하여 선택하거나 Enter 키를 눌러 강조 표시된 노드를 작업공간의 중심에 추가합니다.

![](images/3-2/userinterface-search.jpg)

#### 계층별 검색

키워드를 사용하여 노드를 찾는 것 외에도, 검색 필드 또는 코드 블록(_Dynamo 텍스트 언어_ 사용)에서 계층을 점으로 구분하여 입력할 수 있습니다.

각 라이브러리의 계층 구조는 작업공간에 추가된 노드의 이름에 반영됩니다.

라이브러리 계층에 있는 노드 위치의 서로 다른 부분을 `library.category.nodeName` 형식으로 입력하면 다음과 같이 서로 다른 결과가 반환됩니다.

* `library.category.nodeName`

![](<images/3-2/library-searchbyhierarchygeometrypointbycoordinates(1) (1).jpg>)

* `category.nodeName`

![](images/3-2/library-searchbyhierarchy2pointbycoordinates.jpg)

* `nodeName` 또는 `keyword`

![](images/3-2/library-searchbyhierarchy3bycoordinates.jpg)

일반적으로 작업공간에서 노드의 이름은 `category.nodeName` 형식으로 렌더링됩니다. 단, Input 및 View 카테고리에는 특히 몇 가지 중요한 예외가 있습니다.

유사한 이름의 노드에 유의하고 카테고리 차이를 기록해 두십시오.

* 대부분 라이브러리의 노드에는 카테고리 형식이 포함됩니다.

![](images/3-2/library-nodecategorydifferences1.jpg)

* `Point.ByCoordinates` 및 `UV.ByCoordinates`는 이름은 같지만 출처 카테고리는 서로 다릅니다.

![](images/3-2/library-nodecategorydifferences2.jpg)

* 중요한 예외에는 내장된 함수, Core.Input, Core.View 및 연산자가 포함됩니다.

![](images/3-2/library-nodecategorydifferences3.jpg)

### 자주 사용하는 노드

Dynamo의 기본 설치에는 수백 가지의 노드가 포함되어 있습니다. 이 중에서 시각적 프로그램 개발에 필수적인 노드는 무엇일까요? 프로그램의 매개변수(**Input**)를 정의할 수 있도록 하는 노드를 집중적으로 살펴보고, 노드 작업(**Watch**) 결과를 확인하고, 바로 가기(**Code Block**)를 통해 입력 또는 기능을 정의해 보겠습니다.

#### 입력 노드

Input 노드는 시각적 프로그램의 사용자(자신 또는 다른 사용자)가 핵심 매개변수를 사용하기 위한 주요 수단입니다. 다음은 Core 라이브러리에서 제공되는 일부 노드입니다.

| 노드           |                                           | 노드           |                                           |
| -------------- | ----------------------------------------- | -------------- | ----------------------------------------- |
| 부울        | ![](images/3-2/library-boolean.jpg)       | 수         | ![](images/3-2/library-number.jpg)        |
| 문자열         | ![](images/3-2/library-string.jpg)        | 번호 슬라이더  | ![](images/3-2/library-numberslider.jpg)  |
| 디렉토리 경로 | ![](images/3-2/library-directorypath.jpg) | 정수 슬라이더 | ![](images/3-2/library-integerslider.jpg) |
| 파일 경로      | ![](images/3-2/library-filepath.jpg)      |                |                                           |

#### Watch 및 Watch3D

Watch 노드는 시각적 프로그램을 통해 흐르는 데이터를 관리하는 데 필수적입니다. 노드 위로 마우스를 가져가면 **노드 데이터 미리보기**를 통해 노드의 결과를 볼 수 있습니다.

![](images/3-2/library-nodepreview.jpg)

**Watch** 노드에 계속 표시하면 유용합니다.

![](images/3-2/library-watchnode.jpg)

또는 **Watch3D** 노드를 통해 형상 결과를 확인할 수 있습니다.

![](images/3-2/library-watch3dnode.gif)

이러한 두 노드는 Core 라이브러리의 View 카테고리에 있습니다.

{% hint style="info" %} 팁: 시각적 프로그램에 많은 노드가 포함되어 있는 경우 3D 미리보기가 혼란스러울 수 있습니다. 설정 메뉴에서 배경 미리보기 표시 옵션의 선택을 취소하고 Watch3D 노드를 사용하여 형상을 미리 보는 방법을 고려하십시오. {% endhint %}

#### Code Block

Code Block 노드는 줄을 세미콜론으로 구분하여 코드 블록을 정의하는 데 사용할 수 있습니다. 이는 `X/Y`만큼이나 간단합니다.

Code Block을 숫자 입력 정의에 대한 바로 가기나 다른 노드 기능에 대한 호출로 사용할 수도 있습니다. 이렇게 하는 구문은 Dynamo 텍스트 언어인 [DesignScript](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md)의 명명 규칙을 따릅니다.

다음은 스크립트에서 Code Block을 사용하는 간단한 데모(지침 포함)입니다.

![](images/3-2/library-codeblockdemo.gif)

1. 두 번 클릭하여 Code Block 노드를 작성합니다.
2. `Circle.ByCenterPointRadius(x,y);`를 입력합니다.
3. 작업공간을 클릭하여 선택을 취소합니다. 그러면 `x` 및 `y` 입력이 자동으로 추가됩니다.
4. Point.ByCoordinates 노드와 Number Slider를 작성한 다음, Code Block의 입력에 연결합니다.
5. 시각적 프로그램을 실행한 결과가 3D 미리보기에 원으로 표시됩니다.
