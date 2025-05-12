# Forma で Dynamo Player を設定する


Dynamo を Forma と一緒に使用するには、クラウドベースの Dynamo as a Service (DaaS)とデスクトップ版 Dynamo の 2 つのオプションがあります。どちらのオプションも何をしたいかに応じてそれぞれメリットがあるため、開始する前に、どちらのオプションが目的のニーズに最も適しているかを検討してください。ただし、これらのオプションはいつでも切り替えることができます。

**Dynamo as a Service と Dynamo Desktop の比較**

<table><thead><tr><th>Dynamo as a Service</th><th>Desktop</th><th data-hidden></th></tr></thead><tbody><tr><td>セットアップが簡単</td><td>インストールのプロセスには複数のステップがある</td><td></td></tr><tr><td>Dynamo をインストールしたり開いたりする必要はない</td><td>Dynamo を開く必要がある</td><td></td></tr><tr><td>Dynamo で既に開いているグラフは認識されない</td><td>Dynamo で開いているグラフをワンクリックするだけで Player で開くことができる</td><td></td></tr><tr><td>Python は使用できない</td><td>Python は使用できる</td><td></td></tr><tr><td>認可されたパッケージのみ使用できる</td><td>どのパッケージでも使用できる</td><td></td></tr></tbody></table>

このページでは、両方のオプションの設定方法を説明します。

### Dynamo as a Service を設定する

Dynamo in Forma Beta は、現時点では早期アクセス用のオープン ベータとして提供されているため、機能やユーザ インタフェースが頻繁に変更される可能性があります。

まず、Forma に Dynamo Player をインストールします。

1. Forma サイトで、左側のサイドバーの[**Extensions**]に移動し、[**Add extension**]をクリックします。これにより、Autodesk App Store の Web サイトが開きます。
2. Dynamo を検索し、Dynamo Player Beta を追加します。免責事項を読んで、[**Agree**]をクリックします。

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. これで、Extensions の中に Dynamo Player が加わりました。これをクリックして開きます。
4. これで、Dynamo Player を使用する準備が整いました。

### Dynamo Desktop を設定する

Dynamo Desktop を使用するには、スタンドアロンの Dynamo Sandbox か、Revit や Civil 3D に接続されている Dynamo が必要になります。また、DynamoFormaBeta パッケージも必要です。

#### Revit

Revit および DynamoFormaBeta パッケージで Dynamo をセットアップするには、次の手順に従います。

1. Revit 2024.1 以降がインストールされているようにします。
2. [管理] > [Dynamo]の順に移動して、Revit から Dynamo を開きます。
3. Dynamo で、DynamoFormaBeta パッケージをインストールします。[パッケージ] > [Package Manager]に移動して、DynamoFormaBeta を検索します。
   1. Revit 2024 を使用している場合は、DynamoFormaBeta for 2.x パッケージをインストールします。
   2. Revit 2025 を使用している場合は、DynamoFormaBeta パッケージをインストールします。

#### Civil 3D

Civil 3D および DynamoFormaBeta パッケージで Dynamo をセットアップするには、次の手順を実行します。

1. Civil 3D 2024.1 以降がインストールされているようにします。
2. [管理] > [Dynamo]の順に移動して、Civil 3D から Dynamo を開きます。
3. Dynamo で、DynamoFormaBeta パッケージをインストールします。[パッケージ] > [Package Manager]に移動して、DynamoFormaBeta を検索します。
   1. Civil 3D 2024 を使用している場合は、DynamoFormaBeta for 2.x パッケージをインストールします。
   2. Civil 3D 2025 を使用している場合は、DynamoFormaBeta パッケージをインストールします。

#### Dynamo Sandbox

Dynamo Sandbox と DynamoFormaBeta パッケージをインストールするには、次の手順に従います。

1. [Dynamo ビルド](https://dynamobuilds.com/)で Dynamo 2.18.0 以降をダウンロードします。最高のエクスペリエンスを得るには、上部にリストされている最も安定しているバージョンの最新のものを選択してください。
   1. Daily バージョンは開発バージョンであり、不完全な機能や開発中の機能が含まれている場合があります。
2. [7zip](https://7-zip.opensource.jp/) を使用して、指定したフォルダに Dynamo を抽出します。
3. Dynamo のインストール フォルダにある DynamoSandbox.exe を実行します。
4. Dynamo で、DynamoFormaBeta パッケージをインストールします。[パッケージ] > [Package Manager]に移動して、DynamoFormaBeta を検索します。
   1. Dynamo 2.x を使用している場合は、DynamoFormaBeta for 2.x パッケージをインストールします。
   2. Dynamo 3.x を使用している場合は、DynamoFormaBeta パッケージをインストールします。

Dynamo をインストールすると、Forma で使用できるようになります。Forma で Dynamo のデスクトップ オプションを実行する場合、Dynamo Player 拡張機能を使用できるようにするには、Dynamo を開く必要があります。

#### Forma で Dynamo Desktop にアクセスする

まず、Forma に Dynamo Player をインストールします。

1. Forma サイトで、左側のサイドバーの[**Extensions**]に移動し、[**Add extension**]をクリックします。これにより、Autodesk App Store の Web サイトが開きます。
2. Dynamo を検索し、Dynamo Player Beta を追加します。免責事項を読んで、[**Agree**]をクリックします。

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. これで、Extensions の中に Dynamo Player が加わりました。これをクリックして開きます。
4. 上部の[Desktop]をクリックして、Dynamo Desktop にアクセスします。

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. これで、Dynamo Player を使用する準備が整いました。Dynamo で既にグラフを開いている場合は、[**Connected graph**]の下の[Open]をクリックするだけで、Player にグラフが表示されます。
