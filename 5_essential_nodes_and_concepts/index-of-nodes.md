# ノードの索引

この索引では、この手引で言及しているすべてのノードと他の便利なコンポーネントについて、補足情報を提供します。ここで紹介するのは、Dynamo で使用できる 500 個のノードのうち一部にすぎません。

## Display

### Color ノード

|                                                  |                                                                                                                       |                                                                 |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
|                                                  | CREATE                                                                                                                |                                                                 |
| ![](images/ColorbyARGB.jpg)          | <p><strong>Color.ByARGB</strong><br>アルファ、赤、緑、青の各成分から色を作成します。</p>                  | ![](<images/index of nodes - color byargb.jpg>)     |
| ![](<images/Color Range.jpg>)        | <p><strong>Color Range</strong><br>開始色と終了色間の色のグラデーションから色を取得します。</p>      | ![](<images/Color Range.jpg>)        |
|                                                  | ACTIONS                                                                                                               |                                                                 |
| ![](images/ColorBrightness.jpg)      | <p><strong>Color.Brightness</strong><br>色の明度の値を取得します。</p>                                 | ![](images/ColorBrightness.jpg)   |
| ![](<images/ColorComponent.jpg>) | <p><strong>Color.Components</strong><br>色の各成分を、アルファ、赤、緑、青の順のリストとして返します。</p> | ![](<images/index of nodes - color component.jpg>)  |
| ![](images/ColorSaturation.jpg)      | <p><strong>Color.Saturation</strong><br>色の彩度の値を取得します。</p>                                  | ![](<images/index of nodes - color saturation.jpg>) |
| ![](images/ColorHue.jpg)             | <p><strong>Color.Hue</strong><br>色の色相の値を取得します。</p>                                               | ![](<images/index of nodes - color hue.jpg>)        |
|                                                  | QUERY                                                                                                                 |                                                                 |
| ![](<images/ColorAlpha.jpg>)     | <p><strong>Color.Alpha</strong><br>色のアルファ成分の値(0 ～ 255)を取得します。</p>                                 | ![](<images/index of nodes - color alpha.jpg>)      |
| ![](images/ColorBlue.jpg)            | <p><strong>Color.Blue</strong><br>色の青色成分の値(0 ～ 255)を取得します。</p>                                   | ![](<images/index of nodes - color blue.jpg>)       |
| ![](images/ColorGreen.jpg)           | <p><strong>Color.Green</strong><br>色の緑色成分の値(0 ～ 255)を取得します。</p>                                 | ![](<images/index of nodes - color green.jpg>)      |
| ![](images/ColorRed.jpg)             | <p><strong>Color.Red</strong><br>色の赤色成分の値(0 ～ 255)を取得します。</p>                                     | ![](<images/index of nodes - color red.jpg>)        |

|                                                               |                                                                                           |                                                                                 |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
|                                                               | CREATE                                                                                    |                                                                                 |
| ![](<images/index of nodes - geometry color by geometry color.jpg>) | <p><strong>GeometryColor.ByGeometryColor</strong><br>任意の色を使用してジオメトリを表示します。</p> | ![](<images/index of nodes - geometry color by geometry color.jpg>) |

### Watch ノード

|                                             |                                                                               |                                                                 |
| ------------------------------------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------- |
|                                             | ACTIONS                                                                       |                                                                 |
| ![](<images/View watch.jpg>)    | <p><strong>View.Watch</strong><br>ノードの出力を視覚化します。</p>           | ![](<images/index of nodes - view watch.jpg>)       |
| ![](<images/View watch 3d.jpg>) | <p><strong>View.Watch 3D</strong><br>ジオメトリのダイナミック プレビューを表示します。</p> | ![](<images/index of nodes - view watch.3Djpg.jpg>) |

## 入力

