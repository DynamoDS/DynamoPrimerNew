# 확장 

확장은 Dynamo 에코시스템의 강력한 개발 도구입니다. 개발자는 이러한 확장을 통해 Dynamo 상호 작용 및 논리를 기반으로 사용자 지정 기능을 구동할 수 있습니다. 확장은 두 가지 주요 카테고리, 즉 확장과 뷰 확장으로 나눌 수 있습니다. 이름에서 알 수 있듯이, 뷰 확장 프레임워크를 사용하면 사용자 지정 메뉴 항목을 등록하여 Dynamo UI를 확장할 수 있습니다. 일반 확장은 UI를 제외하면 매우 유사한 방식으로 작동합니다. 예를 들어, Dynamo 콘솔에 특정 정보를 기록하는 확장을 빌드할 수 있습니다. 이 시나리오에서는 사용자 지정 UI가 필요하지 않으므로 확장을 사용하여 수행할 수도 있습니다.

#### 확장 사례 연구 <a href="#extension-case-study" id="extension-case-study"></a>

DynamoSamples Github 리포지토리의 SampleViewExtension 예제에 따라 그래프에 활성 노드를 실시간으로 표시하는 간단한 모델리스 창을 생성하는 데 필요한 단계를 살펴보겠습니다. 뷰 확장을 사용하려면 창에 대한 UI를 생성하고 뷰 모델에 값을 바인딩해야 합니다.

![뷰 확장 창](images/dyn-viewextension.jpg)

> 1. 뷰 확장 창은 Github 리포지토리의 SampleViewExtension 예제를 따라 개발되었습니다.

예제를 처음부터 빌드할 예정이지만, 참조하기 위해 DynamoSamples 리포지토리를 다운로드하고 빌드할 수도 있습니다.

