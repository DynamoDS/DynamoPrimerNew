# ノードの索引

この索引では、この手引で言及しているすべてのノードと他の便利なコンポーネントについて、補足情報を提供します。ここで紹介するのは、Dynamo で使用できる 500 個のノードのうち一部にすぎません。

## Display

### Color ノード

|                                               |                                                                                                                       |                                                                   |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
|                                               | CREATE                                                                                                                |                                                                   |
|![](<images/5-1/ColorbyARGB (1) (1).jpg>)     | <p><strong>Color.ByARGB</strong><br>アルファ、赤、緑、青の各成分から色を作成します。</p>                  |![](<images/5-1/indexofnodes-colorbyargb(1) (1).jpg>)             |
| ![](images/5-1/ColorRange.jpg)                | <p><strong>Color Range</strong><br>開始色と終了色間の色のグラデーションから色を取得します。</p>      |![](<images/5-1/indexofnodes-colorrange(1) (1) (1).jpg>)          |
|                                               | ACTIONS                                                                                                               |                                                                   |
|![](<images/5-1/ColorBrightness (1) (1).jpg>) | <p><strong>Color.Brightness</strong><br>色の明度の値を取得します。</p>                                 |![](<images/5-1/indexofnodes-colorbrightness(1) (1) (1) (1).jpg>) |
|![](<images/5-1/ColorComponent (1).jpg>)      | <p><strong>Color.Components</strong><br>色の各成分を、アルファ、赤、緑、青の順のリストとして返します。</p> | ![](images/5-1/indexofnodes-colorcomponent.jpg)                   |
|![](<images/5-1/ColorSaturation (1) (1).jpg>) | <p><strong>Color.Saturation</strong><br>色の彩度の値を取得します。</p>                                  | ![](images/5-1/indexofnodes-colorsaturation.jpg)                  |
|![](<images/5-1/ColorHue (1).jpg>)            | <p><strong>Color.Hue</strong><br>色の色相の値を取得します。</p>                                               | ![](images/5-1/indexofnodes-colorhue.jpg)                         |
|                                               | QUERY                                                                                                                 |                                                                   |
|![](<images/5-1/ColorAlpha(1)(1) (1).jpg>)    | <p><strong>Color.Alpha</strong><br>色のアルファ成分の値(0 ～ 255)を取得します。</p>                                 | ![](images/5-1/indexofnodes-coloralpha.jpg)                       |
|![](<images/5-1/ColorBlue (1) (1).jpg>)       | <p><strong>Color.Blue</strong><br>色の青色成分の値(0 ～ 255)を取得します。</p>                                   | ![](images/5-1/indexofnodes-colorblue.jpg)                        |
|![](<images/5-1/ColorGreen(1)(1) (1).jpg>)    | <p><strong>Color.Green</strong><br>色の緑色成分の値(0 ～ 255)を取得します。</p>                                 | ![](images/5-1/indexofnodes-colorgreen.jpg)                       |
|![](<images/5-1/ColorRed (1) (1).jpg>)        | <p><strong>Color.Red</strong><br>色の赤色成分の値(0 ～ 255)を取得します。</p>                                     | ![](images/5-1/indexofnodes-colorred.jpg)                         |

|                                                               |                                                                                           |                                                               |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
|                                                               | CREATE                                                                                    |                                                               |
|![](<images/5-1/GeometryColorByGeometryColor(1) (1) (1).jpg>) | <p><strong>GeometryColor.ByGeometryColor</strong><br>任意の色を使用してジオメトリを表示します。</p> | ![](images/5-1/indexofnodes-geometrycolorbygeometrycolor.jpg) |

### Watch ノード

|                                 |                                                                               |                                                  |
| ------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------ |
|                                 | ACTIONS                                                                       |                                                  |
| ![](images/5-1/Viewwatch.jpg)   | <p><strong>View.Watch</strong><br>ノードの出力を視覚化します。</p>           | ![](images/5-1/indexofnodes-viewwatch.jpg)       |
| ![](images/5-1/Viewwatch3d.jpg) | <p><strong>View.Watch 3D</strong><br>ジオメトリのダイナミック プレビューを表示します。</p> | ![](images/5-1/indexofnodes-viewwatch.3Djpg.jpg) |

## Input ノード