|                                              |                                                                                                          |                                                               |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
|                                              | ACTIONS                                                                                                  |                                                               |
| ![](images/Boolean.jpg)          | <p><strong>Boolean</strong><br>True と False のいずれかを選択します。</p>                                   | ![](<images/index of nodes - boolean.jpg>)        |
| ![](<images/CodeBlock.jpg>)  | <p><strong>Code Block</strong><br>DesignScript のコードを直接作成することができます。</p>              | ![](<images/index of nodes - code block.jpg>)     |
| ![](<images/Directory Path.jpg>) | <p><strong>フォルダ パス</strong><br>システム上で任意のフォルダを選択して、そのパスを取得することができます。</p> | ![](<images/index of nodes - directory path.jpg>) |
| ![](<images/File Path.jpg>)      | <p><strong>ファイル パス</strong><br>システム上で任意のファイルを選択して、そのファイル名を取得することができます。</p>       | ![](<images/index of nodes - file path.jpg>)      |
| ![](<images/Integer slider.jpg>) | <p><strong>整数スライダ</strong><br>整数値を生成するスライダです。</p>                         | ![](<images/index of nodes - integer slider.jpg>) |
| ![](images/number.jpg)           | <p><strong>Number</strong><br>数値を作成します。</p>                                                      | ![](images/number.jpg)          |
| ![](<images/Number slider.jpg>)  | <p><strong>数値スライダ</strong><br>数値を生成するスライダです。</p>                          | ![](<images/index of nodes - number slider.jpg>)  |
| ![](images/string.jpg)           | <p><strong>String</strong><br>文字列を作成します。</p>                                                      | ![](<images/index of nodes - string.jpg>)         |
| ![](<images/Object is Null.jpg>) | <p><strong>Object.IsNull</strong><br>指定されたオブジェクトが NULL であるかどうかを判断します。</p>                         | ![](<images/index of nodes - object is null.jpg>) |

## List ノード

|                                                          |                                                                                                                                                                                                                                               |                                                                         |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
|                                                          | CREATE                                                                                                                                                                                                                                        |                                                                         |
| ![](<images/List Create.jpg>)                | <p><strong>List.Create</strong><br>与えられた入力に基づいて新しいリストを作成します。</p>                                                                                                                                                              | ![](<images/index of nodes - list create.jpg>)          |
| ![](<images/List Combine.jpg>)               | <p><strong>List.Combine</strong><br>2 つのシーケンスの各要素にコンビネータを適用します。</p>                                                                                                                                                 | ![](<images/index of nodes - list combine.jpg>)             |
| ![](images/Range.jpg)                        | <p><strong>Number Range</strong><br>指定された範囲内で数値のシーケンスを作成します。</p>                                                                                                                                                  | ![](images/Range.jpg)                     |
| ![](images/Sequence.jpg)                     | <p><strong>Number Sequence</strong><br>数値のシーケンスを作成します。</p>                                                                                                                                                                     | ![](<images/index of nodes - sequence.jpg>)                 |
|                                                          | ACTIONS                                                                                                                                                                                                                                       |                                                                         |
| ![](<images/List Chop.jpg>)                  | <p><strong>List.Chop</strong><br>リストを、それぞれ指定された個数の項目から成るリストの集合に分割します。</p>                                                                                                                               | ![](<images/index of nodes - list chop.jpg>)                |
| ![](<images/index of nodes - count.jpg>)                   | <p><strong>List.Count</strong><br>指定されたリストに格納されている項目の数を返します。</p>                                                                                                                                                   | ![](<images/index of nodes - count.jpg>)                    |
| ![](<images/List Flatten.jpg>)               | <p><strong>List.Flatten</strong><br>ネストされたリストのリストを、指定された量だけフラットにします。</p>                                                                                                                                                  | ![](<images/index of nodes - list flatten.jpg>)             |
| ![](<images/List Filter by Bool Mask.jpg>)   | <p><strong>List.FilterByBoolMask</strong><br>別個のブール値を要素に持つリスト内で対応するインデックスを検索して、シーケンスをフィルタします。</p>                                                                                                       | ![](<images/index of nodes - list filter by bool mask.jpg>) |
| ![](<images/List Get Item At Index.jpg>)     | <p><strong>List.GetItemAtIndex</strong><br>リストの、指定されたインデックスにある項目を取得します。</p>                                                                                                                        | ![](<images/index of nodes - list get item at index.jpg>)   |
|                                                          | <p><strong>List.Map</strong><br>リスト内のすべての要素に関数を適用し、その結果から新しいリストを生成します。</p>                                                                                                                    | ![](<images/index of nodes - list map.jpg>)                 |
|                                                          | <p><strong>List.Reverse</strong><br>指定されたリスト内の項目を逆順で含む新しいリストを作成します。</p>                                                                                                                        | ![](<images/index of nodes - list reverse.jpg>)             |
| ![](<images/List Replace Item At Index.jpg>) | <p><strong>List.ReplaceItemAtIndex</strong><br>リストの、指定されたインデックスにある項目を置き換えます。</p>                                                                                                                  | ![](<images/index of nodes - replace item at index.jpg>)    |
| ![](<images/List Shift Indices.jpg>)         | <p><strong>List.ShiftIndices</strong><br>リスト内のインデックスを、指定された量だけ右に移動します。</p>                                                                                                                                      | ![](<images/index of nodes - list shift indices.jpg>)       |
| ![](<images/List Take Every Nth Item.jpg>)   | <p><strong>List.TakeEveryNthItem</strong><br>指定されたオフセットの後、指定された値の倍数であるインデックスの項目を、指定されたリストから取得します。</p>                                                                                  | ![](<images/index of nodes - list take every nth item.jpg>) |
| ![](<images/List Transpose.jpg>)             | <p><strong>List.Transpose</strong><br>任意のリストのリストの行と列を入れ替えます。他の行よりも短い行がある場合は、作成される配列が常に長方形になるように、プレースホルダーとして NULL 値が挿入されます。</p> | ![](<images/index of nodes - list transpose.jpg>)           |

