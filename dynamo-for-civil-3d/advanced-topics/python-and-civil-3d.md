# Python と Civil 3D

Dynamo は[ビジュアル プログラミング](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) ツールとして非常に強力ですが、ノードやワイヤを超えて、テキスト形式でコードを記述することもできます。これを行うには、次の 2 つの方法があります。

1. Code Block ノードを使用して **DesignScript** を記述する
2. Python ノードを使用して **Python** を記述する

このセクションでは、Civil 3D 環境で Python を活用して、AutoCAD および Civil 3D .NET API を利用する方法について説明します。

{% hint style="info" %} Dynamo での Python の使用に関する一般情報については、「[8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention")」セクションを参照してください。{% endhint %}

## API ドキュメント

AutoCAD と Civil 3D にはどちらにも、開発者がカスタム機能を使用してコア製品を拡張できるようにする複数の API が用意されています。Dynamo のコンテキストで関連するのは、**Managed .NET API** です。次のリンクは、API の構造とその仕組みを理解するために不可欠です。

[AutoCAD .NET API 開発者用ガイド](https://help.autodesk.com/view/OARX/2024/JPN/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[AutoCAD .NET API リファレンス ガイド](https://help.autodesk.com/view/OARX/2024/JPN/?guid=OARX-ManagedRefGuide-What_s_New)

[Civil 3D .NET API 開発者用ガイド](https://help.autodesk.com/view/CIV3D/2024/JPN/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Civil 3D .NET API リファレンス ガイド](https://help.autodesk.com/view/CIV3D/2024/JPN/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %} このセクションを進めていくと、データベース、トランザクション、メソッド、プロパティなど、馴染みのない概念が出てくるかもしれません。これらの概念の多くは、.NET API を使用するための中核であり、Dynamo や Python に固有のものではありません。これらの項目の詳細については、Primer のこのセクションでは取り上げません。詳細については、上記のリンクを頻繁に参照することをお勧めします。{% endhint %}

## コード テンプレート

新しい Python ノードを初めて編集すると、開始するためのテンプレート コードがあらかじめ入力されます。ここでは、テンプレートの概要と各ブロックに関する説明を示します。

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Civil 3D の既定の Python テンプレート</p></figcaption></figure>

> 1. `sys` モジュールおよび `clr` モジュールを読み込みます。どちらも Python インタプリタが正しく機能するために必要なモジュールです。特に、`clr` モジュールを使用すると、.NET 名前空間を基本的に Python パッケージとして扱うことができます。
> 2. AutoCAD および Civil 3D のマネージド .NET API を使用するための標準アセンブリ(DLL)をロードします。
> 3. 標準の AutoCAD および Civil 3D 名前空間に参照設定を追加します。これらは、それぞれ、C# または VB.NET の `using` ディレクティブまたは `Imports` ディレクティブに相当します。
> 4. ノードの入力ポートには、`IN` と呼ばれる定義済みのリストを使用してアクセスできます。特定のポートのデータには、そのインデックス番号を使用してアクセスできます(例: `dataInFirstPort = IN[0]`)。
> 5. アクティブなドキュメントおよびエディタを取得します。
> 6. ドキュメントをロックし、データベース トランザクションを開始します。
> 7. ここに、スクリプトのロジックの大部分を配置する必要があります。
> 8. メインの作業が完了した後にトランザクションをコミットするには、この行のコメントを解除します。
> 9. ノードからデータを出力する場合は、スクリプトの最後にある変数 `OUT` に出力するデータを割り当てます。

{% hint style="info" %} **カスタマイズする場合**\
既定の Python テンプレートは、`C:\ProgramData\Autodesk\C3D <version>\Dynamo` 内の `PythonTemplate.py` ファイルを編集することで修正できます。{% endhint %}

## 例

Dynamo for Civil 3D で Python スクリプトを作成する場合の基本的な概念について、例を見ていきましょう。

### 目標

> :datt: 図面内のすべての集水域の境界ジオメトリを取得します。

### データセット

この演習で参照できるサンプル ファイルを次に示します。

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### 対処法の概要

このグラフのロジックの概要を次に示します。

> 1. Civil 3D API のドキュメントを確認する
> 2. レイヤ名でドキュメント内のすべての集水域を選択する
> 3. Dynamo オブジェクトを「アンラップ」して、内部の Civil 3D API メンバーにアクセスする
> 4. AutoCAD の点から Dynamo の点を作成する
> 5. 点からポリカーブを作成する

以上です。

### API ドキュメントを確認する

グラフの作成とコードの作成を開始する前に、Civil 3D API のドキュメントを参照して、API が提供する機能を理解することをお勧めします。この場合、集水域の境界点を返す[プロパティが Catchment クラス](https://help.autodesk.com/view/CIV3D/2024/JPN/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c)に存在します。このプロパティは、Dynamo が処理する必要があるオブジェクトとは異なる `Point3dCollection` オブジェクトを返すことに注意してください。つまり、`Point3dCollection` からポリカーブを作成することはできないため、最終的にはすべてを Dynamo の点に変換する必要があります。詳細については、後で説明します。

### すべての集水域を取得する

これで、グラフ ロジックの作成を開始することができます。最初に、ドキュメント内のすべての集水域のリストを取得します。これに使用できるノードがあるため、Python スクリプトに含める必要はありません。ノードを使用すると、(Python スクリプトに多くのコードを埋め込むんだ場合に比べて)、他のユーザはグラフを読みやすくなり、Python スクリプトは集水域の境界点を返すという 1 つの事柄にも対処できます。

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>ドキュメント内のすべての集水域をレイヤごとに取得する</p></figcaption></figure>

**All Objects on Layer** ノードからの出力は、CivilObjects のリストであることに注意してください。これは、Dynamo for Civil 3D には現在、集水域を操作するためのノードが存在しないためです。これが、Python を使用して API にアクセスする必要がある理由です。

### オブジェクトをアンラップする

先に進む前に、重要な概念について簡単に説明する必要があります。「[node-library.md](../node-library.md "mention")」セクションで、オブジェクトと CivilObjects の関係について説明しました。これをもう少し詳しく説明すると、**Dynamo オブジェクト**は** AutoCAD 図形**のラッパーです。同様に、**Dynamo CivilObject** は、**Civil 3D 図形**のラッパーです。`InternalDBObject` プロパティまたは `InternalObjectId` プロパティにアクセスすることで、オブジェクトを「アンラップ」することができます。

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Dynamo タイプ</th><th width="373">ラップ</th></tr></thead><tbody><tr><td><strong>オブジェクト</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>図形</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>図形</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} 経験則として、`InternalObjectId` プロパティを使用してオブジェクト ID を取得し、トランザクションでラップされたオブジェクトにアクセスする方が一般的に安全です。これは、`InternalDBObject` プロパティは書き込み可能な状態でない AutoCAD DBObject を返すためです。 {% endhint %}

### Python スクリプト

内部の集水域オブジェクトにアクセスして境界点を取得する作業を行う完全な Python スクリプトを以下に示します。ハイライト表示された行は、既定のテンプレート コードから修正または追加された行を表します。

{% hint style="info" %}スクリプト内の下線付きのテキストをクリックすると、その行の説明が表示されます。{% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Python 標準ライブラリと DesignScript ライブラリをロードします
import sys
import clr

# AutoCAD および Civil3D のアセンブリを追加します
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# AutoCAD から参照設定をインポートします
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Civil3D から参照設定をインポートします
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# このノードへの入力は、IN 変数にリストとして保存されます。
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("The input is null or empty.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>   
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>             
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>               
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                   
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>           
            # トランザクション終了前にコミットする
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# OUT 変数に出力をアサインします。
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} 経験則として、スクリプト ロジックの大部分をトランザクション内に含めることをお勧めします。これにより、スクリプトが読み取り/書き込みを行うオブジェクトに安全にアクセスできるようになります。多くの場合、トランザクションを省略すると致命的なエラーが発生する可能性があります。{% endhint %}

### ポリカーブを作成する

この段階では、Python スクリプトは、背景プレビューで確認できるように Dynamo の点のリストを出力する必要があります。最後の手順は、点から単純にポリカーブを作成します。これは Python スクリプトで直接行うこともできますが、より見やすくするために、ノードのスクリプトの外側に意図的に配置しています。最終的なグラフは次のようなものになります。

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>最終的なグラフ</p></figcaption></figure>

### 結果

最終的な Dynamo ジオメトリは次のとおりです。

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>集水域境界の、結果として生じた Dynamo ポリカーブ</p></figcaption></figure>

> :tada: ミッションが達成されました。

## IronPython と CPython

最後に、注意事項を簡単に説明します。使用している Civil 3D のバージョンに応じて、Python ノードの設定が異なる場合があります。**Civil 3D 2020 および 2021** では、Dynamo は **IronPython** というツールを使用して .NET オブジェクトと Python スクリプトの間でデータを移動しました。**Civil 3D 2022** では、Dynamo は Python 3 を使用するのではなく、標準のネイティブ Python インタプリタ(**CPython** とも呼ばれる)を使用するように移行しました。この移行には、人気のある最新ライブラリや新しいプラットフォーム機能、基本的なメンテナンス、セキュリティ パッチへのアクセスなどのメリットがあります。

{% hint style="info" %} この移行の詳細と、従来のスクリプトのアップグレード方法については、[Dynamo Blog](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/) を参照してください。IronPython を今後も使用する場合は、Dynamo Package Manager を使用して **DynamoIronPython2.7** パッケージをインストールする必要があります。{% endhint %}

[^1]: 既定では、Dynamo ジオメトリ ライブラリは Python 環境に追加されません。このスクリプトの目的は、集水域境界の Dynamo の点のリストを出力することです。点を後で作成するには、この行を追加する必要があります。

[^2]: この行は、Dynamo ジオメトリ ライブラリから必要な特定のクラスを取得します。ここでは、`import *` ではなく `import Point as DynPoint` を指定します。前者の場合は名前の衝突が発生するからです。

[^3]: ここでは、分かりやすくするため、既定の `dataEnteringNode` 変数の名前を `objs` に変更します。

[^4]: ここでは、すべての入力のリスト全体を参照する既定の `IN` の代わりに、どの入力ポートに必要なデータが含まれるかを正確に指定します。

[^5]: これにより空のリストが作成され、後で出力データを追加します。

[^6]: これは、スクリプトに対する潜在的な空または null 入力から保護するためのものです。

[^7]: スクリプトを中断する代わりに、スクリプトを続行できない理由を説明する有用なメッセージを出力します。

[^8]: スクリプトの残りの部分では、(単一のオブジェクトではなく)、作業するオブジェクトのリストが存在すると仮定しています。ただし、オブジェクトが 1 つしかない場合(集水域が 1 つの図面など)に備えて、スクリプトを実行できるようにしておきます。

[^9]: 入力がリストでない場合(つまり、単一のオブジェクトの場合)は、オブジェクトを唯一の項目とする新しいリストを作成します。

[^10]: リスト内の各オブジェクトのループを開始します。

[^11]: オブジェクト ID を取得して Dynamo オブジェクトを「アンラップ」します。

[^12]: AutoCAD データベースから「ラップされた」オブジェクトを取得します。オブジェクトを編集する予定がないため、OpenMode はここで `ForRead` に設定されます。単にデータを「クエリー」しているだけです。

[^13]: オブジェクトの入力リストには、集水域項目と非集水域項目が混在している可能性があります。この状況を確認して適切に処理する必要があります(つまり、項目が実際に集水域である場合にのみ、ループのこの反復を続行します)。

[^14]: スクリプトがここまで進むと、オブジェクトが実際に集水域であることが分かります。名前を分かりやすく整理するために、ここで新しい変数を追加します。

[^15]: API ドキュメントで先ほど参照した適切なプロパティを使用して、集水域境界点を取得します。前述したように、これにより、基本的に AutoCAD の点のリストである `Point3dCollection` オブジェクトが得られます。役に立つように、これらの点を Dynamo の点に「変換」する必要があります。

[^16]: この集水域の境界点を保存する空のリストを作成します。

[^17]: `Point3dCollection` 内の各 `Point3d` オブジェクトに対してループを開始します。

[^18]: AutoCAD の点の座標を使用して Dynamo の点を作成します。

[^19]: リストに点を追加します。

[^20]: すべての AutoCAD の点を Dynamo の点に「変換」したら、結果のリストを出力に追加します。この後、外側のループは入力リスト内の次のオブジェクトに移動します。

[^21]: この行のコメントを解除してトランザクションをコミットします。

[^22]:最後に、Dynamo の点のリストを出力します。
