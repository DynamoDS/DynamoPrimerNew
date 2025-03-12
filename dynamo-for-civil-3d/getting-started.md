# スタートアップ

全体像について少し詳しく理解できたところで、Civil 3D で最初の Dynamo グラフを作成してみましょう。

{% hint style="info" %} これは、Dynamo の基本的な機能を説明することを意図した簡単な例です。新しい空の Civil 3D ドキュメントを使用して手順を実行することをお勧めします。{% endhint %}

## Dynamo を開く

最初の手順は、Civil 3D で空のドキュメントを開くことです。次に、Civil 3D リボンの **[管理]** タブにナビゲートし、**[ビジュアル プログラミング]** パネルを探します。

\![](<../.gitbook/assets/image (7).png>)

**Dynamo** ボタンをクリックすると、別のウィンドウで Dynamo が起動します。

{% hint style="info" %} **Dynamo と Dynamo プレーヤの違いとは**

Dynamo は、グラフの作成と実行に使用します。Dynamo プレーヤは、Dynamo でグラフを開くことなく簡単にグラフを実行できます。

使用してみる場合は、「[dynamo-player.md](dynamo-player.md "mention")」 セクションに進んでください。 {% endhint %}

## 新しいグラフを開始する

Dynamo を開くと、開始画面が表示されます。**[新規]** をクリックして、空のワークスペースを開きます。

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Dynamo の開始画面</p></figcaption></figure>

{% hint style="info" %} **サンプルについて**

Dynamo for Civil 3D には、いくつかのグラフが事前に作成されています。これらのグラフを参考にして、Dynamo の使用方法に関してアイデアを発展させてください。いずれかの時点で、これらのサンプル グラフと、Primer 内の [sample-workflows](sample-workflows/ "mention") を確認することをお勧めします。 {% endhint %}

## ノードを追加する

現在、空のワークスペースが表示されているはずです。Dynamo を実際に使用してみましょう。目標は次のとおりです。

>  :dart: **モデル空間にテキストを挿入する Dynamo グラフを作成します。**

とても簡単ですよね?しかし、開始する前に、いくつかの基礎事項を確認する必要があります。

Dynamo グラフの主要な構成要素は、**ノード**と呼ばれます。ノードは小さなマシンのようなものです。ノードにデータを入れると、そのデータに対して何らかの処理が行われ、結果が出力されます。Dynamo for Civil 3D には、ノードの**ライブラリ**が用意されています。このライブラリを**ワイヤ**で接続して、ノードが単独で実行する以上の規模と内容を実行できる**グラフ**を形成することができます。

{% hint style="info" %} **Dynamo を使用したことがない場合はどうすればよいですか?**

見慣れないものがあるかもしれませんが、大丈夫です。次のセクションが役に立ちます。

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") {% endhint %}

では、グラフを作成しましょう。必要なすべてのノードのリストを以下に示します。

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

これらのノードを見つけるには、ライブラリの検索バーにノード名を入力するか、キャンバス内の任意の場所を右クリックして検索します。

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>ノードは、ライブラリから配置することも、キャンバス内で右クリックして配置することもできる</p></figcaption></figure>

{% hint style="info" %} **使用するノードとそれらがある場所を調べる方法**

ライブラリ内のノードは、動作に基づいて論理的なカテゴリにグループ化されています。詳細な説明については、[node-library.md](node-library.md "mention") セクションを参照してください。 {% endhint %}

最終的なグラフは次ようになります。

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>完成したグラフ</p></figcaption></figure>

ここで行った作業をまとめてみましょう。

> 1. 作業するドキュメントを選択しました。この場合(多くの場合)、Civil 3D のアクティブなドキュメントで作業します。
> 2. Text オブジェクトを作成する対象のブロック(この場合はモデル空間)を定義しました。
> 3. _String_ ノードを使用して、テキストを配置するレイヤを指定しました。
> 4. _Point.ByCoordinates_ ノードを使用して点を作成し、テキストを配置する位置を定義しました。
> 5. 2 つの _Number Slider_ ノードを使用して、テキスト挿入位置の X 座標と Y 座標を定義しました。
> 6. 別の _String_ ノードを使用して、Text オブジェクトの内容を定義しました。
> 7. 最後に、Text オブジェクトを作成しました。

真新しいグラフの結果を確認しましょう。

## 結果を確認する

Civil 3D に戻り、**[モデル]** タブが選択されていることを確認します。Dynamo で作成された新しい Text オブジェクトが表示されるはずです。

{% hint style="info" %} テキストが表示されない場合は、[ズーム] -> [範囲]コマンドを実行して適切な位置にズームする必要があります。 {% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

素晴らしい。次に、テキストを更新します。

Dynamo グラフに戻り、テキスト文字列、挿入位置の座標などの入力値をいくつか変更します。Civil 3D でテキストが自動的に更新されます。また、入力ポートの 1 つを接続解除すると、テキストが削除されることにも注意してください。元通りにつなぎ直すと、テキストが再度作成されます。

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>完成したグラフの動作</p></figcaption></figure>

</div>

{% hint style="info" %} **グラフを実行するたびに Dynamo が新しい Text オブジェクトを挿入しない理由とは**

既定では、Dynamo は作成したオブジェクトを「記憶」します。ノード入力値を変更すると、まったく新しいオブジェクトが作成されるのではなく、Civil 3D のオブジェクトが更新されます。この動作の詳細については、「[object-binding.md](advanced-topics/object-binding.md "mention")」セクションを参照してください。 {% endhint %}

> :tada: ミッションが達成されました。

## 次のステップ 

この例では、Dynamo for Civil 3D で実行できる機能の基礎に触れただけです。詳細については読み続けてください。
