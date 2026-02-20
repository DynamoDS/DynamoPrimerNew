# ライブラリ

ライブラリには、付属の 10 の既定カテゴリ ノード、追加でロードされるカスタム ノードやパッケージなど、ロードされたすべてのノードが格納されます。ライブラリ内のノードは、ライブラリ、カテゴリ、サブカテゴリ(該当する場合)内で階層で整理されます。

\![](<../.gitbook/assets/library - library UI.jpg>)

* 基本ノード: 既定のインストールに付属します。
* カスタム ノード: 頻繁に使用するルーチンまたは特殊なグラフをカスタム ノードとして格納します。また、カスタム ノードをコミュニティに共有することもできます。
* Package Manager のノード: パブリッシュされたカスタム ノードのコレクション。

[ノードの階層](2-library.md#library-hierarchy-for-categories)カテゴリを確認し、[ライブラリからすばやく検索する](2-library.md#search-by-hierarchy)方法を示し、その中で[頻繁に使用するノード](2-library.md#frequently-used-nodes)の一部について学習します。

### カテゴリのライブラリ階層

これらのカテゴリを参照すると、ワークスペースに追加できるノードの階層や、これまでに使用したことがない新しいノードを探す最適な方法について、簡単に理解することができます。

メニューをクリックしてライブラリを参照し、各カテゴリとそのサブカテゴリを展開します。

{% hint style="info" %}[Geometry]には最も多くのノードが含まれているため、ノードを探す場合は最初にこれらのカテゴリを使用することをお勧めします。{% endhint %}

\![](<../.gitbook/assets/library - modified and resize library categories.jpg>)

> 1. ライブラリ
> 2. カテゴリ
> 3. サブカテゴリ
> 4. ノード

この階層によってノードは細かく分類され、データを **Create** するノードか、**Action** を実行するノードか、データを **Query** するノードかに基づいて、同じサブカテゴリに分類されます。

* \![](<../.gitbook/assets/user interface - create.jpg>) **Create**: 最初からジオメトリを作成または構築します。たとえば、円を作成します。
* \![](<../.gitbook/assets/user interface - action.jpg>) **Action**: オブジェクトに対してアクションを実行します。たとえば、円のスケールを変更します。
* \![](<../.gitbook/assets/user interface - query.jpg>) **Query**: 既に存在するオブジェクトのプロパティを取得します。たとえば、円の半径を取得します。

ノードの名前とアイコン以外の詳細情報を表示するには、ノードにマウスを合わせます。これにより、そのノードの機能、必要な入力、生成される出力について、すぐに確認できます。

\![](<../.gitbook/assets/user interface - node description.jpg>)

> 1. ノードに関する簡単な説明
> 2. [ライブラリ]メニューの大きなアイコン
> 3. 入力(名前、データ タイプ、データ構造)
> 4. 出力(データ タイプと構造)

### ライブラリ内のクイック検索

ワークスペースに追加するノードが相対的に特定できるか分かっている場合は、[**検索**]フィールドに入力して、一致するすべてのノードを検索します。

追加するノードをクリックして選択するか、[Enter]を押してハイライト表示されたノードをワークスペースの中心に追加します。

\![](<../.gitbook/assets/user interface - search.jpg>)

#### 階層ごとの検索

キーワードを使用してノードを検索するだけでなく、検索フィールドまたは Code Block (_Dynamo テキスト言語_ を使用)でピリオドで区切って階層を入力することもできます。

各ライブラリの階層は、ワークスペースに追加されたノードの名前に反映されます。

ライブラリ階層内のノードの場所の任意の部分を `library.category.nodeName` という形式で入力すると、次のように異なる結果が返されます。

* `library.category.nodeName`

\![](<../.gitbook/assets/library - search by hierarchy 1 geometry point by coordinates.jpg>)

* `category.nodeName`

\![](<../.gitbook/assets/library - search by hierarchy 2 point by coordinates.jpg>)

* `nodeName`または`keyword`

\![](<../.gitbook/assets/library - search by hierarchy 3 by coordinates.jpg>)

通常、ワークスペース内のノードの名前は `category.nodeName` という形式で表示されますが、いくつかの例外があります。特に注意が必要な例外は、Input カテゴリと View カテゴリです。

ノードの名前は同じですが、カテゴリが異なっていることに注意してください。

* ほとんどのライブラリのノードには、カテゴリ形式が含まれています。

\![](<../.gitbook/assets/library - node category differences 1.jpg>)

* `Point.ByCoordinates` ノードと `UV.ByCoordinates` ノードは、名前は同じですがカテゴリが異なっています。

\![](<../.gitbook/assets/library - node category differences 2.jpg>)

* 注意する例外としては、Built-in Functions、Core.Input、Core.View、Operators などがあります。

\![](<../.gitbook/assets/library - node category differences 3.jpg>)

### 頻繁に使用されるノード

Dynamo の既定のインストールには、数百個のノードが付属しています。では、ビジュアル プログラムを作成するために必要なノードはどれでしょうか。ここでは、プログラムのパラメータを定義するノード(**Input**)に注目し、ノードのアクション(**Watch**)の結果を確認して、ショートカット(**Code Block**)を使用して入力や機能を定義してみましょう。

#### Input ノード

Input ノードは、ビジュアル プログラムのユーザが重要なパラメータを使用する場合の主要な手段です。Core ライブラリから以下のものを利用できます。

| ノード           |                                                        | ノード           |                                                        |
| -------------- | ------------------------------------------------------ | -------------- | ------------------------------------------------------ |
| Boolean        | \![](<../.gitbook/assets/library - boolean.jpg>)        | Number         | \![](<../.gitbook/assets/library - number.jpg>)         |
| String         | \![](<../.gitbook/assets/library - string.jpg>)         | Number Slider  | \![](<../.gitbook/assets/library - number slider.jpg>)  |
| Directory Path | \![](<../.gitbook/assets/library - directory path.jpg>) | Integer Slider | \![](<../.gitbook/assets/library - integer slider.jpg>) |
| File Path      | \![](<../.gitbook/assets/library - file path.jpg>)      |                |                                                        |

#### Watch と Watch3D

Watch ノードは、ビジュアル プログラムを経由してやり取りされるデータを管理するために必要なノードです。ノードの上にマウス カーソルを合わせると、**ノード データのプレビュー**でノードの結果を表示できます。

\![](<../.gitbook/assets/library - node preview.jpg>)

**Watch** ノードで表示したままにしておくと便利です。

\![](<../.gitbook/assets/library - watch node.jpg>)

または、**Watch3D** ノードを使用してジオメトリの結果を確認します。

\![](<../.gitbook/assets/library - watch3d node.gif>)

これらのノードは、どちらも Core ライブラリの View カテゴリに含まれています。

{% hint style="info" %}ヒント: ビジュアル プログラムに多数のノードが含まれている場合、3D プレビューの表示が見にくくなることがあります。その場合は、[設定]メニューの[背景 3D プレビューの表示]オプションを選択解除し、Watch3D ノードを使用してジオメトリをプレビューすることをお勧めします。{% endhint %}

#### Code Block

Code Block ノードでセミコロン区切りの行を使用して、Code Block を定義することができます。これは、`X/Y` のように単純な場合もあります。

また、Code Block ノードをショートカットとして使用して Number Input ノードを定義したり、別のノードの機能を呼び出すこともできます。構文は、Dynamo テキスト言語である [DesignScript](../8_coding_in_dynamo/8-1_code-blocks-and-design-script/2-design-script-syntax.md) の命名規則に従って作成されます。

次に、スクリプトで Code Block を使用するための簡単なデモンストレーションを示します(手順を含む)。

![](../.gitbook/assets/library-codeblockdemo.gif)

1. ダブルクリックして Code Block ノードを作成します。
2. `Circle.ByCenterPointRadius(x,y);` と入力します。
3. ワークスペースをクリックして選択をクリアすると、`x` と `y` の入力が自動的に追加されます。
4. Point.ByCoordinates ノードと Number Slider ノードを作成して Code Block の入力に接続します。
5. ビジュアル プログラムの実行結果は、3D プレビューに円として表示されます。
