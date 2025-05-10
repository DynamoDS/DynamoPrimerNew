# よくある質問(FAQ)

## Dynamo ビルドの利用方法

### 毎日のビルドと安定のビルド

オートデスク チームの Dynamo では、コミットごとの毎日のビルドと、システムのテストとリリース サイクル後の安定したリリース ビルドの両方をリリースすることで、速いペースで反復を維持するという伝統があります。チームでは、毎日のビルドと安定のビルドを再開して、ディスク上の DynamoCore の抽出場所をローカルでコントロールできるようにし、他のオートデスク製品の Dynamo に影響を与えることなくユーザが安心して使用できるようにしたいと考えています。この目的のため、.nupkg ファイル、.zip ファイル、ユーザがインストール パスやその他のオプションを選択できる専用インストーラなど、いくつかの候補が挙がっています。

できるだけシンプルな方法で最新のコードをユーザに提供するという目標を考慮し、Revit なしで使用できる(いくつかの制約あり) DynamoCore バイナリと Dynamo Sandbox を含む .zip ファイルを提供することにしました。

### Dynamo Zip ビルド

#### 定義とソース

DynamoCoreRuntime zip ビルドは、自動ビルド中に作成される DynamoCore バイナリのスナップショットです。

抽出したフォルダ内で DynamoSandbox.exe を起動し、最小限のセットアップで Dynamo を使用できるようにする必要があります。

#### 必要なコンポーネント

| Dynamo のバージョン | Microsoft Visual C++    | DirectX                         |   |   |   |   |
| --------------     | --------------------    | ------------------------------- | - | - | - | - | 
| 2.0 - 2.6          | 2015 再頒布可能パッケージ | 10                              |   |   |   |   | 
| 2.7                | 2019 再頒布可能パッケージ | 11/12 (Windows 10 に付属        |   |   |   |   | 
| >=2.8              | 2019 再頒布可能パッケージ | 11/12 (Windows 10 に付属        |   |   |   |   |

**Microsoft DirectX は、**[**こちら**](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)の Dynamo Github リポジトリでも公開されています。

**パッケージの解凍に使用する 7zip は**[**こちら**](https://7-zip.opensource.jp/download.html)

**Microsoft Visual C++ 2015-2024 再頒布可能パッケージ(x64)**[**リンク**](https://aka.ms/vs/17/release/vc_redist.x64.exe)

**オプションのコンポーネント**

ジオメトリ ライブラリ(Revit、Civil 3D、Advance Steel などの特定のオートデスク モデリング ツールでのみ利用可能)

### トラブルシューティング

ビルド解凍後、DynamoSandbox.exe をまったく起動できないという場合は、[7zip](https://7-zip.opensource.jp/download.html) を使用してビルドを解凍するようにしてください。使用するマシンの権限を持っている場合は、_抽出前に_ .zip アーカイブのブロックを手動で解除することもできます。

![](images/a-7/dynamo-builds-1.png)

必要なコンポーネントが存在しない場合、Dynamo の使用中に問題が発生したり、UI の一部のロードに失敗することがあります。

次のスクリーンショットの例では、GPU なしのクリーンな Windows 10 VM でのビルド解凍後、マシンに必要なコンポーネントが両方とも欠落しています。これは、Dynamo コンソールに表示されます。

![](images/a-7/dynamo-builds-2.png)

**DirectX をインストールする**

DirectX が既にインストールされている場合は、こちらの Microsoft の指示に従ってください。まだインストールしていない場合は、[こちら](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)の Dynamo Github リポジトリを使用して DXSETUP.exe を開きます。以下のダイアログが表示されたら、[次へ]をクリックして DirectX を既定の場所にインストールしてください。

![](images/a-7/dynamo-builds-3.png)

**Microsoft Visual C++ 2015-2024 再頒布可能パッケージ(x64)のインストール**

最新版は[こちら](https://aka.ms/vs/17/release/vc_redist.x64.exe)からダウンロードしてください。これで、ブラウザのダウンロード場所で vc_redist.x64.exe という名前のインストーラを実行できるようになります。以下のダイアログが表示されたら、[インストール]をクリックしてこのコンポーネントを既定の場所に配置してください。

![](images/a-7/dynamo-builds-4.png)

上記のリンクから両方の必要なコンポーネントをインストールした後、DynamoSandbox.exe を再起動すると、次の結果が表示されます。

![](images/a-7/dynamo-builds-5.png)

**3D グラフィックスが見つからない場合。**

また、Sandbox を初めて実行する際にグラフィックスの問題が発生する可能性もあります。この場合、グラフィックスの問題に関する標準のよくある質問(FAQ)をご覧ください。

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

一般的には、DynamoSandbox.exe を使用する場合、グラフィックス カードに対して高パフォーマンス GPU モードを強制適用する必要があります

_NVIDIA コントロール パネルの例:_

![](images/a-7/dynamo-builds-6.png)

**WebView2 ランタイムのインストール**

現在、次期 Dynamo モジュールでは WebView2 コンポーネント(ドキュメント ブラウザ、ガイド ツアー、ライブラリ)が使用されています。そのため、Dynamo のこの部分で Web コンテンツが正しく表示されるようにするには、WebView2 Evergreen ランタイム インストーラをインストールする必要があります(このランタイムがコンピュータに既にインストールされているか、インストールする必要があるかを検証する必要があります)。

WebView2 ランタイム インストール用のリンクはこちらです。[https://developer.microsoft.com/ja-jp/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/ja-jp/microsoft-edge/webview2/#download-section)

![](images/a-7/dynamo-builds-7.png)

インストール対象は、Evergreen Bootstrapper または Evergreen Standalone Installer のいずれか 1 つです。前者は 1.50 MB、後者は 130 MB のインストーラがダウンロードされます。

ランタイムをインストールすると、Dynamo の次期コンポーネントが正しく動作するようになります。

![](images/a-7/dynamo-builds-8.png)

**Dynamo Excel ノードに関する問題**

問題の診断については、こちらの[記事](https://www.autodesk.com/jp/support/technical/article/caas/sfdcarticles/sfdcarticles/JPN/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html)を参照してください。

### Dynamo ビルドの場所

安定版リリース

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

日次ビルドと安定版リリース

[https://dynamobuilds.com/](https://dynamobuilds.com/)
