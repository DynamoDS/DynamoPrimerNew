# 記錄

編輯文件的參數將遵循先前諸節學習的課程。在本節中，我們將瞭解編輯參數，這些參數不會影響元素的幾何性質，而是會準備供記錄的 Revit 檔案。

### 偏差

在以下練習中，我們將使用平面節點的基本偏差，以建立供記錄的 Revit 圖紙。在以參數式方式定義的屋頂結構上，每個面板都有不同的偏差值，我們希望使用顏色以及安排自適應點來指定值的範圍，以便交給正面顧問、工程師或承包商。

![偏差](images/6/deviation.jpg)

> 平面節點的偏差將計算一組四個點與各點之間最佳擬合平面的距離。這是快速輕鬆的建構研究方式。

## 練習

### 第 I 部分：根據與平面節點的偏差設定面板孔徑比

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供範例檔案的完整清單。

{% file src="datasets/6/Revit-Documenting.zip" %}

從本節的 Revit 檔案開始 (或繼續使用上一節課的檔案)。此檔案的屋頂上具有一系列 ETFE 面板。我們在此練習中會參考這些面板。

![](<images/6/documenting - exercise I - 01.jpg>)

> 1. 在圖元區加入 _Family Types_ 節點，然後選擇 _「ROOF-PANEL-4PT」_ 。
> 2. 將此節點插入 _All Elements of Family Type_ 節點，以便將 Revit 的所有元素匯入 Dynamo。

![](<images/6/documenting - exercise I - 02.jpg>)

> 1. 使用 _AdaptiveComponent.Locations_ 節點查詢每個元件的自適應點位置。
> 2. 使用 _Polygon.ByPoints_ 節點從這四點建立多邊形。請注意，現在我們已在 Dynamo 中建立抽象版本的面板化系統，而無需匯入 Revit 元素的完整幾何圖形。
> 3. 使用 _Polygon.PlaneDeviation_ 節點計算平面偏差。

就像上一個練習一樣，只是為了好玩，接下來我們根據平面偏差設定每個面板的孔徑比。

![](<images/6/documenting - exercise I - 03.jpg>)

> 1. 在圖元區加入 _Element.SetParameterByName_ 節點，並將自適應元件連接至 _element_ 輸入。將名為 _「Aperture Ratio」_ 的 _Code Block_ 連接至 _parameterName_ 輸入。
> 2. 我們無法將偏差結果直接連接至值輸入，因為需要將值重新對映至參數範圍。

![](<images/6/documenting - exercise I - 04.jpg>)

> 1. 使用 _Math.RemapRange_，透過在 _Code Block_ 中輸入 `0.15; 0.45;`，將偏差值重新對映至從 0.15 到 0_._45 之間的範圍。
> 2. 將這些結果重新插入 _Element.SetParameterByName_ 的 value 輸入。

回到 Revit，我們會 _稍微_ 理解曲面上孔徑的變更。

![練習](../.gitbook/assets/13.jpg)

拉近時，可以更清晰地看到封閉面板向曲面的轉角加重。開放轉角朝向頂部。轉角代表偏差較大的區域，而凸度具有最小的曲率，因此這合乎邏輯。

![練習](../.gitbook/assets/13a.jpg)

### 第 II 部分：顏色和記錄

設定孔徑比不會清楚展示屋頂上面板的偏差，我們還要變更實際元素的幾何圖形。假設我們只希望從製造可行性的觀點研究偏差。根據記錄的偏差範圍對面板上色會很有幫助。我們可以透過以下一系列步驟，採用與上述步驟非常相似的程序來達成。

![](<images/6/documenting - exercise II - 01.jpg>)

> 1. 移除 _Element.SetParameterByName_ 及其輸入節點，並加入 _Element.OverrideColorInView_。
> 2. 在圖元區加入 _Color Range_ 節點，並插入 _Element.OverrideColorInView_ 的 color 輸入。我們仍必須將偏差值連接至顏色範圍，以建立漸層。
> 3. 將游標懸停在 _value_ 輸入上，我們可以看到輸入的值必須介於 _0_ 與 _1_ 之間，才能將顏色對映至每個值。我們需要將偏差值重新對映至此範圍。

![](<images/6/documenting - exercise II - 02.jpg>)