## ロジック

|                                |                                                                                                                                                                                                              |                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- |
|                                | ACTIONS                                                                                                                                                                                                      |                                                   |
| ![](images/If.jpg) | <p><strong>If</strong><br>条件ステートメントです。テスト入力のブール値をチェックします。テスト入力が true である場合は、結果として true の入力を出力します。false である場合は、結果として false の入力を出力します。</p> | ![](<images/index of nodes - if.jpg>) |

## 数学的方法

|                                                       |                                                                                                                            |                                                                        |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
|                                                       | ACTIONS                                                                                                                    |                                                                        |
| ![](<images/Math cos.jpg>)                | <p><strong>Math.Cos</strong><br>角度の余弦を求めます。</p>                                                          | ![](<images/index of nodes - math cos.jpg>)                |
| ![](<images/Math degrees to radians.jpg>) | <p><strong>Math.DegreesToRadians</strong><br>度単位の角度をラジアン単位の角度に変換します。</p>                      | ![](<images/index of nodes - math degrees to radians.jpg>) |
| ![](<images/Math pow.jpg>)                | <p><strong>Math.Pow</strong><br>指定された指数に対して値を累乗します。</p>                                                | ![](<images/index of nodes - math pow.jpg>)                |
| ![](<images/Math radians to degrees.jpg>) | <p><strong>Math.RadiansToDegrees</strong><br>ラジアン単位の角度を度単位の角度に変換します。</p>                      | ![](<images/index of nodes - math radians to degrees.jpg>) |
| ![](<images/Math remap range.jpg>)        | <p><strong>Math.RemapRange</strong><br>分布比率を保持しながら数値のリストの範囲を調整します。</p> | ![](<images/index of nodes - math remap range.jpg>)        |
| ![](<images/Math sin.jpg>)                | <p><strong>Math.Sin</strong><br>角度の正弦を求めます。</p>                                                            | ![](<images/index of nodes - math sin.jpg>)                |
| ![](images/Map.jpg)                  | <p><strong>Map</strong><br>値を入力された範囲にマッピングします。</p>                                                            | ![](<images/index of nodes - math map.jpg>)                |

## 文字列

