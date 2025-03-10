# 패키지 게시하기

### 패키지 게시하기 <a href="#publish-a-package" id="publish-a-package"></a>

패키지는 노드를 저장하고 Dynamo 커뮤니티와 공유할 수 있는 편리한 방법입니다. 패키지에는 Dynamo 작업공간에서 생성된 사용자 지정 노드부터 NodeModel 파생 노드까지 모든 것이 포함될 수 있습니다. 패키지는 패키지 관리자를 사용하여 게시되고 설치됩니다. 이 페이지 외에도 [입문서](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction)에 패키지에 대한 일반적인 설명이 나와 있습니다.

#### 패키지 관리자란 무엇입니까? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Dynamo 패키지 관리자는 Dynamo 또는 웹 브라우저에서 액세스할 수 있는 소프트웨어 레지스트리(npm과 유사)입니다. 패키지 관리자에서는 패키지 설치, 게시, 업데이트 및 보기를 수행할 수 있습니다. 패키지 관리자는 npm과 마찬가지로 다양한 버전의 패키지를 유지합니다. 또한 프로젝트의 종속성을 관리하는 데도 도움이 됩니다.

브라우저에서 패키지를 검색하고 통계를 확인하십시오([https://dynamopackages.com/](https://dynamopackages.com)).

* Dynamo의 패키지 관리자에서는 패키지 설치, 게시 및 업데이트를 수행할 수 있습니다.

![패키지 검색하기](images/dynamopackagemanager.jpg)

> 1. 온라인으로 패키지 검색: `Packages > Search for a Package...`
> 2. 설치된 패키지 보기/편집: `Packages > Manage Packages...`
> 3. 새 패키지 게시: `Packages > Publish New Package...`

#### 패키지 게시하기 <a href="#publishing-a-package" id="publishing-a-package"></a>

패키지는 Dynamo 내의 패키지 관리자를 통해 게시됩니다. 권장되는 프로세스는 로컬에 게시하고, 패키지를 테스트한 다음 온라인에 게시하여 커뮤니티와 공유하는 것입니다. NodeModel 사례 연구를 참조하여 RectangularGrid 노드를 로컬에 패키지로 게시한 다음 온라인에 게시하는 데 필요한 단계를 수행합니다.

Dynamo를 시작하고 `Packages > Publish New Package...`를 선택하여 `Publish a Package` 창을 엽니다.

![패키지 게시하기](images/dyn-publish-package-add-files.jpg)

> 1. `Add file...`을 선택하여 패키지에 추가할 파일을 찾습니다.
> 2. NodeModel 사례 연구에서 두 개의 `.dll` 파일을 선택합니다.
> 3. `Ok`를 선택합니다.

패키지 콘텐츠에 파일을 추가한 상태에서 패키지에 이름, 설명 및 버전을 지정합니다. Dynamo를 사용하여 패키지를 게시하면 `pkg.json` 파일이 자동으로 생성됩니다.

![패키지 설정](images/dyn-publish-package.jpg)

> 게시할 준비가 된 패키지가 있습니다.
>
> 1. 이름, 설명 및 버전과 관련하여 필요한 정보를 입력합니다.
> 2. "로컬로 게시"를 클릭하여 게시하고 Dynamo의 패키지 폴더(`AppData\Roaming\Dynamo\Dynamo Core\1.3\packages`)를 선택하여 노드를 코어에서 사용할 수 있도록 합니다. 패키지를 공유할 준비가 될 때까지 항상 로컬에 게시합니다.

패키지를 게시하면 Dynamo 라이브러리의 `CustomNodeModel` 카테고리 아래에서 노드를 사용할 수 있습니다.

![Dynamo 라이브러리의 패키지](images/dyn-publish-package-library.jpg)

> 1. Dynamo 라이브러리에서 방금 생성한 패키지

패키지를 온라인에 게시할 준비가 되면 패키지 관리자를 열고 `Publish`를 선택한 다음 `Publish Online`을 선택합니다.

![패키지 관리자에서 패키지 게시하기](images/dyn-publish-package-directory.jpg)

> 1. Dynamo가 패키지를 어떻게 포맷했는지 확인하려면 "CustomNodeModel" 오른쪽에 있는 세 개의 세로 점을 클릭하고 "루트 디렉토리 표시"를 선택합니다.
> 2. "Dynamo 패키지 게시" 창에서 `Publish`를 선택한 다음 `Publish Online`을 선택합니다.
> 3. 패키지를 삭제하려면 `Delete`를 선택합니다.

#### 패키지를 업데이트하려면 어떻게 해야 합니까? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

패키지 업데이트는 게시하는 것과 프로세스가 유사합니다. 패키지 관리자를 열고 업데이트해야 하는 패키지에 대해 `Publish Version...`을 선택한 후 상위 버전을 입력합니다.

![패키지 버전 게시하기](images/dyn-publish-package-version.jpg)

> 1. `Publish Version`을 선택하여 루트 디렉토리의 새 파일로 기존 패키지를 업데이트한 다음 로컬에 게시할지 아니면 온라인에 게시할지 선택합니다.

#### 패키지 관리자 웹 클라이언트 <a href="#package-manager-web-client" id="package-manager-web-client"></a>

패키지 관리자 웹 클라이언트는 사용자가 패키지 데이터를 검색하고 버전 정보, 다운로드 통계 및 기타 관련 정보를 볼 수 있게 해줍니다. 또한 패키지 작성자는 웹 클라이언트를 통해 로그인하여 호환성 정보와 같은 패키지 정보를 직접 업데이트할 수 있습니다.

이러한 기능에 대한 자세한 내용은 블로그 게시글([https://dynamobim.org/discover-the-new-dynamo-package-management-experience/](https://dynamobim.org/discover-the-new-dynamo-package-management-experience/))을 참고하십시오.

패키지 관리자 웹 클라이언트는 [https://dynamopackages.com/](https://dynamopackages.com) 링크에서 액세스할 수 있습니다.

![패키지 관리자 웹 클라이언트](images/packagemanager-browser.jpg)

##### 패키지 정보 업데이트

작성자는 다음 단계에 따라 패키지 설명, 웹사이트 링크 및 리포지토리 링크를 편집할 수 있습니다.  

> 1. **내 패키지**에서 패키지를 선택하고 **패키지 정보 편집**을 클릭합니다.  
> 2. 각 필드를 사용하여 **웹사이트** 및 **리포지토리** 링크를 추가하거나 수정합니다.  
> 3. 필요에 따라 **패키지 설명**을 업데이트합니다.  
> 4. **변경사항 저장**을 클릭하여 업데이트를 적용합니다.  

 **참고**: 서버 업데이트에는 다소 시간이 걸리므로 Dynamo 내 Package Manager에서 업데이트를 새로 고치는 데 최대 15분이 걸릴 수 있습니다. 이러한 지연을 줄이기 위한 작업이 진행 중입니다.  

 ![게시된 패키지의 패키지 정보를 업데이트하기 위한 새로운 UI](images/Package-Manager_Image_5.png)

##### 게시된 패키지 버전에 대한 호환성 정보 편집  

호환성 정보는 이전에 게시된 패키지 버전에 대해 소급하여 업데이트할 수 있습니다. 다음 단계를 수행합니다.  

![게시된 패키지에 대한 호환성 정보 편집 - 1단계](images/Package-Manager_Image_6.png)

**1단계:**  

1. 업데이트하려는 패키지 버전을 클릭합니다.  
2. **종속 대상** 리스트는 패키지가 종속된 패키지로 자동으로 채워집니다.  
3. **호환성** 옆에 있는 연필 아이콘을 클릭하여 **호환성 정보 편집** 워크플로우를 엽니다.  

**2단계:**  

아래 순서도를 따르고 아래 표를 참조하면 패키지에 가장 적합한 옵션이 무엇인지 이해할 수 있습니다.

!["호환성 정보 편집" 워크플로우에서 선택할 수 있는 옵션](images/Package-Manager_Image_7.png)

몇 가지 예를 사용하여 시나리오 몇 개를 살펴보겠습니다.

**예제 패키지 1** \- Civil Connection: 이 패키지는 Revit 및 Civil 3D 모두에 대한 API 종속성이 있으며 핵심 노드 모음(예: 형상 함수, 수학 함수 및/또는 리스트 관리)을 포함하지 않습니다. 따라서 이 경우 이상적인 옵션은 옵션 1을 선택하는 것입니다. 패키지는 Revit과 Civil 3D에서 버전 범위 및/또는 개별 버전 리스트와 일치하는 경우 호환 가능으로 표시됩니다.

**예제 패키지 2** \- Rhythm: 이 패키지는 Revit 전용 노드의 모음과 Core 노드의 모음을 포함하고 있습니다. 이 경우 패키지는 호스트 종속성이 있습니다. 하지만 이 패키지는 Dynamo Core에서 작동하는 코어 노드도 포함하고 있습니다. 따라서 이 경우 이상적인 옵션은 옵션 2입니다. 패키지는 Revit 및 Dynamo Core(Dynamo Sandbox라고도 함) 환경에서 버전 범위 및/또는 개별 버전 리스트와 일치하는 경우 호환 가능으로 표시됩니다.

**예제 패키지 3** \- Mesh Toolkit: 이 패키지는 호스트 종속성이 없는 형상 노드의 모음인 Dynamo Core 패키지입니다. 따라서 이 경우 이상적인 옵션은 옵션 3입니다. 패키지는 Dynamo 및 모든 호스트 환경에서 버전 범위 및/또는 개별 버전 리스트와 일치하는 경우 호환 가능으로 표시됩니다

![호환성 정보 옵션 편집](images/Package-Manager_Image_8.png)

선택한 옵션에 따라 아래 이미지와 같이 Dynamo 및/또는 호스트별 필드가 팝업됩니다.

![호환성 정보 편집 - 2단계](images/Package-Manager_Image_9.png)