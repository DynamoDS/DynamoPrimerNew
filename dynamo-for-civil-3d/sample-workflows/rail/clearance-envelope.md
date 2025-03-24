# クリアランスのエンベロープ

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

クリアランス検証のための運動学的エンベロープの開発は、鉄道設計の重要な部分です。Dynamo を使用すると、複雑なコリドー サブアセンブリを作成して管理する代わりに、エンベロープのソリッドを生成してこのジョブを実行できます。

## 目標

> :dart: 車両の縦断ブロックを使用して、コリドーに沿ってクリアランスのエンベロープ 3D ソリッドを生成します。

## 主要な概念

> * コリドー計画線を操作する
> * 座標系間でジオメトリを変換する
> * ロフトによりソリッドを作成する
> * レーシング設定を使用してノードの動作をコントロールする

## バージョンの互換性

{% hint style="success" %} このグラフは **Civil 3D 2020** 以降で実行できます。 {% endhint %}

## データセット

まず、以下のサンプル ファイルをダウンロードし、DWG ファイルと Dynamo グラフを開きます。

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## 対処法

このグラフのロジックの概要を以下に示します。

> 1. 指定したコリドー基線から計画線を取得する
> 2. コリドー計画線に沿って目的の間隔で座標系を生成する
> 3. プロファイルの Block ジオメトリを座標系に変換する
> 4. プロファイル間でソリッドをロフトする
> 5. Civil 3Dでソリッドを作成する

以上です。

### コリドー データを取得する

最初の手順は、コリドー データを取得することです。コリドー モデルを名前で選択し、コリドー内の特定の基線を取得します。次に、基線内の計画線をそのポイント コードで取得します。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>コリドー、基線、および計画線を選択する</p></figcaption></figure>

### 座標系を生成する

ここで、指定した開始測点と終了測点の間に、コリドー計画線に沿って**座標系**を生成します。これらの座標系は、車両プロファイルの Block ジオメトリを コリドーに位置合わせするために使用されます。

{% hint style="info" %} 座標系を初めて使用する場合は、「 [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") 」セクションを参照してください。 {% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>コリドー計画線に沿って座標系を取得する</p></figcaption></figure>

> 1. ノードの右下隅にある小さな **XXX** に注目してください。これは、ノードのレーシング設定が _[直積]_ に設定されていることを意味します。これは、両方の計画線に対して同じ測点値で座標系を生成するために必要です。

{% hint style="info" %} ノード レーシングを初めて使用する場合は、「 [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention") 」セクションを参照してください。 {% endhint %}

### Block ジオメトリを変換する

ここで、計画線に沿って車両プロファイルの配列を何らかの方法で作成する必要があります。これから行う作業では、**Geometry.Transform** ノードを使用して、車両プロファイルのブロック定義からジオメトリを変換します。これは視覚化が難しい概念なので、ノードを確認する前に、次のグラフィックスでこれから起こる動作を示します。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>座標系間でのジオメトリの変換の視覚化。</p></figcaption></figure>

基本的に、_単一の_ Block 定義から Dynamo ジオメトリを取得し、移動または回転しながら、計画線に沿って配列を作成します。いいですね。ノード シーケンスは次のようになります。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. これにより、ドキュメントから Block 定義を取得します。
> 2. これらのノードは、Block 内のオブジェクトの Dynamo ジオメトリを取得します。
> 3. これらのノードは、基本的にジオメトリの _変換元_ の座標系を定義します。
> 4. 最後に、このノードはジオメトリを変換する実際の作業を行います。
> 5. このノードの _最長_ レーシングに注目してください。

Dynamo では、次のようになります。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>変換後の車両プロファイルの Block ジオメトリ</p></figcaption></figure>

### ソリッドを生成する

とっておきの朗報です。大変な作業は終わりました。次に行う必要があるのは、プロファイル間にソリッドを生成することだけです。これは、**Solid.ByLoft** ノードを使用して簡単に行うことができます。

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

結果は次のようになります。これらは Dynamo ソリッドです。これらを Civil 3D で作成する必要があります。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>ロフト後の Dynamo ソリッド</p></figcaption></figure>

### Civil 3D にソリッドを出力する

最後の手順は、生成されたソリッドをモデル空間に出力することです。見やすくするために、色付けもします。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>ソリッドを Civil 3D に出力する</p></figcaption></figure>

### 結果

以下に、**Dynamo プレーヤ**を使用してグラフを実行する例を示します。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Dynamo プレーヤを使用してグラフを実行し、Civil 3D で結果を確認する</p></figcaption></figure>

{% hint style="info" %} Dynamo プレーヤを初めて使用する場合は、「 [dynamo-player.md](../../dynamo-player.md "mention") 」セクションを参照してください。 {% endhint %}

> :tada: ミッションが達成されました。

## アイデア

このグラフの機能を拡張する方法について、いくつかのアイデアを示します。

{% hint style="info" %} 各トラックに対して **異なる測点範囲** を個別に使用する機能を追加します。 {% endhint %}

{% hint style="info" %} 小さなセグメントに **ソリッドを分割** し、クラッシュを個別に解析できるようにします。 {% endhint %}

{% hint style="info" %} エンベロープ ソリッドが **フィーチャと交差** するかを確認し、クラッシュするソリッドに色を付けます。 {% endhint %}
