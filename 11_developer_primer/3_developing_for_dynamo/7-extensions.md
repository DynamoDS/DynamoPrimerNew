# 扩展

扩展是 Dynamo 生态系统中一款功能强大的开发工具。它们允许开发人员基于 Dynamo 交互和逻辑来派生自定义功能。扩展可以分为两大类：扩展和视图扩展。顾名思义，视图扩展框架允许您通过注册自定义菜单项来扩展 Dynamo UI。除了 UI 之外，常规扩展的运行方式非常相似。例如。我们可以构建一个将特定信息记录到 Dynamo 控制台的扩展。此场景不需要自定义 UI，因此也可以使用扩展完成。

#### 扩展案例研究 <a href="#extension-case-study" id="extension-case-study"></a>

按照 DynamoSamples Github 存储库中的 SampleViewExtension 示例，我们将逐步介绍创建一个实时显示图形中活动节点的简单无模式窗口所需的步骤。视图扩展要求我们为窗口创建 UI，并将值绑定到视图模型。

![视图扩展窗口](images/dyn-viewextension.jpg)

> 1. 视图扩展窗口是按照 Github 存储库中的 SampleViewExtension 示例开发的。

尽管我们将从头开始构建示例，但您也可以下载并构建 DynamoSamples 存储库以用作参照。