|                                                |                                                                                                                                                      |                                                                 |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
|                                                | ACTIONS                                                                                                                                              |                                                                 |
| ![](<images/String concat.jpg>)    | <p><strong>String.Concat</strong><br>複数の文字列を 1 つの文字列に連結します。</p>                                                         | ![](<images/index of nodes - string concat.jpg>)    |
| ![](<images/String contains.jpg>)  | <p><strong>String.Contains</strong><br>指定された文字列に指定されたサブストリングが含まれているかどうかを判断します。</p>                                              | ![](<images/index of nodes - string contains.jpg>)  |
| ![](<images/String join.jpg>)      | <p><strong>String.Join</strong><br>複数の文字列を 1 つの文字列に連結し、結合されるそれぞれの文字列の間に区切り文字を挿入します。</p> | ![](<images/index of nodes - string join.jpg>)      |
| ![](<images/String split.jpg>)     | <p><strong>String.Split</strong><br>1 つの文字列を文字列のリストに分割します。指定された区切り文字によって分割場所が決定されます。</p>    | ![](<images/index of nodes - string split.jpg>)     |
| ![](<images/String to number.jpg>) | <p><strong>String.ToNumber</strong><br>文字列を整数または倍精度浮動小数点数に変換します。</p>                                                              | ![](<images/index of nodes - string to number.jpg>) |

## ジオメトリ

### Circle ノード

|                                                             |                                                                                                                                                          |                                                                                     |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
|                                                             | CREATE                                                                                                                                                   |                                                                                     |
| ![](<images/Circle by center point radius.jpg>) | <p><strong>Circle.ByCenterPointRadius</strong><br>入力された中心点と半径をワールド座標系の XY 平面に持ち、ワールド座標系の Z 軸を法線とする円を作成します。</p> | ![](<images/index of nodes - circle by center point radius normal.jpg>) |
| ![](<images/Circle by plane radius.jpg>)        | <p><strong>Circle.ByPlaneRadius</strong><br>入力された平面の基準点(ルート)に中心を持ち、指定された半径を持つ円を平面上に作成します。</p>  | ![](<images/index of nodes - circle by plane radius.jpg>)               |

|                                                                           |                                                                                                                                                                                                    |                                                                                              |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
|                                                                           | CREATE                                                                                                                                                                                             |                                                                                              |
| ![](<images/Coordinate system by origin.jpg>)                 | <p><strong>CoordinateSystem.ByOrigin</strong><br>入力された点に基準点を持ち、X 軸と Y 軸を WCS(ワールド座標系)の X 軸および Y 軸に設定した座標系を作成します。</p>                                               | ![](<images/index of nodes - coordinates system by origin.jpg>)                  |
| ![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) | <p><strong>CoordinateSystem.ByCylindricalCoordinates</strong><br>指定された座標系に対して、指定された円柱座標パラメータに基づいて座標系を作成します。</p> | ![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) |

### Cuboid ノード

|                                                                  |                                                                                                                                            |                                                                                    |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
|                                                                  | CREATE                                                                                                                                     |                                                                                    |
| ![](<images/Cuboid by length.jpg>)                   | <p><strong>Cuboid.ByLengths</strong><br>ワールド座標系の基準点を中心として、幅、長さ、高さを持つ直方体を作成します。</p>                        | ![](<images/index of nodes - cuboid by lengths.jpg>)                   |
| ![](<images/index of nodes - cuboid by lengths origin.jpg>)            | <p><strong>Cuboid.ByLengths</strong> (origin)</p><p>中心を入力された点に設定し、指定された幅、長さ、高さの直方体を作成します。</p> | ![](<images/index of nodes - cuboid by lengths origin.jpg>)            |
| ![](<images/index of nodes - cuboid by lengths coordinate system.jpg>) | <p><strong>Cuboid.ByLengths</strong> (coordinateSystem)</p><p>ワールド座標系の基準点を中心として、幅、長さ、高さを持つ直方体を作成します。</p>  | ![](<images/index of nodes - cuboid by lengths coordinate system.jpg>) |
| ![](<images/index of nodes - cuboid by corners.jpg>)                 | <p><strong>Cuboid.ByCorners</strong></p><p>lowPoint から highPoint までの範囲に広がる直方体を作成します。</p>                                      | ![](<images/index of nodes - cuboid by corners.jpg>)                   |
| ![](<images/Cuboid length.jpg>)                      | <p><strong>Cuboid.Length</strong></p><p>実際のワールド空間寸法ではなく、直方体の入力寸法を返します。**</p>           | ![](<images/index of nodes - cuboid length.jpg>)                       |
| ![](<images/index of nodes - cuboid width.jpg>)                     | <p><strong>Cuboid.Width</strong></p><p>実際のワールド空間寸法ではなく、直方体の入力寸法を返します。**</p>            | ![](<images/index of nodes - cuboid width.jpg>)                        |
| ![](<images/index of nodes - cuboid height.jpg>)                    | <p><strong>Cuboid.Height</strong></p><p>実際のワールド空間寸法ではなく、直方体の入力寸法を返します。**</p>           | ![](<images/index of nodes - cuboid height.jpg>)                       |
| ![](<images/Bounding box to cuboid.jpg>)             | <p><strong>BoundingBox.ToCuboid</strong></p><p>ソリッドの直方体として境界ボックスを取得します。</p>                                                  | ![](<images/index of nodes - bounding box to cuboid.jpg>)              |

