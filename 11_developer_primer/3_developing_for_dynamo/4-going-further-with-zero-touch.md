# Zero-Touch の詳細を確認する

Zero-Touch プロジェクトの作成方法を理解できたので、Dynamo GitHub にあるサンプル ZeroTouchEssentials を使用して、ノードの作成について詳しく説明します。

![Zero-Touch ノード](images/ootbzerotouch.png)

> Dynamo の標準ノードの多くは基本的な Zero-Touch ノードです。上記のほとんどの Math、Color、DateTime ノードと同様です。

まず、[https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials) から ZeroTouchEssentials プロジェクトをダウンロードします。

Visual Studio で、ソリューション ファイル `ZeroTouchEssentials.sln` を開いてソリューションをビルドします。

![Visual Studio の ZeroTouchEssentials](images/vs-build-zte.jpg)

> `ZeroTouchEssentials.cs` ファイルには、Dynamo に読み込むメソッドがすべて含まれています。

Dynamo を開いて `ZeroTouchEssentials.dll` を読み込み、次の例で参照するノードを取得します。

コード サンプルは [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs) から取得され、通常は一致します。XML ドキュメントは簡潔にするために削除されています。各コード サンプルは上のイメージのノードを作成します。

#### 既定の入力値 <a href="#default-input-values" id="default-input-values"></a>

Dynamo では、ノードの入力ポートに対する既定値の定義がサポートされています。これらの既定値は、ポートに接続がない場合にノードに渡されます。既定では、「[C# プログラミング ガイド](https://msdn.microsoft.com/en-us/library/dd264739.aspx)」のオプションの引数を指定する C# メカニズムを使用して表します。既定では次のように指定します。

* メソッドのパラメータを既定値 `inputNumber = 2.0` に設定します

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![既定値](images/defaultval.jpg)

> 1. ノードの入力ポートにカーソルを合わせると、既定値が表示されます。

#### 複数の値を返す <a href="#returning-multiple-values" id="returning-multiple-values"></a>

複数の値を返すのは、複数の入力を作成するよりも少し複雑で、ディクショナリを使用して返す必要があります。ディクショナリのエントリはノードの出力側のポートになります。戻り値の複数のポートは次のように作成します。

* `using System.Collections.Generic;` を追加して `Dictionary<>` を使用します。
* `using Autodesk.DesignScript.Runtime;` を追加して、`MultiReturn` 属性を使用します。これは、DynamoServices NuGet パッケージから「DynamoServices.dll」を参照します。
* `[MultiReturn(new[] { "string1", "string2", ... more strings here })]` 属性をメソッドに追加します。文字列はディクショナリ内のキーを参照して、出力ポート名になります。
* 属性のパラメータ名に一致するキーを使用して関数から `Dictionary<>` を返します。`return new Dictionary<string, object>`

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70) でこのコード サンプルを参照してください。

複数の出力を返すノードです。

![複数の出力](images/multipleoutputs.png)

> 1. ディクショナリのキーに入力した文字列に従って、2 つの出力ポートに名前が付けられています。

#### ドキュメント、ツールチップ、検索 <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

ノードの関数、入力、出力、検索タグなどを記述したドキュメントを Dynamo ノードに追加することをお勧めします。追加するには、XML ドキュメント タグを使用します。XML ドキュメントは次のように作成されます。

* 3 つのスラッシュが先頭に付いたすべてのコメント テキストは、ドキュメントとみなされます
  * 例: `/// Documentation text and XML goes here`
* 3 つのスラッシュの後に、.dll の読み込み時に Dynamo が読み込むメソッドの上に XML タグを作成します。
  * 例: `/// <summary>...</summary>`
* `Project > Project Properties > Build` を選択して `XML documentation file` をオンにすることで、Visual Studio で XML ドキュメントを有効にします。

![XML ファイルを生成する](images/vs-xml.jpg)

> 1. Visual Studio は、指定された場所に XML ファイルを生成します。

タグのタイプは次のとおりです。

* `/// <summary>...</summary>` は、ノードのメインのドキュメントであり、左側の検索サイド バーでノードの上にツールチップとして表示されます
* `/// <param name="inputName">...</param>` は、特定の入力パラメータのドキュメントを作成します。
* `/// <returns>...</returns>` は、出力パラメータのドキュメントを作成します。
* `/// <returns name = "outputName">...</returns>` は、複数の出力パラメータのドキュメントを作成します。
* `/// <search>...</search>` は、カンマ区切りのリストに基づいて、ノードを検索結果と一致させます。たとえば、メッシュを再分割するノードを作成する場合は、「mesh」、「subdivision」、「catmull-clark」などのタグを追加します。

入力と出力の説明と、ライブラリに表示される概要を含むサンプル ノードを次に示します。

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80) でこのコード サンプルを参照してください。

このサンプル ノードのコードには次が含まれています。

> 1. ノードの概要。
> 2. 入力の説明。
> 3. 出力の説明。