DynamoSamples 리포지토리: [https://github.com/DynamoDS/DynamoSamples](https://github.com/DynamoDS/DynamoSamples)

> 이 연습에서는 `DynamoSamples/src/`에 있는 SampleViewExtension이라는 프로젝트를 참조합니다.

#### 뷰 확장을 구현하는 방법 <a href="#how-to-implement-a-view-extension" id="how-to-implement-a-view-extension"></a>

뷰 확장에는 다음과 같은 세 가지 필수적인 부분이 있습니다.

* 뷰 모델을 생성하는 클래스와 `IViewExtension`을 구현하는 클래스가 포함된 어셈블리
* 런타임 시 이 어셈블리를 찾아야 하는 위치와 확장의 유형을 Dynamo에 알려주는 `.xml` 파일
* 그래픽 화면표시에 데이터를 바인딩하고 창의 모양을 결정하는 `.xaml` 파일

**1\. 프로젝트 구조 생성하기**

먼저 이름이 `SampleViewExtension`인 새 `Class Library` 프로젝트를 생성합니다.

![새 클래스 라이브러리 생성하기](images/vs-new-project-viewextension-1.jpg)

![새 프로젝트 구성하기](images/vs-new-project-viewextension-2.jpg)

> 1. `File > New > Project`를 선택하여 새 프로젝트를 생성합니다.
> 2. `Class Library`를 선택합니다.
> 3. 프로젝트의 이름을 `SampleViewExtension`으로 지정합니다.
> 4. `Ok`를 선택합니다.

이 프로젝트에는 두 개의 클래스가 필요합니다. 한 클래스는 `IViewExtension`을 구현하고, 다른 클래스는 `NotificationObject.`을 구현합니다. `IViewExtension`은 확장이 배포, 로드, 참조 및 삭제되는 방식에 대한 모든 정보를 포함하고 있습니다. `NotificationObject`는 Dynamo 및 `IDisposable`의 변경 사항에 대한 알림을 제공합니다. 변경이 이루어지면 그에 따라 개수가 업데이트됩니다.

![뷰 확장 클래스 파일](images/vs-viewextension-classes.jpg)

> 1. `IViewExtension`을 구현할 `SampleViewExtension.cs`라는 이름의 클래스 파일
> 2. `NotificationObject`를 구현할 `SampleWindowViewMode.cs`라는 이름의 클래스 파일

`IViewExtension`을 사용하려면 WpfUILibrary NuGet 패키지가 필요합니다. 이 패키지를 설치하면 Core, Services 및 ZeroTouchLibrary 패키지가 자동으로 설치됩니다.

![뷰 확장 패키지](images/vs-viewextension-packages.jpg)

> 1. WpfUILibrary를 선택합니다.
> 2. `Install`을 선택하여 모든 종속 패키지를 설치합니다.

**2\. IViewExtension 클래스 구현하기**

`IViewExtension` 클래스에서 Dynamo를 시작할 때, 확장이 로드될 때, Dynamo가 종료될 때 수행되는 작업을 결정해 보겠습니다. `SampleViewExtension.cs` 클래스 파일에서 다음 코드를 추가합니다.

```
using System;
using System.Windows;
using System.Windows.Controls;
using Dynamo.Wpf.Extensions;

namespace SampleViewExtension
{

    public class SampleViewExtension : IViewExtension
    {
        private MenuItem sampleMenuItem;

        public void Dispose()
        {
        }

        public void Startup(ViewStartupParams p)
        {
        }

        public void Loaded(ViewLoadedParams p)
        {
            // Save a reference to your loaded parameters.
            // You'll need these later when you want to use
            // the supplied workspaces

            sampleMenuItem = new MenuItem {Header = "Show View Extension Sample Window"};
            sampleMenuItem.Click += (sender, args) =>
            {
                var viewModel = new SampleWindowViewModel(p);
                var window = new SampleWindow
                {
                    // Set the data context for the main grid in the window.
                    MainGrid = { DataContext = viewModel },

                    // Set the owner of the window to the Dynamo window.
                    Owner = p.DynamoWindow
                };

                window.Left = window.Owner.Left + 400;
                window.Top = window.Owner.Top + 200;

                // Show a modeless window.
                window.Show();
            };
            p.AddMenuItem(MenuBarType.View, sampleMenuItem);
        }

        public void Shutdown()
        {
        }

        public string UniqueId
        {
            get
            {
                return Guid.NewGuid().ToString();
            }  
        } 

        public string Name
        {
            get
            {
                return "Sample View Extension";
            }
        } 

    }
}
```

`SampleViewExtension` 클래스는 클릭 가능한 메뉴 항목을 생성하여 창을 열고 뷰 모델 및 창에 연결합니다.

* `public class SampleViewExtension : IViewExtension` `SampleViewExtension`은 `IViewExtension` 인터페이스에서 상속되어 메뉴 항목을 생성하는 데 필요한 모든 것을 제공합니다.
* `sampleMenuItem = new MenuItem { Header = "Show View Extension Sample Window" };`는 메뉴 항목을 생성하여 `View` 메뉴에 추가합니다.

![메뉴 항목](images/dyn-menuitem.jpg)

> 1. 메뉴 항목

* `sampleMenuItem.Click += (sender, args)`는 메뉴 항목을 클릭할 때 새 창을 여는 이벤트를 트리거합니다.
* `MainGrid = { DataContext = viewModel }`은 창에서 주 그리드에 대한 데이터 컨텍스트를 설정하며, 생성하려고 하는 `.xaml` 파일에서 `Main Grid`를 참조합니다.
* `Owner = p.DynamoWindow`는 팝업 창의 소유자를 Dynamo로 설정합니다. 즉, 새 창이 Dynamo에 종속되므로 Dynamo 최소화, 최대화 및 복원과 같은 작업을 수행하면 새 창이 이와 동일한 동작을 따르게 됩니다.
* `window.Show();`는 추가 창 특성이 설정된 창을 표시합니다.

**3\. 뷰 모델 구현하기**

지금까지 창의 기본 매개변수 몇 가지를 설정했으므로, 이제 다양한 Dynamo 관련 이벤트에 응답하기 위한 논리를 추가하고 이러한 이벤트에 따라 UI가 업데이트되도록 하겠습니다. 다음 코드를 `SampleWindowViewModel.cs` 클래스 파일에 복사합니다.

```
using System;
using Dynamo.Core;
using Dynamo.Extensions;
using Dynamo.Graph.Nodes;

namespace SampleViewExtension
{
    public class SampleWindowViewModel : NotificationObject, IDisposable
    {
        private string activeNodeTypes;
        private ReadyParams readyParams;

        // Displays active nodes in the workspace
        public string ActiveNodeTypes
        {
            get
            {
                activeNodeTypes = getNodeTypes();
                return activeNodeTypes;
            }
        }

        // Helper function that builds string of active nodes
        public string getNodeTypes()
        {
            string output = "Active nodes:\n";

            foreach (NodeModel node in readyParams.CurrentWorkspaceModel.Nodes)
            {
                string nickName = node.Name;
                output += nickName + "\n";
            }

            return output;
        }

        public SampleWindowViewModel(ReadyParams p)
        {
            readyParams = p;
            p.CurrentWorkspaceModel.NodeAdded += CurrentWorkspaceModel_NodesChanged;
            p.CurrentWorkspaceModel.NodeRemoved += CurrentWorkspaceModel_NodesChanged;
        }

        private void CurrentWorkspaceModel_NodesChanged(NodeModel obj)
        {
            RaisePropertyChanged("ActiveNodeTypes");
        }

        public void Dispose()
        {
            readyParams.CurrentWorkspaceModel.NodeAdded -= CurrentWorkspaceModel_NodesChanged;
            readyParams.CurrentWorkspaceModel.NodeRemoved -= CurrentWorkspaceModel_NodesChanged;
        }
    }
}
```

이 뷰 모델 클래스 구현은 `CurrentWorkspaceModel`을 수신하고 작업공간에서 노드가 추가되거나 제거될 때 이벤트를 트리거합니다. 이렇게 하면 데이터가 변경되어 업데이트가 필요하다는 것을 UI 또는 바인딩된 요소에 알리는 특성 변경이 발생합니다. 내부적으로 추가 도우미 함수 `getNodeTypes()`를 호출하는 `ActiveNodeTypes` getter가 호출됩니다. 이 함수는 캔버스의 모든 활성 노드를 반복하고, 이러한 노드의 이름이 포함된 문자열을 채우고, 이 문자열을 팝업 창에 표시할 .xaml 파일의 바인딩으로 반환합니다.

확장의 핵심 논리가 정의되었으므로, 이제 `.xaml` 파일을 사용하여 창의 모양 세부 사항을 지정하겠습니다. `TextBlock` `Text`에서 `ActiveNodeTypes` 특성 바인딩을 통해 문자열을 표시하는 간단한 창만 있으면 됩니다.

![창 추가하기](images/vs-window.jpg)

> 1. 프로젝트를 마우스 오른쪽 버튼으로 클릭하고 `Add > New Item...`을 선택합니다.
> 2. 창을 생성하기 위해 변경할 사용자 컨트롤 템플릿을 선택합니다.
> 3. 새 파일의 이름을 `SampleWindow.xaml`로 지정합니다.
> 4. `Add`를 선택합니다.

창 `.xaml` 코드에서 `SelectedNodesText`를 텍스트 블록에 바인딩해야 합니다. 다음 코드를 `SampleWindow.xaml`에 추가합니다.

```
<Window x:Class="SampleViewExtension.SampleWindow"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:SampleViewExtension"
             mc:Ignorable="d" 
             d:DesignHeight="300" d:DesignWidth="300"
            Width="500" Height="100">
    <Grid Name="MainGrid" 
          HorizontalAlignment="Stretch"
          VerticalAlignment="Stretch">
        <TextBlock HorizontalAlignment="Stretch" Text="{Binding ActiveNodeTypes}" FontFamily="Arial" Padding="10" FontWeight="Medium" FontSize="18" Background="#2d2d2d" Foreground="White"/>
    </Grid>
</Window>
```

* `Text="{Binding ActiveNodeTypes}"`는 `SampleWindowViewModel.cs`에서 `ActiveNodeTypes`의 특성 값을 창의 `TextBlock` `Text` 값에 바인딩합니다.

이제 .xaml C# 백킹 파일 `SampleWindow.xaml.cs`에서 샘플 창을 초기화하겠습니다. 다음 코드를 `SampleWindow.xaml`에 추가합니다.

```
using System.Windows;

namespace SampleViewExtension
{
    /// <summary>
    /// Interaction logic for SampleWindow.xaml
    /// </summary>
    public partial class SampleWindow : Window
    {
        public SampleWindow()
        {
            InitializeComponent();
        }
    }
}
```

이제 뷰 확장을 빌드하고 Dynamo에 추가할 준비가 되었습니다. Dynamo에서 출력 `.dll`을 확장으로 등록하려면 `xml` 파일이 필요합니다.

![새 XML 추가하기](images/vs-viewextension-xml.jpg)

> 1. 프로젝트를 마우스 오른쪽 버튼으로 클릭하고 `Add > New Item...`을 선택합니다.
> 2. XML 파일을 선택합니다.
> 3. 파일 이름을 `SampleViewExtension_ViewExtensionDefinition.xml`로 지정합니다.
> 4. `Add`를 선택합니다.

* 파일 이름은 `"extensionName"_ViewExtensionDefinition.xml`과 같이 확장 어셈블리를 참조하기 위해 Dynamo 표준을 따릅니다.

`xml` 파일에서 다음 코드를 추가하여 Dynamo에 확장 어셈블리를 찾을 위치를 알려줍니다.

```
<ViewExtensionDefinition>
  <AssemblyPath>C:\Users\username\Documents\Visual Studio 2015\Projects\SampleViewExtension\SampleViewExtension\bin\Debug\SampleViewExtension.dll</AssemblyPath>
  <TypeName>SampleViewExtension.SampleViewExtension</TypeName>
</ViewExtensionDefinition>
```

* 이 예제에서는 기본 Visual Studio 프로젝트 폴더에 어셈블리를 빌드했습니다. `<AssemblyPath>...</AssemblyPath>` 대상을 어셈블리의 위치로 바꿉니다.

마지막 단계는 `SampleViewExtension_ViewExtensionDefinition.xml` 파일을 Dynamo Core 설치 디렉토리 `C:\Program Files\Dynamo\Dynamo Core\1.3\viewExtensions`에 있는 Dynamo의 뷰 확장 폴더에 복사하는 것입니다. `extensions` 및 `viewExtensions`를 위한 별도의 폴더가 있습니다. `xml` 파일을 잘못된 폴더에 배치하면 런타임 시 제대로 로드되지 않을 수 있습니다.

![확장 폴더에 복사된 XML 파일](images/fe-viewextension-xml.jpg)

> 1. Dynamo의 뷰 확장 폴더에 복사한 `.xml` 파일

여기에는 뷰 확장에 대한 기본적인 소개만 나와 있습니다. 보다 정교한 사례 연구는 Github의 오픈 소스 프로젝트인 DynaShape 패키지를 참조하십시오. 이 패키지는 Dynamo 모델 뷰에서 실시간으로 편집할 수 있는 뷰 확장을 사용합니다.

DynaShape용 패키지 설치 프로그램은 Dynamo 포럼([https://forum.dynamobim.com/t/dynashape-published/11666](https://forum.dynamobim.com/t/dynashape-published/11666))에서 다운로드할 수 있습니다.

소스 코드는 Github([https://github.com/LongNguyenP/DynaShape](https://github.com/LongNguyenP/DynaShape))에서 복제할 수 있습니다.
