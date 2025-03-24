# ノード ライブラリ

以前に、**ノード**は Dynamo グラフの主要な構成要素であり、**ライブラリ**内で論理グループに編成されていると説明しました。Dynamo for Civil 3D では、ライブラリ内に、Alignments、Profiles、Corridors、Block References など、AutoCAD および Civil 3D オブジェクトを操作するための専用ノードを含む 2 つのカテゴリ(**シェルフ**)があります。ライブラリの残りの部分には、本質的により汎用的であり、Dynamo のすべての「フレーバー」(Revit 用の Dynamo、Dynamo Sandbox など)間で一貫しているノードが含まれています。

{% hint style="info" %} コアの Dynamo ライブラリでノードを構成する方法の詳細については、「 [2-library.md](../3\_user\_interface/2-library.md "mention") 」セクションを参照してください。 {% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Dynamo for Civil 3D のノード ライブラリ</p></figcaption></figure>

> 1. AutoCAD および Civil 3D オブジェクトを操作するための特定のノード
> 2. 汎用ノード
> 3. 個別にインストールできるサードパーティ製**パッケージ**のノード

{% hint style="warning" %} AutoCAD および Civil 3D シェルフにあるノードを使用すると、Dynamo グラフは Dynamo for Civil 3D でのみ動作します。Dynamo for Civil 3D グラフを別の場所(たとえば Revit 用の Dynamo)で開くと、これらのノードに警告フラグが設定され、実行されません。 {% endhint %}

{% hint style="info" %} **AutoCAD と Civil 3D に 2 つの別個のシェルフがある理由とは** 

この構成により、ネイティブ AutoCAD オブジェクト(Lines、Polylines、Block References など)のノードと Civil 3D オブジェクト(Alignments、Corridors、Surfaces など)のノードが区別されます。また、技術的には、AutoCAD と Civil 3D は 2 つの別個の製品です。AutoCAD は基礎となるアプリケーションであり、Civil 3D はその上に構築されています。 {% endhint %}

## ノード階層

AutoCAD ノードと Civil 3D ノードを使用するには、各シェルフ内のオブジェクト階層をしっかり理解しておくことが重要です。生物学ではあらゆる生物が体系的に分類されており、その分類法は、上位から下位にかけて、界、門、網、目、科、属、種という階層構造から成り立っています。AutoCAD と Civil 3D のオブジェクトも同様に分類されています。いくつかの例を使用して説明しましょう。

### Civil オブジェクト

例として Alignment を使用しましょう。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

目的が Alignment の名前を変更することであるとします。次に追加するノードは **CivilObject.SetName** ノードです。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

最初は、あまり直感的には見えないかもしれません。**CivilObject** とは何ですか?ライブラリに **Alignment.SetName** ノードがないのはなぜですか?この答えは、_再利用_ と _簡易化_ に関連しています。考えてみると、Civil 3D オブジェクトの名前を変更するプロセスは、オブジェクトが Alignment、Corridor、Profile など何であっても同じです。したがって、基本的にすべて同じ処理を行う繰り返しノード(**Alignment.SetName、Corridor.SetName、Profile.SetName** など)を使用する代わりに、その機能を単一のノードにラップすることをお勧めします。これはまさに、**CivilObject.SetName** で行うことです。

これについては、_関係_の観点で考えることもできます。Alignment と Corridor は、リンゴと梨の両方が果実の一種であるように、どちらも **Civil オブジェクト**の一種です。Civil オブジェクト ノードはあらゆるタイプの Civil オブジェクトに適用されますが、これは、リンゴと梨の両方の皮をむくのに単一のピーラーを使用するのに似ています。果物の種類ごとに専用のピーラーを揃えていたら、キッチンはひどく散らかってしまいます。その意味で、Dynamo ノード ライブラリはキッチンとまったく同じです。

### オブジェクト

では、この手順を一歩進めてみましょう。Alignment のレイヤを変更するとします。使用するノードは、**Object.SetLayer** ノードです。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

**CivilObject.SetLayer** というノードがないのはなぜでしょうか?前述の再利用とシンプルさの原則が、ここにも適用されます。_layer_ プロパティは、Line、Polyline、Text、Block Reference など、AutoCAD で描画または挿入できるオブジェクトに共通のものです。Alignment や Corridor などの Civil 3D オブジェクトは同じカテゴリに分類されるため、**オブジェクト**に適用されるノードは、どの **Civil オブジェクト**でも使用できます。

