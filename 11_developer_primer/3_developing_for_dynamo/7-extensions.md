# 拡張機能

拡張機能は、Dynamo エコシステムの強力な開発ツールです。開発者は、Dynamo の相互作用とロジックに基づいてカスタム機能を実行することができます。拡張機能は、2 つの主なカテゴリ、拡張機能とビュー拡張機能に分けることができます。名前が示すように、ビューの拡張機能フレームワークでは、カスタム メニュー項目を登録することで Dynamo UI を拡張することができます。通常の拡張機能は、UI を除いてビュー拡張機能と非常によく似た動作をします。たとえば、特定の情報を Dynamo コンソールに記録する拡張機能を作成することができます。このシナリオの場合、カスタム UI は必要ないため、通常の拡張機能を使用して実現することができます。

#### 拡張機能のケース スタディ <a href="#extension-case-study" id="extension-case-study"></a>

DynamoSamples GitHub リポジトリの SampleViewExtension のサンプルに従って、グラフ内のアクティブなノードをリアルタイムで表示する、簡易なモードレス ウィンドウを作成するために必要な手順を説明します。ビュー拡張機能には、ウィンドウの UI を作成し、ビュー モデルに値をバインドする必要があります。

![ビュー拡張機能ウィンドウ](images/dyn-viewextension.jpg)

> 1. GitHub リポジトリの SampleViewExtension のサンプルに従って開発された、ビュー拡張機能ウィンドウ。

サンプルを最初から作成しますが、DynamoSamples リポジトリをダウンロードしてビルドすることで、参照として使用することもできます。