{% hint style="warning" %} **つまり、直方体の幅(X 軸)の長さ 10 を作成し、それを X 軸で 2 倍のスケーリングを行う座標系に変換しても、幅は 10 のままです。ASM では、ボディの頂点を予測可能な順序で抽出することができないため、変換後に寸法を決定することはできません。{% endhint %}

### Curve ノード

|                                                        |                                                                                                                                                  |                                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
|                                                        | ACTIONS                                                                                                                                          |                                                                         |
| ![](<images/Curve extrude.jpg>)            | <p><strong>Curve.Extrude</strong> (distance)<br>法線ベクトルの方向に曲線を押し出します。</p>                                             | ![](<images/index of nodes - curve extrude.jpg>)            |
| ![](<images/Curve point at parameter.jpg>) | <p><strong>Curve.PointAtParameter</strong><br>StartParameter() から EndParameter() までの範囲の指定されたパラメータで曲線上の点を取得します。</p> | ![](<images/index of nodes - curve point at parameter.jpg>) |

### ジオメトリ モディファイア

|                                                        |                                                                                                                                    |                                                                         |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
|                                                        | ACTIONS                                                                                                                            |                                                                         |
| ![](<images/Geometry distance to.jpg>)     | <p><strong>Geometry.DistanceTo</strong><br>このジオメトリから別のジオメトリへの距離を取得します。</p>                                  | ![](<images/index of nodes - geometry distance to.jpg>)     |
| ![](<images/Geometry explode.jpg>)         | <p><strong>Geometry.Explode</strong><br>複合要素または分割されていない要素をコンポーネント パーツに分割します。</p>                | ![](<images/index of nodes - geometry explode.jpg>)         |
| ![](<images/Geometry import from SAT.jpg>) | <p><strong>Geometry.ImportFromSAT</strong><br>読み込まれたジオメトリのリストです。</p>                                                      | ![](<images/index of nodes - geometry import from sat.jpg>) |
| ![](<images/Geometry rotate.jpg>)          | <p><strong>Geometry.Rotate</strong> (basePlane)<br>平面の基準点と法線を中心にオブジェクトを指定された角度だけ回転させます。</p> | ![](<images/index of nodes - geometry rotate.jpg>)          |
| ![](<images/Geometry translate.jpg>)       | <p><strong>Geometry.Translate</strong><br>指定された方向に距離を指定して、ジオメトリ タイプを平行移動させます。</p>           | ![](<images/index of nodes - geometry translate.jpg>)       |

### Line