|                                             |                                                                                                          |                                                          |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
|                                             | ACTIONS                                                                                                  |                                                          |
| ![](images/5-1/Boolean.jpg)                 | <p><strong>Boolean</strong><br>True と False のいずれかを選択します。</p>                                   | ![](images/5-1/indexofnodes-boolean.jpg)                 |
|![](<images/5-1/CodeBlock(1)(1) (1).jpg>)   | <p><strong>Code Block</strong><br>DesignScript のコードを直接作成することができます。</p>              | ![](images/5-1/indexofnodes-codeblock.jpg)               |
| ![](images/5-1/DirectoryPath.jpg)           | <p><strong>Directory Path</strong><br>システム上で任意のフォルダを選択して、そのパスを取得することができます。</p> | ![](images/5-1/indexofnodes-directorypath.jpg)           |
| ![](images/5-1/FilePath.jpg)                | <p><strong>File Path</strong><br>システム上で任意のファイルを選択して、そのファイル名を取得することができます。</p>        | ![](images/5-1/indexofnodes-filepath.jpg)                |
| ![](images/5-1/Integerslider.jpg)           | <p><strong>Integer Slider</strong><br>整数値を生成するスライダです。</p>                         | ![](images/5-1/indexofnodes-integerslider.jpg)           |
|![](<images/5-1/number(1) (1) (1) (1).jpg>) | <p><strong>Number</strong><br>数値を作成します。</p>                                                      |![](<images/5-1/indexofnodes-number(1) (1) (1) (1).jpg>) |
| ![](images/5-1/Numberslider.jpg)            | <p><strong>Number Slider</strong><br>数値を生成するスライダです。</p>                          | ![](images/5-1/indexofnodes-numberslider.jpg)            |
|![](<images/5-1/string(1) (1) (1) (1).jpg>) | <p><strong>String</strong><br>文字列を作成します。</p>                                                      | ![](images/5-1/indexofnodes-string.jpg)                  |
| ![](images/5-1/ObjectisNull.jpg)            | <p><strong>Object.IsNull</strong><br>指定されたオブジェクトが NULL であるかどうかを判断します。</p>                         | ![](images/5-1/indexofnodes-objectisnull.jpg)            |

## List ノード

|                                            |                                                                                                                                                                                                                                               |                                                            |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
|                                            | CREATE                                                                                                                                                                                                                                        |                                                            |
| ![](images/5-1/ListCreate.jpg)             | <p><strong>List.Create</strong><br>与えられた入力に基づいて新しいリストを作成します。</p>                                                                                                                                                              | ![](images/5-1/indexofnodes-listcreate.jpg)                |
| ![](images/5-1/ListCombine.jpg)            | <p><strong>List.Combine</strong><br>2 つのシーケンスの各要素にコンビネータを適用します。</p>                                                                                                                                                 | ![](images/5-1/indexofnodes-listcombine.jpg)               |
| ![](images/5-1/Range.jpg)                  | <p><strong>Number Range</strong><br>指定された範囲内で数値のシーケンスを作成します。</p>                                                                                                                                                  |![](<images/5-1/indexofnodes-range(1) (1) (1).jpg>)        |
| ![](images/5-1/Sequence.jpg)               | <p><strong>Number Sequence</strong><br>数値のシーケンスを作成します。</p>                                                                                                                                                                     | ![](images/5-1/indexofnodes-sequence.jpg)                  |
|                                            | ACTIONS                                                                                                                                                                                                                                       |                                                            |
| ![](images/5-1/ListChop.jpg)               | <p><strong>List.Chop</strong><br>リストを、それぞれ指定された個数の項目から成るリストの集合に分割します。</p>                                                                                                                               | ![](images/5-1/indexofnodes-listchop.jpg)                  |
|![](<images/5-1/Count(1) (1) (1) (1).jpg>) | <p><strong>List.Count</strong><br>指定されたリストに格納されている項目の数を返します。</p>                                                                                                                                                   |![](<images/5-1/indexofnodes-count(1)(1) (1) (2) (6).jpg>) |
| ![](images/5-1/ListFlatten.jpg)            | <p><strong>List.Flatten</strong><br>ネストされたリストのリストを、指定された量だけフラットにします。</p>                                                                                                                                                  | ![](images/5-1/indexofnodes-listflatten.jpg)               |
| ![](images/5-1/ListFilterbyBoolMask.jpg)   | <p><strong>List.FilterByBoolMask</strong><br>別個のブール値を要素に持つリスト内で対応するインデックスを検索して、シーケンスをフィルタします。</p>                                                                                                       | ![](images/5-1/indexofnodes-listfilterbyboolmask.jpg)      |
| ![](images/5-1/ListGetItemAtIndex.jpg)     | <p><strong>List.GetItemAtIndex</strong><br>リストの、指定されたインデックスにある項目を取得します。</p>                                                                                                                        | ![](images/5-1/indexofnodes-listgetitematindex.jpg)        |
|                                            | <p><strong>List.Map</strong><br>リスト内のすべての要素に関数を適用し、その結果から新しいリストを生成します。</p>                                                                                                                    | ![](images/5-1/indexofnodes-listmap.jpg)                   |
|                                            | <p><strong>List.Reverse</strong><br>指定されたリスト内の項目を逆順で含む新しいリストを作成します。</p>                                                                                                                        | ![](images/5-1/indexofnodes-listreverse.jpg)               |
| ![](images/5-1/ListReplaceItemAtIndex.jpg) | <p><strong>List.ReplaceItemAtIndex</strong><br>リストの、指定されたインデックスにある項目を置き換えます。</p>                                                                                                                  | ![](images/5-1/indexofnodes-replaceitematindex.jpg)        |
| ![](images/5-1/ListShiftIndices.jpg)       | <p><strong>List.ShiftIndices</strong><br>リスト内のインデックスを、指定された量だけ右に移動します。</p>                                                                                                                                      | ![](images/5-1/indexofnodes-listshiftindices.jpg)          |
| ![](images/5-1/ListTakeEveryNthItem.jpg)   | <p><strong>List.TakeEveryNthItem</strong><br>指定されたオフセットの後、指定された値の倍数であるインデックスの項目を、指定されたリストから取得します。</p>                                                                                  | ![](images/5-1/indexofnodes-listtakeeverynthitem.jpg)      |
| ![](images/5-1/ListTranspose.jpg)          | <p><strong>List.Transpose</strong><br>任意のリストのリストの行と列を入れ替えます。他の行よりも短い行がある場合は、作成される配列が常に長方形になるように、プレースホルダーとして NULL 値が挿入されます。</p> | ![](images/5-1/indexofnodes-listtranspose.jpg)             |

