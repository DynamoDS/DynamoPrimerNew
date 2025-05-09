# Dynamo の統合

こちらはビジュアル プログラミング言語 Dynamo の統合に関するドキュメントです。

このガイドでは、ユーザがビジュアル プログラミングを使用してアプリケーションを操作できるように、アプリケーションで Dynamo をホストする方法のさまざまな側面について説明します。

目次:

* [この概要](13-dynamo-integration.md#dynamo-integration): このガイドの内容と Dynamo の概要について説明します。
* [Dynamo のカスタム エントリ ポイント](13-dynamo-integration.md#dynamo-custom-entry-point): DynamoModel の作成方法と開始手順について説明します。
* [要素のバインドとトレース](13-dynamo-integration.md#-element-binding-and-trace): Dynamo のトレース メカニズムを使用して、グラフ内のノードをホスト内の結果にバインドします。
* [Dynamo Revit 選択ノード](13-dynamo-integration.md#-dynamo-revit-selection-nodes): ホストからオブジェクトまたはデータを選択し、それらを Dynamo グラフへの入力として渡すことができるノードを実装する方法について説明します。
* [Dynamo 組み込みパッケージの概要](13-dynamo-integration.md#dynamo-built-in-packages-overview): Dynamo 標準ライブラリとは何か、および基盤となるメカニズムを使用して統合にパッケージを出荷する方法について説明します。

**いくつかの辞書:**

これらのドキュメントでは、ユーザが Dynamo で作成するコードを指す「Dynamo スクリプト」、「グラフ」、「プログラム」という用語を同じ意味で使用します。

## Dynamo カスタム エントリ ポイント

#### Dynamo Revit の例

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

`DynamoModel` は、Dynamo をホストするアプリケーションのエントリ ポイントで、Dynamo アプリケーションを示しています。モデルは、Dynamo アプリケーションおよび DesignScript 仮想マシンを構成する他の重要なデータ構造やオブジェクトへの参照を含む、最上位のルート オブジェクトです。

コンフィギュレーション オブジェクトは、構成時に `DynamoModel` の共通パラメータセットを設定するために使用されます。

このドキュメントの例は、Revit がアドインとして `DynamoModel` をホストする統合である DynamoRevit の実装から取られたものです。(Revit のプラグイン アーキテクチャ)このアドインをロードすると、`DynamoModel` が起動し、`DynamoView` と `DynamoViewModel` と共にユーザに表示されます。

Dynamo は C# .net プロジェクトであり、処理中にアプリケーションで使用するには、.net コードをホストして実行する必要があります。

DynamoCore はクロス プラットフォームの計算エンジンであり、.net または Mono (将来的には .net core)で構築できるコア モデルのコレクションです。ただし、DynamoCoreWPF には Dynamo の Windows 専用 UI コンポーネントが含まれており、他のプラットフォームではコンパイルできません。

### Dynamo エントリ ポイントをカスタマイズする手順

`DynamoModel` を初期化するには、インテグレータがホストのコードのいずれかの場所でこれらの手順を実行する必要があります。

### 共有されている Dynamo Dll をホストから事前ロードします。

現在、D4R のリストには `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.` のみ含まれています。これは、Dynamo と Revit 間のライブラリ バージョンの競合を回避するために行われます。例:`AvalonEdit` で競合が発生すると、コード ブロックの関数が完全に壊れる可能性があります。指摘事項は、Dynamo 1.1.x の https://github.com/DynamoDS/Dynamo/issues/7130 で、手動で再現することもできます。インテグレータがホスト関数と Dynamo の間でライブラリの競合に気付いた場合は、最初の手順としてこれを実施することをお勧めします。これは、他のプラグインまたはホスト アプリケーション自体が互換性のないバージョンを共有依存関係としてロードしないようにするために必要になる場合があります。適切な解決策は、バージョンを揃えてバージョンの競合を解決するか、可能であればホストの app.config で .net バインディング リダイレクトを使用することです。

### ASM をロードする

#### ASM と LibG とは

ASM は、Dynamo の基盤となる ADSK ジオメトリ ライブラリです。

LibG は、ASM ジオメトリ カーネルの .Net ユーザ フレンドリーなラッパーです。libG は ASM とバージョン管理スキームを共有しています。つまり、ASM の同じメジャー バージョン番号とマイナー バージョン番号を使用して、特定の ASM バージョンの対応するラッパーであることを示しています。ASM バージョンを指定すると、対応する libG バージョンも同じになります。ほとんどの場合、LibG は特定のメジャー バージョンの ASM の全バージョンで動作します。たとえば、LibG 223 は任意の ASM 223 のバージョンをロードできるはずです。

#### Dynamo Sandbox で ASM をロードする

Dynamo Sandbox は、複数の ASM バージョンで動作するように設計されており、これを実現するために、複数の libG バージョンがバンドルされ、コアとともに出荷されています。Dynamo Shape Manager には、ASM に付属しているオートデスク製品を検索する機能が組み込まれているため、Dynamo ではこれらの製品から ASM をロードして、ホスト アプリケーションに明示的にロードすることなくジオメトリ ノードを動作させることができます。現在存在する製品リストは次のとおりです。

```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```

Dynamo は、Windows レジストリを検索して、このリスト内のオートデスク製品がユーザのマシンにインストールされているかどうかを検索します。これらの製品のいずれかがインストールされている場合は、ASM バイナリを検索してバージョンを取得し、Dynamo 内の対応する libG バージョンを検索します。

ASM のバージョンを指定すると、次の ShapeManager API によって、ロードする libG プリローダの場所が選択されます。完全に一致するバージョンがある場合はそのバージョンが使用され、それ以外の場合は、以下の最も近いバージョン番号の libG でメジャー バージョンが同じバージョンがロードされます。

例:Dynamo が Revit 開発ビルドと統合されており、新しい ASM ビルド 225.3.0 がある場合、Dynamo は libG 225.3.0 が存在する場合はそれの使用を試みます。存在しない場合は、最初の選択肢より前の最も近いメジャー バージョン(225.0.0)の使用を試みます。

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)`

#### Dynamo のインプロセス統合でホストから ASM をロードする

Revit は、ASM 製品検索リストの最初のエントリです。つまり、既定では、`DynamoSandbox.exe` は最初に Revit から ASM をロードしようとしますが、統合された D4R ワーキング セッションでも現在の Revit ホストから ASM をロードされるようにする必要があります。たとえば、ユーザのコンピュータに R2018 と R2020 の両方がある場合、R2020 から D4R を起動すると、D4R が R2018 の ASM 223 ではなく、R2020 の ASM 225 を使用するようにします。インテグレータは、指定されたバージョンを強制的にロードするよう、次と同様の呼び出しを実装する必要があります。

```
internal static Version PreloadAsmFromRevit() 

{ 

     var asmLocation = AppDomain.CurrentDomain.BaseDirectory; 
     Version libGVersion = findRevitASMVersion(asmLocation); 
     var dynCorePath = DynamoRevitApp.DynamoCorePath; 
     var preloaderLocation = DynamoShapeManager.Utilities.GetLibGPreloaderLocation(libGVersion, dynCorePath); 
     Version preLoadLibGVersion = PreloadLibGVersion(preloaderLocation); 
     DynamoShapeManager.Utilities.PreloadAsmFromPath(preloaderLocation, asmLocation); 
     return preLoadLibGVersion; 

} 
```

#### Dynamo でカスタマイズ パスから ASM をロードする

最近、`DynamoSandbox.exe` と `DynamoCLI.exe` で特定の ASM バージョンをロードする機能を追加しました。通常のレジストリ検索動作をスキップするには、`�gp` フラグを使用して、強制的に Dynamo が特定のパスから ASM をロードするようにします。

`DynamoSandbox.exe -gp �somePath/To/ASMDirectory/�`

### StartConfiguration を作成する

StartupConfiguration は、DynamoModel を初期化するためのパラメータとして渡すために使用されます。このパラメータは、Dynamo セッション設定のカスタマイズ方法に関するほぼすべての定義が含まれていることを示します。次に示すプロパティの設定方法により、Dynamo の統合がインテグレータ間で異なる場合があります。たとえば、インテグレータが異なれば、表示される Python テンプレート パスや数値形式の設定も異なる可能性があります。

このファイルの構成は、次のとおりです。

* DynamoCorePath // ロードする DynamoCore バイナリの配置場所
* DynamoHostPath // Dynamo 統合バイナリの配置場所
* GeometryFactoryPath // ロードされた libG バイナリの配置場所
* PathResolver //さまざまなファイルの解決に役立つオブジェクト
* PreloadLibraryPaths // プリロードされたノード バイナリの配置場所(例: DSOffice.dll)
* AdditionalNodeDirectories // 追加のノード バイナリの配置場所
* AdditionalResolutionPaths // ライブラリのロード中に必要になる可能性がある、その他の依存関係の追加のアセンブリ解決処理パス
* UserDataRootFolder // ユーザのデータ フォルダ、例: `"AppData\Roaming\Dynamo\Dynamo Revit"`
* CommonDataRootFolder // カスタム定義、サンプルなどを保存する既定フォルダ
* Context // インテグレータのホスト名 + バージョン `(Revit<BuildNum>)`
* SchedulerThread // `ISchedulerThread` を実装する、インテグレータ スケジューラのスレッド。ほとんどのインテグレータの場合、これはメイン UI スレッドか、API にアクセスできる任意のスレッドになります。
* StartInTestMode // 現在のセッションがテストの自動化セッションであるかどうか。一連の Dynamo の動作を変更します。テストを記述していない限りは使用しないでください。
* AuthProvider // インテグレータによる IAuthProvider の実装(例: RevitOxygenProvider の実装の場合は Greg.dll の中)。packageManager アップロードの統合に使用されます。

### 基本設定

既定の基本設定の設定パスは、`PathManager.PreferenceFilePath` によって管理されます(例: `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`)。インテグレータは、カスタマイズされた基本設定の設定ファイルについても、パス マネージャと整合させる必要がある場所に送るかどうかを決定できます。以下は、シリアル化される基本設定の設定プロパティです。

* IsFirstRun // このバージョンの Dynamo を初めて実行するかどうかを示します(例: GA オプトイン/オプトアウトのメッセージを表示する必要があるかどうかを決定するために使用)。また、新しいバージョンの Dynamo の起動時に旧式の Dynamo 基本設定を移行する必要があるかどうかを決定するためにも使用され、ユーザのエクスペリエンスに一貫性を持たせることができます。
* IsUsageReportingApproved // 使用状況レポートが承認されているかどうかを示します
* IsAnalyticsReportingApproved // 分析レポートが承認されているかどうかを示します
* LibraryWidth // Dynamo 左側のライブラリ パネルの幅。
* ConsoleHeight // コンソール表示の高さ。
* ShowPreviewBubbles // プレビュー バルーンを表示するかどうかを示します
* ShowConnector // コネクタを表示するかどうかを示します
* ConnectorType // コネクタのタイプ(ベジェまたはポリライン)を示します
* BackgroundPreviews // 指定した背景プレビューのアクティブ状態を示します
* RenderPrecision // レンダリング精度のレベル。低い場合、三角形の数が少ないメッシュが生成されます。高い場合は、背景プレビューでより滑らかなジオメトリが生成されます。ジオメトリを素早くプレビューするには、128 が適した数値です。
* ShowEdges // サーフェスとソリッドのエッジをレンダリングするかどうかを示します
* ShowDetailedLayout // 未使用
* WindowX, WindowY // Dynamo ウィンドウの最後の X 座標と Y 座標
* WindowW, WindowH // Dynamo ウィンドウの最後の幅と高さ
* UseHardwareAcceleration // Dynamo でハードウェア アクセラレーションがサポートされている場合、それを使用するかどうかを示します
* NumberFormat // プレビュー バブル toString() に数値を表示するために使用される小数点の精度。
* MaxNumRecentFiles // 保存する最近使用したファイル パスの最大数
* RecentFiles // 最近開いたファイル パスのリスト。このリストを操作すると、Dynamo の起動ページにある最近使用したファイルのリストに直接影響します
* BackupFiles // バックアップ ファイル パスのリスト
* CustomPackageFolders // パッケージとカスタム ノードがスキャンされる Zero-Touch バイナリとディレクトリ パスを含むフォルダのリスト。
* PackageDirectoriesToUninstall // Package Manager が、削除対象としてマークされているパッケージを決定するために使用するパッケージの一覧。これらのパスは、可能な限り Dynamo の起動時に削除されます。
* PythonTemplateFilePath // 新しい PythonScript ノードの作成時に開始テンプレートとして使用する Python(.py)ファイルへのパス。これを使用して、統合用のカスタム Python テンプレートをセットアップできます。
* BackupInterval // グラフを自動的に保存する期間(ミリ秒単位)を示します
* BackupFilesCount // 作成されるバックアップの数を示します
* PackageDownloadTouAccepted // ユーザが Package Manager からパッケージをダウンロードするための利用規約に同意したかどうかを示します
* OpenFileInManualExecutionMode // OpenFileDialog の[手動モードで開く]チェックボックスの既定の状態を示します
* NamespacesToExcludeFromLibrary // Dynamo ノード ライブラリに表示しない名前空間(存在する場合)を示します。文字列形式: "[ライブラリ名]:[完全な名前空間]"

シリアル化された基本設定の例:

```xml
<PreferenceSettings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> 

<IsFirstRun>false</IsFirstRun> 

<IsUsageReportingApproved>false</IsUsageReportingApproved> 

<IsAnalyticsReportingApproved>false</IsAnalyticsReportingApproved> 

<LibraryWidth>204</LibraryWidth> 

<ConsoleHeight>0</ConsoleHeight> 

<ShowPreviewBubbles>true</ShowPreviewBubbles> 

<ShowConnector>true</ShowConnector> 

<ConnectorType>BEZIER</ConnectorType> 

<BackgroundPreviews> 

<BackgroundPreviewActiveState> 

<Name>IsBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

<BackgroundPreviewActiveState> 

<Name>IsRevitBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

</BackgroundPreviews> 

<IsBackgroundGridVisible>true</IsBackgroundGridVisible> 

<RenderPrecision>128</RenderPrecision> 

<ShowEdges>false</ShowEdges> 

<ShowDetailedLayout>true</ShowDetailedLayout> 

<WindowX>553</WindowX> 

<WindowY>199</WindowY> 

<WindowW>800</WindowW> 

<WindowH>676</WindowH> 

<UseHardwareAcceleration>true</UseHardwareAcceleration> 

<NumberFormat>f3</NumberFormat> 

<MaxNumRecentFiles>10</MaxNumRecentFiles> 

<RecentFiles> 

<string></string> 

</RecentFiles> 

<BackupFiles> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\backup\backup.DYN</string> 

</BackupFiles> 

<CustomPackageFolders> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\2.5</string> 

</CustomPackageFolders> 

<PackageDirectoriesToUninstall /> 

<PythonTemplateFilePath /> 

<BackupInterval>60000</BackupInterval> 

<BackupFilesCount>1</BackupFilesCount> 

<PackageDownloadTouAccepted>true</PackageDownloadTouAccepted> 

<OpenFileInManualExecutionMode>false</OpenFileInManualExecutionMode> 

<NamespacesToExcludeFromLibrary> 

<string>ProtoGeometry.dll:Autodesk.DesignScript.Geometry.TSpline</string> 

</NamespacesToExcludeFromLibrary> 

</PreferenceSettings> 
```

* Extensions // IExtension を実装する拡張機能のリスト。NULL の場合、Dynamo は既定のパス(Dynamo フォルダ下の `extensions` フォルダ)から拡張機能をロードします
* IsHeadless // Dynamo が UI なしで起動されたかどうかを示します。これは解析に影響します。
* UpdateManager // UpdateManager のインテグレータによる実装。上記説明を参照
* ProcessMode // TaskProcessMode と同様。テスト モードの場合は同期、それ以外の場合は非同期になります。これはスケジューラの動作をコントロールします。シングル スレッド環境では、これを同期に設定することもできます。

ターゲットの StartConfiguration を使用して、`DynamoModel` を起動します。

StartConfig が渡されて `DynamoModel` が起動すると、DynamoCore は実際の詳細を監視して、Dynamo セッションが指定された詳細情報で正しく初期化されるようにします。`DynamoModel` 初期化後、各インテグレータは、セットアップ後の手順をいくつか実行する必要があります。たとえば D4R では、Revit ホスト トランザクションやドキュメントの更新、Python ノードのカスタマイズなどを監視するためにイベントがサブスクライブされます。

### 「ビジュアル プログラミング」の部分に進みましょう

`DynamoViewModel` と `DynamoView` を初期化するには、まず `DynamoViewModel` を構築する必要があります。これは `DynamoViewModel.Start` 静的メソッドを使用して実行できます。下記を参照してください。

```c#

    viewModel = DynamoViewModel.Start(
                    new DynamoViewModel.StartConfiguration()
                    {
                        CommandFilePath = commandFilePath,
                        DynamoModel = model,
                        Watch3DViewModel = 
                            HelixWatch3DViewModel.TryCreateHelixWatch3DViewModel(
                                null,
                                new Watch3DViewModelStartupParams(model), 
                                model.Logger),
                        ShowLogin = true
                    });
     
     var view = new DynamoView(viewModel);

```

`DynamoViewModel.StartConfiguration` のオプションは、モデルの設定よりもはるかに少なくなっています。ほとんどが読むと意味を理解できます。テスト ケースを記述している場合を除き、`CommandFilePath` は無視できます。

`Watch3DViewModel` パラメータは、背景、プレビュー、および watch3d ノードが 3D ジオメトリをどのように表示するかをコントロールします。必要なインタフェースを実装する場合は、独自の実装を使用できます。

`DynamoView` を構築するには、`DynamoViewModel` のみが必要です。ビューはウィンドウ コントロールです。WPF を使用して表示できます。

### DynamoSandbox.exe の例:

DynamoSandbox.exe は、DynamoCore を使用してテスト、使用、実験を行うための開発環境です。これは、`DynamoCore` および `DynamoCoreWPF` コンポーネントがどのようにロードおよびセットアップされるかを確認するためのチェックアウトの最適な例です。エントリ ポイントの一部は、[こちら](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37)で確認できます。

## 要素のバインドとトレース

#### 概要

_トレース_は Dynamo コアのメカニズムで、データを .dyn (Dynamo ファイル)にシリアル化することができます。重要なのは、このデータが Dynamo グラフ内のノードの呼び出しサイトにキー設定されることです。

Dynamo グラフをディスクから開くと、ディスクに保存されたトレース データがグラフのノードに再度関連付けられます。

#### 用語集:

* トレース メカニズム:
  * Dynamo で要素のバインドを実装します
  * トレース メカニズムを使用すると、オブジェクトが作成したジオメトリに確実に再バインドされるようにすることができます
  * Callsite およびトレース メカニズム ハンドルは、ノード実装者が再リンクに使用できる永続的な GUID を提供します
* Callsite
  * 実行可能ファイルには複数の Callsite が含まれています。これらの Callsite は、ディスパッチする必要があるさまざまな場所に実行をディスパッチするために使用されます。
    * C# ライブラリ
    * 組み込みメソッド
    * DesignScript 関数
    * カスタム ノード(DS 関数)
* TraceSerializer
  * `ISerializable` およびマークされたクラス `[Serializable]` をトレースにシリアル化します。
  * トレースへのデータのシリアル化と逆シリアル化を処理します。
  * TraceBinder は、逆シリアル化されたデータをランタイム型にバインドすることをコントロールします。(リアル クラスのインスタンスを作成します)

#### それはどのようなものか。

***

トレース データは、Bindings というプロパティ内の .dyn ファイルにシリアル化されます。これは、callsites-ids -> データの配列です。Callsite は、DesignScript 仮想マシン内でノードが呼び出される特定の場所およびインスタンスです。Dynamo グラフ内のノードは複数回呼び出される場合があるため、1 つのノード インスタンスに対して複数の Callsite が作成される可能性があることに注意してください。

```json
"Bindings": [
    {
      "NodeId": "1e83cc25-7de6-4a7c-a702-600b79aa194d",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_1e83cc25-7de6-4a7c-a702-600b79aa194d":  "Base64 Encoded Data"
      }
    },
    {
      "NodeId": "c69c7bec-d54b-4ead-aea8-a3f45bea9ab2",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_c69c7bec-d54b-4ead-aea8-a3f45bea9ab2": "Base64 Encoded Data"
      }
    }
  ],

 
```

シリアル化された base64 でエンコードされたデータの形式に依存することは_できません_。

#### どんな問題を解決しようとしているのか。

***

関数の実行の結果として任意のデータを保存する理由は多数ありますが、この場合のトレースでは、ユーザがホスト アプリケーションで要素を作成するソフトウェア プログラムを構築し、反復作業を実行する際に発生することが多い特定の問題を解決するために開発されました。

問題は、`Element Binding` と呼ばれる問題です。この概念は以下のようになります。

ユーザが Dynamo グラフを開発、実行すると、ホスト アプリケーション モデルに新しい要素が生成される可能性があります。例として、ユーザが建築モデルで 100 個のドアを生成する小さなプログラムを持っているとします。これらのドアの数と場所は、プログラムによってコントロールされます。

ユーザが初めてプログラムを実行すると、これらの 100 個のドアが生成されます。

後でユーザーがプログラムへの入力を変更して再実行すると、プログラムは_(要素のバインドなしで)_ 100 個の新しいドアを作成しますが、古いドアも新しいドアとともにモデルに存在し続けます。

***

Dynamo はライブ プログラミング 環境であり、グラフへの変更が新しい実行をトリガする `"Automatic"` 実行モードを備えているため、プログラムを多数実行した結果、モデルが急速に乱雑になる可能性があります。

これは通常ユーザが期待する挙動ではないことが判明したため、要素のバインドを有効にすると、グラフ実行の以前の結果がクリーンアップされ、削除または変更されるようになりました。どちらになるか(_削除されるか変更されるか_)は、ホストの API の柔軟性によって異なります。要素のバインドを有効にすると、ユーザが Dynamo プログラムを何度実行しても、モデル内のドアは 100 個のみになります。

この場合、単にデータを .dyn ファイルへシリアル化できるというだけでは不十分です。次に示すように、DynamoRevit には、トレース上に構築された、これらの再バインド ワークフローをサポートするメカニズムがあります。

***

この機会に、Revit などのホストに対する要素のバインドに関するその他の重要な使用例について説明します。要素のバインドを有効にしたときに作成された要素は、既存の要素 ID を保持しようとする(既存の要素を修正する)ため、ホスト アプリケーションでこれらの要素の上に構築されたロジックは、Dynamo プログラム実行後も存在し続けます。例:

建築モデルの例に戻りましょう。

まず、要素のバインドを無効にした例を見てみましょう。このユーザには、意匠壁を生成するプログラムがあります。

プログラムを実行すると、ホスト アプリケーション内にいくつかの壁が生成されます。次に、Dynamo グラフを終了し、通常の Revit ツールを使用してそれらの壁に窓をいくつか配置します。窓は、Revit モデルの一部としてこれらの特定の壁にバインドされます。

ユーザは再度 Dynamo を起動してグラフを再実行します。前回の例と同様に、2 組の壁が作成されます。最初のセットには窓が追加されていますが、新しい壁には窓が追加されていません。

要素のバインドが有効になっている場合、Dynamo を使用しなくても、ホスト アプリケーションで手動で行った既存の作業を保持することができます。たとえば、ユーザが 2 回目にプログラムを実行したときにバインドを有効にした場合、壁は削除されずに修正され、下流のホスト アプリケーションで行われた変更が保持されます。モデルには、状態が異なる 2 つの壁セットではなく、窓のある壁が含まれます。

***

![壁を作成](images/creates_walls.png)

#### トレースと要素バインドの比較

***

トレースは Dynamo コアのメカニズムです。このメカニズムは、前述のように、Callsite からデータへの静的変数を使用して、グラフ内の関数の Callsite にデータをマッピングします。

また、Zero-touch Dynamo ノードの作成時に、任意のデータを .dyn ファイルにシリアル化することもできます。これは、転送の可能性がある Zero-Touch コードが Dynamo Core への依存性を持つことを意味するため、あまりお勧めしません。

.dyn ファイル内のデータのシリアル化された形式に依存しないよう、代わりに[Serializable]属性とインタフェースを使用します。

一方、ElementBinding はトレース API 上で構築され、Dynamo の統合_(DynamoRevit、Dynamo4Civil など)_に実装されます。

#### トレース API

知っておく価値のある低レベルのトレース API には、次のようなものがあります。

```c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

これらの使用例を以下の例で確認できます

Dynamo が既存のファイルからロードしたトレース データや生成しているトレース データを操作するには、次の項目を参照してください。

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```

[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)

#### ノードからの単純なトレースの例

***

トレースを直接使用する Dynamo ノードの例については、こちらの [DynamoSamples リポジトリ](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs)を参照してください

そのクラスの概要では、トレースが何であるかに関する要点を説明しています。

```
  /*
     * After a graph update, Dynamo typically disposes of all
     * objects created during the graph update. But what if there are 
     * objects which are expensive to re-create, or which have other
     * associations in a host application? You wouldn't want those those objects
     * re-created on every graph update. For example, you might 
     * have an external database whose records contain data which needs
     * to be re-applied to an object when it is created in Dynamo.
     * In this example, we use a wrapper class, TraceExampleWrapper, to create 
     * TraceExampleItem objects which are stored in a static dictionary 
     * (they could be stored in a database as well). On subsequent graph updates, 
     * the objects will be retrieved from the data store using a trace id stored 
     * in the trace cache.
     */
```

この例では、DynamoCore のトレース API を直接使用して、特定のノードが実行されるたびにデータを保存します。この場合、ディクショナリは、Revit のモデル データベースのようなホスト アプリケーション モデルの役割を果たします。

大まかな設定は次のとおりです。

静的な util クラス `TraceExampleWrapper` は、Dynamo にノードとして読み込まれます。これには、`TraceExampleItem` を作成する単一のメソッド `ByString` が含まれています。これらは、`description` プロパティを含む通常の .net オブジェクトです。

各 `TraceExampleItem` はトレースにシリアル化され、`TraceableId` として表されます。これは、`[Serializeable]` とマークされた `IntId` を含むクラスなので、`SOAP` フォーマッタでシリアル化できます。[Serializable 属性の詳細については、こちら](https://docs.microsoft.com/ja-jp/dotnet/api/system.serializableattribute?view=netframework-4.8)を参照してください

また、[こちら](https://docs.microsoft.com/ja-jp/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8)で定義されている `ISerializable` インタフェースも実装する必要があります。

```c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

このクラスは、トレースに保存する `TraceExampleItem` ごとに作成およびシリアル化され、base64 でエンコードされて、グラフ保存時にディスクに保存されます。これにより、後で既存の要素ディクショナリの上でグラフを再度開いたときにバインドを再度関連付けることができます。この例の場合、ディクショナリは Revit ドキュメントのように実際には永続的ではないため、これはうまく機能しません。

計算式の最後の部分は `TraceableObjectManager` です。これは `DynamoRevit` の `ElementBinder` に似ています。ホストのドキュメント モデルに存在するオブジェクトと Dynamo トレースに格納されているデータとの関係を管理するものです。

ユーザが `TraceExampleWrapper.ByString` ノードを含むグラフを初めて実行する際、新しい ID で新しい `TraceableId` が作成されると、`TraceExampleItem` はその新しい ID にマッピングされたディクショナリに保存され、トレースに `TraceableID` が格納されます。

グラフの次の実行では、トレースを確認して、そこに格納した ID と、その ID にマップされたオブジェクトを見つけて、そのオブジェクトを返します。新しいオブジェクトを作成する代わりに、既存のオブジェクトを変更します。

1 つの `TraceExampleItem` を作成するグラフを 2 回連続して実行するフローは次のようになります。

![1 回目の呼び出し](images/Trace-first-call.png)

![2 回目の呼び出し](images/Trace-second-call.png)

同じ考え方を、より現実的な DynamoRevit ノードの使用例である次の例に示します。

#### トレース図

![ステップをトレース](images/trace_diagram.png) ![フローをトレース](images/trace_alt_diagram.png)

#### 注:

Dynamo の最新バージョンでは、TLS (スレッド ローカル ストレージ)の使用は静的メンバの使用に置き換えられました。

#### 要素バインディングの実装例

***

要素のバインドを使用するノードが DynamoRevit 向けに実装されるとどのように見えるかをざっと見てみましょう。これは、上記の壁の作成例で使用したノードのタイプと似ています。

***

```c#
    private void InitWall(Curve curve, Autodesk.Revit.DB.WallType wallType, Autodesk.Revit.DB.Level baseLevel, double height, double offset, bool flip, bool isStructural)
        {
            // This creates a new wall and deletes the old one
            TransactionManager.Instance.EnsureInTransaction(Document);

            //Phase 1 - Check to see if the object exists and should be rebound
            var wallElem =
                ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>(Document);

            bool successfullyUsedExistingWall = false;
            //There was a modelcurve, try and set sketch plane
            // if you can't, rebuild 
            if (wallElem != null && wallElem.Location is Autodesk.Revit.DB.LocationCurve)
            {
                var wallLocation = wallElem.Location as Autodesk.Revit.DB.LocationCurve;
                <SNIP>

                    if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;

                  <SNIP>
                
            }

            var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
            InternalSetWall(wall);

            TransactionManager.Instance.TransactionTaskDone();

            // delete the element stored in trace and add this new one
            ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
        }
```

上のコードは、壁要素のサンプル コンストラクタを示しています。このコンストラクタは、`Wall.byParams` などの Dynamo のノードから呼び出されます。

コンストラクタの実行の重要なフェーズは、要素のバインディングに関連しています。

1. `elementBinder` を使用して、過去の実行でこの Callsite にバインドされた以前に作成済みのオブジェクトがあるかどうかを確認します。`ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. ある場合は、新しい壁を作成するのではなく、その壁を修正してみてください。

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. ない場合は、新しい壁を作成します。

```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. トレースから取得したばかりの古い要素を削除し、新しい要素を追加して、将来的にこの要素を検索できるようにします。

```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### 詳解

#### 効率性

* 現在、それぞれのシリアル化トレース オブジェクトは、SOAP xml 形式を使用してシリアル化されています。これは非常に冗長で、多くの情報が重複しています。次に、データは base64 で 2 回エンコードされます。これは、シリアル化または逆シリアル化の点で効率的ではありません。これは、内部形式が上に構築されない場合、将来的に改善される可能性があります。繰り返しになりますが、シリアル化された保存データの形式に依存しないようにしてください。

#### ElementBinding は既定でオンにする必要がありますか?

* 使用事例によっては、要素のバインドが望ましくない場合もあります。上級 Dynamo ユーザが、ランダムなグループ要素を生成するため、複数回実行の必要があるプログラムを開発している場合はどうなるでしょうか。このプログラムの目的は、プログラム実行のたびに追加の要素を作成することです。この使用事例は、要素のバインドが機能しないようにする回避策がなければ簡単に実現できません。統合レベルで elementBinding を無効にすることは可能ですが、これは Dynamo のコア機能とする必要があるでしょう。この機能の粒度をどこまで細かくすべきか(ノード レベル Callsite レベル、Dynamo セッション全体、ワークスペース等)が明確ではないためです。

## Dynamo Revit 選択ノード(その詳細)

一般に、これらのノードを使用することで、ユーザは参照先にするアクティブな Revit ドキュメントのサブセットを何らかの形で記述できます。ユーザにより、Revit 要素を参照する方法はさまざまです(以下で説明します)。また、ノードの結果の出力は、Revit 要素のラッパー(DynamoRevit ラッパー)または Dynamo ジオメトリ(Revit ジオメトリから変換)になります。この出力タイプの違いは、他のホスト統合のコンテキストを考慮する場合に役立ちます。

大まかに言うと、**これらのノードは、要素 ID を受け取り、その要素またはその要素を表すジオメトリへのポインタを返す関数として概念化することができます。**

DynamoRevit には複数の `�Selection�` ノードがあります。これは少なくとも 2 つのグループに分けることができます。

![Revit 選択ノード](images/revitSelectionNodes.png)

1.  ユーザ UI の選択:

    このカテゴリの `DynamoRevit` ノードの例は `SelectModelElement` と `SelectElementFace` です

    これらのノードを使用すると、ユーザは Revit UI コンテキストに切り替えて要素または要素のセットを選択できます。これらの要素の ID がキャプチャされ、変換関数が実行されて、ラッパーが作成されるかジオメトリが要素から抽出されて変換されます。実行される変換は、ユーザが選択したノードのタイプによって異なります。
2.  Document Query:

    このカテゴリのサンプル ノードは `AllElementsOfClass` と `AllElementsOfCategory` です

    これらのノードを使用すると、ユーザは要素のサブセットのドキュメント全体をクエリーできます。これらのノードは通常、基礎となる Revit 要素を指すラッパーを返します。これらのラッパーは DynamoRevit の環境に不可欠であり、要素のバインドなどの高度な機能を使用できます。また、Dynamo インテグレータは、ユーザにノードとして公開されるホスト API を選択することができます。

### Dynamo Revit ユーザ ワークフロー:

#### 事例

1.
   * ユーザが `SelectModelElement` で Revit の壁を選択すると、Dynamo 壁ラッパーがグラフに返されます(ノードのプレビュー バルーンに表示されます)
   * ユーザは Element.Geometry ノードを配置し、この新しいノードに `SelectModelElement` 出力をアタッチします。ラップされた壁のジオメトリが抽出され、libG API を使用して Dynamo ジオメトリに変換されます。
   * ユーザは、グラフを自動実行モードに切り替えます。
   * ユーザが Revit で元の壁を変更します。
   * Revit ドキュメントが一部の要素が更新されたことを通知するイベントを発生させると、グラフが自動的に再実行されます。選択ノードはこのイベントを監視して、選択した要素の ID が変更されたことを確認します。

### DynamoCivil ユーザ ワークフロー:

D4C のワークフローは、上記で説明した Revit の場合と非常によく似ています。ここでは、D4C の典型的な 2 つの選択ノードのセットを紹介します。

![Civil 3D 選択ノード](images/civilSelectionNodes.png)

### 指摘事項:

*   ドキュメント変更アップデータにより、無限ループを実装する `DynamoRevit` の選択ノードを簡単に構築できます。ノードがすべての要素のドキュメントを監視し、その後このノードの下流のどこかに新しい要素を作成すると考えてください。このプログラムを実行すると、ループがトリガされます。`DynamoRevit` は、トランザクション ID を使用してさまざまな方法でこのようなケースを捕捉しようとし、要素コンストラクタへの入力が変更されていない場合にドキュメントを変更しないようにします。

    選択した要素がホスト アプリケーションで修正されたときにグラフの自動実行が開始される場合は、この点を考慮する必要があります。
* `DynamoRevit` の選択ノードは、WPF を参照する `RevitUINodes.dll` プロジェクトに実装されます これは非指摘事項の可能性がありますが、ターゲットのプラットフォームによっては注意する価値があります。

### データ フロー図

![選択フロー](images/selectModelElement.png)

![選択フロー 2](images/selectElementFace.png)

### 技術的な実装: (上の図を参照):

選択ノードは、一般的な `SelectionBase` 型である `SelectionBase<TSelection, TResult>` とメンバーの最小セットを継承することによって実装されます。

* `BuildOutputAST` メソッドの実装 このメソッドは、将来のノードの実行時に実行される AST を返す必要があります。選択ノードの場合は、要素 ID から要素またはジオメトリを返す必要があります。 https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280
* `BuildOutputAST` の実装は、`NodeModel` / UI ノードの実装で最も難しい部分の 1 つです。できるだけ多くのロジックを C# 関数に組み込み、AST 関数呼び出しノードを AST に埋め込むのがベストです。ここで `node` は抽象構文ツリーの AST ノードであり、Dynamo グラフのノードではないことに注意してください。

![選択フロー 2](images/selectionAST.png)

* シリアル化について
  *   これらは明示的な `NodeModel` 派生型(ZeroTouch ではない)であるため、.dyn ファイルからノードを逆シリアル化する際に使用される[JsonConstructor]を実装する必要もあります。

      ユーザがこのノードを含むグラフを開いたときに選択が設定されたままになるように、ホストからの要素参照を .dyn ファイルに保存する必要があります。Dynamo の NodeModel ノードは json.net を使用してシリアル化します。パブリック プロパティは、Json.net を使用して自動的にシリアル化されます。ここでは[JsonIgnore]属性を使用して、必要なものだけをシリアル化します。
* Document Query ノードは Element ID の参照先を保存する必要がないため、少しシンプルになります。`ElementQueryBase` クラスと派生クラスの実装については、以下を参照してください。これらのノードを実行すると、Revit API を呼び出して基礎となるドキュメントの要素をクエリーし、前述のジオメトリまたは Revit 要素ラッパーへの変換を実行します。

### 参考資料

#### DynamoCore 基本クラス:

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs)
* [NodeModel ケース スタディ - カスタム UI](11_developer_primer/3_developing_for_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Dynamo 2.x 用のパッケージと Dynamo ライブラリを更新する](11_developer_primer/3_developing_for_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Dynamo 3.x 用のパッケージと Dynamo ライブラリを更新する](11_developer_primer/3_developing_for_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)

#### DynamoRevit:

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs)
* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)

## Dynamo 組み込みパッケージの概要

組み込みパッケージのメカニズムは、`PackageLoader` および `PackageManager` Extension によって実装される Dynamo パッケージのロード機能を活用することで、コア自体を拡張することなく、より多くのノード コンテンツを Dynamo Core にバンドルしようというものです。

このドキュメントでは、「組み込みパッケージ」と「Dynamo 組み込みパッケージ」という用語を同じ意味で使用します。

### パッケージを組み込みパッケージとして出荷する必要がありますか?

* パッケージには署名付きバイナリ エントリ ポイントが必要です。これがない場合、パッケージはロードされません。
* これらのパッケージに破壊的変更が加えられないように、あらゆる努力を払う必要があります。つまり、パッケージの内容に対する自動テストが必要です。
* セマンティック バージョン管理を使用できます。セマンティック バージョン管理スキームを使用してパッケージをバージョン管理し、パッケージの説明またはドキュメントでユーザに伝えることをお勧めします。
* 自動テストもあります。上記を参照してください。パッケージが組み込みパッケージ メカニズムを使用して含められている場合、ユーザには製品の一部であるように見えるため、製品としてテストされることになります。
* 高度な洗練。アイコン、ノード ドキュメント、コンテンツのローカライズなど。
* 自分やチームが保守できないパッケージを出荷してはいけません。
* この方法でサード パーティのパッケージを出荷してはいけません(上記を参照)。

基本的には、パッケージを完全にコントロールし、修正や更新を行い、Dynamo と製品に加えられた最新の変更に対してテストを行う必要があります。また、署名する機能も必要です。

### 組み込みパッケージとホスト統合固有のパッケージ

この `Built-In Packages` は、Package Manager にアクセスできない場合でも、すべてのユーザがアクセスできるパッケージのセットであるコア機能とすることを意図しています。現在、この機能をサポートする基本的なメカニズムは、Dynamo のコア フォルダに直接格納されるパッケージの追加の既定ロード場所です(DynamoCore.dll を基準とする)。

制約がある場合、この場所は ADSK Dynamo クライアントとインテグレータが統合固有のパッケージを配布するために使用できます。_(たとえば、Dynamo Formit の統合にはカスタム Dynamo Formit パッケージが必要です)。_

基盤となるロードのメカニズムはコアとホスト固有のパッケージの両方で同じであるため、この方法パッケージを配布する場合、コアの `Built-In Packages` パッケージなのか、単一のホスト製品でのみ利用可能な統合固有のパッケージなのかについて、ユーザを混乱させないようにする必要があります。ユーザの混乱を避けるために、Dynamo チームと話し合い、ホスト固有のパッケージを導入することをお勧めします。

### パッケージのローカリゼーション

`Built-In Packages` に含まれるパッケージはより多くの顧客が利用可能であり、このパッケージについて当社が行う保証はより厳格になるため(上記参照)、ローカライズの必要があります。

`Built-In Packages` に含めることを意図した内部 ADSK パッケージの場合、現在はローカライズされたコンテンツを Package Manager にパブリッシュできないという制限がありますが、パッケージは必ずしも Package Manager にパブリッシュする必要がないため、問題にはなりません。

回避策を使用すると、パッケージの /bin フォルダ内にカルチャ サブディレクトリを含むパッケージを手動で作成(およびパブリッシュ)することもできます。

まず、パッケージ `/bin` フォルダの下に必要なカルチャ固有のサブフォルダを手動で作成します。

何らかの理由でパッケージを Package Manager にもパブリッシュする必要がある場合は、最初にこれらのカルチャ サブフォルダがないバージョンのパッケージをパブリッシュし、次に DynamoUI `publish package version` を使用して、新しいバージョンのパッケージをパブリッシュする必要があります。Dynamo の新しいバージョンのアップロードでは、Windows ファイル エクスプローラを使用して手動で追加した `/bin` のフォルダやファイルは削除されません。将来的には、ローカライズされたファイルの要件に対応するために、Dynamo でのパッケージのアップロード プロセスを更新する予定です。

これらのカルチャ サブディレクトリは、ノード/拡張バイナリと同じフォルダに配置されている場合、.net ランタイムによって問題なくロードされます。

リソース アセンブリと .resx ファイルの詳細については、[https://docs.microsoft.com/ja-jp/dotnet/framework/resources/creating-resource-files-for-desktop-apps](https://docs.microsoft.com/ja-jp/dotnet/framework/resources/creating-resource-files-for-desktop-apps) を参照してください。

`.resx` ファイルを作成し、Visual Studio でコンパイルすることになります。特定のアセンブリ `xyz.dll`(結果のリソースは新しいアセンブリ `xyz.resources.dll` にコンパイルされます)の場合、上記のように、このアセンブリの場所と名前が重要です。

生成された `xyz.resources.dll` は、次のように配置する必要があります。`package\bin\culture\xyz.resources.dll`

パッケージ内のローカライズされた文字列にアクセスする場合、ResourceManager を使用できますが、`.resx` ファイルを追加したアセンブリ内から `Properties.Resources.YourLocalizedResourceName` を参照する方がより簡単です。例については以下をご覧ください。

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457) ローカライズされたエラー メッセージの例

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19) ローカライズされた Dynamo 固有の NodeDescription 属性文字列の例

[https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs) 別の例

### ノード ライブラリのレイアウト

通常、Dynamo でパッケージからノードをロードすると、ノードは ノード ライブラリの `Addons` セクションに配置されます。組み込みパッケージ ノードと他の組み込みコンテンツをより適切に統合するために、組み込みパッケージの作成者が部分的に`layout specification` ファイルを入力する機能が追加され、新しいノードを `default` ライブラリ セクションの正しい最上位レベル カテゴリに配置できるようになりました。

たとえば、次のレイアウト仕様の json ファイルは、パス `package/extra/layoutspecs.json` で見つかった場合、`path` で指定されたノードが、メインの組み込みノード セクションである `default` セクションの `Revit` カテゴリに配置されます。

組み込みパッケージから読み込まれたノードは、レイアウト仕様に含まれるパスとの一致が検討されると、接頭辞 `bltinpkg://` が付きます。

```json
{
  "sections": [
    {
      "text": "default",
      "iconUrl": "",
      "elementType": "section",
      "showHeader": false,
      "include": [ ],
      "childElements": [
        {
          "text": "Revit",
          "iconUrl": "",
          "elementType": "category",
          "include": [],
          "childElements": [
            {
              "text": "some sub group name",
              "iconUrl": "",
              "elementType": "group",
              "include": [
                {
                  "path": "bltinpkg://namespace.namespace",
                  "inclusive": false
                }
              ],
              "childElements": []
            }
          ]
        }
      ]
    }
  ]
}
```

複雑なレイアウトの変更は十分にテストされておらず、サポートもされていません。この特定のレイアウト仕様名称をロードする目的は、パッケージの名前空間全体を `Revit` や `Formit` などの特定のホスト カテゴリに移動することです。