|                                                                    |                                                                                                                                                          |                                                                                     |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
|                                                                    | CREATE                                                                                                                                                   |                                                                                     |
| ![](<images/Line by best fit through points.jpg>)      | <p><strong>Line.ByBestFitThroughPoints</strong><br>点の散布図に最もよく近似する直線を作成します。</p>                                       | ![](<images/index of nodes - line by best fit through points.jpg>)      |
| ![](<images/Line by start point direction length.jpg>) | <p><strong>Line.ByStartPointDirectionLength</strong><br>開始点から始まり、ベクトルの向きに指定された長さだけ延長する線分を作成します。</p> | ![](<images/index of nodes - line by start point direction length.jpg>) |
| ![](<images/Linebystartpointendpoint.jpg>)         | <p><strong>Line.ByStartPointEndPoint</strong><br>入力された 2 点を端点とする線分を作成します。</p>                                                   | ![](<images/index of nodes - line by start point end point.jpg>)        |
| ![](<images/Line by tangency.jpg>)                     | <p><strong>Line.ByTangency</strong><br>入力された曲線に接し、曲線のパラメータで指定された点に位置する直線を作成します。</p>               | ![](<images/index of nodes - line by tangency.jpg>)                     |
|                                                                    | QUERY                                                                                                                                                    |                                                                                     |
| ![](<images/Line direction.jpg>)                       | <p><strong>Line.Direction</strong><br>曲線の方向を返します。</p>                                                                                    | ![](<images/index of nodes - line direction.jpg>)                       |

### NurbsCurve ノード

|                                                             |                                                                                                               |                                                                              |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
|                                                             | 作成                                                                                                        |                                                                              |
| ![](<images/Nurbs curve by control points.jpg>) | <p><strong>NurbsCurve.ByControlPoints</strong><br>明示的な制御点を使用して B スプライン曲線を作成します。</p> | ![](<images/index of nodes - nurbs curve by control points.jpg>) |
| ![](<images/Nurbs curve by points.jpg>)         | <p><strong>NurbsCurve.ByPoints</strong><br>点間を補間して B スプライン曲線を作成します。</p>          | ![](<images/index of nodes - nurbs curve by points.jpg>)         |

### NurbsSurface ノード

|                                                               |                                                                                                                                                                                            |                                                                                |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
|                                                               | 作成                                                                                                                                                                                     |                                                                                |
| ![](<images/Nurbs surface by control points.jpg>) | <p><strong>NurbsSurface.ByControlPoints</strong><br>明示的な制御点と指定された U 次数と V 次数を使用して NURBS 曲面 を作成します。</p>                                             | ![](<images/index of nodes - nurbs surface by control points.jpg>) |
| ![](<images/Nurbs surface by points.jpg>)         | <p><strong>NurbsSurface.ByPoints</strong><br>指定された補間される点、U 次数、V 次数を使用して NURBS 曲面を作成します。作成されるサーフェスはすべての指定された点を通過します。</p> | ![](<images/index of nodes - nurbs surface by points.jpg>)         |

### Plane

|                                                      |                                                                                                                  |                                                                       |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
|                                                      | CREATE                                                                                                           |                                                                       |
| ![](<images/Plane by origin normal.jpg>) | <p><strong>Plane.ByOriginNormal</strong><br>中心をルート点に持ち、入力された法線ベクトルを持つ平面を作成します。</p> | ![](<images/index of nodes - plane by origin normal.jpg>) |
| ![](<images/Plane XY.jpg>)               | <p><strong>Plane.XY</strong><br>ワールド座標系の XY に平面を作成します。</p>                                              | ![](<images/index of nodes - plane xy.jpg>)               |

### ポイント

