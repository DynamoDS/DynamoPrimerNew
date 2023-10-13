# 構造物の名前を変更する

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

パイプ ネットワークにパイプと構造物を追加する場合、Civil 3D はテンプレートを使用して名前を自動的に割り当てます。通常、初期の配置時にはこの値で十分ですが、設計が進行するにつれて、必然的に名前を変更する必要が出てきます。さらに、要求される命名パターンは多数あります。たとえば、パイプ配管内で最も下流の構造物から始めて構造物に順番に命名したり、現地の関係機関のデータ スキーマに沿った命名パターンに従ったりすることです。この例では、Dynamo を使用して、あらゆるタイプの命名方法を定義して、一貫して適用する方法を示します。

## 目標

> :dart: Alignment の測点に基づいて、パイプ ネットワーク構造物の名前を順番に変更します。

## 主要な概念

> * バウンディング ボックスを操作する
> * **List.FilterByBoolMask** ノードを使用してデータをフィルタする
> * **List.SortByKey** ノードを使用してデータを並べ替える
> * テキスト文字列を生成および修正する

## バージョンの互換性

{% hint style="success" %}
 このグラフは **Civil 3D 2020** 以降で実行できます。 
{% endhint %}

## データセット

まず、以下のサンプル ファイルをダウンロードし、DWG ファイルと Dynamo グラフを開きます。

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## 対処法

このグラフのロジックの概要を以下に示します。

> 1. レイヤ別に構造物を選択する
> 2. 構造物の位置を取得する
> 3. 構造物をオフセットでフィルタし、測点で並べ替える
> 4. 新しい名前を生成する
> 5. 構造物の名前を変更する

以上です。

### 構造物を選択する

最初に、作業する構造物をすべて選択する必要があります。これを行うには、単純に特定のレイヤ上のすべてのオブジェクトを選択します。つまり、異なるパイプ ネットワークから構造物を選択できます(同じレイヤを共有していると仮定します)。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>特定のレイヤ上の構造物を選択する</p></figcaption></figure>

> 1. このノードを使用すると、構造物と同じレイヤを共有する可能性のある不要なオブジェクト タイプを誤って取り出すことがなくなります。

### 構造物の位置を取得する

構造物が取得できたので、空間内での構造物の位置を特定し、位置に応じて構造物を並べ替えられるようにする必要があります。これを行うには、各オブジェクトのバウンディング ボックスを利用します。オブジェクトの**バウンディング ボックス**は、オブジェクトのジオメトリ範囲を完全に含む最小サイズのボックスです。バウンディング ボックスの中心を計算することで、構造物の挿入位置に非常に近い値が得られます。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>バウンディング ボックスを使用して各構造物のおおよその挿入位置を取得する</p></figcaption></figure>

これらの挿入位置を使用し、選択した Alignment を基準にして構造物の測点とオフセットを取得します。

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### フィルタと並べ替え

ここでは、少し面倒な作業を開始します。この段階では、指定したレイヤ上のすべての構造物の大きなリストを取得し、それを並べ替える Alignment を選択しました。問題は、名前を変更したくない構造物がリストに含まれている可能性があることです。たとえば、構造物が対象となる特定の経路に含まれていない場合があります。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. 選択された Alignment
> 2. 名前を変更する構造物
> 3. 無視する必要がある構造物

そのため、Alignment からの特定のオフセットよりも外にある構造物は対象に含めないように、構造物のリストをフィルタする必要があります。これは、**List.FilterByBoolMask** ノードを使用して行うのが最適です。構造物のリストをフィルタした後、**List.SortByKey** ノードを使用して、測点値で並べ替えます。

{% hint style="info" %}
 リストを初めて使用する場合は、「[2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention")」 セクションを参照してください。 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>構造物をフィルタし、並べ替える</p></figcaption></figure>

> 1. 構造物のオフセットがしきい値より小さいかどうかを確認する
> 2. Null 値を _false_ に置き換える
> 3. 構造物と測点のリストをフィルタする
> 4. 構造物を測点で並べ替える

### 新しい名前を生成する

最後に、構造物の新しい名前を作成する必要があります。使用する形式は `<alignment name>-STRC-<number>` です。ここに、必要に応じてゼロを追加して数値に埋め込むための追加ノードがいくつかあります(例: 「1」を「01」に)。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>新しい構造物の名前を生成する</p></figcaption></figure>

### 構造物の名前を変更する

最後に、構造物の名前を変更します。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>構造物の名前を設定する</p></figcaption></figure>

### 結果

以下に、**Dynamo プレーヤ**を使用してグラフを実行する例を示します。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Dynamo プレーヤを使用してグラフを実行し、Civil 3D で結果を確認する</p></figcaption></figure>

{% hint style="info" %}
 Dynamo プレーヤを初めて使用する場合は、「[dynamo-player.md](../../dynamo-player.md "mention")」 セクションを参照してください。 
{% endhint %}

> :tada: ミッションが達成されました。

### ボーナス: Dynamo での視覚化

Dynamo の 3D バックグラウンド プレビューを利用して、最終的な結果だけでなく、グラフの中間出力を視覚化すると便利です。簡単に行える方法の 1 つは、構造物のバウンディング ボックスを表示することです。さらに、この特定のデータセットにはドキュメントにコリドーがあるため、コリドー計画線ジオメトリを Dynamo に取り込んで、構造物が空間内のどこに位置するかを確認することができます。グラフがコリドーを持たないデータセットで使用されている場合、これらのノードは何も実行しません。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>構造物とコリドー計画線のジオメトリを視覚化する</p></figcaption></figure>

これで、オフセットによる構造物のフィルタ処理がどのように機能するかを理解できるようになりました。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Alignment オフセットのしきい値を調整し、影響を受ける構造物を Dynamo で視覚化する</p></figcaption></figure>

## アイデア

このグラフの機能を拡張する方法について、いくつかのアイデアを示します。

{% hint style="info" %}
 特定の Alignment を選択するのではなく、**最も近い Alignment** に基づいて構造物の名前を変更します。 
{% endhint %}

{% hint style="info" %}
 構造物に加えて **パイプの名前も変更します**。 
{% endhint %}

{% hint style="info" %}
 経路に基づいて構造物の **レイヤを設定します**。 
{% endhint %}
