# 시작하기

개발 단계로 넘어가기 전에 새 프로젝트를 위한 강력한 기반을 구축하는 것이 중요합니다. Dynamo 개발자 커뮤니티에는 시작할 때 사용할 수 있는 유용한 여러 프로젝트 템플릿이 있지만, 프로젝트를 처음부터 시작하는 방법을 이해하는 것이 훨씬 더 중요합니다. 프로젝트를 처음부터 새로 구축하면 개발 프로세스를 더 깊이 이해할 수 있습니다.

![Visual Studio](images/visual-studio.jpg)

### Visual Studio 프로젝트 생성하기 <a href="#creating-a-visual-studio-project" id="creating-a-visual-studio-project"></a>

Visual Studio는 프로젝트를 생성하고, 참조를 추가하고, `.dlls`를 빌드하고, 디버깅할 수 있는 강력한 IDE입니다. Visual Studio에서 새 프로젝트를 생성하면 프로젝트를 구성하기 위한 구조인 솔루션도 생성됩니다. 하나의 솔루션 내에 여러 프로젝트가 존재할 수 있으며 여러 프로젝트를 함께 구축할 수 있습니다. ZeroTouch 노드를 만들려면 C# 클래스 라이브러리를 작성하고 `.dll`을 빌드할 새 Visual Studio 프로젝트를 시작해야 합니다.

![Visual Studio에서 새 프로젝트 생성하기](images/vs-new-project-1.jpg)

![Visual Studio에서 새 프로젝트 구성하기](images/vs-new-project-2.jpg)

> Visual Studio의 새 프로젝트 창
>
> 1. 먼저 Visual Studio를 열고 새 프로젝트를 생성합니다. 이를 위해 `File > New > Project`로 이동합니다.
> 2. `Class Library` 프로젝트 템플릿을 선택합니다.
> 3. 프로젝트 이름을 지정합니다(여기서는 프로젝트 이름을 MyCustomNode로 지정함).
> 4. 프로젝트의 파일 경로를 설정합니다. 이 예에서는 기본 위치를 사용할 것입니다.
> 5. `Ok`를 선택합니다.

Visual Studio에서 자동으로 C# 파일을 생성하고 엽니다. 적절한 이름을 지정하고 작업공간을 설정한 다음 기본 코드를 이 곱하기 메서드로 바꿔야 합니다.

```
 namespace MyCustomNode
 {
     public class SampleFunctions
     {
         public static double MultiplyByTwo(double inputNumber)
         {
             return inputNumber * 2.0;
         }
     }
 }
```

![솔루션 탐색기 사용하기](images/vs-edit-class.jpg)

> 1. `View`에서 솔루션 탐색기 및 출력 창을 엽니다.
> 2. 오른쪽의 솔루션 탐색기에서 `Class1.cs` 파일의 이름을 `SampleFunctions.cs`로 바꿉니다.
> 3. 곱하기 함수에 대한 위의 코드를 추가합니다. Dynamo에서 C# 클래스를 읽는 방법에 대해서는 나중에 자세히 다룰 것입니다.
> 4. 솔루션 탐색기: 프로젝트의 모든 항목에 액세스할 수 있습니다.
> 5. 출력 창: 나중에 이 창에서 빌드가 성공했는지 확인할 수 있습니다.

다음 단계는 프로젝트를 빌드하는 것이지만, 그 전에 몇 가지 설정을 확인해야 합니다. 먼저, `Any CPU` 또는 `x64`가 플랫폼 대상으로 선택되었고 프로젝트 특성에서 `Prefer 32-bit`가 선택 취소되어 있는지 확인합니다.

![Visual Studio 빌드 설정](images/vs-build-settings.jpg)

> 1. `Project > "ProjectName" Properties`를 선택하여 프로젝트 특성을 엽니다.
> 2. `Build` 페이지를 선택합니다.
> 3. 드롭다운 메뉴에서 `Any CPU` 또는 `x64`를 선택합니다.
> 4. `Prefer 32-bit`가 선택 취소되어 있는지 확인합니다.

이제 프로젝트를 빌드하여 `.dll`을 생성할 수 있습니다. 이렇게 하려면 `Build` 메뉴에서 `Build Solution`을 선택하거나 단축키 `CTRL+SHIFT+B`를 사용합니다.

![솔루션 빌드하기](images/vs-build.jpg)

> 1. `Build > Build Solution`을 선택합니다.
> 2. 출력 창에서 프로젝트가 성공적으로 빌드되었는지 확인할 수 있습니다

