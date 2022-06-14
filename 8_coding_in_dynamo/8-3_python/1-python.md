# Python 節點

為什麼要在 Dynamo 的視覺程式設計環境中使用文字程式設計？[視覺程式設計](../../a\_appendix/visual-programming-and-dynamo.md)有許多優點。您可藉此在直觀視覺介面中建立程式時，不必學習特殊語法。但是，視覺程式可能會變得雜亂，有時會缺少功能。例如，Python 會提供可實現程度高得多的方法，以編寫條件陳述式 (if/then) 及迴圈。Python 是功能強大的工具，可以延伸 Dynamo 的功能，您可藉此使用幾行簡潔的程式碼來取代許多節點。

**視覺程式：**

![](<../images/8-3/1/python node - visual vs textual programming.jpg>)

**文字程式：**

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

solid = IN[0]
seed = IN[1]
xCount = IN[2]
yCount = IN[3]

solids = []

yDist = solid.BoundingBox.MaxPoint.Y-solid.BoundingBox.MinPoint.Y
xDist = solid.BoundingBox.MaxPoint.X-solid.BoundingBox.MinPoint.X

for i in xRange:
	for j in yRange:
		fromCoord = solid.ContextCoordinateSystem
		toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin,Vector.ByCoordinates(0,0,1),(90*(i+j%val)))
		vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
		toCoord = toCoord.Translate(vec)
		solids.append(solid.Transform(fromCoord,toCoord))

OUT = solids
```

### Python 節點

與程式碼區塊類似，Python 節點是視覺程式設計環境中的指令碼撰寫介面。Python 節點位於資源庫中的「Script」>「Editor」>「Python Script」下。

![](<../images/8-3/1/python node - the python node 01.jpg>)

按兩下節點將開啟 Python 指令碼編輯器 (您也可以在節點上按一下右鍵，然後選取_「編輯...」_)。您會注意到頂部有一些重複使用文字，其目的是協助您參考將需要的資源庫。輸入儲存於 IN 陣列中。透過為 OUT 變數指定值，會將這些值傳回至 Dynamo

![](<../images/8-3/1/python node - the python node 02.jpg>)

藉由 Autodesk.DesignScript.Geometry 資源庫，您可以使用與程式碼塊類似的圓點符號。如需 Dynamo 語法的更多資訊，請參閱 [7-2\_design-script-syntax.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "提及")以及 [DesignScript 指南](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf) (若要下載此 PDF 文件，請在連結上按一下右鍵，然後選擇「另存連結為...」)。鍵入幾何圖形類型，例如「Point.」。將顯示建立及查詢點的方法清單。

![](<../images/8-3/1/python node - the python node 03.jpg>)

> 這些方法包括建構函式 (例如 _ByCoordinates_)、動作 (例如 _Add_) 以及查詢 (例如 _X_、_Y_ 與 _Z_ 座標)。

## 練習：使用 Python 指令碼從實體模組建立樣式的自訂節點

### 第 I 部分：設定 Python 指令碼

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供範例檔案的完整清單。

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

在此範例中，我們將編寫從實體模組建立樣式的 Python 指令碼，然後將其轉換為自訂節點。首先，使用 Dynamo 節點建立實體模組。

![](<../images/8-3/1/python node - exercise pt I-01.jpg>)

> 1. **Rectangle.ByWidthLength：**建立將做為實體基準的矩形。
> 2. **Surface.ByPatch：**將矩形連接至「_closedCurve_」輸入以建立底部曲面。

![](<../images/8-3/1/python node - exercise pt I-02.jpg>)

> 1. **Geometry.Translate：**將矩形連接至「_geometry_」輸入以將其上移，使用程式碼塊指定實體的基準厚度。
> 2. **Polygon.Points：**查詢平移的矩形以萃取角點。
> 3. **Geometry.Translate：**使用程式碼塊建立包含四個值 (對應於四個點) 的清單，同時將實體的一個角點上移。
> 4. **Polygon.ByPoints：**使用平移的點重新建構頂部多邊形。
> 5. **Surface.ByPatch：**連接多邊形以建立頂部曲面。

現在我們已建立頂部與底部曲面，接下來在兩個縱斷面之間進行斷面混成，以建立實體的側面。

![](<../images/8-3/1/python node - exercise pt I-03.jpg>)

> 1. **List.Create：**將底部矩形與頂部多邊形連接至索引輸入。
> 2. **Surface.ByLoft：**對兩個縱斷面進行斷面混成，以建立實體的側面。
> 3. **List.Create：**將頂部、側面與底部的曲面連接至索引輸入，以建立曲面清單。
> 4. **Solid.ByJoinedSurfaces：**接合曲面以建立實體模組。

現在我們已建立實體，接下來將 Python Script節點放至工作區。

![](<../images/8-3/1/python node - exercise pt I-04.jpg>)

> 1. 若要在節點中加入其他輸入，請按一下節點上的「+」圖示。輸入命名為 IN\[0]、IN\[1] 等以指示其代表清單中的項目。

我們從定義輸入與輸出開始。按兩下節點以開啟 python 編輯器。請依照下面的程式碼，在編輯器中修改程式碼。

![](<../images/8-3/1/python node - exercise pt I-05.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []

# Place your code below this line


# Assign your output to the OUT variable.
OUT = solids
```

