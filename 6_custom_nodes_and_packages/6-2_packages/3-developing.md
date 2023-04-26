# 開發套件

Dynamo 提供了多種套件建立方式，供個人使用或與 Dynamo 社群分享。在以下研究的案例中，我們將瞭解如何透過解構既有的套件以設置套件。此案例研究以上一章的課程為基礎，會提供一組自訂節點，以便在 Dynamo 曲面之間依 UV 座標對映幾何圖形。

## MapToSurface 套件

我們將使用的範例套件會演示曲面之間點的 UV 對映。我們已在此手冊的[建立自訂節點](../10\_custom-nodes/10-2\_creating.md)一節中建置了工具的基礎內容。以下檔案將示範我們如何利用 UV 對映的概念，以及如何為可發佈資源庫開發一組工具。

在此影像中，我們將使用 UV 座標在曲面之間對映點。套件以此概念為基礎，但具有更複雜的幾何圖形。

![](../images/6-2/3/uvMap.jpg)

### 安裝套件

在上一章中，我們探索了根據 XY 平面中定義的曲線在 Dynamo 中將曲面面板化的方式。此案例研究將針對幾何圖形的更多標註延伸這些概念。我們會將此套件安裝為已建置的套件，以演示其開發方式。在下一節，我們將示範此套件的發佈方式。

在 Dynamo 中，按一下「套件」>「搜尋套件...」，然後搜尋套件「MapToSurface」(全部一個字)。按一下「安裝」以開始下載，並將套件加入您的資源庫。

![](../images/6-2/3/developpackage-installpackage01.jpg)

安裝後，「Add-ons」>「Dynamo Primer」區段下應該會出現自訂節點。

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

現在已安裝套件，接下來瞭解其設置方式。

### 自訂節點

我們將建立的套件會使用已建置供參考的五個自訂節點。接下來瞭解每個節點的行為。某些自訂節點會建置其他自訂節點，圖表的配置可供其他使用者以簡單的方式進行瞭解。

這是具有五個自訂節點的簡單套件。在以下步驟中，我們將簡要討論每個自訂節點的設置。

![](<../images/6-2/3/develop package - custom nodes 01 (1) (3).jpg>)

#### **PointsToSurface**

這是一個基本自訂節點，是其他所有對映節點的基礎。簡言之，節點會將來源曲面 UV 座標的點對映至目標曲面 UV 座標的位置。由於點是最基本的幾何圖形，以此為基礎會建置更複雜的幾何圖形，因此我們可以使用此邏輯在曲面之間對映 2D 甚至 3D 幾何圖形。

![](../images/6-2/3/developpackage-pointToSurface.jpg)

#### **PolygonsToSurface**

只需使用這裡的多邊形，即可示範將對映點從 1D 幾何圖形延伸至 2D 幾何圖形的邏輯。請注意，我們已將 _PointsToSurface_ 節點巢狀插入此自訂節點中。使用此方式，我們可以將每個多邊形的點對映到曲面，然後從這些對映的點重新產生多邊形。透過保持正確的資料結構 (點清單的清單)，我們可以在多邊形精簡為一組點後保持多邊形的獨立性。

![](../images/6-2/3/developpackage-polygonsToSurface.jpg)

#### **NurbsCrvtoSurface**

這裡套用的邏輯與 _PolygonsToSurface_ 節點中相同。但不是對映多邊形點，而是對映 NURBS 曲線的控制點。

![](../images/6-2/3/developpackage-nurbsCrvtoSurface.jpg)

**OffsetPointsToSurface**

此節點稍微複雜一些，但概念很簡單：此節點與 _PointsToSurface_ 節點類似，可在曲面之間對映點。但是，它也會考慮到不在原始來源曲面上的點，會取得這些點距最近 UV 參數的距離，並將此距離對映到對應 UV 座標處的目標曲面法線。如果查看範例檔案，會比較有感覺。

![](../images/6-2/3/developpackage-OffsetPointsToSurface.jpg)

#### **SampleSrf**

這個簡單節點會建立一個參數式曲面，從來源格線對映到範例檔案中的波浪曲面。

![](../images/6-2/3/developpackage-sampleSrf.jpg)

### 範例檔案

範例檔案可在套件的根資料夾中找到。按一下「Dynamo」>「偏好」>「Package Manager」

在 MapToSurface 旁邊，按一下垂直圓點功能表 >「展示根目錄」

![](../images/6-2/3/developpackage-examplefiles01.jpg)

接著開啟 _「extra」_ 資料夾，此資料夾包含套件中不是自訂節點的所有檔案。這是 Dynamo 套件的範例檔案 (若存在) 的儲存位置。以下螢幕擷取畫面討論每個範例檔案中示範的概念。

#### **01-PanelingWithPolygons**

此範例檔案示範如何根據矩形的格線使用 _PointsToSurface_ 將曲面平板化。這看起來應該很熟悉，因為我們在[上一章](../10\_custom-nodes/10-2\_creating.md)示範了類似的工作流程。

![](../images/6-2/3/developpackage-samplefile01.jpg)

#### **02-PanelingWithPolygons-II**

此練習檔案使用類似的工作流程，展示在從一個曲面將圓 (或表示圓的多邊形) 對映到另一個曲面的設置。此練習檔案使用 _PolygonsToSurface_ 節點。

![](../images/6-2/3/developpackage-samplefile02.jpg)

#### **03-NurbsCrvsAndSurface**

此範例檔案使用「NurbsCrvToSurface」節點，因此複雜性更高。會將目標曲面偏移指定的距離，並將 NURBS 曲線對映至原始目標曲面與偏移曲面。由此對對映的兩條曲線執行斷面混成以建立曲面，然後增厚該曲面。產生的這個實體具有代表目標曲面法線的波浪線。

![](../images/6-2/3/developpackage-samplefile03.jpg)

#### **04-PleatedPolysurface-OffsetPoints**

此範例檔案示範如何將褶狀的 PolySurface 從來源曲面對映到目標曲面。來源曲面與目標曲面分別是跨越格線的矩形曲面與旋轉曲面。

![](../images/6-2/3/developpackage-samplefile04a.jpg)

來源 PolySurface 從來源曲面對映到目標曲面。

![](../images/6-2/3/developpackage-samplefile04b.jpg)

#### **05-SVG-Import**

由於自訂節點可以對映不同類型的曲線，因此這最後一個檔案參考從 Illustrator 匯出的 SVG 檔案，並將匯入的曲線對映到目標曲面。

![](../images/6-2/3/developpackage-samplefile05a.jpg)

剖析整個 .svg 檔案的語法，將曲線從 .xml 格式轉換為 Dynamo polycurve。

![](../images/6-2/3/developpackage-samplefile05b.jpg)

將匯入的曲線對映到目標曲面。我們可藉此以明確方式 (點選) 在 Illustrator 中設計一個平板化物件，匯入至 Dynamo，然後套用到目標曲面。

![](../images/6-2/3/developpackage-samplefile05c.jpg)
