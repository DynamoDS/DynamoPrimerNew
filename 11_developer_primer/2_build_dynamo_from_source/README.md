# ソースから Dynamo をビルドする

Dynamo のソースは、クローンを作成して他のユーザに提供するために、GitHub でホストされています。この章では、git を使用してリポジトリをクローン作成する方法、Visual Studio を使用してソース ファイルをコンパイルする方法、ローカル ビルドを実行およびデバッグする方法、および GitHub から新しい変更を取得する方法について説明します。

#### GitHub で Dynamo リポジトリを検索する <a href="#locating-the-dynamo-repositories-on-github" id="locating-the-dynamo-repositories-on-github"></a>

GitHub は、利用者間の変更を追跡し、作業を調整するためのバージョン管理システムである [git](https://help.github.com/articles/git-and-github-learning-resources/) に基づくホスト サービスです。git は、Dynamo のソース ファイルをダウンロードして、いくつかのコマンドでそれらを更新するために活用できるツールです。この方法を使用すると、更新のたびにソース ファイルをダウンロードし、手動で置き換えるといった面倒な作業を回避できます。git バージョン管理システムは、ローカルおよびリモート間のコード リポジトリの違いを追跡します。

Dynamo のソースは、次のリポジトリの DynamoDS GitHub でホストされます。 [https://github.com/DynamoDS/Dynamo](https://github.com/DynamoDS/Dynamo)

![Dynamo ソース ファイル](images/github.jpg)
> Dynamo ソース ファイルです。
>
> 1. リポジトリ全体をクローン作成またはダウンロード
> 2. 他の DynamoDS リポジトリを表示
> 3. Dynamo のソース ファイル
> 4. git 固有ファイル

#### git を使用して Dynamo リポジトリをプルする <a href="#pulling-the-dynamo-repository-using-git" id="pulling-the-dynamo-repository-using-git"></a>

リポジトリをクローン作成する前に、git をインストールする必要があります。インストール手順、および GitHub ユーザ名と電子メールの設定方法については、この[簡易ガイド](https://help.github.com/articles/set-up-git/#setting-up-git)に従ってください。この例では、コマンド ラインで git を実行します。このガイドでは、Windows を使用することを前提としていますが、Mac または Linux で git を使用して Dynamo ソースをクローン作成することもできます。

クローン作成する Dynamo リポジトリの URL が必要です。これは、リポジトリのページの「クローンまたはダウンロード」ボタンにあります。URL をコピーしてコマンド プロンプトに貼り付けます。

![リポジトリをクローン作成する](images/github-clone.png)
> 1. 「クローンまたはダウンロード」を選択する
> 2. URL をコピーする

git がインストールされていると、Dynamo リポジトリをクローン作成できます。はじめに、コマンド プロンプトを開きます。次に、フォルダの変更コマンド `cd` を使用して、クローン作成するソース ファイルのあるフォルダに移動します。この例では、`Github` というフォルダを `Documents` に作成します。

`cd C:\Users\username\Documents\GitHub`

> 「username」をユーザ名と置き換えます

![コマンド プロンプト](images/cli-1.jpg)

次の手順では、git コマンドを実行して、指定した場所に Dynamo リポジトリをクローン作成します。コマンドの URL は、GitHub の「クローンまたはダウンロード」ボタンをクリックして取得します。コマンド ターミナルでこのコマンドを実行します。これにより、Dynamo の最新のコードである Dynamoリポジトリのマスター ブランチをクローン作成します。このマスター ブランチには、最新バージョンの Dynamo コードが含まれます。このブランチは毎日変更されます。

`git clone https://github.com/DynamoDS/Dynamo.git`

![git のクローン作成操作の結果](images/cli-2.jpg)

クローン作成操作が正常に完了したことで、git が動作していることを確認できました。ファイルのエクスプローラで、クローン作成したフォルダに移動してソース ファイルを表示します。フォルダ構造は、GitHub の Dynamo リポジトリのマスター ブランチと同じに見える必要があります。

![Dynamo のソース ファイル](images/source-files.jpg)

> 1. Dynamo のソース ファイル
> 2. git ファイル

#### Visual Studio を使用してリポジトリをビルドする <a href="#building-the-repository-using-visual-studio" id="building-the-repository-using-visual-studio"></a>

ソース ファイルがローカル マシンにクローン化されたため、Dynamo の実行可能ファイルを構築できるようになりました。これを進めるには、Visual Studio IDE をセットアップし、.NET Framework と DirectX がインストールされていることを確認する必要があります。

* 完全な機能を備えた無料の IDE (統合開発環境) [Microsoft Visual Studio Community 2015](https://my.visualstudio.com/Downloads/Results) をダウンロードしてインストールします(これ以降のバージョンでも動作する可能性があります)。
* [Microsoft .NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653) 以降をダウンロードしてインストールします
* ローカルの Dynamo リポジトリ(`Dynamo\tools\install\Extra\DirectX\DXSETUP.exe`)から、Microsoft DirectX をインストールします

> .NET および DirectX は、すでにインストールされている可能性があります。

インストールがすべて完了したら、Visual Studio を起動して、`Dynamo\src` にある `Dynamo.All.sln` ソリューションを開きます。

![ソリューション ファイルを開く](images/vs-open-dynamo.jpg)

> 1. `File > Open > Project/Solution` を選択します。
> 2. Dynamo リポジトリを参照して `src` フォルダを開きます。
> 3. ソリューション ファイル `Dynamo.All.sln` を選択します。
> 4. `Open` を選択します。

ソリューションを構築する前に、いくつか設定が必要です。まず、Dynamo のデバッグバージョンを構築し、デバッグ中に Visual Studio が詳細情報を収集することで、それを開発に役立てることができます。また、AnyCPU をターゲットにします。

![ソリューション設定](images/vs-dynamo-build-settings.jpg)

> `bin` フォルダ内のフォルダとして設定されます。
>
> 1. このサンプルでは、ソリューション設定として `Debug` を選択します
> 2. ソリューション プラットフォームを `Any CPU` に設定します

プロジェクトを開いて、ソリューションを構築できます。このプロセスでは、実行可能な DynamoSandbox.exe ファイルを作成します。

![ソリューションをビルドする](images/vs-build-dynamo.jpg)

> プロジェクトをビルドすると、NuGet の依存関係が復元されます。
>
> 1. `Build > Build Solution` を選択します。
> 2. 出力ウィンドウで正常にビルドされたことを確認します。次のように表示されます。 `==== Build: 69 succeeded, 0 failed, 0 up-to-date, 0 skipped ====`

#### ローカル ビルドを実行する <a href="#running-a-local-build" id="running-a-local-build"></a>

Dynamo が正常にビルドを完了すると、DynamoSandbox.exe ファイルを含む `bin` フォルダが Dynamo リポジトリに作成されます。このサンプルでは、デバッグ オプションを使用してビルドしているため、実行可能ファイルは `bin\AnyCPU\Debug` に配置されます。これ実行すると、Dynamo のローカル ビルドが開きます。

![DynamoSandbox 実行ファイル](images/ex-dynamosandbox.jpg)

> 1. 先ほどビルドした DynamoSandbox の実行可能ファイルです。これを実行して Dynamo を起動します。

これで、Dynamo の開発を始めるための設定はほぼ完了しました。

その他のプラットフォーム(Linux や OS X など)向けに Dynamo を構築する手順については、[Wiki ページ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-on-Linux,-Mac)を参照してください。

#### Visual Studio を使用してローカル ビルドをデバッグする <a href="#debugging-a-local-build-using-visual-studio" id="debugging-a-local-build-using-visual-studio"></a>

デバッグは、不具合や問題を特定し、分離、修正するプロセスです。Dynamo がソースから正常に構築されると、Visual Studio のいくつかのツールを使用して、DynamoRevit アドインなど実行中のアプリケーションをデバッグすることができます。ソース コードを分析して問題の原因を特定したり、現在実行中のコードを検討することができます。Visual Studio のコードのデバッグ方法およびナビゲーション方法の詳細については、「[Visual Studio ドキュメント](https://docs.microsoft.com/en-us/visualstudio/debugger/navigating-through-code-with-the-debugger)」を参照してください。

デバッグ用の 2 つのオプション、スタンドアロンの Dynamo アプリケーション、DynamoSandbox について説明します。

* Visual Studio から直接 Dynamo をビルドして起動する
* Dynamo の実行中のプロセスに Visual Studio をアタッチする

Visual Studio から Dynamo を起動すると、必要に応じてデバッグ セッションごとにソリューションが再構築されるため、ソースに変更を加えた場合は、デバッグ時に反映されます。`Dynamo.All.sln` ソリューションを開いたまま、ドロップダウン メニューから `Debug`、`AnyCPU`、`DynamoSandbox` を選択し、[`Start`]をクリックします。これにより、Dynamo が構築され、新しいプロセス(DynamoSandbox.exe)が開始されて、Visual Studio のデバッガがアタッチされます。

![Visual Studio からアプリケーションをビルドおよび開始する](images/vs-debug-options.jpg)

> Visual Studio から直接アプリケーションを構築して起動します
>
> 1. 環境設定を `Debug` に設定します
> 2. プラットフォームを `Any CPU` に設定します
> 3. スタートアップ プロジェクトを `DynamoSandbox` に設定します
> 4. デバッグ プロセスを開始するには[`Start`]をクリックします

あるいは、特定のグラフやパッケージが開いている場合の問題をトラブルシューティングするために、すでに実行中の Dynamo プロセスをデバッグすることもできます。そのためには、Visual Studio でプロジェクトのソース ファイルを開き、`Attach to Process` デバッグ メニュー項目を使用して実行中の Dynamo プロセスにアタッチします。

![プロセス ダイアログにアタッチする](images/vs-attach-dynamosandbox.jpg)

> 実行中のプロセスを Visual Studio にアタッチする
>
> 1. `Debug > Attach to Process...` を選択します。
> 2. `DynamoSandbox.exe` を選びます。
> 3. `Attach` を選択します。

いずれの場合も、デバッグするプロセスにデバッガをアタッチしています。デバッガを起動する前または後に、コードが実行される直前にプロセスを一時停止させるブレーク ポイントをコードに設定することができます。デバッグ中にキャッチされていない例外が発生した場合、Visual Studio はソース コード内の発生した場所にジャンプします。これは、単純なクラッシュ、未処理の例外を検出し、アプリケーションの実行フローを理解できる効率的な方法です。

![ブレーク ポイントを設定する](images/vs-debug-dynamocore.jpg)

> DynamoSandbox のデバッグ中に、Color.ByARGB ノードのコンストラクタにブレーク ポイントを設定します。このコンストラクタにより、ノードがインスタンス化されたときに Dynamo プロセスが一時停止します。このノードが例外をスローしたり、Dynamo がクラッシュする原因となっている場合は、コンストラクタの各行をステップごとに調べて、問題が発生している場所を特定することができます。
>
> 1. ブレーク ポイント
> 2. 現在実行中の関数と以前の関数呼び出しを示すコール スタック

次のセクション「**ソースから DynamoRevit をビルドする**」では、デバッグの特定のサンプルについて説明し、ブレーク ポイントの設定、コードのステップ実行、コール スタックの読み込み方法について説明します。

#### 最新ビルドをプルする <a href="#pulling-latest-build" id="pulling-latest-build"></a>

Dynamo ソースは GitHub でホストされていることから、ローカル ソース ファイルを更新する最も簡単な方法は、git コマンドを使用して変更をプルすることです。

コマンド ラインを使用して、現在のフォルダを Dynamo リポジトリに設定します。

`cd C:\Users\username\Documents\GitHub\Dynamo`

> `"username"` をユーザ名に置き換えます。

次のコマンドを実行して、最新の変更を取得します。

`git pull origin master`

![更新されたローカル リポジトリ](images/cli-pull-changes.jpg)

> 1. ここでは、ローカル リポジトリがリモートからの変更によって更新されていることが確認できます。

更新プログラムの取得に加えて、さらに 4 つの git ワークフローについて理解しておく必要があります。

* Dynamo リポジトリを**フォーク**して、元のリポジトリとは別にコピーを作成します。ここで行った変更は元のリポジトリには影響せず、プル リクエストを使用して更新を取得または送信することができます。フォークは git コマンドではありませんが、GitHub が追加するワークフローです。フォーク、プル リクエスト モデルは、オープン ソース プロジェクトにオンラインで貢献するための最も一般的なワークフローの 1 つです。Dynamo に寄与したいと考えている場合は、学習する価値があります。
* **ブランチ** \- ブランチの他の作業か切り離して、実験や新機能に関して作業します。これにより、プル リクエストの送信が容易になります。
* 作業単位の完了後、および元に戻したい可能性のある変更を行った後は、頻繁に**コミット**を行ってください。コミット レコードはリポジトリに変更され、メインの Dynamo リポジトリにプル リクエストを行うときに表示されます。
* メインの Dynamo リポジトリに正式に変更を提案する準備ができたら、**プル リクエスト**を作成します。

Dynamo チームには、プル リクエストの作成に関する具体的な手順があります。対処する項目の詳細については、このドキュメントの「プル リクエスト」セクションを参照してください。

git コマンドのリファレンス リストについては、[ドキュメント ページ](https://git-scm.com/docs)を参照してください。
