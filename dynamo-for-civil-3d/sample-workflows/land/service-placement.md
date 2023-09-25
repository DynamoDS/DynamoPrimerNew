# 服務放置

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

典型住宅開發的工程設計涉及使用數個地下公共設施，例如污水管、雨水排水、飲用水或其他。此範例將示範如何使用 Dynamo 繪製從分佈主線到指定基地 (即宗地) 的服務連接。每個基地通常都需要連接服務，這使得放置所有服務的工作相當繁瑣。Dynamo 可以自動精確繪製必要的幾何圖形，並提供可做調整以符合本端代理標準的彈性輸入，來加快流程。

## 目標

> :dart：將供水服務計量器圖塊參考放置在距界址線的指定偏移處，並為與分佈主線互垂的每個服務連接繪製一條線。

## 主要概念

> * 使用 **Select Object** 節點進行使用者輸入
> * 使用座標系統
> * 使用幾何作業，例如 **Geometry.DistanceTo** 和 **Geometry.ClosestPointTo**
> * 建立圖塊參考
> * 控制物件併入設定

## 版本相容性

{% hint style="success" %} 此圖表將在 **Civil 3D 2020** 及更高版本上執行。{% endhint %}

## 資料集

首先，下載以下範例檔案，然後開啟 DWG 檔案和 Dynamo 圖表。

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## 解決方法

以下是此圖表中的邏輯概觀。

> 1. 取得分佈主線的曲線幾何圖形
> 2. 取得使用者所選界址線的曲線幾何圖形，如有必要可反轉
> 3. 產生服務計量器的插入點
> 4. 取得分佈主線最接近服務計量器位置的點
> 5. 在模型空間中建立圖塊參考和線

我們開始吧！

### 取得分佈主線幾何圖形

我們的第一步是讓分佈主線的幾何圖形進入 Dynamo。我們要取得特定圖層上的所有物件，並接合在一起成為 Dynamo PolyCurve，而不是選取個別的線或聚合線。

{% hint style="info" %} 如果您不熟悉 Dynamo 曲線幾何圖形，請查看[4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention")一節。{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>從 Civil 3D 取得物件，並將所有物件接合在一起成為單一條 PolyCurve</p></figcaption></figure>

### 取得界址線幾何圖形

接下來，我們需要讓所選界址線的幾何圖形進入 Dynamo，以便我們可以使用它。適合進行此工作的工具是 **Select Object** 節點，可讓圖表的使用者在 Civil 3D 中點選特定物件。

我們還需要處理可能發生的潛在問題。界址線有起點和終點，這表示它有方向。為了讓圖表產生一致的結果，我們需要所有界址線的方向都一致。我們可以直接在圖表邏輯中說明此條件，這可以讓圖表更具彈性。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>選取界址線並確保其方向正確</p></figcaption></figure>

> 1. 取得界址線的起點和終點。
> 2. 測量每個點到分佈主線的距離，然後計算出哪個距離比較大。
> 3. 想要的結果是，線的起點最接近分佈主線。如果不是如此，我們就反轉界址線的方向。否則，我們只需傳回原始界址線。

### 產生插入點

現在要來找出服務計量器的放置位置。放置通常由本端代理需求決定，因此我們只要提供可變更以滿足各種條件的輸入值。我們將使用沿界址線的**座標系統**做為建立點的參考。這可讓您非常輕鬆地定義相對於界址線的偏移，而不論其方位為何。

{% hint style="info" %} 如果您不熟悉座標系統，請查看[2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention")一節。{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>建立服務計量器的插入點</p></figcaption></figure>

### 取得連接點

現在，我們需要取得分佈主線最接近服務計量器位置的點。這可讓我們在模型空間中繪製服務連接，以便能永遠與分佈主線互垂。**Geometry.ClosestPointTo** 節點是理想的解決方案。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>取得分佈主線上的互垂點</p></figcaption></figure>

> 1. 這是分佈主線 PolyCurve
> 2. 這些是服務計量器插入點

### 建立物件

最後一步是在模型空間中實際建立物件。我們將使用先前產生的插入點來建立圖塊參考，然後使用分佈主線上的點來繪製連到服務連接的線。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### 結果

當您執行圖表時，您應該會在模型空間中看到新的圖塊參考和服務連接線。請嘗試變更某些輸入，您會看到所有內容都會自動更新！

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>在 Dynamo 中調整輸入參數，在 Civil 3D 中立即看到結果</p></figcaption></figure>

### 附註：啟用連續放置

您可能會發現，在為一條界址線放置物件後，選取不同的界址線會導致物件被「移動」。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>開啟物件併入時的行為</p></figcaption></figure>

這是 Dynamo 的預設行為，在許多情況下非常有用。但是，您可能要依序放置多個服務連接，讓 Dynamo 在每次執行時建立新物件，而不是修改原始物件。您可以變更物件併入設定來控制此行為。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Dynamo 的物件併入設定</p></figcaption></figure>

{% hint style="info" %} 請查看[object-binding.md](../../advanced-topics/object-binding.md "mention")一節，以取得更多資訊。{% endhint %}

變更此設定將強制 Dynamo「忘記」每次執行時所建立的物件。以下是在關閉物件併入的情況下，使用 **Dynamo 播放器**執行圖表的範例。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>使用 Dynamo 播放器執行圖表，然後在 Civil 3D 中查看結果</p></figcaption></figure>

{% hint style="info" %} 如果您不熟悉 Dynamo 播放器，請查看 [dynamo-player.md](../../dynamo-player.md "mention")一節。{% endhint %}

> :tada：任務完成！

## 構想

以下是一些如何擴充此圖表功能的構想。

{% hint style="info" %} 同時放置**多個服務連接**，而不是選取每條界址線。{% endhint %}

{% hint style="info" %} 將輸入調整為改放置**污水管清掃口**，而不是供水服務計量器。{% endhint %}

{% hint style="info" %} **加入開關**，以允許在界址線的特定一側 (而非兩側) 放置單一服務連接。{% endhint %}
