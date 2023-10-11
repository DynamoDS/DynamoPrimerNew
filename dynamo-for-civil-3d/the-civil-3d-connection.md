# Civil 3D の接続

<figure><img src="../.gitbook/assets/DynamoSwissKnife-WhiteBackground_edit (2).jpg" alt="" width="563"><figcaption></figcaption></figure>

Dynamo for Civil 3D は、土木インフラ プロジェクトで作業するエンジニアや設計者に_ビジュアル プログラミング_のパラダイムを提供します。Dynamo は、Civil 3D ユーザ向けの一種のデジタル マルチツールと考えることができ、タスクが何であれ、作業に適したツールを提供します。直感的なインタフェースにより、一行もコードを記述することなく、強力でカスタマイズ可能なルーチンを作成できます。Dynamo を使用するにはプログラマで_ある_必要はありませんが、プログラマのロジックを使用して_考える_ことができる必要があります。この章は、Primer の他の章と併せて、計算設計の観点からどのようなタスクにも取り組むことができるように、ロジック スキルを構築するのに役立ちます。

## 履歴

Dynamo は Civil 3D 2020 で初めて導入され、その後も進化を続けています。当初はソフトウェアの更新プログラムを介して個別にインストールされていましたが、今では Civil 3D のすべてのバージョンにバンドルされるようになりました。使用している Civil 3D のバージョンによっては、Dynamo のインタフェースがこの章で示す例とは若干異なる場合があります。これは、Civil 3D 2023 のインタフェースが大幅に改良されたためです。

<figure><img src="../.gitbook/assets/c3d-ui-old.png" alt=""><figcaption><p>Dynamo インタフェース、Civil 3D 2020 ～ 2022</p></figcaption></figure>

<figure><img src="../.gitbook/assets/c3d-ui-new.png" alt=""><figcaption><p>Dynamo インタフェース、Civil 3D 2023 ～現在</p></figcaption></figure>

Dynamo の開発に関する最新情報については、[Dynamo Blog](https://dynamobim.org/blog/) を参照してください。次の表は、Dynamo for Civil 3D のライフスパンにおける主要なマイルストーンの一覧です。

<table data-full-width="false"><thead><tr><th width="180">Civil 3D バージョン</th><th width="161">Dynamo バージョン</th><th>メモ</th></tr></thead><tbody><tr><td>2024.1</td><td>2.18</td><td></td></tr><tr><td>2024</td><td>2.17</td><td>Dynamo プレーヤのユーザ インタフェースの更新</td></tr><tr><td>2023.2</td><td>2.15</td><td></td></tr><tr><td>2023</td><td>2.13</td><td>Dynamo のユーザ インタフェースの更新</td></tr><tr><td>2022.1</td><td>2.12</td><td><ul><li>オブジェクト バインド データ ストレージ設定を追加</li><li>オブジェクト バインドをコントロールするための新しいノード</li></ul></td></tr><tr><td>2022</td><td>2.10</td><td><ul><li>Civil 3D のメイン インストールに追加</li><li>IronPython から Python.NET に移行</li></ul></td></tr><tr><td>2021</td><td>2.5</td><td></td></tr><tr><td>2020.2</td><td>2.4</td><td></td></tr><tr><td>2020 Update 2</td><td>2.4</td><td>新しいノードの追加</td></tr><tr><td>2020.1</td><td>2.2</td><td></td></tr><tr><td>2020</td><td>2.1</td><td>初回リリース</td></tr></tbody></table>