|                                                              |                                                                                                                                           |                                                                               |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
|                                                              | CREATE                                                                                                                                    |                                                                               |
| ![](<images/Point by cartesian coordinates.jpg>) | <p><strong>Point.ByCartesianCoordinates</strong><br>指定された座標系と 3 つのデカルト座標で点を作成します。</p>          | ![](<images/index of nodes - point by cartesian coordinates.jpg>) |
| ![](<images/Point by coordinates 2D.jpg>)        | <p><strong>Point.ByCoordinates</strong> (2d)<br>指定された 2 つのデカルト座標を使用して、XY 平面に点を作成します。Z コンポーネントは 0 です。</p> | ![](<images/index of nodes - point by coordinates 2D.jpg>)        |
| ![](<images/Point by coordinates 3D.jpg>)        | <p><strong>Point.ByCoordinates</strong> (3d)<br>指定された 3 つのデカルト座標を使用して点を作成します。</p>                                           | ![](<images/index of nodes - point by coordinates 3D.jpg>)        |
| ![](<images/Point origin.jpg>)                   | <p><strong>Point.Origin</strong><br>基準点 (0,0,0)を取得します。</p>                                                                      | ![](<images/index of nodes - point origin.jpg>)                   |
|                                                              | ACTIONS                                                                                                                                   |                                                                               |
| ![](<images/Point add.jpg>)                      | <p><strong>Point.Add</strong><br>点にベクトルを追加します。Translate(Vector)と同じ操作です。</p>                                             | ![](<images/index of nodes - point add.jpg>)                      |
|                                                              | QUERY                                                                                                                                     |                                                                               |
| ![](<images/Point x.jpg>)                        | <p><strong>Point.X</strong><br>点の X 座標を取得します。</p>                                                                         | ![](<images/index of nodes - point x.jpg>)                        |
| ![](<images/Point y.jpg>)                        | <p><strong>Point.Y</strong><br>点の Y 座標を取得します。</p>                                                                         | ![](<images/index of nodes - point y.jpg>)                        |
| ![](<images/Point z.jpg>)                        | <p><strong>Point.Z</strong><br>点の Z 座標を取得します。</p>                                                                         | ![](<images/index of nodes - point z.jpg>)                        |

### Polycurve ノード

|                                                   |                                                                                                                                                                                       |                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
|                                                   | CREATE                                                                                                                                                                                |                                                                    |
| ![](<images/Polycurve by points.jpg>) | <p><strong>Polycurve.ByPoints</strong><br>点をつなげる線分のシーケンスからポリカーブを作成します。閉じた曲線を作成するには、最後の点の位置を始点の位置と同じにします。</p> | ![](<images/index of nodes - polycurve by points.jpg>) |

### 長方形

|                                                         |                                                                                                                                                                               |                                                                          |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
|                                                         | CREATE                                                                                                                                                                        |                                                                          |
| ![](<images/Rectangle by width length.jpg>) | <p><strong>Rectangle.ByWidthLength</strong> (Plane)<br>入力された幅(平面の X 軸の長さ)と高さ(平面の Y 軸の長さ)を使用して、平面のルートを中心とする長方形を作成します。</p> | ![](<images/index of nodes - rectangle by width length.jpg>) |

### Sphere ノード

|                                                             |                                                                                                                             |                                                                              |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
|                                                             | CREATE                                                                                                                      |                                                                              |
| ![](<images/Sphere by center point radius.jpg>) | <p><strong>Sphere.ByCenterPointRadius</strong><br>入力された点を中心とし、指定された半径を持つソリッド球体を作成します。</p> | ![](<images/index of nodes - sphere by center point radius.jpg>) |

### サーフェス

|                                                           |                                                                                                                                                      |                                                                           |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
|                                                           | CREATE                                                                                                                                               |                                                                           |
| ![](<images/Surfacebyloft.jpg>) | <p><strong>Surface.ByLoft</strong><br>入力された断面曲線間をロフトしてサーフェスを作成します。</p>                                             | ![](<images/index of nodes - surface by loft.jpg>)            |
| ![](<images/Surfacebyloft.jpg>) | <p><strong>Surface.ByPatch</strong><br>入力された曲線で設定される閉じた境界の内部を塗り潰してサーフェスを作成します。</p>                 | ![](images/Surface.ByPatch.png)              |
|                                                           | ACTIONS                                                                                                                                              |                                                                           |
| ![](<images/Surface offset.jpg>)              | <p><strong>Surface.Offset</strong><br>サーフェスの法線の方向に指定された距離だけサーフェスをオフセットします。</p>                                        | ![](<images/index of nodes - surface offset.jpg>)             |
| ![](images/Surfacepointatparameter.jpg)  | <p><strong>Surface.PointAtParameter</strong><br>指定された U および V パラメータの点を返します。</p>                                              | ![](<images/index of nodes - surface point at parameter.jpg>) |
| ![](images/Surfacethicken.jpg)           | <p><strong>Surface.Thicken</strong><br>サーフェスに厚みを持たせてソリッドを作成します。サーフェスを法線の方向に両側に押し出します。</p> | ![](<images/index of nodes - surface thicken.jpg>)            |

### UV ノード

