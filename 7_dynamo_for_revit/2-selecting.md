# 選取

### 選取 Revit 元素

Revit 是資料豐富的環境。這能為我們提供許多選取功能，而不僅僅是「點選」。我們可以查詢 Revit 資料庫，並將 Revit 元素動態連結至 Dynamo 幾何圖形，同時執行參數式作業。

使用者介面中的 Revit 資源庫提供「Selection」品類，藉此可採用許多方式選取幾何圖形。

![](../.gitbook/assets/select\_revit\_elements\_01.jpg)

### Revit 階層

若要正確選取 Revit 元素，請務必全面理解 Revit 元素階層。要選取專案中所有的牆嗎？請依品類選取。要選取中世紀現代大廳中的每把 Eames 椅子嗎？請依族群選取。

我們來快速複習 Revit 階層。

![](images/2/hierarchy.png)

記得生物學的分類法嗎？界、門、綱、目、科、屬、種？Revit 元素的分類方式與此類似。在基本層級，可將 Revit 階層分為不同的品類、族群、類型*及例證。例證是個別模型元素 (具有唯一的 ID)，而品類可定義一般群組 (例如「牆」或「地板」)。以此方式組織 Revit 資料庫後，我們可以選取一個元素，然後根據階層中的指定層級選擇所有類似元素。

{% hint style="warning" %} *Revit 中類型的定義與程式設計中的類型不同。在 Revit 中，類型是指階層的分支，而非「資料類型」。{% endhint %}

### 使用 Dynamo 節點進行資料庫導覽

以下三個影像分別展示了 Dynamo 中 Revit 元素選取的主要品類。這些工具十分適合互相搭配使用，在後續練習中會研究其中部分工具。

_點選_ 是直接選取 Revit 元素最簡單的方式。您可以選取完整的模型元素，也可以選取其拓樸的一部分 (例如一個面或一條邊)。這會與該 Revit 物件保持動態連結，因此在 Revit 檔案更新其位置或參數時，參考的 Dynamo 元素在圖表中也將更新。

![](../.gitbook/assets/selecting\_database\_navigation\_with\_dynamo\_nodes\_01.jpg)

_下拉式功能表_ 會建立 Revit 專案中所有可存取元素的清單。您可以使用下拉式功能表參考視圖中不一定可見的 Revit 元素。這是非常強大的工具，可用於在 Revit 專案或族群編輯器中查詢既有元素或建立新元素。

\![](../.gitbook/assets/selecting _database_navigation_with_dynamo_nodes_02.png)

您也可以依 _Revit 階層_中的特定層級選取 Revit 元素。這是一個功能強大的選項，可自訂大型資料陣列，以準備進行記錄或生產實體化及客製化。

![UI](../.gitbook/assets/allelements.jpg)

記住以上三個影像，接下來深入練習，練習會選取基本 Revit 專案中的元素，為我們將在本章其餘各節建立的參數式應用程式做好準備。

## 練習

> 在下方的連結按一下，下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="datasets/2/Revit-Selecting.zip" %}

在此範例 Revit 檔案中，包含一個簡單建築的三種元素類型。我們會以此為例，在 Revit 階層的環境中選取 Revit 元素。

\![](<../.gitbook/assets/selecting_exercise_01 (1) (1).jpg>)

> 1. 建築量體
> 2. 樑 (結構框架)
> 3. 桁架 (自適應元件)

根據 Revit 專案視圖中目前存在的元素，我們可以做出哪些結論？若要選取適當的元素，我們需要在階層中下移多遠？處理大型專案時，這無疑會變為更複雜的工作。有許多選項可供使用，選取元素時可依據品類、層級、族群、例證等。

### 選取量體和表面

![](../.gitbook/assets/selecting\_exercise\_02.jpg)

> 1. 由於我們使用基本設置，因此我們在「Categories」下拉式節點中選擇 _「Mass」_ 來選取建築量體。您可以在「Revit」>「Selection」頁籤中找到。
> 2. 「Mass」品類的輸出是品類自身。我們需要選取元素。為了執行此作業，我們使用 _All Elements of Category_ 節點。

此時請注意，我們在 Dynamo 中看不到任何幾何圖形。我們已選取 Revit 元素，但尚未將該元素轉換為 Dynamo 幾何圖形。這是重要的區分。選取大量元素時，不建議在 Dynamo 中預覽所有元素，因為這樣會拖慢所有作業的速度。Dynamo 這個工具無需執行幾何運算，即可管理 Revit 專案。本章的下一節會進一步說明該功能。

在此案例中，我們將使用簡單的幾何圖形，因此希望將幾何圖形引入 Dynamo 預覽。上面 Watch 節點中的「BldgMass」旁有一個綠色數字。這代表該元素的 ID，可看出我們目前在處理 Revit 元素，而不是 Dynamo 幾何圖形。下一步是將此 Revit 元素轉換為 Dynamo 中的幾何圖形。

![](../.gitbook/assets/selecting\_exercise\_03.jpg)

> 1. 使用 _Element.Faces_ 節點，可取得一份曲面清單，代表 Revit 量體的每一個面。我們現在可以在 Dynamo 視埠中看到幾何圖形，可以開始參考用於參數式作業的面。

以下是替代方法。在此案例中，我們不是透過 Revit 階層選取 _(「All Elements of Category」)_，而是選擇在 Revit 中明確選取幾何圖形。

![](../.gitbook/assets/selecting\_exercise\_04.jpg)