프로젝트가 성공적으로 빌드된 경우 프로젝트의 `bin` 폴더에 이름이 `MyCustomNode`인 `.dll`이 있습니다. 이 예에서는 프로젝트의 파일 경로로 Visual Studio의 기본값인 `c:\users\username\documents\visual studio 2015\Projects`를 사용했습니다. 프로젝트의 파일 구조를 살펴보겠습니다.

![프로젝트의 파일 구조](images/folder-structure.jpg)

> 1. `bin` 폴더에는 Visual Studio에서 빌드된 `.dll`이 포함되어 있습니다.
> 2. Visual Studio 프로젝트 파일
> 3. 클래스 파일
> 4. 솔루션 구성이 `Debug`로 설정되었으므로 `.dll`은 `bin\Debug`에 생성됩니다.

이제 Dynamo를 열고 `.dll`을 가져올 수 있습니다. 추가 기능을 사용하여 프로젝트의 `bin` 위치로 이동한 후 `.dll`을 선택하여 엽니다.

![프로젝트의 dll 파일 열기](images/dyn-import-dll.jpg)

> 1. 추가 버튼을 선택하여 `.dll`을 가져옵니다.
> 2. 프로젝트 위치로 이동합니다. 프로젝트는 Visual Studio의 기본 파일 경로인 `C:\Users\username\Documents\Visual Studio 2015\Projects\MyCustomNode`에 있습니다.
> 3. 가져올 `MyCustomNode.dll`을 선택합니다.
> 4. `Open`을 클릭하여 `.dll`을 로드합니다.

`MyCustomNode`라는 라이브러리에 카테고리가 생성되면 .dll을 성공적으로 가져온 것입니다. 하지만, 우리는 하나의 노드만 만들려고 했는데 Dynamo에서 두 개의 노드를 만들었습니다. 다음 섹션에서는 이러한 상황이 발생하는 이유와 Dynamo에서 .dll을 읽는 방법에 대해 설명합니다.

![사용자 지정 노드](images/dyn-customnode.jpg)

> 1. Dynamo 라이브러리의 MyCustomNode. 라이브러리 범주는 `.dll` 이름에 따라 결정됩니다.
> 2. 캔버스의 SampleFunctions.MultiplyByTwo

### Dynamo에서 클래스 및 메서드를 읽는 방법 <a href="#how-dynamo-reads-classes-and-methods" id="how-dynamo-reads-classes-and-methods"></a>

Dynamo에서 .dll을 로드하면 모든 공용 정적 메서드가 노드로 노출됩니다. 생성자, 메서드 및 특성은 각각 Create, Action 및 Query 노드로 변환됩니다. 곱하기 예제에서 `MultiplyByTwo()` 메서드는 Dynamo의 Action 노드가 됩니다. 그 이유는 노드의 이름이 메서드 및 클래스를 기반으로 지정되었기 때문입니다.

![그래프의 SampleFunction.MultiplyByTwo 노드](images/multiplybytwo.png)

> 1. 입력 이름은 메서드의 매개변수 이름에 따라 `inputNumber`로 지정됩니다.
> 2. 출력 이름은 기본적으로 `double`로 지정되는데, 그 이유는 double이 반환되는 데이터 유형이기 때문입니다.
> 3. 노드의 이름은 클래스 및 메서드 이름에 해당하는 `SampleFunctions.MultiplyByTwo`로 지정됩니다.

위의 예에서는 생성자를 명시적으로 제공하지 않아 생성자가 자동으로 생성되었기 때문에 추가 `SampleFunctions` Create 노드가 만들어졌습니다. `SampleFunctions` 클래스에서 빈 Private 생성자를 생성하면 이 문제를 방지할 수 있습니다.

