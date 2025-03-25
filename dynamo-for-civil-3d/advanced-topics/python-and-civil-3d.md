# Python 和 Civil 3D

雖然 Dynamo 作為[視覺程式設計](../../a\_appendix/a-1\_visual-programming-and-dynamo.md)工具的功能非常強大，但它不僅僅只有節點和線路，還能以文字形式撰寫程式碼。有兩種方法可以達到這個目的：

1. 使用程式碼區塊撰寫 **DesignScript**
2. 使用 Python 節點撰寫 **Python**

本節會重點介紹如何在 Civil 3D 環境中運用 Python，以善加活用 AutoCAD 和 Civil 3D .NET API。

{% hint style="info" %} 
請查看 [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") 一節，以取得有關在 Dynamo 中使用 Python 的更多一般資訊。 
{% endhint %}

## API 文件

AutoCAD 和 Civil 3D 兩者都有數個可用的 API，可讓像您這樣的開發人員透過自訂功能延伸核心產品。在 Dynamo 環境中，相關的是 **Managed .NET API**。以下連結對於瞭解 API 的結構及其運作方式非常重要。

[AutoCAD .NET API 開發指南](https://help.autodesk.com/view/OARX/2024/CHT/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2) (英文)

[AutoCAD .NET API 參考指南](https://help.autodesk.com/view/OARX/2024/CHT/?guid=OARX-ManagedRefGuide-What_s_New) (英文)

[Civil 3D .NET API 開發指南](https://help.autodesk.com/view/CIV3D/2024/CHT/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0) (英文)

[Civil 3D .NET API 參考指南](https://help.autodesk.com/view/CIV3D/2024/CHT/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331) (英文)

{% hint style="info" %} 
在您進行本節時，可能會有一些您不熟悉的概念，例如資料庫、交易、方法、性質等等。這當中的許多概念是使用 .NET API 的核心，並非專屬於 Dynamo 或 Python。詳細討論這些內容超出 Primer 本節的範圍，因此我們建議您經常參考上述連結以取得更多資訊。 
{% endhint %}

## 程式碼樣板

當您第一次編輯新的 Python 節點時，會預先填入樣板程式碼讓您開始使用。以下是樣板的分解，其中包含有關每個圖塊的說明。

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Civil 3D 中的預設 Python 樣板</p></figcaption></figure>

> 1. 匯入 `sys` 和 `clr` 模組，Python 解譯器必須有這兩個模組才能正常運作。尤其是 `clr` 模組，可讓 .NET 名稱空間基本上被視為 Python 套件。
> 2. 載入標準組合 (即 DLL)，以搭配 AutoCAD 和 Civil 3D 的 Managed .NET API 使用。
> 3. 加入標準 AutoCAD 和 Civil 3D 名稱空間的參考。這些參考分別相當於 C# 的 `using` 指示詞或 VB.NET 的 `Imports` 指示詞。
> 4. 使用名為 `IN` 的預先定義清單可存取節點的輸入埠。您可以使用埠的索引號碼存取特定埠的資料，例如 `dataInFirstPort = IN[0]`。
> 5. 取得作用中的文件和編輯器。
> 6. 鎖住文件並啟動資料庫交易。
> 7. 將大部分指令碼邏輯放在這裡。
> 8. 將這一行取消註解，即可在主要工作完成後提交交易。
> 9. 如果您要輸出節點中的任何資料，請在指令碼結尾將資料指定給 `OUT` 變數。

{% hint style="info" %} 
**想要自訂？**\
您可以編輯 `C:\ProgramData\Autodesk\C3D <version>\Dynamo` 中的 `PythonTemplate.py` 檔案，修改預設的 Python 樣板。 
{% endhint %}

## 範例

接下來我們透過一個範例來示範在 Dynamo for Civil 3D 中撰寫 Python 指令碼的一些基本概念。

### 目標

> :dart: 取得圖面中所有集水區的邊界幾何圖形。

### 資料集

以下是您可在此練習中參考的範例檔案。

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### 解決方案概觀

以下是此圖表中的邏輯概觀。

> 1. 檢閱 Civil 3D API 文件
> 2. 依圖層名稱選取文件中所有的集水區
> 3. 「拆開」Dynamo 物件以存取內部的 Civil 3D API 成員
> 4. 從 AutoCAD 點建立 Dynamo 點
> 5. 從點建立 PolyCurve

我們開始吧！

### 檢閱 API 文件

在開始建置圖表和撰寫程式碼之前，最好先查看 Civil 3D API 文件，瞭解 API 可提供哪些內容。在此案例中，[集水區類別中有一個性質](https://help.autodesk.com/view/CIV3D/2024/CHT/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c)，將傳回集水區的邊界點。請注意，此性質會傳回 `Point3dCollection` 物件，但 Dynamo 不會知道該如何處理這個物件。換言之，我們無法從 `Point3dCollection` 建立 PolyCurve，因此最終我們需要將所有內容轉換為 Dynamo 點。我們稍後會再說明。

### 取得所有集水區

現在，我們可以開始建置圖表邏輯。首先，取得文件中所有集水區的清單。有一些節點可以進行，因此我們不需要在 Python 指令碼中包含這項作業。使用節點可讓其他人 (比起在 Python 指令碼中放入大量程式碼) 更容易閱讀圖表，也可以讓 Python 指令碼只專注在一件事：傳回集水區的邊界點。

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>依圖層取得文件中所有的集水區</p></figcaption></figure>

請注意，**All Objects on Layer** 節點的輸出是一個 CivilObject 的清單。這是因為 Dynamo for Civil 3D 目前沒有任何節點可處理集水區，因此我們需要透過 Python 存取 API。

### 拆開物件

進一步瞭解之前，先需要簡單討論一個重要概念。在[node-library.md](../node-library.md "mention")一節，我們討論了 Object 與 CivilObject 的關聯方式。再更詳細一點，**Dynamo Object** 是 **AutoCAD Entity** 的一個包裝函式。同樣地，**Dynamo CivilObject** 是 **Civil 3D Entity** 的一個包裝函式。您可以存取物件的 `InternalDBObject` 或 `InternalObjectId` 性質來「拆開」物件。

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Dynamo 類型</th><th width="373">包裝</th></tr></thead><tbody><tr><td><strong>Object</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entity</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entity</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} 根據經驗法則，使用 `InternalObjectId` 性質取得物件 ID，然後在交易中存取包裝後的物件通常比較安全。這是因為 `InternalDBObject` 性質會傳回非處於可寫入狀態的 AutoCAD DBObject。 
{% endhint %}

### Python 指令碼

以下是存取內部集水區物件並取得其邊界點的完整 Python 指令碼。亮顯的行表示從預設樣板程式碼修改/增加的行。

{% hint style="info" %} 
按一下指令碼中加底線的文字，可查看每一行的說明。 
{% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># 載入 Python 標準和 DesignScript 資源庫
import sys
import clr

# 加入 AutoCAD 和 Civil3D 的組合
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# 從 AutoCAD 匯入參考
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# 從 Civil3D 匯入參考
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# 此節點的輸入將以清單方式儲存在 IN 變數中。
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("The input is null or empty.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>   
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>             
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>               
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                   
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>           
            # 在結束交易前提交
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# 將輸出指定給 OUT 變數。
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} 根據經驗法則，最好是將大部分指令碼邏輯內容放在交易內。這可確保安全地存取指令碼讀取/寫入的物件。在許多情況下，忽略交易可能會導致嚴重錯誤。 
{% endhint %}

### 建立 PolyCurve

在此階段，Python 指令碼應該會輸出一個 Dynamo 點清單，您可以在背景預覽中看到這些點。最後一步只是從這些點建立 PolyCurve。請注意，您也可以直接在 Python 指令碼中完成這一步，但我們刻意將它放在指令碼外的節點中，這樣可以看得更清楚。這是最終圖表的外觀。

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>最終圖表</p></figcaption></figure>

### 結果

這是最終的 Dynamo 幾何圖形。

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>集水區邊界產生的 Dynamo PolyCurve</p></figcaption></figure>

> :tada: 任務完成！

## IronPython 與 CPython

在收尾前，我們在這裡快速地總結。根據您使用的 Civil 3D 版本，Python 節點的規劃可能會有所不同。在 **Civil 3D 2020 和 2021** 中，Dynamo 使用一個稱為 **IronPython** 的工具，在 .NET 物件與 Python 指令碼之間移動資料。但是，在 **Civil 3D 2022** 中，Dynamo 已轉變為使用標準原生的 Python 解譯器 (也稱為 **CPython**)，而不是使用 Python 3。這項轉換的優點包括可存取常見的新式資源庫和新的平台功能、基本維護和安全性修補。

{% hint style="info" %} 
您可以在 [Dynamo 部落格](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/) 閱讀更多有關此項轉換，以及如何升級舊式指令碼的資訊。如果您想要繼續使用 IronPython，只需使用 Dynamo Package Manager 安裝 **DynamoIronPython2.7** 套件。 
{% endhint %}

[^1]: Dynamo 幾何圖形資源庫預設不會加到 Python 環境中。這個指令碼的目標是輸出集水區邊界的 Dynamo 點清單，因此我們需要加入這一行，稍後才能建立點。

[^2]: 這一行會從 Dynamo 幾何圖形資源庫取得我們需要的特定類別。請注意，我們在這裡指定 `import Point as DynPoint`，而不是 `import *`，因為後者會導致命名衝突。

[^3]: 我們在這裡將預設的 `dataEnteringNode` 變數更名為 `objs`，讓它更清楚。

[^4]: 我們在這裡明確指定哪個輸入埠包含需要的資料，而不是預設的 `IN`，後者指的是所有輸入的整個清單。

[^5]: 這會建立一個空清單，我們稍後將在當中加入輸出資料。

[^6]: 這是為了防止指令碼中可能存在空白輸入或空值輸入。

[^7]: 我們不是中斷指令碼而是輸出有用的訊息，以說明指令碼無法繼續的原因。

[^8]: 指令碼的剩餘部分假設我們有一個要處理的物件清單 (而非單一物件)。但是，如果只有一個物件 (即只有單一集水區的圖面)，我們仍希望指令碼能夠執行。

[^9]: 如果輸入不是清單 (即單一物件)，我們只需建立一個新清單，物件是唯一的項目。

[^10]: 開始一個迴圈，對清單中的每個物件執行。

[^11]: 取得 Dynamo 物件的物件 ID，來「拆開」Dynamo 物件。

[^12]: 從 AutoCAD 資料庫擷取「包裝後」的物件。請注意，這裡將 OpenMode 設定為 `ForRead`，因為我們不打算對物件進行任何編輯。我們只是在「查詢」資料。

[^13]: 物件的輸入清單可能混合集水區和其他非集水區項目。我們需要檢查是否有這樣的情況並適當處理 (即只有當項目確實是集水區時，才繼續迭代迴圈)。

[^14]: 如果指令碼執行到這裡，我們就知道物件確實是集水區。我們在這裡加入一個新變數，只是為了讓命名維持簡潔且易於理解。

[^15]: 我們在這裡使用先前在 API 文件中尋找的適當性質擷取集水區邊界點。如先前所述，這會產生一個 `Point3dCollection` 物件，這本質上是 AutoCAD 點的清單。我們需要將這些點「轉換」為 Dynamo 點，讓它們能夠使用。

[^16]: 建立一個空清單以儲存此集水區的邊界點。

[^17]: 開始一個迴圈，對 `Point3dCollection` 中的每個 `Point3d` 物件執行。

[^18]: 使用 AutoCAD 點的座標建立 Dynamo 點。

[^19]: 在清單中加入點。

[^20]: 將所有 AutoCAD 點「轉換」成 Dynamo 點後，我們在輸出中加入產生的清單。之後，外層迴圈會繼續進行到輸入清單中的下一個物件。

[^21]: 將這一行取消註解以提交交易。

[^22]: 最後 (也是同樣重要的)，我們輸出 Dynamo 點的清單。
