# 資源庫

資源庫包含所有載入的節點，其中包括 10 個安裝隨附的預設品類節點，以及已載入的其他所有自訂節點或套件。資源庫中的節點在資源庫、品類和子品類 (如果有) 內以階層方式組織。

![](images/3-2/library-libraryUI.jpg)

* 基本節點：預設安裝隨附。
* 自訂節點：將常用常式或特殊圖表儲存為自訂節點。您也可以與社群共用您的自訂節點
* Package Manager 中的節點：收集已發佈的自訂節點。

我們將瀏覽[節點品類的階層](3-3\_dynamo\_libraries.md#library-hierarchy-for-categories)，示範如何[從資源庫快速搜尋](3-3\_dynamo\_libraries.md#quick-search-in-library)，並了解其中某些[常用節點](3-3\_dynamo\_libraries.md#frequently-used-nodes)。

### 品類的資源庫階層

透過在這些品類中進行瀏覽，能以最快方式瞭解可以加入到工作區的項目所在的階層，並以最佳方式探索尚未使用的新節點。

透過四處按一下功能表瀏覽資源庫，展開每個品類及其子品類

{% hint style="info" %} Geometry 功能表很適合在開始探索時使用，因為它們包含最多數量的節點。{% endhint %}

![](images/3-2/library-modifiedandresizelibrarycategories.jpg)

> 1. 資源庫
> 2. 品類
> 3. 子品類
> 4. 節點

這些項目會根據節點是**建立**資料、執行**動作**或**查詢**資料，進一步分類成有相同子品類的節點。

* \![](<images/3-2/user interface - create.jpg>) **建立**：從頭開始建立或建構幾何圖形。例如圓。
* \![](<images/3-2/user interface - action.jpg>) **動作**：對物件執行動作。例如，調整圓的比例。
* \![](<images/3-2/user interface - query.jpg>) **查詢**：取得已存在物件的性質。例如，取得圓的半徑。

將滑鼠懸停在節點上，可顯示名稱和圖示之外更詳細的資訊。我們由此可以快速了解節點的功能、所需的輸入及其提供的輸出。

\![](<images/3-2/user interface - node description.jpg>)

> 1. 描述 - 節點的普通語言描述
> 2. 圖示 -「資源庫」功能表中更大版本的圖示
> 3. 輸入 - 名稱、資料類型與資料結構
> 4. 輸出 - 資料類型與結構

### 在資源庫中快速搜尋

如果您知道希望加入至工作區的節點相關特性，在 **「搜尋」** 欄位中鍵入可查詢所有相符的節點。

選擇按一下要加入的節點，或按 Enter 將亮顯的節點加入工作區的中心。

\![](<images/3-2/user interface - search.jpg>)

#### 依階層搜尋

除了使用關鍵字嘗試尋找節點，我們還可以在「搜尋欄位」中鍵入以句點分隔的階層，或使用程式碼區塊 (使用 _Dynamo 文字語言_)。

每個資源庫的階層都會反映在加入工作區的節點名稱中。

在資源庫階層中以 `library.category.nodeName` 格式鍵入節點位置的不同部分，會傳回不同的結果

* `library.category.nodeName`

![](images/3-2/library-searchbyhierarchygeometrypointbycoordinates\(1\).jpg)

* `category.nodeName`

![](images/3-2/library-searchbyhierarchy2pointbycoordinates.jpg)

* `nodeName` 或 `keyword`

![](images/3-2/library-searchbyhierarchy3bycoordinates.jpg)

通常，工作區中節點的名稱將以 `category.nodeName` 格式呈現，但在「Input」與「View」品類中有一些明顯的例外。

請注意名稱相似的節點，並注意品類差異：

* 大多數資源庫中的節點將包括品類格式

![](images/3-2/library-nodecategorydifferences1.jpg)

* `Point.ByCoordinates` 和 `UV.ByCoordinates` 的名稱相同，但來自不同品類

![](images/3-2/library-nodecategorydifferences2.jpg)

* 明顯的例外包括內建函數、Core.Input、Core.View 及運算子

![](images/3-2/library-nodecategorydifferences3.jpg)

### 常用的節點

Dynamo 的基本安裝中包括數百個節點，哪些節點對於開發視覺程式非常重要？接下來我們著重了解定義程式參數 (**Input**)、查看節點動作結果 (**Watch**) 以及透過捷徑 (**Code Block**) 定義輸入或功能所使用的節點。

#### 輸入節點

輸入節點是視覺程式的使用者 (不論是您自己還是他人) 與關鍵參數結合的主要方式。以下是核心資源庫中一些可用的項目：

| 節點           |                                           | 節點           |                                           |
| -------------- | ----------------------------------------- | -------------- | ----------------------------------------- |
| Boolean        | ![](images/3-2/library-boolean.jpg)       | Number         | ![](images/3-2/library-number.jpg)        |
| String         | ![](images/3-2/library-string.jpg)        | Number Slider  | ![](images/3-2/library-numberslider.jpg)  |
| Directory Path | ![](images/3-2/library-directorypath.jpg) | Integer Slider | ![](images/3-2/library-integerslider.jpg) |
| File Path      | ![](images/3-2/library-filepath.jpg)      |                |                                           |

#### Watch 與 Watch3D

Watch 節點對於管理流經視覺程式的資料非常重要。您可以透過**節點資料預覽**，將滑鼠懸停在節點上，來檢視節點的結果。

![](images/3-2/library-nodepreview.jpg)

在 **Watch** 節點中保持顯示會很有用

![](images/3-2/library-watchnode.jpg)

或透過 **Watch3D** 節點查看幾何圖形結果。

![](images/3-2/library-watch3dnode.gif)

這兩個節點都位於核心資源庫內的 View 品類中。

{% hint style="info" %} 秘訣：若視覺程式包含許多節點，3D 預覽有時可能會分散您的注意力。請考慮不勾選「設定」功能表中的「展示背景預覽」選項，並使用 Watch3D 節點預覽幾何圖形。{% endhint %}

#### Code Block

Code Block 節點可以用於定義一塊程式碼 (以分號分隔各行)。這可以像 `X/Y` 一樣簡單。

我們也可以使用 Code Block 做為捷徑定義數字輸入或呼叫其他節點的功能。執行此作業的語法遵循 Dynamo 文字語言 [DesignScript](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md) 的命名慣例。

以下是在腳本中使用 Code Block 的簡單示範 (含指示)。

\![](<images/3-2/library-code block demo.gif>)

1. 按兩下以建立 Code Block 節點
2. 鍵入 `Circle.ByCenterPointRadius(x,y);`
3. 按一下工作區以清除選取，這會自動加入 `x` 和 `y` 輸入。
4. 建立 Point.ByCoordinates 節點與 Number Slider，然後將其連接至 Code Block 的輸入。
5. 執行視覺程式的結果如 3D 預覽中顯示為圓
