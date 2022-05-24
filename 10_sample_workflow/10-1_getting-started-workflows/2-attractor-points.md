# Attractor Points

Attractor points are great for experimenting with geometric patterns. They can be used to create gradual changes to objects based on their distance.

This workflow will teach you how to:

* Create, manage and edit lists.
* Move points in the 3D preview using direct manipulation.
* 變更顯示模式

![](../images/10-1/2/attractor1.gif)

## Defining our Objectives

在本練習中，我們希望建立圓 (_目標_)，其半徑輸入由距附近點的距離定義 (_關係_)。

![手繪圓](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> 定義距離式關係的點通常稱為「牽引點」。在此，距牽引點的距離將用於指定圓的大小。

## 後續步驟

> Download the example file by clicking on the link below.
>
> 附錄中提供範例檔案的完整清單。

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

現在，我們已草繪目標與關係，可以開始建立圖表。 我們需要節點展示 Dynamo 將執行的動作序列。****************

![]

> 1. Input > Basic > **Number**
> 2. Input > Basic > **Number Slider**
> 3. ****
> 4. Geometry > Geometry > **DistanceTo**
> 5. ****

### 使用線連接節點

現在，我們已建立一些節點，需要使用線連接這些節點的連接埠。這些連接將定義資料的流動。

![]

> 1. **Number** 至 **Point.ByCoordinates**
> 2. **Number Sliders** 至 **Point.ByCoordinates**
> 3. **Point.ByCoordinates** (2) 至 **DistanceTo**
> 4. **Point.ByCoordinates** 與 **DistanceTo** 至 **Circle.ByCenterPointRadius**

### 執行程式

定義程式流動後，只需告知 Dynamo 執行該程式即可。執行程式 (自動執行或在手動模式中按一下「執行」) 後，資料將通過線，我們將在 3D 預覽中看到結果。

![]

> 1. (按一下「執行」) - 如果執行列處於手動模式，我們需要按一下「執行」，以執行圖表
> 2. 節點預覽 - 將滑鼠懸停於節點右下角的方塊上，將為您提供結果的快顯資訊
> 3. 3D 預覽 - 如果任何節點建立幾何圖形，我們會在 3D 預覽中看到。
> 4. 建立節點上的輸出幾何圖形。

### ****

若程式在工作中，我們會在 3D 預覽中看到通過牽引點的圓。這很好，但我們可能需要加入更多詳圖或更多控制項。接下來調整圓節點的輸入，以便可以校正對半徑的影響。加入另一個 **Number Slider** 至工作區，然後按兩下工作區的空白區域以加入 **Code Block** 節點。在 Code Block 中編輯欄位，指定 `X/Y`X/Y。

![]

> 1. **Code Block**
> 2. **DistanceTo** 與 **Number Slider** 至 **Code Block**
> 3. **Code Block** 至 **Circle.ByCenterPointRadius**

### Using Sequences

從簡易的內容開始，然後提高複雜性，這是逐步開發程式的有效方式。在建立一個圓後，接下來我們應用程式的強大功能建立多個圓。現在，如果我們不是使用一個中心點，而是使用點的格線，並在產生的資料結構中包含變更，程式將建立多個圓，其中每個圓都具有由距牽引點的校正距離定義的唯一半徑值。

![]

> 1. 加入 **Number Sequence** 節點，並取代 **Point.ByCoordinates** 的輸入 - 在 Point.ByCoordinates 上按一下右鍵，然後選取「鑲邊」>「交互參考」
> 2. 在 Point.ByCoordinates 後加入 **Flatten** 節點。若要完全展平清單，請將 `amt`amt`-1` 輸入保留為預設的 -1
> 3. 3D 預覽將更新，以顯示圓的格線

### 透過直接操控進行調整

有時數字操控方法並不合適。現在，您在背景 3D 預覽中導覽時，可以手動推拉點幾何圖形。我們還可以控制由點建構的其他幾何圖形。例如，**Sphere.ByCenterPointRadius** 也可以進行直接操控。我們可以透過 **Point.ByCoordinates** 使用一系列 X、Y 與 Z 值控制點的位置。但是，使用直接操控方法，您可以在 **3D 預覽導覽**模式中手動移動點，以更新滑棒的值。這樣可以更直觀地控制識別點位置的一組離散值。

![]

> 1. 若要使用**直接操控**，請選取要移動的點的面板，在所選點的上方將顯示箭頭。
> 2. 切換至 **3D 預覽導覽**模式。

![](../images/10-1/2/attractor\(8\).png)

> 1. 將游標懸停在點上方，將顯示 X、Y 與 Z 軸。
> 2. 按一下並拖曳彩色箭頭以移動對應的軸，**Number Slider** 值將根據手動移動的點而即時更新。

![]

> 1. 請注意，在**直接操控**之前，只有一個滑棒插入到 **Point.ByCoordinates** 元件中。在 X 方向手動移動點時，Dynamo 會為 X 輸入自動產生新的 **Number Slider**。

