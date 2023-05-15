# Dynamo 向けの開発

経験のレベルに関係なく、Dynamo プラットフォームはすべてのユーザが開発に参加できるように設計されています。さまざまな能力やスキル レベルを対象とする開発オプションがあり、それぞれの目的によって利点と欠点があります。ここでは開発オプションの概要と、その選び方について説明します。

![3 つの開発環境](images/developing-for-dynamo.png)

> 次の 3 つの開発環境があります: Visual Studio、Python Editor、Code Block DesignScript

#### オプションの選び方について <a href="#what-are-my-options" id="what-are-my-options"></a>

Dynamo の開発オプションは、主に Dynamo 向け__の開発と、Dynamo __での開発の 2 つに分類されます。この 2 つのカテゴリは、Dynamo IDE を使用して作成されたコンテンツを Dynamo で使用する「Dynamo での開発」と、外部ツールを使用して作成されたコンテンツを Dynamo に読み込んで使用する「Dynamo 向けの開発」を意味します。このガイドでは、__Dynamo 向けの開発に焦点を当てていますが、すべてのプロセスのリソースについて以下で説明します。

#### Dynamo 向けの開発<a href="#for-dynamo" id="for-dynamo"></a>

これらのノードを使用することにより、最高レベルのカスタマイズが可能になります。多くのパッケージがこの方法を使用して構築されており、Dynamo のソースに貢献するために必要な手法です。このガイドでは、これらの構築プロセスについて説明します。

* Zero-Touch ノード
* NodeModel 派生ノード
* 拡張機能

> Primer には、[Zero-Touch ライブラリをインポートする](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/5-zero-touch)ための手順が記載されています。

次の説明では、Zero-Touch ノードおよび NodeModel ノードの開発環境として Visual Studio を使用します。

![Visual Studio インタフェース](images/vs-devenv.jpg)

> これから開発するプロジェクトがある Visual Studio インタフェース

#### Dynamo での開発<a href="#in-dynamo" id="in-dynamo"></a>

