# 燈柱放置

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Dynamo 的其中一個主要使用案例，是沿廊道模型動態放置離散物件。物件通常需要放置在與廊道上插入的組合無關的位置，這是一個非常冗長乏味而需手動完成的工作。當廊道的水平或垂直幾何圖形發生變更時，會產生大量重複工作。

## 目標

> :dart：在 Excel 檔案中指定的樁號值處，沿廊道放置燈柱圖塊參考。

## 主要概念

> * 從外部檔案 (在此範例中為 Excel) 讀取資料
> * 以字典組織資料
> * 使用座標系統控制位置/比例/旋轉
> * 放置圖塊參考
> * 在 Dynamo 中視覺化幾何圖形

## 版本相容性

{% hint style="success" %} 此圖表將在 **Civil 3D 2020** 及更高版本上執行。{% endhint %}

## 資料集

首先，下載以下範例檔案，然後開啟 DWG 檔案和 Dynamo 圖表。

{% hint style="info" %} Excel 檔案與 Dynamo 圖表最好儲存在同一個目錄中。{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## 解決方法

以下是此圖表中的邏輯概觀。

> 1. 讀取 Excel 檔案，將資料匯入至 Dynamo
> 2. 從指定的廊道基準線取得地勢線
> 3. 沿廊道地勢線在所需樁號處產生座標系統
> 4. 使用座標系統在模型空間中放置圖塊參考

我們開始吧！

### 取得 Excel 資料

在此範例圖表中，我們使用 Excel 檔案來儲存 Dynamo 將用於放置燈柱圖塊參考的資料。表格看起來像下面這樣。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Excel 檔案表格結構</p></figcaption></figure>

{% hint style="info" %} 使用 Dynamo 從外部檔案 (例如 Excel 檔案) 讀取資料是一個很好的策略，尤其是當需要與其他團隊成員共用資料時。{% endhint %}

Excel 資料會像下面這樣匯入至 Dynamo。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>將 Excel 資料匯入至 Dynamo</p></figcaption></figure>

我們現在有了資料，需要依欄 (_Corridor_、_Baseline_、_PointCode_ 等) 把資料分開，以便在圖表的其餘部分使用。一個執行此作業的常見方法是使用 **List.GetItemAtIndex** 節點，並指定所需每欄的索引號碼。例如，_Corridor_ 欄是在索引 0,_Baseline_ 欄是在索引 1 等等。

看起來沒問題，對吧？但是這個方法有一個潛在的問題。如果 Excel 檔案中欄的順序在將來發生變更，該怎麼辦？或是兩欄之間加入新的一欄？如此一來，圖表將無法正常運作而需要更新。我們可以將資料放入**字典**，將 Excel 欄標題做為_鍵_，其餘資料做為_值_，讓圖表能繼續使用。

{% hint style="info" %} 如果您不熟悉字典，請查看[5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention")一節。{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>將 Excel 資料放入字典</p></figcaption></figure>

這可讓圖表變得更具彈性，因為它允許變更 Excel 中欄的順序。只要欄標題保持不變，您就可以使用其_鍵_ (即欄標題) 從字典中擷取資料，這是我們接下來要執行的作業。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>從字典中擷取資料</p></figcaption></figure>

### 取得廊道地勢線

我們現在已匯入 Excel 資料並準備好了，接下來我們開始使用它從 Civil 3D 取得有關廊道模型的一些資訊。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. 依名稱選取廊道模型。
> 2. 取得廊道內的特定基準線。
> 3. 透過基準線的點代碼取得基準線內的地勢線。

### 產生座標系統

我們現在要沿廊道地勢線，在 Excel 檔案中指定的樁號值處產生**座標系統**。這些座標系統將用於定義燈柱圖塊參考的位置、旋轉和比例。

{% hint style="info" %} 如果您不熟悉座標系統，請查看[2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention")一節。{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>沿廊道地勢線取得座標系統</p></figcaption></figure>

請注意，在此處使用程式碼區塊 (Code Block) 是為了根據座標系統在基準線哪一側來旋轉座標系統。您也可以使用幾個節點來達成這個目標，但這是一個很好的範例，說明撰寫出來更容易。

{% hint style="info" %} 如果您不熟悉程式碼區塊，請查看[8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention")一節。{% endhint %}

### 建立圖塊參考

我們快完成了！我們有實際放置圖塊參考所需的所有資訊。首先，使用 Excel 檔案中的 _BlockName_ 欄取得圖塊定義。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>從文件取得所需的圖塊定義</p></figcaption></figure>

從這裡，最後一步是建立圖塊參考。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>在模型空間中建立圖塊參考</p></figcaption></figure>

### 結果

當您執行圖表時，您應該會看到新的圖塊參考沿廊道展示在模型空間中。以下是最酷的部分 - 如果圖表的執行模式設定為「自動」，而且您編輯了 Excel 檔案，圖塊參考會自動更新！

{% hint style="info" %} 您可以在[3_user_interface](../../../3\_user\_interface/ "mention")一節閱讀有關圖表執行模式的更多資訊。{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>更新 Excel 檔案，在 Civil 3D 中很快就會看到結果</p></figcaption></figure>

以下是使用 **Dynamo 播放器**執行圖表的範例。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>使用 Dynamo 播放器執行圖表，然後在 Civil 3D 中查看結果</p></figcaption></figure>

{% hint style="info" %} 如果您不熟悉 Dynamo 播放器，請查看 [dynamo-player.md](../../dynamo-player.md "mention")一節。{% endhint %}

> :tada：任務完成！

### 附註：在 Dynamo 中視覺化

在 Dynamo 中視覺化廊道幾何圖形，有助於提供情境脈絡。此特定模型已在模型空間中萃取出廊道實體，因此我們將這些實體帶入 Dynamo。

但還有其他事情需要考慮。實體相對而言是「比較重」的幾何圖形類型，這表示此作業將減慢圖表速度。如果有一個簡單的方式可以_選擇_是否要檢視實體會更好。很明顯的解決方法是要拔掉 **Corridor.GetSolids** 節點，但這會對所有下游節點產生警告，而這會有點混亂。這時就是 **ScopeIf** 節點真正發揮功能的時候了。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. 請注意 **Object.Geometry** 節點底部有一條灰色列。這表示節點預覽已關閉 (在節點上按一下右鍵可存取)，這樣 **GeometryColor.ByGeometryColor** 就可以避免為了背景預覽的顯示優先順序而與其他幾何圖形「競爭」。
> 2. **ScopeIf** 節點基本上可讓您選擇性地執行一條完整的節點分支。如果 _test_ 輸入為 false，則連接至 **ScopeIf** 節點的每個節點都不會執行。

以下是 Dynamo 背景預覽的結果。

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>在 Dynamo 中視覺化廊道幾何圖形</p></figcaption></figure>

## 構想

以下是一些如何擴充此圖表功能的構想。

{% hint style="info" %} 在 Excel 檔案中新增**旋轉**一欄，就可以使用它驅動座標系統的旋轉。{% endhint %}

{% hint style="info" %} 在 Excel 檔案中新增**水平或垂直偏移**，就可以視需要讓燈柱偏離廊道地勢線。{% endhint %}

{% hint style="info" %} **直接在 Dynamo 中**中使用起點樁號和典型間距產生樁號值，而不使用內含樁號值的 Excel 檔案。{% endhint %}