#### オブジェクト <a href="#objects" id="objects"></a>

Dynamo には `new` キーワードがないため、静的な作成メソッドを使用してオブジェクトを作成する必要があります。オブジェクトは次のように作成されます。

* 他に必要のない限り、コンストラクタの内部を `internal ZeroTouchEssentials()` にします。
* `public static ZeroTouchEssentials ByTwoDoubles(a, b)` のような静的メソッドを使用してオブジェクトを作成します。

> 注: Dynamo では、静的メソッドがコンストラクタであることを示すために接頭辞「By」が使用されます。これは省略可能ですが、「By」を使用すると、ライブラリを Dynamo の既存のスタイルに合わせることができます。

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26) でこのコード サンプルを参照してください。

ZeroTouchEssentials dll が読み込まれると、ライブラリに ZeroTouchEssentials ノードが追加されます。このオブジェクトは、`ByTwoDoubles` ノードを使用して作成できます。

![ByTwoDoubles ノード](images/dyn-constructor.jpg)

#### Dynamo のジオメトリ タイプを使用する <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

Dynamo ライブラリでは、ネイティブの Dynamo ジオメトリ タイプを入力として使用し、新しいジオメトリを出力として作成できます。ジオメトリ タイプは次のように作成されます。

* C# ファイルの先頭に `using Autodesk.DesignScript.Geometry;` を追加し、プロジェクトに ZeroTouchLibrary NuGet パッケージを追加して、プロジェクトで「ProtoGeometry.dll」を参照します。
* **重要:** 関数から返されないジオメトリ リソースを管理するには、下の「**Dispose/using ステートメント**」セクションを参照してください。

> 注: Dynamo ジオメトリ オブジェクトは、関数に渡されるその他のオブジェクトと同様に使用されます。

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86) でこのコード サンプルを参照してください。

曲線の長さを取得し、その値を倍にするノード。

![曲線の入力](images/doublelength.png)

> 1. このノードは、入力として Curve ジオメトリタイプを受け取ります。

#### Dispose/using ステートメント <a href="#disposeusing-statements" id="disposeusing-statements"></a>

関数から返されないジオメトリ リソースは、Dynamo バージョン 2.5 以降を使用していない限り、手動で管理する必要があります。Dynamo 2.5 以降のバージョンでは、ジオメトリ リソースはシステムによって内部で処理されますが、複雑な使用事例がある場合や決まった時間にメモリの使用を削減する必要がある場合には、ジオメトリを手動で破棄する必要があります。Dynamo エンジンは、関数から返されるすべてのジオメトリ リソースを処理します。返されないジオメトリ リソースは、次のように手動で処理できます。

*   using ステートメントを使用する場合は次のようになります。

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > using ステートメントについては、[こちら](https://msdn.microsoft.com/en-us/library/yh598w02.aspx)で説明しています。
    >
    > Dynamo 2.5 で導入された安定性に関する新機能の詳細については、「[Dynamo ジオメトリの安定性の向上](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297)」を参照してください
*   手動の Dispose の呼び出しを使用する場合は次のようになります。

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

#### マイグレーション <a href="#migrations" id="migrations"></a>

新しいバージョンのライブラリをパブリッシュすると、ノード名が変更される場合があります。名前の変更をマイグレーション ファイルで指定できるため、旧バージョンのライブラリで作成されたグラフが更新されても正しく動作し続けます。マイグレーションは次のように実装されます。

* `.dll` と同じフォルダに、「BaseDLLName」.Migrations.xml という形式で `.xml` ファイルを作成します。
* `.xml` で、単一の `<migrations>...</migrations>` 要素を作成します。
* マイグレーション要素内で、名前の変更ごとに `<priorNameHint>...</priorNameHint>` 要素を作成します。
* 名前の変更ごとに、`<oldName>...</oldName>` および `<newName>...</newName>` 要素を指定します。

![マイグレーション ファイル](images/vs-migrations-file.jpg)

> 1. 右クリックして、`Add > New Item` を選択します。
> 2. `XML File` を選びます。
> 3. このプロジェクトでは、マイグレーション ファイルに `ZeroTouchEssentials.Migrations.xml` という名前を付けます。

このサンプル コードでは、`GetClosestPoint` という名前のノードが `ClosestPointTo` に変更されたことを Dynamo に伝えています。

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml) でこのコード サンプルを参照してください。

#### ジェネリクス <a href="#generics" id="generics"></a>

Zero-Touch は現在、ジェネリクスの使用をサポートしていません。使用できますが、タイプが設定されていない場合には直接読み込まれるコードで使用できません。汎用的でタイプが設定されていないメソッド、プロパティ、またはクラスは公開できません。

下の例では、タイプ `T` の Zero-Touch ノードは読み込まれません。ライブラリの残りの部分を Dynamo に読み込むと、タイプの例外が失われます。

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

次の例では、タイプを設定して汎用的なタイプを使用すると、Dynamo に読み込まれます。

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
