# Zero-Touch ノードで Python スクリプトを実行する(C#)

### Zero-Touch ノードで Python スクリプトを実行する(C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Python でのスクリプトの記述に慣れているユーザが、Dynamo の標準的な Python ノード以外の機能を必要とする場合は、Zero-Touch を使用して独自のノードを作成できます。最初に紹介する簡単な例では、Python スクリプトを文字列として Zero-Touch ノードに渡すと、スクリプトが実行されて結果が返されます。このケース スタディは、「スタートアップ」セクションの説明と例に基づいているため、Zero-Touch ノードを初めて作成する場合はそちらを参照してください。

![Python スクリプトの文字列を実行する Zero-Touch ノード](../../.gitbook/assets/python-case-study.png)

> Python スクリプトの文字列を実行する Zero-Touch ノード

#### Python エンジン <a href="#python-engine" id="python-engine"></a>

PythonNet3 が既定のエンジンになり、CPython から移行する場合のエクスペリエンスがよりスムーズになりました。Dynamo 4.0+ で作成されたすべての新しい Python ノードは、PythonNet3 で開始されます。

このノードは、IronPython スクリプト エンジンのインスタンスに依存します。これを行うには、いくつかの追加のアセンブリを参照する必要があります。Visual Studio で基本的なテンプレートを設定するには、次の手順を実行します。

* 新しい Visual Studio クラス プロジェクトを作成します。
* `C:\Program Files (x86)\IronPython 2.7\IronPython.dll` にある `IronPython.dll` への参照を追加します。
* `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll` にある `Microsoft.Scripting.dll` への参照を追加します。
* クラスに `IronPython.Hosting` および `Microsoft.Scripting.Hosting` の `using` ステートメントを含めます。
* パッケージを含む Dynamo ライブラリに追加のノードが作成されないように、空のプライベート コンストラクタを追加します。
* 単一の文字列を入力パラメータとして受け取る新しいメソッドを作成します。
* このメソッド内で、新しい Python エンジンをインスタンス化し、スクリプトの空のスコープを作成します。このスコープは、Python インタプリタのインスタンス内のグローバル変数と考えることができます。
* 次に、エンジンで `Execute` を呼び出して入力文字列とスコープをパラメータとして渡します。
* 最後に、スコープで `GetVariable` を呼び出し、返そうとしている値を含む変数の名前を Python スクリプトから渡して、スクリプトの結果を取得して返します。(詳細については、下の例を参照してください)

次のコードは、上記の手順の例を示しています。ソリューションをビルドすると、プロジェクトの bin フォルダに新しい `.dll` が作成されます。この `.dll` をパッケージの一部として、または `File < Import Library...` にナビゲートして Dynamo に読み込むことができます。

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Python スクリプトが変数 `output` を返すため、Python スクリプトには `output` という変数が必要です。このサンプル スクリプトを使用して、Dynamo でノードをテストします。Dynamo で Python ノードを使用したことがある場合は、同じように操作できます。詳細については、[Primer の「Python」のセクション](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python)を確認してください。

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### 複数の出力 <a href="#multiple-outputs" id="multiple-outputs"></a>

標準の Python ノードの制限事項として、出力ポートは 1 つしかありません。そのため、複数のオブジェクトを返す場合は、リストを作成してその中で各オブジェクトを取得する必要があります。ディクショナリを返すように上記の例を変更すると、必要な数だけ出力ポートを追加できます。ディクショナリの詳細については、「Zero-Touch の詳細を確認する」の「複数の値を返す」セクションを参照してください。

![このノードを使用すると、直方体の体積と図心の両方を返すことができます。](../../.gitbook/assets/python-multi-case-study.png)

> このノードを使用すると、直方体の体積と図心の両方を返すことができます。

次の手順で前の例を変更します。

* NuGet パッケージ マネージャから `DynamoServices.dll` への参照を追加します。
* 以前のアセンブリに加えて、`System.Collections.Generic` および `Autodesk.DesignScript.Runtime` を含めます。
* 出力を含むディクショナリを返すように、メソッドの戻り値のタイプを変更します。
* 各出力は、スコープから個別に取得する必要があります(大きな出力セットに対して単純なループを設定することを検討してください)。

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