## ロジック

|                        |                                                                                                                                                                                                              |                                     |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------- |
|                        | ACTIONS                                                                                                                                                                                                      |                                     |
| ![](images/5-1/If.jpg) | <p><strong>If</strong><br>条件ステートメントです。テスト入力のブール値をチェックします。テスト入力が true である場合は、結果として true の入力を出力します。false である場合は、結果として false の入力を出力します。</p> | ![](images/5-1/indexofnodes-if.jpg) |

## Math ノード

|                                          |                                                                                                                              |                                                       |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
|                                          | ACTIONS                                                                                                                      |                                                       |
| ![](images/5-1/Mathcos.jpg)              | <p><strong>Math.Cos</strong><br>角度の余弦を求めます。</p>                                                            | ![](images/5-1/indexofnodes-mathcos.jpg)              |
| ![](images/5-1/Mathdegreestoradians.jpg) | <p><strong>Math.DegreesToRadians</strong><br>度単位の角度をラジアン単位の角度に変換します。</p>                        | ![](images/5-1/indexofnodes-mathdegreestoradians.jpg) |
| ![](images/5-1/Mathpow.jpg)              | <p><strong>Math.Pow</strong><br>指定された指数に対して値を累乗します。</p>                                                  | ![](images/5-1/indexofnodes-mathpow.jpg)              |
| ![](images/5-1/Mathradianstodegrees.jpg) | <p><strong>Math.RadiansToDegrees</strong><br>ラジアン単位の角度を度単位の角度に変換します。</p>                        | ![](images/5-1/indexofnodes-mathradianstodegrees.jpg) |
| ![](images/5-1/Mathremaprange.jpg)       | <p><strong>Math.RemapRange</strong><br>分布比率を保持しながら数値のリストの範囲を調整します。</p>   | ![](images/5-1/indexofnodes-mathremaprange.jpg)       |
| ![](images/5-1/Mathsin.jpg)              | <p><strong>Math.Sin</strong><br>角度の正弦を求めます。</p>                                                              | ![](images/5-1/indexofnodes-mathsin.jpg)              |
| ![](images/5-1/Formula.jpg)              | <p><strong>Formula</strong><br>数学式を評価します。NCalc を評価に使用します。次を参照してください。http://ncalc.codeplex.com</p> | ![](images/5-1/indexofnodes-formula.jpg)              |
|![](<images/5-1/Map(1) (1) (1) (1).jpg>) | <p><strong>Map</strong><br>値を入力された範囲にマッピングします。</p>                                                              | ![](images/5-1/indexofnodes-mathmap.jpg)              |

## String ノード

