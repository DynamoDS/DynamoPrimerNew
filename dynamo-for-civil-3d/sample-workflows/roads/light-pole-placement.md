# 照明柱の配置

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Dynamo の多くの優れた使用事例の 1 つは、コリドー モデルに沿って個別のオブジェクトを動的に配置することです。多くの場合、オブジェクトはコリドーに沿って挿入されたアセンブリとは独立した位置に配置する必要があります。これは、手動で行うにはきわめて面倒な作業です。コリドーの水平ジオメトリまたは垂直ジオメトリが変更されると、大量の再作業が必要になります。

## 目標

> :dart: Excel ファイルで指定した測点値でコリドーに沿って照明柱のブロック参照を配置します。

## 主要な概念

> * 外部ファイル(この場合は Excel)からデータを読み込む
> * ディクショナリ内のデータを編成する
> * 座標系を使用して位置/尺度/回転をコントロールする
> * ブロック参照を配置する
> * Dynamo でジオメトリを視覚化する

## バージョンの互換性

{% hint style="success" %}\r\n このグラフは **Civil 3D 2020** 以降で実行できます。 \r\n{% endhint %}

## データセット

まず、以下のサンプル ファイルをダウンロードし、DWG ファイルと Dynamo グラフを開きます。

{% hint style="info" %}\r\n Excel ファイルを Dynamo グラフと同じフォルダに保存することをお勧めします。\r\n{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## 対処法

このグラフのロジックの概要を以下に示します。

> 1. Excel ファイルを読み込み、データを Dynamo に読み込む
> 2. 指定したコリドー基線から計画線を取得する
> 3. 目的の測点でコリドー計画線に沿って座標系を生成する
> 4. 座標系を使用してモデル空間にブロック参照を配置する

以上です。

### Excel データを取得する

このサンプル グラフでは、Excel ファイルを使用して、Dynamo が照明柱のブロック参照を配置するために使用するデータを保存します。表は次のようになります。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Excel ファイルのテーブル構造</p></figcaption></figure>

{% hint style="info" %}\r\n Dynamo を使用して外部ファイル(Excelファイルなど)からデータを読み込む方法は、特にデータを他のチーム メンバーと共有する必要がある場合に最適です。\r\n{% endhint %}

Excel データは、次ように Dynamo に読み込まれます。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Excel データを Dynamo に読み込む</p></figcaption></figure>

データが得られたら、列(_Corridor_、_Baseline_、_PointCode_ など)で分割して、グラフの残りの部分で使用できるようにする必要があります。これを行う一般的な方法は、**List.GetItemAtIndex** ノードを使用して、必要な列のインデックス番号を指定することです。たとえば、_[Corridor]_ 列はインデックス 0、_[Baseline]_ 列はインデックス 1、などです。

問題なさそうですね?しかし、このアプローチには潜在的な問題があります。Excel ファイルの列の順序が将来変更されたらどうなるでしょうか?あるいは、2 つの列の間に新しい列が追加されたら?その場合は、グラフが正しく機能しなくなり、更新が必要になります。Excel の列ヘッダーを _キー_ として、残りのデータを _値_ として使用して、データを**ディクショナリ**に入れることで、グラフを将来も使用できるように保証します。

{% hint style="info" %}\r\n ディクショナリを初めて使用する場合は、「[5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention")」セクションを参照してください。 \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Excel データをディクショナリに入れる</p></figcaption></figure>

これにより、Excel で列の順序を柔軟に変更できるため、グラフの柔軟性が向上します。列ヘッダーが同じであるかぎり、_キー_(列ヘッダー)を使用してディクショナリからデータを取得できます。これを次に実行します。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>ディクショナリからデータを取得する</p></figcaption></figure>

### コリドー計画線を取得する

これで Excel データが読み込まれ、準備ができたので、このデータを使用して、コリドー モデルに関する情報を Civil 3D から取得しましょう。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. コリドー モデルを名前で選択します。
> 2. コリドー内の特定の基線を取得します。
> 3. 基線内の計画線をポイント コードにより取得します。

### 座標系を生成する

ここで、Excel ファイルで指定した測点値でコリドー計画線に沿って**座標系**を生成します。これらの座標系は、照明柱のブロック参照の位置、回転、および尺度を定義するために使用されます。

{% hint style="info" %}\r\n 座標系を初めて使用する場合は、「[2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention")」 セクションを参照してください。 \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>コリドー計画線に沿って座標系を取得する</p></figcaption></figure>

基線のどちら側に座標系があるかに応じて座標系を回転するために、ここでコード ブロックを使用します。これは複数のノードのシーケンスを使用して実現できますが、これは単に書き出すほうが簡単である良い例です。

{% hint style="info" %}\r\n コード ブロックを初めて使用する場合は、「[8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention")」 セクションを参照してください。 \r\n{% endhint %}

### ブロック参照を作成する

あと少しです。ブロック参照を実際に配置するために必要なすべての情報が揃いました。まず、Excel ファイルの _BlockName_ 列を使用して必要なブロック定義を取得します。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>ドキュメントから必要なブロック定義を取得する</p></figcaption></figure>

ここから、最後の手順としてブロック参照を作成します。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>モデル空間でブロック参照を作成する</p></figcaption></figure>

### 結果

グラフを実行すると、コリドーに沿ってモデル空間に新しいブロック参照が表示されます。これが素晴らしいところで、グラフの実行モードが[自動]に設定されている状態で Excel ファイルを編集すると、ブロック参照が自動的に更新されます。

{% hint style="info" %}\r\n グラフの実行モードの詳細については、「[3_user_interface](../../../3\_user\_interface/ "mention")」 セクションを参照してください。 \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Excel ファイルを更新して、Civil 3D で結果をすばやく表示する</p></figcaption></figure>

以下に、**Dynamo プレーヤ**を使用してグラフを実行する例を示します。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Dynamo プレーヤを使用してグラフを実行し、Civil 3D で結果を確認する</p></figcaption></figure>

{% hint style="info" %}\r\n Dynamo プレーヤを初めて使用する場合は、「[dynamo-player.md](../../dynamo-player.md "mention")」セクションを参照してください。 \r\n{% endhint %}

> :tada: ミッションが達成されました。

### ボーナス: Dynamo での視覚化

コンテキストを提供するために、Dynamo でコリドー ジオメトリを視覚化すると便利です。この特定のモデルでは、モデル空間で既に抽出されたコリドーのソリッドが存在するため、Dynamo に取り込んでみましょう。

しかし、他にも検討する必要があることがあります。ソリッドは比較的重いジオメトリ タイプであるため、この操作によってグラフの速度が低下します。ソリッドを表示するかどうかを、簡単に _選択_ できる方法があれば便利です。解決策は非常に単純で、 **Corridor.GetSolids** ノードの接続を解除するだけですが、これによってすべての下流ノードに警告が生成されてしまう点が少々面倒です。この状況では、**ScopeIf** ノードが役に立ちます。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. **Object.Geometry** ノードの下部にグレーのバーがあることに注目してください。これは、ノードのプレビューがオフになり(ノードを右クリックしてアクセス可能)、**GeometryColor.ByGeometryColor** が、背景プレビューでの表示優先順位に関して他のジオメトリとの「競合」を回避できることを意味します。
> 2. **ScopeIf** ノードを使用すると、基本的にノードのブランチ全体を選択的に実行できます。_test_ 入力が false の場合、**ScopeIf** ノードに接続されたすべてのノードは実行されません。

Dynamo の背景プレビューの結果は次のとおりです。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Dynamo でコリドー ジオメトリを視覚化する</p></figcaption></figure>

## アイデア

このグラフの機能を拡張する方法について、いくつかのアイデアを示します。

{% hint style="info" %}\r\n Excel ファイルに **回転** 列を追加し、それを使用して座標系の回転を駆動します。 \r\n{% endhint %}

{% hint style="info" %}\r\n 必要に応じて照明柱がコリドー計画線から外れるようにするため、Excel ファイルに **水平オフセットまたは垂直オフセット** を追加します。 \r\n{% endhint %}

{% hint style="info" %}\r\n Excel ファイルで測点値を使用する代わりに、開始測点と標準間隔を使用して、**Dynamo で直接** 測点値を生成します。 \r\n{% endhint %}