DynamoSamples 存储库：[https://github.com/DynamoDS/DynamoSamples](https://github.com/DynamoDS/DynamoSamples)

> 此漫游将专门参照位于 `DynamoSamples/src/` 中名为 SampleViewExtension 的项目。

#### 如何实现视图扩展 <a href="#how-to-implement-a-view-extension" id="how-to-implement-a-view-extension"></a>

视图扩展有三个基本部分：

* 一个程序集，包含实现 `IViewExtension` 的类以及创建视图模型的类
* 一个 `.xml` 文件，告知 Dynamo 在运行时应在何处查找此程序集以及扩展类型
* 一个 `.xaml` 文件，将数据绑定到图形显示并确定窗口外观

**1\.创建项目结构**

首先，创建一个名为 `SampleViewExtension` 的 `Class Library` 新项目。

![创建新类库](images/vs-new-project-viewextension-1.jpg)

![配置新项目](images/vs-new-project-viewextension-2.jpg)

> 1. 通过选择 `File > New > Project` 来创建新项目
> 2. 选择 `Class Library`
> 3. 将项目命名为 `SampleViewExtension`
> 4. 选择 `Ok`

在此项目中，我们需要两个类。一个类将实现 `IViewExtension`，另一个类将实现 `NotificationObject.`。`IViewExtension` 将包含有关如何展开、载入、参照和处理我们扩展的所有信息。`NotificationObject` 将为 Dynamo 和 `IDisposable` 中的更改提供通知；当发生更改时，计数将随之更新。

![视图扩展类文件](images/vs-viewextension-classes.jpg)

> 1. 一个名为 `SampleViewExtension.cs` 的类文件，将实现 `IViewExtension`
> 2. 一个名为 `SampleWindowViewMode.cs` 的类文件，将实现 `NotificationObject`

要使用 `IViewExtension`，我们需要 WpfUILibrary NuGet 软件包。安装此软件包将自动安装 Core、Services 和 ZeroTouchLibrary 软件包。

![视图扩展包](images/vs-viewextension-packages.jpg)

> 1. 选择 WpfUILibrary
> 2. 选择 `Install` 以安装所有相关软件包

**2\.实现 IViewExtension 类**

在 `IViewExtension` 类中，我们将确定 Dynamo 启动、载入扩展以及 Dynamo 关闭时会出现的情况。在 `SampleViewExtension.cs` 类文件中，添加以下代码：

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
            p.AddExtensionMenuItem(sampleMenuItem);
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

`SampleViewExtension` 类创建用于打开窗口的可单击菜单项，并将其连接到视图模型和窗口。

* `public class SampleViewExtension : IViewExtension` `SampleViewExtension` 继承自 `IViewExtension` 接口，提供我们创建菜单项所需的一切。
* `sampleMenuItem = new MenuItem { Header = "Show View Extension Sample Window" };` 创建 MenuItem 并将其添加到 `View` 菜单。

![菜单项](images/dyn-menuitem.jpg)

> 1. 菜单项

* `sampleMenuItem.Click += (sender, args)` 会触发在单击菜单项时将打开一个新窗口的事件
* `MainGrid = { DataContext = viewModel }` 会为窗口中的主网格设置数据上下文，参考我们将创建的 `.xaml` 文件中的 `Main Grid`
* `Owner = p.DynamoWindow` 会将我们弹出窗口的所有者设置为 Dynamo。这意味着新窗口依赖于 Dynamo，因此最小化、最大化和恢复 Dynamo 等操作将导致新窗口遵循此相同行为
* `window.Show();` 会显示已设置其他窗口特性的窗口

**3\.实现视图模型**

现在，我们已经建立了窗口的一些基本参数，我们将添加用于响应各种 Dynamo 相关事件的逻辑，并指示 UI 根据这些事件进行更新。将以下代码复制到 `SampleWindowViewModel.cs` 类文件：

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

视图模型类的此实现会侦听 `CurrentWorkspaceModel`，并会在工作空间添加或删除节点时触发事件。这将引发特性更改，通知 UI 或绑定图元：数据已更改并需要更新。`ActiveNodeTypes` getter 会被调用，它将在内部调用一个附加辅助函数 `getNodeTypes()`。此函数会遍历画布上的所有活动节点、填充包含这些节点名称的字符串，并将此字符串返回给 .xaml 文件中要在弹出窗口中显示的绑定。

在定义扩展的核心逻辑后，我们现在将使用 `.xaml` 文件指定窗口的外观详细信息。我们所需要的只是一个简单的窗口，它将通过 `TextBlock` `Text` 中的 `ActiveNodeTypes` 特性绑定来显示字符串。

![添加窗口](images/vs-window.jpg)

> 1. 在项目上单击鼠标右键，然后选择 `Add > New Item...`
> 2. 选择我们将对其进行更改以创建窗口的“用户控制”模板
> 3. 将新文件命名为 `SampleWindow.xaml`
> 4. 选择 `Add`

在 `.xaml` 窗口代码中，我们需要将 `SelectedNodesText` 绑定到文本块。将以下代码添加到 `SampleWindow.xaml`：

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

* `Text="{Binding ActiveNodeTypes}"` 会将 `SampleWindowViewModel.cs` 中的 `ActiveNodeTypes` 特性值绑定到窗口中的 `TextBlock` `Text` 值。

现在，我们将初始化 .xaml C# 备份文件 `SampleWindow.xaml.cs` 中的样例窗口。将以下代码添加到 `SampleWindow.xaml`：

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

视图扩展现在已准备好构建并添加到 Dynamo 中。Dynamo 需要一个 `xml` 文件，才能将我们的输出 `.dll` 注册为扩展。

![添加新 XML](images/vs-viewextension-xml.jpg)

> 1. 在项目上单击鼠标右键，然后选择 `Add > New Item...`
> 2. 选择 XML 文件
> 3. 将文件命名为 `SampleViewExtension_ViewExtensionDefinition.xml`
> 4. 选择 `Add`

* 文件名遵循 Dynamo 标准，以便参照扩展程序集，如下所示：`"extensionName"_ViewExtensionDefinition.xml`

在 `xml` 文件中，添加以下代码以告知 Dynamo 要在何处查找扩展程序集：

```
<ViewExtensionDefinition>
  <AssemblyPath>C:\Users\username\Documents\Visual Studio 2015\Projects\SampleViewExtension\SampleViewExtension\bin\Debug\SampleViewExtension.dll</AssemblyPath>
  <TypeName>SampleViewExtension.SampleViewExtension</TypeName>
</ViewExtensionDefinition>
```

* 在此示例中，我们已将程序集构建到默认的 Visual Studio 项目文件夹。将 `<AssemblyPath>...</AssemblyPath>` 目标替换为程序集的位置。

最后一步是将 `SampleViewExtension_ViewExtensionDefinition.xml` 文件复制到 Dynamo 的视图扩展文件夹，该文件夹位于 Dynamo Core 安装目录 `C:\Program Files\Dynamo\Dynamo Core\1.3\viewExtensions` 中。请务必注意，`extensions` 和 `viewExtensions` 有单独的文件夹。将 `xml` 文件放置在不正确的文件夹中可能会导致在运行时无法正确载入。

![XML 文件已复制到 Extensions 文件夹](images/fe-viewextension-xml.jpg)

> 1. 我们已复制到 Dynamo 的视图扩展文件夹中的 `.xml` 文件

这是对视图扩展的基本介绍。有关更复杂的案例研究，请参见 DynaShape 软件包，它是 Github 上的开源项目。该软件包使用支持在 Dynamo 模型视图中实时编辑的视图扩展。

DynaShape 的软件包安装程序可以从 Dynamo 论坛下载：[https://forum.dynamobim.com/t/dynashape-published/11666](https://forum.dynamobim.com/t/dynashape-published/11666)

源代码可以从 Github 克隆：[https://github.com/LongNguyenP/DynaShape](https://github.com/LongNguyenP/DynaShape)