|                                    |                                                                                                                                                      |                                                          |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
|                                    | ACTIONS                                                                                                                                              |                                                          |
| ![](images/5-1/Stringconcat.jpg)   | <p><strong>String.Concat</strong><br>複数の文字列を 1 つの文字列に連結します。</p>                                                         | ![](images/5-1/indexofnodes-stringconcat.jpg)            |
| ![](images/5-1/Stringcontains.jpg) | <p><strong>String.Contains</strong><br>指定された文字列に指定されたサブストリングが含まれているかどうかを判断します。</p>                                              | ![](images/5-1/indexofnodes-stringcontains.jpg)          |
| ![](images/5-1/Stringjoin.jpg)     | <p><strong>String.Join</strong><br>複数の文字列を 1 つの文字列に連結し、結合されるそれぞれの文字列の間に区切り文字を挿入します。</p> |![](<images/5-1/indexofnodes-stringjoin(1) (1) (2).jpg>) |
| ![](images/5-1/Stringsplit.jpg)    | <p><strong>String.Split</strong><br>1 つの文字列を文字列のリストに分割します。指定された区切り文字によって分割場所が決定されます。</p>    | ![](images/5-1/indexofnodes-stringsplit.jpg)             |
| ![](images/5-1/Stringtonumber.jpg) | <p><strong>String.ToNumber</strong><br>文字列を整数または倍精度浮動小数点数に変換します。</p>                                                              | ![](images/5-1/indexofnodes-stringtonumber.jpg)          |

## ジオメトリ

### Circle ノード

|                                               |                                                                                                                                                          |                                                                  |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
|                                               | CREATE                                                                                                                                                   |                                                                  |
| ![](images/5-1/Circlebycenterpointradius.jpg) | <p><strong>Circle.ByCenterPointRadius</strong><br>入力された中心点と半径をワールド座標系の XY 平面に持ち、ワールド座標系の Z 軸を法線とする円を作成します。</p> | ![](images/5-1/indexofnodes-circlebycenterpointradiusnormal.jpg) |
| ![](images/5-1/Circlebyplaneradius.jpg)       | <p><strong>Circle.ByPlaneRadius</strong><br>入力された平面の基準点(ルート)に中心を持ち、指定された半径を持つ円を平面上に作成します。</p>  | ![](images/5-1/indexofnodes-circlebyplaneradius.jpg)             |

|                                                                               |                                                                                                                                                                                                    |                                                                            |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
|                                                                               | CREATE                                                                                                                                                                                             |                                                                            |
| ![](images/5-1/Coordinatesystembyorigin.jpg)                                  | <p><strong>CoordinateSystem.ByOrigin</strong><br>入力された点に基準点を持ち、X 軸と Y 軸を WCS(ワールド座標系)の X 軸および Y 軸に設定した座標系を作成します。</p>                                               | ![](images/5-1/indexofnodes-coordinatessystembyorigin.jpg)                 |
|![](<images/5-1/Coordinatesystembycylindricalcoordinates(1) (1) (1) (1).jpg>) | <p><strong>CoordinateSystem.ByCyclindricalCoordinates</strong><br>指定された座標系に対して、指定された円柱座標パラメータに基づいて座標系を作成します。</p> | ![](images/5-1/indexofnodes-coordinatessystembycylindricalcoordinates.jpg) |

### Cuboid ノード

|                                                                  |                                                                                                                                            |                                                                  |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------- |
|                                                                  | CREATE                                                                                                                                     |                                                                  |
|![](<images/5-1/Cuboidbylength(1) (1) (1).jpg>)                  | <p><strong>Cuboid.ByLengths</strong><br>ワールド座標系の基準点を中心として、幅、長さ、高さを持つ直方体を作成します。</p>                        | ![](images/5-1/indexofnodes-cuboidbylengths.jpg)                 |
|![](<images/5-1/Cuboidbylengthorigin(1) (1) (1) (1).jpg>)        | <p><strong>Cuboid.ByLengths</strong> (origin)</p><p>中心を入力された点に設定し、指定された幅、長さ、高さの直方体を作成します。</p> | ![](images/5-1/indexofnodes-cuboidbylengthsorigin.jpg)           |
|![](<images/5-1/Cuboidbylengthcoordinatessystem(1) (1) (1).jpg>) | <p><strong>Cuboid.ByLengths</strong> (coordinateSystem)</p><p>ワールド座標系の基準点を中心として、幅、長さ、高さを持つ直方体を作成します。</p>  | ![](images/5-1/indexofnodes-cuboidbylengthscoordinatesystem.jpg) |
|![](<images/5-1/Cuboidbycorners(1) (1) (1) (1).jpg>)             | <p><strong>Cuboid.ByCorners</strong></p><p>lowPoint から highPoint までの範囲に広がる直方体を作成します。</p>                                      | ![](images/5-1/indexofnodes-cuboidbycorners.jpg)                 |
|![](<images/5-1/Cuboidlength(1) (1) (2).jpg>)                    | <p><strong>Cuboid.Length</strong></p><p>実際のワールド空間寸法ではなく、直方体の入力寸法を返します。**</p>           | ![](images/5-1/indexofnodes-cuboidlength.jpg)                    |
|![](<images/5-1/Cuboidwidth(1) (1) (1) (1).jpg>)                 | <p><strong>Cuboid.Width</strong></p><p>実際のワールド空間寸法ではなく、直方体の入力寸法を返します。**</p>            | ![](images/5-1/indexofnodes-cuboidwidth.jpg)                     |
|![](<images/5-1/Cuboidheight(1) (1) (1).jpg>)                    | <p><strong>Cuboid.Height</strong></p><p>実際のワールド空間寸法ではなく、直方体の入力寸法を返します。**</p>           | ![](images/5-1/indexofnodes-cuboidheight.jpg)                    |
|![](<images/5-1/Boundingboxtocuboid(1) (1).jpg>)                 | <p><strong>BoundingBox.ToCuboid</strong></p><p>ソリッドの直方体として境界ボックスを取得します。</p>                                                  | ![](images/5-1/indexofnodes-boundingboxtocuboid.jpg)             |