> 1. 使用 _Math.RemapRange_，將平面偏差值重新對映至從 *0* 到 _1_ 之間的範圍 (注意：您也可以使用 _「MapTo」_ 節點來定義來源範圍)。
> 2. 將結果插入 _Color Range_ 節點。
> 3. 請注意，我們的輸出是一系列顏色，而不是一系列數字。
> 4. 如果您要設定為「手動」，請按一下 _「執行」_ 。從現在起，您應該能設定為「自動」。

返回 Revit，我們可以看到更清晰的漸層，該漸層根據顏色範圍表示平面偏差。如果我們希望自訂顏色會怎樣呢？請注意，最小偏差值以紅色表示，這似乎與我們的預期相反。我們希望以紅色表示最大偏差，以更冷的顏色表示最小偏差。接下來回到 Dynamo 並修正此問題。

![](../.gitbook/assets/09.jpg)

![](<images/6/documenting - exercise II - 04.jpg>)

> 1. 使用 _Code Block_，在不同的兩行程式碼中加入兩個數字：`0;` 與 `255;`。
> 2. 將適合的值插入兩個 _Color.ByARGB_ 節點，以建立紅色與藍色。
> 3. 使用這兩種顏色建立清單。
> 4. 將此清單插入 _Color Range_ 的 _colors_ 輸入，然後查看自訂顏色範圍更新。

回到 Revit，現在我們可以更深刻理解轉角的最大偏差區域。請記住，此節點用於在視圖中取代顏色，因此如果一組圖面中包含著重於特定類型分析的特定圖紙，該節點會很有幫助。

![Exercise](<../.gitbook/assets/07 (6).jpg>)

### 第 III 部分：製作明細表

在 Revit 中選取一個 ETFE 面板，我們可以看到有四個實體參數，分別是 XYZ1、XYZ2、XYZ3 與 XYZ4。這些參數在建立之後都是空白的。這些是需要值的文字參數。我們將使用 Dynamo 將自適應點位置寫入每個參數。這有助於在需要將幾何圖形傳送至正面顧問的工程師時實現互通性。

![](<images/6/documenting - exercise III - 01.jpg>)

在範例圖紙中，我們建立了一個很大的空白明細表。XYZ 參數是 Revit 檔案中的共用參數，我們可藉此將其加入明細表中。

![Exercise](<../.gitbook/assets/03 (8).jpg>)

拉近，XYZ 參數都尚未填寫。前兩個參數由 Revit 負責。

![Exercise](<../.gitbook/assets/02 (9).jpg>)

為了寫入這些值，我們將執行複雜的清單作業。圖表本身很簡單，但概念很大程度上依賴於〈清單〉一章中討論的清單對映。

![](<images/6/documenting - exercise III - 04.jpg>)

> 1. 使用兩個節點選取所有自適應元件。
> 2. 使用 _AdaptiveComponent.Locations_ 擷取每個點的位置。
> 3. 將這些點轉換為字串。請記住，該參數是文字參數，因此我們需要輸入正確的資料類型。
> 4. 建立包含四個字串的清單，這四個字串定義要變更的參數：_XYZ1、XYZ2、XYZ3_ 與 _XYZ4_。
> 5. 將此清單插入 _Element.SetParameterByName_ 的 _parameterName_ 輸入。
> 6. 將 _Element.SetParameterByName_ 連接至 _List.Combine_ 的 _combinator_ 輸入。將 _adaptive components_ 連接至 _list1_。將物件的 _String_ 連接至 _list2_。

我們在此列出對映，因為要為每個元素寫入四個值，這會建立複雜的資料結構。_List.Combine_ 節點會定義資料階層中下一層級的作業。這是為什麼 _Element.SetParameterByName_ 的 element 和 value 輸入皆留空。_List.Combine_ 會根據連接順序，將其輸入的子清單連接至 _Element.SetParameterByName_ 的空白輸入。

在 Revit 中選取面板，現在我們可以看到每個參數都具有字串值。實際上，我們可以建立更簡單的點 (X,Y,Z) 寫入格式。在 Dynamo 中使用字串作業即可實現該功能，但這裡我們略過此內容，而繼續討論本章內容。

![](<../.gitbook/assets/04 (5).jpg>)

已填寫參數的範例明細表視圖。

![](<../.gitbook/assets/01 (9).jpg>)

現在，每個 ETFE 面板都具有針對每個自適應點而寫入的 XYZ 座標，表示用於製作的每個面板的轉角。

![Exercise](<../.gitbook/assets/00 (8).jpg>)
