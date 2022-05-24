# 套件案例研究 - Mesh Toolkit

Dynamo Mesh Toolkit 可提供工具，以匯入外部檔案格式的網格、根據 Dynamo 幾何圖形物件建立網格，並根據網格的頂點與索引手動建置網格。該資源庫還提供工具來修改網格、修復網格，或萃取水平切片，以用於加工。

![](<../images/6-2/2/meshToolkit case study 01.jpg>)

Dynamo Mesh Toolkit 是 Autodesk 持續進行網格研究的一部分，因此在未來的幾年將繼續成長。該工具箱將頻繁推出新方法，請隨時與 Dynamo 團隊聯繫以提供註解、錯誤以及新功能的建議。

### 網格與實體

下面的練習演示了使用 Mesh Toolkit 可執行的一些基本網格作業。在此練習中，我們將網格與一系列的平面相交，如果使用實體執行此作業，則運算成本很高。與實體不同，網格具有一組「解析度」，不以數學方式定義，而是以拓樸方式定義，我們可根據要執行的作業來定義此解析度。有關網格與實體關係的詳細資訊，您可以參考此手冊的[用於計算設計的幾何圖形](../../a-closer-look-at-dynamo-essential-nodes-and-concepts/5\_geometry-for-computational-design/)一章。有關 Mesh Toolkit 的更詳細資訊，您可以參考 [Dynamo Wiki 頁面](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit)。我們跳到下面的練習部分。

### 安裝 Mesh Toolkit

在 Dynamo 中，移至頂部功能表列的_「套件」>「搜尋套件...」_。在搜尋欄位中，鍵入 _"MeshToolkit"_ (一個字，注意大小寫)。按一下「安裝」以開始下載。非常簡單！

![](<../images/6-2/2/meshToolkit case study - install package.jpg>)

## 練習：網格相交

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/6-2/2/MeshToolkit.zip" %}

在此範例中，我們將瞭解 Mesh Toolkit 的 Intersect 節點。我們將匯入網格並將其與一系列輸入平面相交以建立切片。這是準備模型以使用鐳射切割、水刀切割或數控機床進行加工的起點。

首先，在 Dynamo 中開啟 _Mesh-Toolkit\_Intersect-Mesh.dyn。_

![](<../images/6-2/2/meshToolkit case study - exercise 01.jpg>)

> 1. **檔案路徑：**找到要匯入的網格檔案 (_stanford\_bunny\_tri.obj_)。支援的檔案類型為 .mix 和 .obj
> 2. **Mesh.ImportFile：**連接檔案路徑以匯入網格

![](<../images/6-2/2/meshToolkit case study - exercise 02.jpg>)

> 1. **Point.ByCoordinates：**建構一個點 - 這將是弧的中心。
> 2. **Arc.ByCenterPointRadiusAngle：**在點週圍建構一個弧。這條曲線將用來定位一系列的平面。\_\_ 這些設定如下：\_\_ `radius: 40, startAngle: -90, endAngle:0`

建立一系列沿著弧轉向的平面。

![](<../images/6-2/2/meshToolkit case study - exercise 03.jpg>)

> 1. **Code Block**：建立 25 個介於 0 和 1 之間的數字。
> 2. **Curve.PointAtParameter：**將弧連接到 _curve_ 輸入並將 Code Block 輸出連接至 _param_ 輸入以擷取出一系列沿著曲線的點。
> 3. **Curve.TangentAtParameter：**連接與前一個節點相同的輸入。
> 4. **Plane.ByOriginNormal：**將點連接至 _origin_ 輸入並將 vector 連接至 _normal_ 輸入，在每個點建立一系列平面。

接下來，我們將使用這些平面與網格相交。

![](<../images/6-2/2/meshToolkit case study - exercise 04.jpg>)

> 1. **Mesh.Intersect：**將這些平面與匯入的網格相交，建立一系列 PolyCurve 輪廓線。在節點上按一下右鍵，並將交織設定為最長
> 2. **PolyCurve.Curves：**將 PolyCurve 切斷為曲線段。
> 3. **Curve.EndPoint：**擷取每條曲線的端點。
> 4. **NurbsCurve.ByPoints：**使用點來建構 NURBS 曲線。使用設定為_True_ 的 Boolean 節點，以封閉曲線。

在繼續之前，請關閉某些節點 (例如：Mesh.ImportFile、Curve.EndPoint、Plane.ByOriginNormal 以及 Arc.ByCenterPointRadiusAngle) 的預覽，以更清楚地查看結果。

![](<../images/6-2/2/meshToolkit case study - exercise 05.jpg>)

> 1. **Surface.ByPatch：**為每條輪廓線建構曲面修補，以便建立網格的「切片」。

新增第二組切片，產生格子/蛋盒的效果。

![](<../images/6-2/2/meshToolkit case study - exercise 06.jpg>)

您可能會發現與一個差不多的實體相比，網格相交作業的計算速度更快。例如本練習中示範的工作流程非常適合用於網格。