{% hint style="warning" %}
**つまり、直方体の幅(X 軸)の長さ 10 を作成し、それを X 軸で 2 倍のスケーリングを行う座標系に変換しても、幅は 10 のままです。ASM では、ボディの頂点を予測可能な順序で抽出することができないため、変換後に寸法を決定することはできません。
{% endhint %}

### Curve ノード

|                                           |                                                                                                                                                  |                                                        |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ |
|                                           | ACTIONS                                                                                                                                          |                                                        |
| ![](images/5-1/Curveextrude.jpg)          | <p><strong>Curve.Extrude</strong> (distance)<br>法線ベクトルの方向に曲線を押し出します。</p>                                             | ![](images/5-1/indexofnodes-curveextrude.jpg)          |
| ![](images/5-1/Curvepointatparameter.jpg) | <p><strong>Curve.PointAtParameter</strong><br>StartParameter() から EndParameter() までの範囲の指定されたパラメータで曲線上の点を取得します。</p> | ![](images/5-1/indexofnodes-curvepointatparameter.jpg) |

### ジオメトリ モディファイア

|                                           |                                                                                                                                    |                                                        |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
|                                           | ACTIONS                                                                                                                            |                                                        |
| ![](images/5-1/Geometrydistanceto.jpg)    | <p><strong>Geometry.DistanceTo</strong><br>このジオメトリから別のジオメトリへの距離を取得します。</p>                                 | ![](images/5-1/indexofnodes-geometrydistanceto.jpg)    |
| ![](images/5-1/Geometryexplode.jpg)       | <p><strong>Geometry.Explode</strong><br>複合要素または分割されていない要素をコンポーネント パーツに分割します。</p>                | ![](images/5-1/indexofnodes-geometryexplode.jpg)       |
| ![](images/5-1/GeometryimportfromSAT.jpg) | <p><strong>Geometry.ImportFromSAT</strong><br>読み込まれたジオメトリのリストです。</p>                                                      | ![](images/5-1/indexofnodes-geometryimportfromsat.jpg) |
| ![](images/5-1/Geometryrotate.jpg)        | <p><strong>Geometry.Rotate</strong> (basePlane)<br>平面の基準点と法線を中心にオブジェクトを指定された角度だけ回転させます。</p> | ![](images/5-1/indexofnodes-geometryrotate.jpg)        |
| ![](images/5-1/Geometrytranslate.jpg)     | <p><strong>Geometry.Translate</strong><br>指定された方向に距離を指定して、ジオメトリ タイプを平行移動させます。</p>           | ![](images/5-1/indexofnodes-geometrytranslate.jpg)     |

### Line ノード