これらのプロセスはビジュアル プログラミング ワークスペース内に存在し、比較的単純ですが、すべてが Dynamo をカスタマイズするための有用なオプションです。Primer ではこれらの内容を広範囲にカバーし、「[スクリプト作成のガイドライン](http://dynamoprimer.com/en/12\_Best-Practice/12-1\_Scripting-Strategies.html)」の章では、スクリプトのヒントとベスト プラクティスについて説明します。

*   Code Block はビジュアル プログラミング環境で DesignScript を公開し、柔軟なテキスト スクリプトとノード ワークフローを可能にします。Code Block の関数は、ワークスペース内の任意の機能から呼び出すことができます。

    > Code Block サンプルをダウンロードする(右クリックして名前を付けて保存する)か、[Primer](https://primer.dynamobim.org/07\_Code-Block/7-1\_what-is-a-code-block.html) で詳細な手順を確認してください。
*   カスタム ノードは、ノードの集合体またはグラフ全体のコンテナです。頻繁に使用するルーチンを収集し、コミュニティと共有するための効果的な手段です。

    > カスタム ノードのサンプルをダウンロードする(右クリックして名前を付けて保存する)か、[Primer](https://primer.dynamobim.org/10\_Custom-Nodes/10-1\_Introduction.html) で詳細な手順を確認してください。
*   Python ノードは、Code Block と同様の働きをするビジュアル プログラミング ワークスペースのスクリプト インタフェースです。Autodesk.DesignScript ライブラリでは、DesignScript と同様のドット表記を使用します。

    > Python ノードのサンプルをダウンロードする(右クリックして名前を付けて保存する)か、[Primer](https://primer.dynamobim.org/10\_Custom-Nodes/10-4\_Python.html) で詳細な手順を確認してください。

Dynamo ワークスペースでの開発は、即座にフィードバックが得られる強力な手段です。

![Python ノードを使用して Dynamo ワークスペースで開発する](images/python-example.jpg)

> Python ノードを使用して Dynamo ワークスペースで開発する

#### それぞれの利点と欠点について <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Dynamo の開発オプションは、カスタマイズの必要性に伴う複雑な作業に対応できるように設計されています。目的が Python で再帰スクリプトを記述するのか、完全にカスタム化したノード UI を構築するのかに関わらず、起動と実行に必要なものだけでコードを実装できるオプションがあります。

**Dynamo の Code Block 、Python ノード、カスタム ノード**

これらは、Dynamo のビジュアル プログラミング環境でコードを書く場合の簡単なオプションです。Dynamo のビジュアル プログラミング ワークスペースから、Python や DesignScript にアクセスでき、カスタム ノード内に複数のノードを含めることができます。

![Code Block、Python スクリプト、およびカスタム ノード](images/Development-Icons.png)

これらの方法を使用すると、次の操作が可能になります。

* セットアップは最小限のまま、Python または DesignScript の作成を開始します。
* Python ライブラリを Dynamo に読み込みます。
* Dynamo コミュニティで、Code Block、Python ノード、カスタム ノードをパッケージの一部として共有します。

**Zero-Touch ノード**

Zero-Touch とは、C# ライブラリを読み込むための単純なポイント アンド クリック操作のことです。Dynamo は、`.dll` のパブリック メソッドを読み取って Dynamo ノードに変換します。Zero-Touch ノードを使用して、独自のカスタム ノードおよびパッケージを開発できます。

![Zero-Touch ノード](images/ZTImport.png)

この方法では、次の操作が可能になります。

* Primer の「[A-Forge の例](http://dynamoprimer.com/en/10\_Packages/10-5\_Zero-Touch.html)」のように、必ずしも Dynamo 用に開発されたのではないライブラリを読み込み、一連の新しいノードを自動的に作成します。
* C# メソッドを記述し、Dynamo でノードとして簡単に使用することができます。
* パッケージ内の Dynamo コミュニティで C# ライブラリをノードとして共有できます。

**NodeModel 派生ノード**

これらのノードは、Dynamo の構造をさらに一歩前進させたものです。これらは `NodeModel` クラスに基づいて C# で記述されます。この方法は最も柔軟で強力ですが、ノードのほとんどの要素を明示的に定義する必要があることと、関数は別のアセンブリに存在する必要があります。

![NodeModel 派生ノード ](images/Development-Icons-NodeModel.png)

この方法では、次の操作が可能になります。

* スライダ、イメージ、カラーなど、完全にカスタマイズ可能なノード UI を作成します(ColorRange ノードなど)。
* Dynamo キャンバスにアクセスし、そこでの動作に影響を与えることができます。
* レーシングをカスタマイズします。
* Dynamo にパッケージとしてロードします。

#### Dynamo のバージョン管理と API の変更点について理解する(1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Dynamo は定期的に更新されるため、パッケージが使用する API の一部が変更される場合があります。既存のパッケージが正しく動作し続けるようにするためには、これらの変更を追跡することが重要になります。

API の変更は、[Dynamo GitHub Wiki](https://github.com/DynamoDS/Dynamo/wiki/API-Changes) で確認できます。ここでは DynamoCore、ライブラリ、ワークスペースの変更点について調べることができます。

![Dynamo API の変更点に関するドキュメント](images/api-changes.jpg)

今後予定されている重要な変更の例として、バージョン 2.0 でファイル形式が XML から JSON に移行することが挙げられます。NodeModel 派生ノードには、[JSON コンストラクタ](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node)が必要になります。これがない場合、Dynamo 2.0 で開きません。

現在の Dynamo API ドキュメントでは、コアな機能についてカバーしています。[http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI)

![API ドキュメント](images/api-docs.jpg)

#### パッケージのバイナリを配布する権限 <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Package Manager にアップロードされるパッケージに含まれる .dll に注意してください。パッケージの作成者が .dll を作成しなかった場合、それらを共有する権限が必要です。

パッケージにバイナリが含まれている場合、パッケージにバイナリが含まれていることをダウンロード時にユーザに確認する必要があります。
