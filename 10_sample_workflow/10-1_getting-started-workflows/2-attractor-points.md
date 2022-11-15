# 牽引點

牽引點非常適合用於實驗幾何圖形樣式，可用於根據物件的距離建立物件的逐步變化。

此工作流程將教您如何：

* 建立、管理和編輯清單。
* 使用直接操控在 3D 預覽中移動點。
* 變更執行模式。

![](../images/10-1/2/attractor1.gif)

## 定義我們的目標

在本練習中，我們希望建立圓 (_目標_)，其半徑輸入由距附近點的距離定義 (_關係_)。

![手繪圓](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> 定義距離式關係的點通常稱為「牽引點」。 在此，距牽引點的距離將用於指定圓的大小。

## 後續步驟

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供範例檔案的完整清單。

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

現在，我們繪製了目標與關係，可以開始建立圖表。我們需要節點展示 Dynamo 將執行的動作序列。我們先加入以下節點：**Number**、**Number Slider**、**Point.ByCoordinates**、**Geometry.DistanceTo、Circle.ByCenterPointRadius**。

![](../images/10-1/2/attractor(2).png)

> 1. Input > Basic > **Number**
> 2. Input > Basic > **Number Slider**
> 3. Geometry > Points > Point > **By Coordinates(x,y,z)**
> 4. Geometry > Modifiers > Geometry > **DistanceTo**
> 5. Geometry > Curves > Circle > **ByCenterPointRadius**

### 使用線路連接節點

現在，我們已建立一些節點，需要使用線路連接這些節點的埠。這些連接將定義資料的流動。

![](../images/10-1/2/attractor(3).png)

> 1. **Number** 到 **Point.ByCoordinates**
> 2. **Number Sliders** 到 **Point.ByCoordinates**
> 3. **Point.ByCoordinates** (2) 到 **DistanceTo**
> 4. **Point.ByCoordinates** 和 **DistanceTo** 到 **Circle.ByCenterPointRadius**

### 執行程式

定義程式流動後，只需告知 Dynamo 執行該程式即可。執行程式 (自動執行或在手動模式中按一下「執行」) 後，資料將通過線路，我們應該會在 3D 預覽中看到結果。

![](../images/10-1/2/attractor(4).png)

> 1. (按一下「執行」) - 如果執行列處於手動模式，我們需要按一下「執行」，以執行圖表
> 2. 節點預覽 - 將滑鼠懸停於節點右下角的方塊上，將為您提供結果的快顯資訊
> 3. 3D 預覽 - 如果任何節點建立幾何圖形，我們會在 3D 預覽中看到。
> 4. 建立節點上的輸出幾何圖形。

### 加入 **Code Block**

如果程式能運作，我們會在 3D 預覽中看到通過牽引點的圓。這很好，但我們可能需要加入更多詳圖或更多控制項。接下來調整圓節點的輸入，以便可以校正對半徑的影響。在工作區加入另一個 **Number Slider**，然後按兩下工作區的空白區域加入一個 **Code Block** 節點。在 Code Block 中編輯欄位，指定 `X/Y`。

![](../images/10-1/2/attractor(5).png)

> 1. **Code Block**
> 2. **DistanceTo** 和 **Number Slider** 到 **Code Block**
> 3. **Code Block** 到 **Circle.ByCenterPointRadius**

### 使用順序

從簡易的內容開始，然後提高複雜性，這是逐步開發程式的有效方式。在建立一個圓後，接下來我們應用程式的強大功能建立多個圓。現在，如果我們使用一個網格的點而不是使用一個中心點，然後在產生的資料結構中配合變更，程式現在會建立多個圓，其中每個圓都具有由距牽引點的校正距離定義的唯一半徑值。

![](../images/10-1/2/attractor(6).png)

> 1. 加入 **Number Sequence** 節點，並取代 **Point.ByCoordinates** 的輸入 - 在 Point.ByCoordinates 上按一下右鍵，然後選取「交織」>「交互參考」
> 2. 在 Point.ByCoordinates 後加入 **Flatten** 節點。若要完全展平清單，請將 `amt` 輸入保留為預設的 `-1`
> 3. 3D 預覽將更新，以顯示圓的格線

### 透過直接操控進行調整

有時數字操控方法並不合適。現在，您在背景 3D 預覽中導覽時，可以手動推拉點幾何圖形。我們還可以控制由點建構的其他幾何圖形。例如，**Sphere.ByCenterPointRadius** 也可以進行直接操控。我們可以透過 **Point.ByCoordinates** 使用一系列 X、Y 與 Z 值控制點的位置。但是，使用直接操控方法，您可以在 **3D 預覽導覽**模式中手動移動點，以更新滑棒的值。這樣可以更直觀地控制識別點位置的一組離散值。

![](../images/10-1/2/attractor(7).png)

> 1. 若要使用**直接操控**，請選取要移動的點的那一格，在所選點的上方將顯示箭頭。
> 2. 切換至 **3D 預覽導覽**模式。

![](../images/10-1/2/attractor\(8\).png)

> 1. 將游標懸停在點上方，將顯示 X、Y 與 Z 軸。
> 2. 按一下並拖曳彩色箭頭以移動對應的軸，**Number Slider** 值將根據手動移動的點而即時更新。

![](../images/10-1/2/attractor(1).png)

> 1. 請注意，在**直接操控**之前，只有一個滑棒插入 **Point.ByCoordinates** 的分量中。在 X 方向手動移動點時，Dynamo 會為 X 輸入自動產生新的 **Number Slider**。