|                                                     |                                                                                                                                                          |                                                                  |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
|                                                     | CREATE                                                                                                                                                   |                                                                  |
| ![](images/5-1/Linebybestfitthroughpoints.jpg)      | <p><strong>Line.ByBestFitThroughPoints</strong><br>点の散布図に最もよく近似する直線を作成します。</p>                                       | ![](images/5-1/indexofnodes-linebybestfitthroughpoints.jpg)      |
| ![](images/5-1/Linebystartpointdirectionlength.jpg) | <p><strong>Line.ByStartPointDirectionLength</strong><br>開始点から始まり、ベクトルの向きに指定された長さだけ延長する線分を作成します。</p> | ![](images/5-1/indexofnodes-linebystartpointdirectionlength.jpg) |
|![](<images/5-1/Linebystartpointendpoint (1).jpg>)  | <p><strong>Line.ByStartPointEndPoint</strong><br>入力された 2 点を端点とする線分を作成します。</p>                                                   | ![](images/5-1/indexofnodes-linebystartpointendpoint.jpg)        |
| ![](images/5-1/Linebytangency.jpg)                  | <p><strong>Line.ByTangency</strong><br>入力された曲線に接し、曲線のパラメータで指定された点に位置する直線を作成します。</p>               | ![](images/5-1/indexofnodes-linebytangency.jpg)                  |
|                                                     | QUERY                                                                                                                                                    |                                                                  |
| ![](images/5-1/Linedirection.jpg)                   | <p><strong>Line.Direction</strong><br>曲線の方向を返します。</p>                                                                                    | ![](images/5-1/indexofnodes-linedirection.jpg)                   |

### NurbsCurve ノード

|                                               |                                                                                                               |                                                            |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
|                                               | Create                                                                                                        |                                                            |
| ![](images/5-1/Nurbscurvebycontrolpoints.jpg) | <p><strong>NurbsCurve.ByControlPoints</strong><br>明示的な制御点を使用して B スプライン曲線を作成します。</p> | ![](images/5-1/indexofnodes-nurbscurvebycontrolpoints.jpg) |
| ![](images/5-1/Nurbscurvebypoints.jpg)        | <p><strong>NurbsCurve.ByPoints</strong><br>点間を補間して B スプライン曲線を作成します。</p>          | ![](images/5-1/indexofnodes-nurbscurvebypoints.jpg)        |

### NurbsSurface ノード

|                                                 |                                                                                                                                                                                            |                                                              |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                 | Create                                                                                                                                                                                     |                                                              |
| ![](images/5-1/Nurbssurfacebycontrolpoints.jpg) | <p><strong>NurbsSurface.ByControlPoints</strong><br>明示的な制御点と指定された U 次数と V 次数を使用して NURBS 曲面 を作成します。</p>                                             | ![](images/5-1/indexofnodes-nurbssurfacebycontrolpoints.jpg) |
| ![](images/5-1/Nurbssurfacebypoints.jpg)        | <p><strong>NurbsSurface.ByPoints</strong><br>指定された補間される点、U 次数、V 次数を使用して NURBS 曲面を作成します。作成されるサーフェスはすべての指定された点を通過します。</p> | ![](images/5-1/indexofnodes-nurbssurfacebypoints.jpg)        |

### Plane ノード

|                                         |                                                                                                                  |                                                      |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
|                                         | CREATE                                                                                                           |                                                      |
| ![](images/5-1/Planebyoriginnormal.jpg) | <p><strong>Plane.ByOriginNormal</strong><br>中心をルート点に持ち、入力された法線ベクトルを持つ平面を作成します。</p> | ![](images/5-1/indexofnodes-planebyoriginnormal.jpg) |
| ![](images/5-1/PlaneXY.jpg)             | <p><strong>Plane.XY</strong><br>ワールド座標系の XY に平面を作成します。</p>                                              | ![](images/5-1/indexofnodes-planexy.jpg)             |

### Point ノード

|                                                 |                                                                                                                                           |                                                              |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
|                                                 | CREATE                                                                                                                                    |                                                              |
| ![](images/5-1/Pointbycartesiancoordinates.jpg) | <p><strong>Point.ByCartesianCoordinates</strong><br>指定された座標系と 3 つのデカルト座標で点を作成します。</p>          | ![](images/5-1/indexofnodes-pointbycartesiancoordinates.jpg) |
| ![](images/5-1/Pointbycoordinates2D.jpg)        | <p><strong>Point.ByCoordinates</strong> (2d)<br>指定された 2 つのデカルト座標を使用して、XY 平面に点を作成します。Z コンポーネントは 0 です。</p> | ![](images/5-1/indexofnodes-pointbycoordinates2D.jpg)        |
| ![](images/5-1/Pointbycoordinates3D.jpg)        | <p><strong>Point.ByCoordinates</strong> (3d)<br>指定された 3 つのデカルト座標を使用して点を作成します。</p>                                           | ![](images/5-1/indexofnodes-pointbycoordinates3D.jpg)        |
| ![](images/5-1/Pointorigin.jpg)                 | <p><strong>Point.Origin</strong><br>基準点 (0,0,0)を取得します。</p>                                                                      | ![](images/5-1/indexofnodes-pointorigin.jpg)                 |
|                                                 | ACTIONS                                                                                                                                   |                                                              |
| ![](images/5-1/Pointadd.jpg)                    | <p><strong>Point.Add</strong><br>点にベクトルを追加します。Translate(Vector)と同じ操作です。</p>                                             | ![](images/5-1/indexofnodes-pointadd.jpg)                    |
|                                                 | QUERY                                                                                                                                     |                                                              |
| ![](images/5-1/Pointx.jpg)                      | <p><strong>Point.X</strong><br>点の X 座標を取得します。</p>                                                                         | ![](images/5-1/indexofnodes-pointx.jpg)                      |
| ![](images/5-1/Pointy.jpg)                      | <p><strong>Point.Y</strong><br>点の Y 座標を取得します。</p>                                                                         | ![](images/5-1/indexofnodes-pointy.jpg)                      |
| ![](images/5-1/Pointz.jpg)                      | <p><strong>Point.Z</strong><br>点の Z 座標を取得します。</p>                                                                         | ![](images/5-1/indexofnodes-pointz.jpg)                      |