> 1. 使用 _Select Model Element_ 節點，按一下「選取」(或 _「變更」_) 按鈕。在 Revit 視埠中，選取所需的元素。在此案例中，我們將選取建築量體，
> 2. 我們可以使用 _Element.Geometry_ 選取完整量體作為一個實體幾何圖形，而非 _Element.Faces_。這會選取該量體內包含的所有幾何圖形。
> 3. 我們可以使用 _Geometry.Explode_ 再次得到曲面清單。這兩個節點的運作方式與 _Element.Faces_ 相同，但是提供其他選項用於探究 Revit 元素的幾何圖形。

使用一些基本清單作業，我們可以查詢感興趣的面。

\![](images/2/selecting - exercise 05.jpg)

> 1. 首先，將前面選取的元素輸出至 Element.Faces 節點。
> 2. 接著，使用 _List.Count_ 節點顯示出我們正在處理量體中的 23 個曲面。
> 3. 參考此數量，我們將 *Integer Slider* 的最大值變更為 _「22」_ 。
> 4. 使用 _List.GetItemAtIndex_，我們輸入清單和 *Integer Slider* 以提供 _index_。在選取的值之間滑動，到達 _index 9_ 時停止，如此就隔離了支撐桁架的主要正面。

上一個步驟稍顯繁瑣。使用 _Select Face_ 節點可以更快執行此作業。藉此可以隔離 Revit 專案中並非元素本身的面。互動方式與 _Select Model Element_ 相同，只是選取的是曲面，而不是完整的元素。

![](../.gitbook/assets/selecting\_exercise\_06.jpg)

假設我們要隔離建築的主要正面牆。我們可以使用 _Select Faces_ 節點執行此作業。請按一下「選取」按鈕，然後在 Revit 中選取四個主要正面。

![](../.gitbook/assets/selecting\_exercise\_07.jpg)

選取四面牆後，務必在 Revit 中按一下「完成」按鈕。

![](../.gitbook/assets/selecting\_exercise\_08.jpg)

現在，這些面已匯入 Dynamo 成為曲面。

![](../.gitbook/assets/selecting\_exercise\_09.jpg)

### 選取樑

現在，我們看看中庭上方的樑。

![](../.gitbook/assets/selecting\_exercise\_10.jpg)

> 1. 使用 _Select Model Element_ 節點，選取其中一根樑。
> 2. 將樑元素插入 _Element.Geometry_ 節點，現在可在 Dynamo 視埠中看到樑。
> 3. 可以使用 _Watch3D_ 節點拉近幾何圖形 (若未在 Watch 3D 中看到樑，請按一下右鍵，然後按一下「縮放至佈滿」)。

Revit/Dynamo 工作流程中可能經常會遇到以下問題：如何選取一個元素並取得所有類似元素？由於選取的 Revit 元素包含其所有階層資訊，因此我們可以查詢其族群類型，並選取該類型的所有元素。

![](../.gitbook/assets/selecting\_exercise\_11.jpg)

> 1. 將樑元素插入 _Element.ElementType_ 節點。
> 2. _Watch_ 節點顯示現在輸出是族群符號，而不是 Revit 元素。
> 3. _Element.ElementType_ 是個簡單的查詢，因此我們在程式碼區塊執行時可以像使用 `x.ElementType;` 一樣輕鬆，並得到相同結果。

![](../.gitbook/assets/selecting\_exercise\_12.jpg)

> 1. 為了選取其餘的樑，需使用 _All Elements of Family Type_ 節點。
> 2. Watch 節點顯示我們已選取五個 Revit 元素。

![](../.gitbook/assets/selecting\_exercise\_13.jpg)

> 1. 我們也可以將所有這五個元素轉換為 Dynamo 幾何圖形。

如果有 500 根樑會怎樣呢？將所有這些元素轉換為 Dynamo 幾何圖形會非常慢。若 Dynamo 花費很長時間來計算節點，您可能要在開發圖表時，使用「凍結」節點功能以暫停執行 Revit 作業。如需有關凍結節點的更多資訊，請參閱〈實體〉一章中的[凍結](../essential-nodes-and-concepts/5\_geometry-for-computational-design/5-6\_solids.md#freezing)一節。

是否不論何種情況，若要匯入 500 根樑，所有曲面都需要執行所需的參數運算？還是其實可以擷取樑的基本資訊，並使用基本幾何圖形執行生成工作？在逐步瞭解本章內容的過程中，需要隨時考慮這個問題。例如，我們接下來看看桁架系統。

### 選取桁架

使用相同的節點圖表，選取桁架元素而不是樑元素。執行此作業之前，刪除上一步驟中的 Element.Geometry。

![](../.gitbook/assets/selecting\_exercise\_14.jpg)

接下來，我們準備從桁架族群類型擷取一些基本資訊。

![](../.gitbook/assets/selecting\_exercise\_15.jpg)

> 1. 在 _Watch_ 節點中，可以看到我們從 Revit 中選取的自適應元件清單。我們希望擷取基本資訊，因此從自適應點開始。
> 2. 將 _All Elements of Family Type_ 節點插入 _AdaptiveComponent.Location_ 節點。這會產生一個清單的清單，其中每個清單都包含三點，表示自適應點的位置。
> 3. 若連接 _Polygon.ByPoints_ 節點，即會傳回 polycurve，可以在 Dynamo 視埠中看到。透過此方法，我們看到了一個元素的幾何圖形，並提取了其餘一系列元素 (數量可能多於此範例) 的幾何圖形。

{% hint style="info" %} 秘訣：若在 Dynamo 中按一下 Revit 元素的綠色數字，Revit 視埠將縮放至該元素。{% endhint %}
