# 節點資源庫

我們先前提到，**節點**是 Dynamo 圖表的核心建置圖塊，在**資源庫**中被組織為邏輯群組。在 Dynamo for Civil 3D 中，資源庫中有兩個品類 (也就是**層架**)，包含用於處理 AutoCAD 和 Civil 3D 物件 (例如定線、縱斷面、廊道、圖塊參考等) 的專用節點。資源庫的其餘部分包含本質上比較通用的節點，在所有「類型」的 Dynamo (例如，Dynamo for Revit、Dynamo Sandbox 等) 之間都一致。

{% hint style="info" %} 請查看[2-library.md](../3\_user\_interface/2-library.md "mention")一節，以取得節點在核心 Dynamo 資源庫中如何組織的更多資訊。{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Dynamo for Civil 3D 中的節點資源庫</p></figcaption></figure>

> 1. 用於處理 AutoCAD 和 Civil 3D 物件的特定節點
> 2. 一般用途節點
> 3. 您可以單獨安裝的協力廠商**套件**中的節點

{% hint style="warning" %} 如果使用 AutoCAD 和 Civil 3D 層架下的節點，您的 Dynamo 圖表就只能在 Dynamo for Civil 3D 中運作。如果 Dynamo for Civil 3D 圖表在其他位置 (例如在 Dynamo for Revit 中) 開啟，這些節點會標示警告，而且不會執行。{% endhint %}

{% hint style="info" %} **為什麼 AutoCAD 和 Civil 3D 有兩個獨立層架？**

這樣的組織是為了將原生 AutoCAD 物件 (直線、聚合線、圖塊參考等) 的節點和 Civil 3D 物件 (定線、廊道、地形等) 的節點區分開來。從技術角度來看，AutoCAD 和 Civil 3D 是兩個獨立的項目 - AutoCAD 是基礎應用程式，Civil 3D 則建置在其上。{% endhint %}

## 節點階層

若要使用 AutoCAD 和 Civil 3D 節點，請務必確實瞭解每個層架內的物件階層。記得生物學的分類法嗎？界、門、綱、目、科、屬、種？AutoCAD 和 Civil 3D 物件以類似方式分類。我們來瀏覽一些範例。

### Civil 物件

我們以定線為例。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

假設您的目標是變更定線的名稱。下一個您要加入的節點是 **CivilObject.SetName** 節點。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

一開始，這看起來可能不太直覺。**CivilObject** 是什麼？為什麼資源庫沒有 **Alignment.SetName** 節點？答案與_可重複使用性_和_簡易性_有關。請思考一下，無論物件是定線、廊道、縱斷面還是其他物件，變更 Civil 3D 物件名稱的過程都相同。因此，與其讓重複節點基本上都執行相同的作業 (例如 **Alignment.SetName、Corridor.SetName、Profile.SetName** 等)，不如將該功能收闔為單一節點。這正是 **CivilObject.SetName** 的功能！

另一種考量的方式是_關係_。定線和廊道都是一種 **Civil 物件**，就像蘋果和梨一樣都是水果。Civil 物件節點適用於任何類型的 Civil 物件，就像您想要使用一種削皮刀就可以削蘋果和削梨一樣。如果你為每種水果都買一個單獨的削皮刀，你的廚房會變得很混亂！從這個意義上來說，Dynamo 節點資源庫與您的廚房相似。

### 物件

現在，我們更進一步。假設您要變更定線的圖層。您要使用的節點是 **Object.SetLayer** 節點。

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

為什麼沒有名為 **CivilObject.SetLayer** 的節點？我們先前討論的可重複使用性和簡易性原則同樣適用於此處。_圖層_性質是 AutoCAD 中任何可繪製或插入的物件 (例如直線、聚合線、文字、圖塊參考等) 共有的性質。Civil 3D 物件 (例如定線和廊道) 位於同一品類下，因此適用於**物件**的任何節點也可用於任何 **Civil 物件**。