### Polycurve ノード

|                                       |                                                                                                                                                                                       |                                                    |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
|                                       | CREATE                                                                                                                                                                                |                                                    |
| ![](images/5-1/Polycurvebypoints.jpg) | <p><strong>Polycurve.ByPoints</strong><br>点をつなげる線分のシーケンスからポリカーブを作成します。閉じた曲線を作成するには、最後の点の位置を始点の位置と同じにします。</p> | ![](images/5-1/indexofnodes-polycurvebypoints.jpg) |

### Rectangle ノード

|                                            |                                                                                                                                                                               |                                                         |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
|                                            | CREATE                                                                                                                                                                        |                                                         |
| ![](images/5-1/Rectanglebywidthlength.jpg) | <p><strong>Rectangle.ByWidthLength</strong> (Plane)<br>入力された幅(平面の X 軸の長さ)と高さ(平面の Y 軸の長さ)を使用して、平面のルートを中心とする長方形を作成します。</p> | ![](images/5-1/indexofnodes-rectanglebywidthlength.jpg) |

### Sphere ノード

|                                               |                                                                                                                             |                                                            |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
|                                               | CREATE                                                                                                                      |                                                            |
| ![](images/5-1/Spherebycenterpointradius.jpg) | <p><strong>Sphere.ByCenterPointRadius</strong><br>入力された点を中心とし、指定された半径を持つソリッド球体を作成します。</p> | ![](images/5-1/indexofnodes-spherebycenterpointradius.jpg) |

### Surface ノード

|                                                          |                                                                                                                                                      |                                                              |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
|                                                          | CREATE                                                                                                                                               |                                                              |
|![](<images/5-1/Surfacebyloft(1)(1) (1) (1) (1).jpg>)    | <p><strong>Surface.ByLoft</strong><br>入力された断面曲線間をロフトしてサーフェスを作成します。</p>                                             | ![](images/5-1/indexofnodes-surfacebyloft.jpg)               |
|![](<images/5-1/Surfacebyloft(1)(1) (1) (1) (2).jpg>)    | <p><strong>Surface.ByPatch</strong><br>入力された曲線で設定される閉じた境界の内部を塗り潰してサーフェスを作成します。</p>                 |![](<images/5-1/indexofnodes-surfacebypatch(1) (1) (1).jpg>) |
|                                                          | ACTIONS                                                                                                                                              |                                                              |
|![](<images/5-1/Surfaceoffset(1) (1) (2).jpg>)           | <p><strong>Surface.Offset</strong><br>サーフェスの法線の方向に指定された距離だけサーフェスをオフセットします。</p>                                        | ![](images/5-1/indexofnodes-surfaceoffset.jpg)               |
|![](<images/5-1/Surfacepointatparameter(1) (1) (1).jpg>) | <p><strong>Surface.PointAtParameter</strong><br>指定された U および V パラメータの点を返します。</p>                                              | ![](images/5-1/indexofnodes-surfacepointatparameter.jpg)     |
|![](<images/5-1/Surfacethicken(1) (1) (1) (2).jpg>)      | <p><strong>Surface.Thicken</strong><br>サーフェスに厚みを持たせてソリッドを作成します。サーフェスを法線の方向に両側に押し出します。</p> | ![](images/5-1/indexofnodes-surfacethicken.jpg)              |

### UV ノード

|                                                  |                                                                           |                                                  |
| ------------------------------------------------ | ------------------------------------------------------------------------- | ------------------------------------------------ |
|                                                  | CREATE                                                                    |                                                  |
|![](<images/5-1/UVbycoordinates(1) (1) (2).jpg>) | <p><strong>UV.ByCoordinates</strong><br>2 つの倍精度浮動小数点値から UV を作成します。</p> | ![](images/5-1/indexofnodes-UVbycoordinates.jpg) |