DynamoSamples リポジトリの場所: [https://github.com/DynamoDS/DynamoSamples](https://github.com/DynamoDS/DynamoSamples)

> この手順では、`DynamoSamples/src/` にある SampleViewExtension という名前のプロジェクトを具体的に参照します。

#### ビュー拡張機能を実装するには <a href="#how-to-implement-a-view-extension" id="how-to-implement-a-view-extension"></a>

ビュー拡張機能には、次の 3 つの主要な部分があります。

* `IViewExtension` を実装する、クラスとビュー モデルを作成するクラスを含むアセンブリ
* 実行時に、このアセンブリを検索する場所と拡張子のタイプを Dynamo に通知する `.xml` ファイル
* グラフィックス表示にデータをバインドして、ウィンドウの外観を決定する `.xaml` ファイル

**1\.プロジェクトの構造を作成する**

まず、新規の `Class Library` プロジェクトを `SampleViewExtension` という名前で作成します。

![新しいクラス ライブラリを作成する](images/vs-new-project-viewextension-1.jpg)

![新しいプロジェクトを設定する](images/vs-new-project-viewextension-2.jpg)

> 1. `File > New > Project` を選択して新しいプロジェクトを作成します。
> 2. `Class Library` を選択します。
> 3. プロジェクトに `SampleViewExtension` という名前を付けます。
> 4. `Ok` を選択します。

このプロジェクトでは、2 つのクラスが必要です。1 つのクラスが `IViewExtension` を実装し、別のクラスは `NotificationObject.` `IViewExtension` を実装して、拡張機能がどのように配置、ロード、参照、および破棄するか、すべての情報を含めます。`NotificationObject` は、Dynamo と `IDisposable` での変更に関する通知を提供します。変更が発生すると、それに応じてカウントが更新されます。

![ビュー拡張機能クラス ファイル](images/vs-viewextension-classes.jpg)

> 1. `SampleViewExtension.cs` というクラス ファイルは `IViewExtension` を実装します。
> 2. `SampleWindowViewMode.cs` というクラス ファイルは `NotificationObject` を実装します。

`IViewExtension` を使用するには WpfUILibrary NuGet パッケージが必要です。このパッケージをインストールすると、Core、Services、および ZeroTouchLibrary パッケージが自動的にインストールされます。

![ビュー拡張機能パッケージ](images/vs-viewextension-packages.jpg)

> 1. WpfUILibrary を選択します。
> 2. `Install` を選択して、従属パッケージをすべてインストールします。

**2\.IViewExtension クラスを実装する**

`IViewExtension` クラスから、Dynamo の起動時、拡張機能のロード時、 Dynamo のシャットダウン時に何が起きるかを決定します。`SampleViewExtension.cs` クラス ファイルに、次のコードを追加します。

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

`SampleViewExtension` クラスは、ウィンドウを開くクリック可能なメニュー項目を作成し、ビュー モデルとウィンドウに接続します。

* `public class SampleViewExtension : IViewExtension` `SampleViewExtension` は `IViewExtension` インタフェースから継承され、メニュー項目の作成に必要なものがすべて提供されます。
* `sampleMenuItem = new MenuItem { Header = "Show View Extension Sample Window" };` は MenuItem を作成し、`View` メニューに追加します。

![メニュー項目](images/dyn-menuitem.jpg)

> 1. メニュー項目

* `sampleMenuItem.Click += (sender, args)` は、メニュー項目をクリックしたときに、新しいウィンドウを開くイベントが発生します。
* `MainGrid = { DataContext = viewModel }` は、これから作成する `.xaml` ファイル内の `Main Grid` を参照して、ウィンドウ内のメイン グリッドのためのデータ コンテキストを設定します。
* `Owner = p.DynamoWindow` は、ポップアウト ウィンドウの所有者を Dynamo に設定します。つまり、新しいウィンドウは Dynamo に依存しているため、Dynamo の最小化、最大化、復元などの操作を行うと、新しいウィンドウでも同じ動作が実行されます。
* `window.Show();` は、追加のウィンドウ プロパティが設定されたウィンドウを表示します。

**3\.ビュー モデルを実装する**

ウィンドウの基本パラメータをいくつか設定することができました。次に、さまざまな Dynamo 関連のイベントに応答するためのロジックを追加し、これらのイベントに基づいて UI を更新するように指示します。次のコードを、`SampleWindowViewModel.cs` クラス ファイルにコピーします。

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

このビュー モデル クラスの実装は、`CurrentWorkspaceModel` を確認し、ワークスペースにノードが追加または削除されたときにイベントを発生させます。これにより、データが変更され、更新が必要であることを UI またはバインド要素に通知するプロパティの変更が発生します。`ActiveNodeTypes` ゲッターを呼び出すことで、内部で追加のヘルパー関数 `getNodeTypes()` が呼び出されます。この関数は、キャンバス上のすべてのアクティブなノードを反復処理し、これらのノードの名前を含む文字列を生成します。次にこの文字列を、ポップアウト ウィンドウに表示される .xaml ファイル内のバインドに返します。

拡張機能のコア ロジックを定義したため、ウィンドウの外観の詳細を `.xaml` ファイルで指定できるようになりました。単に必要なのは、`TextBlock` `Text` にバインドされている `ActiveNodeTypes` プロパティを介して文字列を表示する、簡易なウィンドウです。

![ウィンドウを追加する](images/vs-window.jpg)

> 1. プロジェクトを右クリックして次を選択します: `Add > New Item...`
> 2. ウィンドウを作成するために変更するユーザ コントロール テンプレートを選択します。
> 3. 新しいファイル名を `SampleWindow.xaml` にします。
> 4. `Add` を選択します。

ウィンドウの `.xaml` コードでは、テキスト ブロックに `SelectedNodesText` をバインドする必要があります。`SampleWindow.xaml` に、次のコードを追加します。

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

* `Text="{Binding ActiveNodeTypes}"` は、`SampleWindowViewModel.cs` 内のプロパティ値 `ActiveNodeTypes` に対して、ウィンドウの `TextBlock` `Text` 値をバインドします。

ここで、.xaml C# バッキング ファイル `SampleWindow.xaml.cs` のサンプル ウィンドウを初期化します。`SampleWindow.xaml` に、次のコードを追加します。

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

ビュー拡張機能は、構築して Dynamo に追加する準備が整いました。Dynamo には、`xml` ファイルが必要です。出力を `.dll` 拡張子として登録するためのファイルです。

![新しい XML を追加する](images/vs-viewextension-xml.jpg)

> 1. プロジェクトを右クリックして次を選択します: `Add > New Item...`
> 2. XML ファイルを選択します。
> 3. ファイル名を `SampleViewExtension_ViewExtensionDefinition.xml` にします。
> 4. `Add` を選択します。

* ファイル名は、Dynamo 標準に従って拡張アセンブリを参照します。 `"extensionName"_ViewExtensionDefinition.xml`

`xml` ファイルに次のコードを追加して、Dynamo に拡張アセンブリの検索場所を指示します。

```
<ViewExtensionDefinition>
  <AssemblyPath>C:\Users\username\Documents\Visual Studio 2015\Projects\SampleViewExtension\SampleViewExtension\bin\Debug\SampleViewExtension.dll</AssemblyPath>
  <TypeName>SampleViewExtension.SampleViewExtension</TypeName>
</ViewExtensionDefinition>
```

* この例では、アセンブリを既定の Visual Studio プロジェクト フォルダに作成しました。`<AssemblyPath>...</AssemblyPath>` ターゲットをアセンブリの場所に置き換えます。

最後の手順では、`SampleViewExtension_ViewExtensionDefinition.xml` ファイルを Dynamo の Core インストール フォルダ `C:\Program Files\Dynamo\Dynamo Core\1.3\viewExtensions` にある Dynamo のビュー拡張機能フォルダにコピーします。`extensions` と `viewExtensions` には別々のフォルダが存在することに注意が必要です。`xml` ファイルを誤ったフォルダに配置すると、実行時に正しくロードされない可能性があります。

![拡張機能フォルダにコピーされた XML ファイル](images/fe-viewextension-xml.jpg)

> 1. Dynamo のビュー拡張機能フォルダにコピーされた `.xml` ファイル

ここではビュー拡張機能の基本的な概要について紹介しました。より高度なケース スタディについては、GitHub のオープン ソース プロジェクトである DynaShape パッケージを参照してください。このパッケージでは、Dynamo モデル ビューでライブ編集を可能にするビュー拡張機能を使用しています。

DynaShape のパッケージ インストーラは、Dynamo フォーラムからダウンロードできます: [https://forum.dynamobim.com/t/dynashape-published/11666](https://forum.dynamobim.com/t/dynashape-published/11666)

ソース コードは、GitHub からクローンで作成することができます: [https://github.com/LongNguyenP/DynaShape](https://github.com/LongNguyenP/DynaShape)
