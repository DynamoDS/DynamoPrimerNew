# 點群組管理

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

對於許多從測量現場到完成工作的流程而言，在 Civil 3D 中使用 COGO 點和點群組是核心要素。Dynamo 在資料管理方面非常出色，我們將在此範例中示範一個可能的使用案例。 

## 目標

> :dart: 為每個唯一的 COGO 點描述建立點群組。

## 主要概念

> * 使用清單
> * 使用 **List.GroupByKey** 節點將類似物件分組
> * 在 Dynamo 播放器中展示自訂輸出

## 版本相容性

{% hint style="success" %}
 此圖表將在 **Civil 3D 2020** 及更高版本上執行。
{% endhint %}

## 資料集

首先，下載以下範例檔案，然後開啟 DWG 檔案和 Dynamo 圖表。

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## 解決方法

以下是此圖表中的邏輯概觀。

> 1. 取得文件中所有的 COGO 點
> 2. 依描述將 COGO 點分組
> 3. 建立點群組
> 4. 將摘要輸出至 Dynamo 播放器

我們開始吧！

### 取得 COGO 點

第一步是取得文件中所有的點群組，然後取得每個群組內的所有 COGO 點。這會產生一個 _巢狀清單_，也就是「清單的清單」，如果稍後使用 **List.Flatten** 節點將所有內容向下展開為單一清單，會更容易處理。

{% hint style="info" %}
 如果您不熟悉使用清單，請查看[2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention")一節。
{% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>取得所有點群組和 COGO 點 </p></figcaption></figure>

### 依描述將點分組

我們現在已有全部的 COGO 點，需要根據其描述分為多個群組。這正是 **List.GroupByKey** 節點所做的工作。它基本上會將共用相同鍵的所有項目分組在一起。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>依描述將 COGO 點分組</p></figcaption></figure>

### 建立點群組

辛苦的工作已經完成！最後一步是從分組的 COGO 點建立新的 Civil 3D 點群組。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>建立新的點群組</p></figcaption></figure>

### 輸出摘要

當您執行圖表時，Dynamo 背景預覽中沒有任何內容可供查看，因為我們沒有處理任何幾何圖形。因此，查看圖表是否正確執行的唯一方法是檢查「工具區」，或查看節點輸出預覽。但是，如果我們使用 **Dynamo 播放器**執行圖表，則可以透過輸出已建立的點群組摘要，提供更多有關圖表結果的回饋。您只需在節點上按一下右鍵，然後設定為 _「是輸出」_ 即可。在此範例中，我們使用更名過的 **Watch** 節點來檢視結果。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>將節點設定為<em>「是輸出」</em>，將在 Dynamo 播放器輸出中顯示其內容</p></figcaption></figure>

### 結果

以下是使用 **Dynamo 播放器**執行圖表的範例。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>使用 Dynamo 播放器執行圖表，然後在「工具區」中查看結果</p></figcaption></figure>

{% hint style="info" %}
 如果您不熟悉 Dynamo 播放器，請查看 [dynamo-player.md](../../dynamo-player.md "mention")一節。
{% endhint %}

> :tada: 任務完成！

## 構想

以下是一些如何擴充此圖表功能的構想。

{% hint style="info" %}
 將點群組修改為根據 **完整描述**，而非原始描述。
{% endhint %}

{% hint style="info" %}
 將點分組，分組時依據您選擇的其他某些 **預先定義的品類**  (例如，「地面快照」、「碑界」等)。
{% endhint %}

{% hint style="info" %}
 為某些群組中的點自動建立不規則三角網地形。
{% endhint %}