### Vector ノード

|                                                  |                                                                                          |                                                      |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------- | ---------------------------------------------------- |
|                                                  | CREATE                                                                                   |                                                      |
|![](<images/5-1/Vectorbycoordinates(1) (1).jpg>) | <p><strong>Vector.ByCoordinates</strong><br>3 つのユークリッド座標でベクトルを形成します。</p> | ![](images/5-1/indexofnodes-vectorbycoordinates.jpg) |
|![](<images/5-1/Vectorxaxis(1) (1) (1) (1).jpg>) | <p><strong>Vector.XAxis</strong><br>基底 X 軸ベクトル(1,0,0)を取得します。</p>         | ![](images/5-1/indexofnodes-vectorx.jpg)             |
|![](<images/5-1/Vectoryaxis(1) (1) (1) (1).jpg>) | <p><strong>Vector.YAxis</strong><br>基底 Y 軸ベクトル(0,1,0)を取得します。</p>         | ![](images/5-1/indexofnodes-vectory.jpg)             |
|![](<images/5-1/Vectorzaxis(1) (1) (1) (1).jpg>) | <p><strong>Vector.ZAxis</strong><br>基底 Z 軸ベクトル(0,0,1)を取得します。</p>         | ![](images/5-1/indexofnodes-vectorz.jpg)             |
|                                                  | ACTIONS                                                                                  |                                                      |
|![](<images/5-1/Vectornormalized(1) (1).jpg>)    | <p><strong>Vector.Normalized</strong><br>正規化されたベクトルを取得します。</p>      | ![](images/5-1/indexofnodes-vectornormalized.jpg)    |

## CoordinateSystem ノード

|                                                                               |                                                                                                                                                                                                    |                                                                            |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
|                                                                               | CREATE                                                                                                                                                                                             |                                                                            |
| ![](images/5-1/Coordinatesystembyorigin.jpg)                                  | <p><strong>CoordinateSystem.ByOrigin</strong><br>入力された点に基準点を持ち、X 軸と Y 軸を WCS(ワールド座標系)の X 軸および Y 軸に設定した座標系を作成します。</p>                                               | ![](images/5-1/indexofnodes-coordinatessystembyorigin.jpg)                 |
|![](<images/5-1/Coordinatesystembycylindricalcoordinates(1) (1) (1) (1).jpg>) | <p><strong>CoordinateSystem.ByCyclindricalCoordinates</strong><br>指定された座標系に対して、指定された円柱座標パラメータに基づいて座標系を作成します。</p> | ![](images/5-1/indexofnodes-coordinatessystembycylindricalcoordinates.jpg) |

## Operators

|                                                |                                                                                                                         |                                                 |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
|![](<images/5-1/addition(1)(1) (1).jpg>)       | <p><strong>+</strong><br>加算</p>                                                                                   | ![](images/5-1/indexofnodes-addition.jpg)       |
|![](<images/5-1/Subtraction(1)(1) (1).jpg>)    | <p><strong>-</strong><br>減算</p>                                                                                | ![](images/5-1/indexofnodes-subtraction.jpg)    |
|![](<images/5-1/Multiplication(1)(1) (1).jpg>) | <p><strong>*</strong><br>乗算</p>                                                                             | ![](images/5-1/indexofnodes-multiplication.jpg) |
|![](<images/5-1/Division(1)(1) (1).jpg>)       | <p><strong>/</strong><br>除算</p>                                                                                   | ![](images/5-1/indexofnodes-division.jpg)       |
|![](<images/5-1/modular(1) (1) (1).jpg>)       | <p><strong>%</strong><br>剰余演算により、1 番目の入力を 2 番目の入力で除算して剰余を取得します。</p> | ![](images/5-1/indexofnodes-modular.jpg)        |
|![](<images/5-1/Lessthan(1) (1) (1).jpg>)      | <p><strong><</strong><br>より小さい</p>                                                                             | ![](images/5-1/indexofnodes-lessthan.jpg)       |
|![](<images/5-1/greaterthan(1) (1).jpg>)       | <p><strong>></strong><br>より大きい</p>                                                                               | ![](images/5-1/indexofnodes-greaterthan.jpg)    |
|![](<images/5-1/==(1) (1).jpg>)                | <p><strong>==</strong><br>2 つの値が等しいかどうか検証します。</p>                                           | ![](images/5-1/indexofnodes-==.jpg)             |
