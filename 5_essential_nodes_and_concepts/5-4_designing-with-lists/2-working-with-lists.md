# 處理清單

### 使用清單

現在我們已建立清單，接下來討論可以對清單執行哪些作業。將清單想像為一副紙牌。這副紙牌是清單，而其中每張紙牌都代表一個項目。

![紙牌](../images/5-4/2/Playing\_cards\_modified.jpg)

> 相片由 [Christian Gidlöf](https://commons.wikimedia.org/wiki/File:Playing\_cards\_modified.jpg) 拍攝

### 查詢

我們可以對清單執行哪些**查詢**？這將存取既有性質。

* 這副紙牌有多少張？52 張。
* 有幾種花色？4 種。
* 用哪種材料製成？紙。
* 長度是多少？3.5" 或 89mm。
* 寬度是多少？2.5" 或 64mm。

### 動作

我們可以對清單執行哪些**動作**？這會根據指定的作業變更清單。

* 我們可以重新洗牌。
* 我們可以根據點數對紙牌排序。
* 我們可以根據花色對紙牌排序。
* 我們可以拆分紙牌。
* 我們可以雙手各握一部分紙牌。
* 我們可以選取其中特定的某張牌。

以上列示的所有作業都有類似的 Dynamo 節點，供您使用一般資料的清單。以下課程將展示我們可以對清單執行的一些基本作業。

## **練習**

### **清單作業**

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/2/List-Operations.dyn" %}

以下影像是基礎圖表，我們在兩個圓之間繪製線以表示基本清單作業。我們將探究如何管理清單內的資料，並透過以下清單動作示範視覺結果。

![](<../images/5-4/2/working with list - list operation.jpg>)

> 1. 首先使用一個 **Code Block**，值為 `500;`
> 2. 插入 **Point.ByCoordinates** 節點的 x 輸入。
> 3. 將上一步驟的節點插入 **Plane.ByOriginNormal** 節點的 origin 輸入。
> 4. 使用 **Circle.ByPlaneRadius** 節點，將上一步驟的節點插入 plane 輸入。
> 5. 使用 **Code Block**，為 radius 指定 `50;` 的值。這是我們要建立的第一個圓。
> 6. 使用 **Geometry.Translate** 節點，在 Z 方向將圓上移 100 個單位。
> 7. 使用 **Code Block** 節點，透過以下程式碼定義 10 個 0 和 1 之間的數字：`0..1..#10;`
> 8. 將上一步驟的 Code Block 插入兩個 **Curve.PointAtParameter** 節點的 _param_ 輸入。將 **Circle.ByPlaneRadius** 插入頂部節點的 curve 輸入，將 **Geometry.Translate** 插入下方節點的 curve 輸入。
> 9. 使用 **Line.ByStartPointEndPoint**，連接兩個 **Curve.PointAtParamete**_r_ 節點。

### List.Count

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/2/List-Count.dyn" %}

_List.Count_ 節點很簡單：它會對清單中的值進行計數，並傳回該數量。使用清單的清單時，此節點將更為精細，不過我們將在後續章節示範該內容。

![Count](<../images/5-4/2/working with list - list operation - list count.jpg>)

> 1. **List.Count**_****_ 節點會傳回 **Line.ByStartPointEndPoint** 節點中的線數量。在此案例中，該值為 10，這與從原始 **Code Block** 節點建立的點數一致。

### List.GetItemAtIndex

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/2/List-GetItemAtIndex.dyn" %}

**List.GetItemAtIndex** 是對清單中的項目進行查詢的基本方式。

![Exercise](<../images/5-4/2/working with list - get item index 01.jpg>)

> 1. 首先，在 **Line.ByStartPointEndPoint** 節點上按一下右鍵以關閉其預覽。
> 2. 使用 **List.GetItemAtIndex** 節點，我們將選取索引 _0_ 或線清單中的第一個項目。

變更介於 0 到 9 之間的滑棒值，以使用 **List.GetItemAtIndex** 選取其他項目。

![](<../images/5-4/2/working with list - get item index 02.gif>)

### List.Reverse

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/2/List-Reverse.dyn" %}

_List.Reverse_ 會反轉清單中所有項目的順序。

![Exercise](<../images/5-4/2/working with list - list reverse.jpg>)

> 1. 若要正確顯示反轉的線清單，請將 **Code Block** 變更為 `0..1..#50;` 以建立更多條線
> 2. 複製 **Line.ByStartPointEndPoint** 節點，在 **Curve.PointAtParameter** 與第二個 **Line.ByStartPointEndPoint** 之間插入 List.Reverse 節點
> 3. 使用 **Watch3D** 節點可預覽兩個不同的結果。第一個節點顯示無反轉清單的結果。線垂直連接至相鄰的點。但是，反轉清單會以另一個清單中的相反順序連接所有點。

### List.ShiftIndices <a href="#listshiftindices" id="listshiftindices"></a>

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/2/List-ShiftIndices.dyn" %}

**List.ShiftIndices** 是建立扭轉或螺旋樣式或任何其他類似資料處理的良好工具。此節點會將清單中的項目移位指定數量的索引。

![Exercise](<../images/5-4/2/working with list - shiftIndices 01.jpg>)

> 1. 採用對反轉清單的相同程序，將 **List.ShiftIndices** 插入 **Curve.PointAtParameter** 與 **Line.ByStartPointEndPoint**。
> 2. 使用 **Code Block**，指定值「1」將清單移位一個索引。
> 3. 請注意，變更很小，但下方 **Watch3D** 節點中所有的線在連接至其他組點時已移位一個索引。

將 **Code Block** 變更為較大的值 (例如 _30_)，我們發現對角線有顯著不同。在此範例中，此移位的作用類似於相機的光圈，對原始圓柱形產生了扭轉。

![](<../images/5-4/2/working with list - shiftIndices 02.jpg>)

### List.FilterByBooleanMask <a href="#listfilterbybooleanmask" id="listfilterbybooleanmask"></a>

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/2/List-FilterByBooleanMask.dyn" %}

![](../images/5-4/2/ListFilterBool.png)

**List.FilterByBooleanMask** 將根據一系列布林值或者「True」或「False」值移除某些項目。

![Exercise](<../images/5-4/2/working with list - filter by bool mask.jpg>)

為了建立一系列「True」或「False」值，我們需要多做一些工作...

> 1. 使用 **Code Block**，採用以下語法定義表示式：`0..List.Count(list);`。將 **Curve.PointAtParameter** 節點連接至 _list_ 輸入。我們將在程式碼區塊一章中更詳細地講解此設置，但此案例中的程式碼行將產生代表 **Curve.PointAtParameter** 節點每個索引的清單。
> 2. 使用_**%**_** (模數) ** 節點，將 _code block_ 的輸出連接至 _x_ 輸入，將值 _4_ 連接至 _y_ 輸入。這會產生索引清單除以 4 時的餘數。模數在建立樣式時是非常有用的節點。4 的所有可能餘數包括：0、1、2、3。
> 3. 從 _**%**_** (模數)** 節點，我們知道值 0 表示索引可由 4 整除 (0、4、8 等)。使用 **==** 節點，我們可以測試餘數的值是否為 _0_，以測試是否能整除。
> 4. **Watch** 節點顯示此狀況：True/False 樣式為：_true,false,false,false..._。
> 5. 使用此 True/False 樣式，連接至兩個 **List.FilterByBooleanMask** 節點的 mask 輸入。
> 6. 將 **Curve.PointAtParameter** 節點連接至 **List.FilterByBooleanMask** 的每個 list 輸入。
> 7. **Filter.ByBooleanMask** 的輸出為 _in_ 與 _out_。_in_ 表示遮罩值為 _true_ 的值，而 _out_ 表示遮罩值為 _false_ 的值。透過將 _in_ 輸出插入 **Line.ByStartPointEndPoint** 節點的 _startPoint_ 與 _endPoint_ 輸入，我們建立出經過篩選的線清單。
> 8. **Watch3D** 節點顯示出線比點少。我們只篩選了 True 值，因此只選取了 25% 的節點。
