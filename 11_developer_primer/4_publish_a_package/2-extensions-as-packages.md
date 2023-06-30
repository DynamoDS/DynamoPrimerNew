# 패키지로 제공되는 확장 

### 패키지로 제공되는 확장 <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### 개요 <a href="#overview" id="overview"></a>

Dynamo 확장은 일반 Dynamo 노드 라이브러리처럼 패키지 관리자에 배포할 수 있습니다. 설치된 패키지에 뷰 확장이 포함되어 있으면 확장은 Dynamo가 로드될 때 런타임에 로드됩니다. Dynamo 콘솔을 확인하여 확장이 제대로 로드되었는지 확인할 수 있습니다.

### 패키지 구조 <a href="#package-structure" id="package-structure"></a>

확장 패키지의 구조는 다음을 포함하는 일반 패키지와 동일합니다.

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

확장을 이미 빌드했다고 가정하면 최소한 .NET 어셈블리와 매니페스트 파일이 있어야 합니다. 어셈블리에는 `IViewExtension` 또는 `IExtension`을 구현하는 클래스가 포함되어 있어야 합니다. 매니페스트 .XML 파일은 확장을 실행하기 위해 인스턴스화해야 하는 클래스를 Dynamo에 알려줍니다. 패키지 관리자가 확장을 올바르게 찾으려면 매니페스트 파일이 어셈블리 위치 및 이름과 정확하게 일치해야 합니다.

어셈블리 파일은 `bin` 폴더에 배치하고 매니페스트 파일은 `extra` 폴더에 배치합니다. 이 폴더에 추가 자산을 배치해도 됩니다.

매니페스트 .XML 파일의 예:

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### 업로드하기 <a href="#uploading" id="uploading"></a>

위에서 설명한 하위 디렉토리가 포함된 폴더가 있으면 패키지 관리자로 푸시(업로드)할 수 있습니다. 한 가지 주의해야 할 사항은 현재 Dynamo 샌드박스에서는 패키지를 게시할 수 없다는 점입니다. 즉, Dynamo Revit을 사용해야 합니다. Dynamo Revit에서 패키지 => 새 패키지 게시로 이동합니다. 그러면 패키지를 연결할 Autodesk 계정에 로그인하라는 메시지가 표시됩니다.

이때 패키지/확장과 관련된 모든 필수 필드를 입력하는 일반 게시 패키지 창이 나타납니다. 어셈블리 파일 중 노드 라이브러리로 표시된 파일이 없는지 확인해야 하는 **매우 중요한** 추가 단계가 하나 있습니다. 이 단계는 가져온 파일(위에서 생성한 패키지 폴더)을 마우스 오른쪽 버튼으로 클릭하여 수행할 수 있습니다. 이 옵션을 선택(또는 선택 취소)하는 옵션을 제공하는 상황에 맞는 메뉴가 나타납니다. 모든 확장 어셈블리는 선택 취소해야 합니다.

![패키지 게시하기](images/ViewExtension_Search.png)

공개적으로 게시하기 전에 항상 로컬에 게시하여 모든 것이 예상대로 작동하는지 확인해야 합니다. 예상대로 작동하는 것을 확인한 후 게시를 선택하면 게시할 준비가 완료된 것입니다.

### 끌어오기 <a href="#pulling" id="pulling"></a>

패키지가 성공적으로 업로드되었는지 확인하려면 게시 단계에서 지정한 이름 및 키워드를 사용하여 패키지를 검색할 수 있어야 합니다. 마지막으로, 동일한 확장을 사용하려면 작동하기 전에 Dynamo를 재부팅해야 합니다. 일반적으로 이러한 확장에는 Dynamo가 부팅될 때 지정되는 매개변수가 필요합니다.

![패키지 검색하기](images/ViewExtension_Search.jpg)
