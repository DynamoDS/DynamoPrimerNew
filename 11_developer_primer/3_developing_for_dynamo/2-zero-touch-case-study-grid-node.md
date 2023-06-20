# Zero-Touch ケース スタディ - グリッド ノード 

Visual Studio プロジェクトを開始できたので、セルの矩形グリッドを作成するカスタム ノードのビルド方法について説明します。矩形グリッドは複数の標準ノードを使用して作成できますが、このカスタム ノードは Zero-Touch ノードに簡単に含めることができる便利なツールです。通芯とは異なり、セルは、中心点を基点にスケールを変更したり、コーナーの頂点がクエリーされたり、面を構成することができます。

この例では、Zero-Touch ノードを作成する際に注意するべき機能と概念を紹介します。カスタム ノードをビルドして Dynamo に追加した後は、「Zero-Touch の詳細を確認する」ページの、「既定の入力値」、「複数の値を返す」、「ドキュメント」、「オブジェクト」、「Dynamo のジオメトリ タイプを使用する」、および「マイグレーション」セクションで詳細を確認してください。

![矩形グリッドのグラフ](images/cover-image.jpg)

#### カスタムの矩形グリッド ノード <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

グリッド ノードのビルドを開始するには、新しい Visual Studio クラス ライブラリ プロジェクトを作成します。プロジェクトの設定方法の詳細な説明については、「スタートアップ」ページを参照してください。

![Visual Studio で新しいプロジェクトを作成する](images/vs-new-project-1.jpg)

![Visual Studio で新しいプロジェクトを構成する](images/vs-new-project-2.jpg)

> 1. プロジェクト タイプとして `Class Library` を選択します
> 2. プロジェクトに `CustomNodes` という名前を付けます

ジオメトリを作成するため、適切な NuGet パッケージを参照する必要があります。NuGet パッケージ マネージャから ZeroTouchLibrary パッケージをインストールします。このパッケージは、`using Autodesk.DesignScript.Geometry;` ステートメントに必要です。

![ZeroTouchLibrary パッケージ](images/vs-nugetpackage.jpg)

> 1. ZeroTouchLibrary パッケージを参照します。
> 2. このノードを Dynamo Studio の現在のビルド(1.3)で使用します。これに一致するパッケージ バージョンを選択します。
> 3. クラス ファイルの名前を `Grids.cs` に変更しています。

次に、RectangularGrid メソッドを配置する名前空間とクラスを設定する必要があります。Dynamo では、メソッド名とクラス名に基づいてノードに名前が付けられます。これを Visual Studio にコピーする必要はまだありません。

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
        //The method for creating a rectangular grid will live in here
        }
    }
}
```

> `Autodesk.DesignScript.Geometry;` で、ZeroTouchLibrary パッケージにある ProtoGeometry.dll を参照します。`System.Collections.Generic` はリストの作成に必要です。

これで、矩形を描画するためのメソッドを追加できます。クラス ファイルを次のようにして、Visual Studio にコピーします。

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                }
            }
            return pList;
        }
    }
}
```

プロジェクトが次のように表示されている場合は、先に進んで `.dll` をビルドしてみます。

![DLL をビルドする](images/vs-grids.jpg)

> 1. Build > Build Solution を選択します。

プロジェクトの `bin` フォルダに `.dll` があるか確認します。正常にビルドされている場合は、Dynamo に `.dll` を追加できます。

![Dynamo のカスタム ノード](images/RectangularGrid-Dynamo.jpg)

> 1. Dynamo ライブラリ内のカスタム RectangularGrids ノード。
> 2. キャンバス上のカスタム ノード。
> 3. Dynamo に `.dll` を追加するための[追加]ボタン。

#### カスタム ノードの修正 <a href="#custom-node-modifications" id="custom-node-modifications"></a>

上記の例では、`RectangularGrids` メソッド以外の定義があまりないかなり単純なノードを作成しました。しかし、入力ポートのツールチップを作成したり、標準の Dynamo ノードと同様にノードの概要を設定することができます。これらの機能をカスタム ノードに追加すると、特にライブラリで検索する場合に、そのカスタム ノードを簡単に使用できるようになります。

![入力のツールチップ](images/nodemodification.png)

> 1. 既定の入力値
> 2. xCount 入力のツールチップ

RectangularGrid ノードには、このような基本的な機能が必要です。次のコードでは、入力と出力ポートの説明、概要、および既定の入力値を追加します。

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }
            return pList;
        }
    }
}
```

* メソッドのパラメータに値を割り当てて、入力の既定値を指定します。`RectangularGrid(int xCount = 10, int yCount = 10)`
* XML ドキュメントの先頭に `///` を付けて、入力および出力のツールチップ、検索キーワード、および概要を作成します。

ツールチップを追加するには、プロジェクト フォルダに xml ファイルが必要です。オプションを有効にすると、Visual Studio で `.xml` が自動的に生成されます。

![XML ドキュメントを有効にする](images/vs-xml.jpg)

> 1. ここで XML ドキュメント ファイルを有効にして、ファイル パスを指定します。これにより、XML ファイルが生成されます。

以上で完了です。いくつかの標準機能を含む新しいノードを作成しました。次の章「Zero-Touch の基本」では、Zero-Touch ノードの開発、および注意すべき問題について詳しく説明します。
