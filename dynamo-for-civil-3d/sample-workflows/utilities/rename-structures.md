# 更名結構

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

在管網中加入管和結構時，Civil 3D 會使用樣板自動指定名稱。這在一開始放置時通常足以應付，但是隨著設計逐漸發展，名稱在未來必然會有所變化。此外，我們可能需要許多不同的命名模式，例如在管路中從最下游的結構開始按順序命名結構，或按照與本端代理的資料架構一致的命名模式來命名結構。此範例將示範如何使用 Dynamo 定義任何類型的命名策略，並且以一致的方式套用。

## 目標

> :dart: 根據定線的樁號標示，按順序更名管網結構。

## 主要概念

> * 使用邊界框
> * 使用 **List.FilterByBoolMask** 節點篩選資料
> * 使用 **List.SortByKey** 節點排序資料
> * 產生和修改文字字串

## 版本相容性

{% hint style="success" %} 此圖表將在 **Civil 3D 2020** 及更高版本上執行。{% endhint %}

## 資料集

首先，下載以下範例檔案，然後開啟 DWG 檔案和 Dynamo 圖表。

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## 解決方法

以下是此圖表中的邏輯概觀。

> 1. 依圖層選取結構
> 2. 取得結構位置
> 3. 依偏移篩選結構，然後依樁號排序
> 4. 產生新名稱
> 5. 更名結構

我們開始吧！

### 選取結構

我們首先需要選取要使用的所有結構。我們會透過只選取特定圖層上的所有物件來執行此作業，這表示我們可以從不同的管網 (假設共用相同圖層) 選取結構。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>選取給定圖層上的結構</p></figcaption></figure>

> 1. 此節點可確保我們不會意外擷取任何不想要但可能與結構共用相同圖層的物件類型。

### 取得結構位置

我們現在有了結構，我們需要找出它們在空間中的位置，以便可以根據位置對結構排序。為了執行此作業，我們將利用每個物件的邊界框。物件的**邊界框**是完全包含物件幾何實際範圍的最小方塊。透過計算邊界框的中心，我們可以得到結構很近似的插入點。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>使用邊界框取得每個結構的近似插入點</p></figcaption></figure>

我們將使用這些點來取得結構相對於所選定線的樁號和偏移。

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### 篩選和排序

從這裡開始，事情會變得有點棘手。在此階段，我們有一個大型清單，列出我們指定的圖層上的所有結構，並選擇了要沿其排序結構的定線。問題是清單中可能有我們不想更名的結構。例如，這類結構可能不是我們感興趣的特定管路。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. 選取的定線
> 2. 要更名的結構
> 3. 應忽略的結構

因此，我們需要篩選結構清單，這樣就不用考慮那些與該定線之間大於特定偏移的結構。這最適合使用 **List.FilterByBoolMask** 節點完成。篩選結構清單後，我們使用 **List.SortByKey** 節點，依其樁號值排序。

{% hint style="info" %} 如果您不熟悉使用清單，請查看[2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention")一節。{% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>篩選和排序結構</p></figcaption></figure>

> 1. 檢查結構的偏移是否小於門檻值
> 2. 將任何空值取代為 _false_
> 3. 篩選結構和樁號的清單
> 4. 依樁號排序結構

### 產生新名稱

我們要做的最後一項工作，是為結構建立新名稱。我們將使用的格式為 `<alignment name>-STRC-<number>`。這裡還有額外幾個節點，是需要時以額外的零填補數字 (例如，「01」而不是「1」)。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>產生新的結構名稱</p></figcaption></figure>

### 更名結構

最後也是同樣重要的，我們更名結構。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>設定結構的名稱</p></figcaption></figure>

### 結果

以下是使用 **Dynamo 播放器**執行圖表的範例。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>使用 Dynamo 播放器執行圖表，然後在 Civil 3D 中查看結果</p></figcaption></figure>

{% hint style="info" %} 如果您不熟悉 Dynamo 播放器，請查看 [dynamo-player.md](../../dynamo-player.md "mention")一節。{% endhint %}

> :tada: 任務完成！

### 附註：在 Dynamo 中視覺化

利用 Dynamo 的 3D 背景預覽來視覺化圖表的中間輸出，而不是只顯示最終結果，會很有幫助。我們可以做一件簡單的事情，就是顯示結構的邊界框。此外，此特定資料集在文件中有廊道，因此我們可以將廊道地勢線幾何圖形帶入 Dynamo，為結構在空間中的位置提供一些情境脈絡。如果圖表是用在沒有任何廊道的資料集，則這些節點就不會執行任何作業。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>視覺化結構和廊道地勢線的幾何圖形</p></figcaption></figure>

現在，我們可以更清楚瞭解透過偏移篩選結構的流程如何運作。

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>調整定線偏移門檻值，然後在 Dynamo 中視覺化受影響的結構</p></figcaption></figure>

## 構想

以下是一些如何擴充此圖表功能的構想。

{% hint style="info" %} 根據結構 **最接近的定線** (而不是選取特定定線) 更名結構。{% endhint %}

{% hint style="info" %} 除了更名結構外，還 **更名管**。{% endhint %}

{% hint style="info" %} 根據結構的管路 **設定圖層**。{% endhint %}