|                                                 |                                                                           |                                                                  |
| ----------------------------------------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------- |
|                                                 | CREATE                                                                    |                                                                  |
| ![](<images/UV by coordinates.jpg>) | <p><strong>UV.ByCoordinates</strong><br>2 つの倍精度浮動小数点値から UV を作成します。</p> | ![](<images/index of nodes - UV by coordinates.jpg>) |

### ベクトル

|                                                         |                                                                                          |                                                                      |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
|                                                         | CREATE                                                                                   |                                                                      |
| ![](<images/Vector by coordinates.jpg>) | <p><strong>Vector.ByCoordinates</strong><br>3 つのユークリッド座標でベクトルを形成します。</p> | ![](<images/index of nodes - vector by coordinates.jpg>) |
| ![](images/Vectorxaxis.jpg)            | <p><strong>Vector.XAxis</strong><br>基底 X 軸ベクトル(1,0,0)を取得します。</p>         | ![](<images/index of nodes - vector x.jpg>)              |
| ![](images/Vectoryaxis.jpg)            | <p><strong>Vector.YAxis</strong><br>基底 Y 軸ベクトル(0,1,0)を取得します。</p>         | ![](<images/index of nodes - vector y.jpg>)              |
| ![](images/Vectorzaxis.jpg)            | <p><strong>Vector.ZAxis</strong><br>基底 Z 軸ベクトル(0,0,1)を取得します。</p>         | ![](<images/index of nodes - vector z.jpg>)              |
|                                                         | ACTIONS                                                                                  |                                                                      |
| ![](<images/Vector normalized.jpg>)     | <p><strong>Vector.Normalized</strong><br>正規化されたベクトルを取得します。</p>      | ![](<images/index of nodes - vector normalized.jpg>)     |

## CoordinateSystem ノード

|                                                                           |                                                                                                                                                                                                    |                                                                                              |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
|                                                                           | CREATE                                                                                                                                                                                             |                                                                                              |
| ![](<images/Coordinate system by origin.jpg>)                 | <p><strong>CoordinateSystem.ByOrigin</strong><br>入力された点に基準点を持ち、X 軸と Y 軸を WCS(ワールド座標系)の X 軸および Y 軸に設定した座標系を作成します。</p>                                               | ![](<images/index of nodes - coordinates system by origin.jpg>)                  |
| ![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) | <p><strong>CoordinateSystem.ByCylindricalCoordinates</strong><br>指定された座標系に対して、指定された円柱座標パラメータに基づいて座標系を作成します。</p> | ![](<images/index of nodes - coordinates system by cylindrical coordinates.jpg>) |

## 演算子

|                                                  |                                                                                                                         |                                                               |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| ![](<images/addition.jpg>)       | <p><strong>+</strong><br>加算</p>                                                                                   | ![](<images/index of nodes - addition.jpg>)       |
| ![](<images/Subtraction.jpg>)    | <p><strong>-</strong><br>減算</p>                                                                                | ![](<images/index of nodes - subtraction.jpg>)    |
| ![](<images/Multiplication.jpg>) | <p><strong>*</strong><br>乗算</p>                                                                             | ![](<images/index of nodes - multiplication.jpg>) |
| ![](<images/Division.jpg>)       | <p><strong>/</strong><br>区分</p>                                                                                   | ![](<images/index of nodes - division.jpg>)       |
| ![](images/modular.jpg)         | <p><strong>%</strong><br>剰余演算により、1 番目の入力を 2 番目の入力で除算して剰余を取得します。</p> | ![](<images/index of nodes - %.jpg>)              |
| ![](<images/index of nodes - less than.jpg>)        | <p><strong><</strong><br>より小さい</p>                                                                             | ![](<images/index of nodes - less than.jpg>)      |
| ![](<images/greater than.jpg>)   | <p><strong>></strong><br>より大きい</p>                                                                               | ![](<images/index of nodes - greater than.jpg>)   |
| ![](<images/==.jpg>)             | <p><strong>==</strong><br>2 つの値が等しいかどうか検証します。</p>                                           | ![](<images/index of nodes - ==.jpg>)             |