```
namespace MyCustomNode
{
    public class SampleFunctions
    {
        //The empty private constructor.
        //This will be not imported into Dynamo.
        private SampleFunctions() { }

        //The public multiplication method. 
        //This will be imported into Dynamo.
        public static double MultiplyByTwo(double inputNumber)
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Create 노드로 가져온 메서드](images/private-constructor.jpg)

> 1. Dynamo에서 Create 노드로 메서드를 가져왔습니다.

### Dynamo NuGet 패키지 참조 추가하기 <a href="#adding-dynamo-nuget-package-references" id="adding-dynamo-nuget-package-references"></a>

곱하기 노드는 매우 간단하므로 Dynamo에 대한 참조가 필요하지 않습니다. 예를 들어 형상을 작성하기 위해 Dynamo의 기능에 액세스하려면 Dynamo NuGet 패키지를 참조해야 합니다.

* [ZeroTouchLibrary](https://www.nuget.org/packages/DynamoVisualProgramming.ZeroTouchLibrary/2.0.0-beta3026) \- DynamoUnits.dll, ProtoGeometry.dll 라이브러리가 포함된 Dynamo용 Zero-Touch 노드 라이브러리를 빌드하기 위한 패키지
* [WpfUILibrary](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) \- DynamoCoreWpf.dll, CoreNodeModels.dll, CoreNodeModelWpf.dll 라이브러리가 포함된 WPF에서 사용자 지정 UI를 사용하여 Dynamo용 노드 라이브러리를 빌드하기 위한 패키지
* [DynamoServices](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) \- Dynamo용 DynamoServices 라이브러리
* [코어](https://www.nuget.org/packages/DynamoVisualProgramming.Core/2.0.0-beta3026) \- DSIronPython.dll, DynamoApplications.dll, DynamoCore.dll, DynamoInstallDetective.dll, DynamoShapeManager.dll, DynamoUtilities.dll, ProtoCore.dll, VMDataBridge.dll 라이브러리가 포함된 Dynamo용 단위 및 시스템 테스트 인프라
* [테스트](https://www.nuget.org/packages/DynamoVisualProgramming.Tests/2.0.0-beta3026) \- DynamoCoreTests.dll, SystemTestServices.dll, TestServices.dll 라이브러리가 포함된 Dynamo용 단위 및 시스템 테스트 인프라
* [DynamoCoreNodes](https://www.nuget.org/packages/DynamoVisualProgramming.DynamoCoreNodes/2.0.0-beta3026) \- Analysis.dll, GeometryColor.dll, DSCoreNodes.dll 라이브러리가 포함된 Dynamo용 코어 노드를 빌드하기 위한 패키지

Visual Studio 프로젝트에서 이러한 패키지를 참조하려면 위 링크의 NuGet에서 패키지를 다운로드하고 수동으로 .dlls를 참조하거나 Visual Studio에서 NuGet 패키지 관리자를 사용하십시오. 먼저 Visual Studio에서 NuGet을 사용하여 이러한 패키지를 설치하는 방법을 살펴보겠습니다.

![NuGet 패키지 관리자 열기](images/vs-nuget-package-manager2.jpg)

> 1. `Tools > NuGet Package Manager > Manage NuGet Packages for Solution...`을 선택하여 NuGet 패키지 관리자를 엽니다.

NuGet Package Manager입니다. 이 창에는 프로젝트를 위해 설치된 패키지가 표시되며, 사용자는 다른 패키지를 찾아볼 수 있습니다. 새 버전의 DynamoServices 패키지가 릴리즈되면 여기에서 패키지를 업데이트하거나 이전 버전으로 되돌릴 수 있습니다.

![NuGet 패키지 관리자](images/vs-nuget-package-manager.jpg)

> 1. 찾아보기를 선택하고 DynamoVisualProgramming을 검색하여 Dynamo 패키지를 시작합니다.
> 2. Dynamo 패키지. 패키지를 선택하면 현재 버전과 버전에 대한 설명이 표시됩니다.
> 3. 필요한 패키지 버전을 선택하고 설치를 클릭합니다. 그러면 작업 중인 특정 프로젝트에 대한 패키지가 설치됩니다. 현재 안정적인 최신 릴리즈인 Dynamo 1.3 버전을 사용하고 있으므로, 해당 패키지 버전을 선택합니다.

브라우저에서 다운로드한 패키지를 수동으로 추가하려면 솔루션 탐색기에서 참조 관리자를 열고 패키지를 찾습니다.

![참조 관리자](images/vs-manual-dynamo-package.jpg)

> 1. 마우스 오른쪽 버튼으로 `References`를 클릭하고 `Add Reference`를 선택합니다.
> 2. 패키지 위치로 이동하려면 `Browse`를 선택합니다.

Visual Studio가 올바르게 구성되었고 Dynamo에 `.dll`을 성공적으로 추가했으므로, 개념을 이해하기 위한 확고한 토대를 갖추게 되었습니다. 이것은 시작에 불과하므로, 계속 진행하면서 사용자 지정 노드를 만드는 방법에 대해 자세히 알아보십시오.