また、サンプルの Python スクリプトに出力変数(`output2`)を追加しています。これらの変数では任意の Python 命名規則を使用することができ、この例ではわかりやすくするために、厳密に output を使用しました。

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### PythonNet3 の既知の制限事項と回避策<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

PythonNet3 を使用する場合の既知の制限事項と回避策を次に示します。

* .NET コレクションは、Python リストに自動的に変換されません
    * ```len()```、インデックス付け、または反復を使用する前に、```list(...)``` を使用して、```.NET``` 配列またはコレクションを明示的に変換する必要があります。
* 一般的な .NET メソッドでは、明示的なタイプ パラメータが必要になる場合があります
    * 一部のメソッド(```GroupBy``` など)は、自動推論に依存する代わりに汎用的なタイプを手動で指定しない限り失敗します。
* 拡張メソッドは、```dir()``` またはオート コンプリートでは検出できません
    * 拡張メソッドは、明示的に呼び出されたときには引き続き機能する可能性がありますが、イントロスペクションやコード補完には表示されません。
* DataTable 拡張メソッドはサポートされていません
    * ```System.Data.DataTableExtensions``` の読み込みは失敗します。これらのヘルパー メソッドは直接使用できません。
* 一部の Dynamo コア メソッドは、PythonNet3 で異なる動作をします
    * 特定の関数(リストの平坦化など)は、厳密なコレクション処理により、期待どおりに動作しないことがあります。
* Python クラスが .NET タイプから継承されている場合、ノード間で Python クラスを渡すことはできません
    * .NET タイプまたはインタフェースから派生したクラスは、Python ノード間で安全に転送することはできません。
* Python の ```set()``` は一部の .NET オブジェクトを受け入れません
    * ```InvalidElementId``` のようなオブジェクトは、フィルタで除外するか、代わりに .NET コレクションを使用して処理する必要があります。
* 頻繁な ```print()``` 呼び出しは、メモリの増加を引き起こす可能性があります
    * ループ内の ```print()``` や実行時間の長いスクリプトを、頻繁に使用することは避けてください。
* Dynamo と Python 間のディクショナリの相互運用性は制限されています
    * Dynamo ディクショナリと Python ディクショナリは完全には互換性がないため、手動で変換しなければならない場合があります。
* 指定したオブジェクトの実行中の COM インスタンスを取得する ```Marshal.GetActiveObject()``` メソッドは使用できなくなりました
    * 使用しているファイルのパスがわかっている場合は、```BindToMoniker``` を使用します。
    * クラス構造 ```Marshal.GetActiveObject()``` を使用して C# でライブラリをコーディングします。

#### CPython3 から PythonNet3 への移行<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo は、CPython ノードを PythonNet 3 に自動的に移行します。次の処理が行われます。

> 1. 元のファイルのバックアップ コピーが自動的に作成されます。
> 2. すべての CPython ノード(CPython を使用するカスタム ノードを含む)が PythonNet3 に変換されます。 
> 3. トースト通知により、移行されたノードの数を把握することができます。
> 4. 保存すると、Python ノードが PythonNet3 を使用するようになったことを通知するメッセージが表示されます。ここでも下位互換性について心配する必要はありません。マルチバージョンのショップ(Revit や Civil 3D の 2025/2026 など)で作業する場合は、互換性を維持するために Dynamo 3.3 ～ 3.6 に PythonNet3 Engine パッケージをインストールしてください。 

#### IronPython2 から PythonNet3 への移行<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

グラフで IronPython エンジンを使用している場合は、自動的に移行されることはありません。 

一致する IronPython パッケージがインストールされている場合、グラフは正常に実行されます。見つからない場合は、ワークスペース参照拡張機能に依存関係の警告が表示され、パッケージをダウンロードするように求められます。パッケージを再インストールすることで、IronPython を引き続き使用することができます。ただし、IronPython は何年も更新されておらず、Dynamo は長い間 Dynamo でこれらのエンジンを積極的にサポートしていないため、今後もグラフを確実に動作させるためには、PythonNet3 に移行することを強くお勧めします。DynamoIronPython2.7 と DynamoIronPython3 は、引き続き Dynamo Package Manager のパッケージとして使用できますが、Dynamo チームによるメンテナンスは行われなくなります。 

この場合、実行可能な移行方法は、Python Editor 内で使用可能な移行アシスタントを使用したノード対ノードの移行です。  

移行の詳細については、[このブログ](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/)を参照してください。