# ポイント グループ管理

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Civil 3D で COGO ポイントとポイント グループを使用して作業することは、多くのデータの一貫処理のプロセスの中核となる要素です。Dynamo はデータ管理に関して非常に優れています。この例では、その潜在的な使用例の 1 つを示します。 

## 目標

> :dart: 一意の COGO ポイントの説明ごとにポイント グループを作成します。

## 主要な概念

> * リストの操作
> * **List.GroupByKey** ノードを使用して類似オブジェクトをグループ化する
> * Dynamo プレーヤでカスタム出力を表示する

## バージョンの互換性

{% hint style="success" %} このグラフは **Civil 3D 2020** 以降で実行できます。 {% endhint %}

## データセット

まず、以下のサンプル ファイルをダウンロードし、DWG ファイルと Dynamo グラフを開きます。

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## 対処法

このグラフのロジックの概要を以下に示します。

> 1. ドキュメント内のすべての COGO ポイントを取得する
> 2. COGO ポイントを説明でグループ化する
> 3. ポイント グループを作成する
> 4. 概要を Dynamo プレーヤに出力する

以上です。

### COGO ポイントを取得する

最初に、ドキュメント内のすべてのポイントグループを取得し、次に各グループ内のすべての COGO ポイントを取得します。これにより、_ネストされたリスト_、つまり「リストのリスト」が得られます。これは、**List.Flatten** ノードを使用してリスト全体をフラット化する場合に、後で操作が簡単になります。

{% hint style="info" %} リストを初めて使用する場合は、「 [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") 」セクションを参照してください。 {% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>すべてのポイント グループと COGO ポイントを取得する </p></figcaption></figure>

### ポイントを説明でグループ化する

すべての COGO ポイントを取得できたので、説明に基づいてグループに分類する必要があります。これは、まさに **List.GroupByKey** ノードが行う処理です。基本的に、同じキーを共有する項目をグループ化します。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>COGO ポイントを説明でグループ化する</p></figcaption></figure>

### ポイント グループを作成する

大変な作業は終わりました。最後の手順は、グループ化された COGO ポイントから新しい Civil 3D ポイント グループを作成することです。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>新しいポイント グループを作成する</p></figcaption></figure>

### 概要を出力する

グラフを実行しても、ジオメトリを操作していないため、Dynamo の背景プレビューには何も表示されません。グラフが正しく実行されたかどうかを確認する唯一の方法は、ツールスペースを確認するか、ノード出力のプレビューを確認することです。ただし、**Dynamo プレーヤ**を使用してグラフを実行する場合は、作成したポイント グループの概要を出力することで、グラフの結果に関するフィードバックを増やすことができます。必要な操作は、ノードを右クリックして[_出力_]に設定することだけです。この場合、名前を変更した **Watch** ノードを使用して結果を表示します。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>ノードを[<em>出力</em>]に設定すると、その内容が Dynamo プレーヤの出力に表示される</p></figcaption></figure>

### 結果

以下に、**Dynamo プレーヤ**を使用してグラフを実行する例を示します。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Dynamo プレーヤを使用してグラフを実行し、ツールスペースで結果を確認する</p></figcaption></figure>

{% hint style="info" %} Dynamo プレーヤを初めて使用する場合は、「 [dynamo-player.md](../../dynamo-player.md "mention") 」セクションを参照してください。 {% endhint %}

> :tada: ミッションが達成されました。

## アイデア

このグラフの機能を拡張する方法について、いくつかのアイデアを示します。

{% hint style="info" %} ポイントのグループ化を、未処理の説明ではなく **完全な説明** に基づいて行うように修正します。 {% endhint %}

{% hint style="info" %} 選択した他の **定義済みカテゴリ** (「グラウンド ショット」、「モニュメント」など)によってポイントをグループ化します。 {% endhint %}

{% hint style="info" %} 特定のグループのポイントに対して TIN サーフェスを自動的に作成します。 {% endhint %}