在我們進行練習時，此程式碼會更容易理解。接下來，我們需要考慮需要哪些資訊以排列實體模組。首先，我們需要知道實體的標註，以確定平移距離。由於存在邊界框錯誤，我們不得不使用邊曲線幾何圖形來建立邊界框。

![](../images/8-3/1/python07.png)

> 看一下 Dynamo 中的 Python 節點。請注意，我們使用的語法與 Dynamo 節點標題中的語法相同。查看下面加上註解的程式碼。

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

# Assign your output to the OUT variable.
OUT = solids
```

由於我們將平移並旋轉實體模組，因此接下來使用 Geometry.Transform 作業。看一下 Geometry.Transform 節點，我們知道需要來源座標系統與目標座標系統，以平移實體。來源是實體的關聯座標系統，而目標是所排列每個模組的不同座標系統。這意味著我們必須循環使用 x 值與 y 值，以便每次以不同方式平移座標系統。

![](<../images/8-3/1/python node - exercise pt I-06.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

#Get the source coordinate system
fromCoord = solid.ContextCoordinateSystem

#Loop through x and y
for i in range(xCount):
    for j in range(yCount):
        #Rotate and translate the coordinate system
        toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin, Vector.ByCoordinates(0,0,1), (90*(i+j%seed)))
        vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
        toCoord = toCoord.Translate(vec)
        #Transform the solid from the source coord syste, to the target coord system and append to the list
        solids.append(solid.Transform(fromCoord,toCoord))

# Assign your output to the OUT variable.
OUT = solids
```

按一下「執行」，然後儲存程式碼。將 Python 節點與既有的指令碼連接，如下所示。

![](<../images/8-3/1/python node - exercise pt I-07.jpg>)

> 1. 將 **Solid.ByJoinedSurfaces** 的輸出連接為 Python 節點的第一個輸入，並使用 Code Block 定義其他輸入。
> 2. 建立 **Topology.Edges** 節點，並使用 Python 節點的輸出做為其輸入。
> 3. 最後，建立 **Edge.CurveGeometry** 節點，並使用 Topology.Edges 的輸出做為其輸入。

請嘗試變更種子值以建立不同的樣式。您也可以變更實體模組本身的參數，以取得不同的效果。

![](../images/8-3/1/python10.png)

### 第 II 部分：將 Python Script 節點轉換為自訂節點

現在我們已建立有用的 Python 指令碼，接下來將其另存成自訂節點。選取 Python Script 節點，在「工作區」上按一下右鍵，然後選取「建立自訂節點」。

![](<../images/8-3/1/python node - exercise pt II-01.jpg>)

指定名稱、描述與品類。

![](<../images/8-3/1/python node - exercise pt II-02.jpg>)

這會開啟一個新的工作區，可從中編輯自訂節點。

![](<../images/8-3/1/python node - exercise pt II-03.jpg>)

> 1. **Input：**變更輸入名稱以更具描述性，然後加入資料類型及預設值。
> 2. **Output：**變更輸出名稱

將節點儲存為 .dyf 檔案，您應該會看到自訂節點反映我們剛才所做的變更。

![](<../images/8-3/1/python node - exercise pt II-04.jpg>)
