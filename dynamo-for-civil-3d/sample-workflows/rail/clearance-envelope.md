# 間隙包絡線

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

開發運動包絡線以進行間隙驗證是軌道設計一個很重要的部分。Dynamo 可用來產生包絡線的實體，而不是建立和管理複雜的廊道次組合來執行工作。

## 目標

> :dart：使用車輛縱斷面圖塊沿廊道產生間隙包絡線 3D 實體。

## 主要概念

> * 使用廊道地勢線
> * 在座標系統之間轉換幾何圖形
> * 透過斷面混成建立實體
> * 使用交織設定控制節點行為

## 版本相容性

{% hint style="success" %} 此圖表將在 **Civil 3D 2020** 及更高版本上執行。{% endhint %}

## 資料集

首先，下載以下範例檔案，然後開啟 DWG 檔案和 Dynamo 圖表。

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## 解決方法

以下是此圖表中的邏輯概觀。

> 1. 從指定的廊道基準線取得地勢線
> 2. 沿廊道地勢線以所需間距產生座標系統
> 3. 將縱斷面圖塊幾何圖形轉換至座標系統
> 4. 在輪廓之間斷面混成實體
> 5. 在 Civil 3D 中建立實體

我們開始吧！

### 取得廊道資料

我們的第一步是取得廊道資料。我們將依名稱選取廊道模型，取得廊道內的特定基準線，然後依其點代碼取得基準線內的地勢線。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>選取廊道、基準線和地勢線</p></figcaption></figure>

### 產生座標系統

我們現在要沿廊道地勢線，在指定的起點樁號和終點樁號之間產生**座標系統**。這些座標系統將用於將車輛縱斷面圖塊幾何圖形與廊道對齊。

{% hint style="info" %} 如果您不熟悉座標系統，請查看[2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention")一節。{% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>沿廊道地勢線取得座標系統</p></figcaption></figure>

> 1. 請注意節點右下角的小 **XXX**。這表示節點的交織設定已設定為_笛卡兒積_，如此才能在兩條地勢線的相同樁號值處產生座標系統。

{% hint style="info" %} 如果您不熟悉節點交織，請查看[1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention")一節。{% endhint %}

### 轉換圖塊幾何圖形

現在，我們需要以某種方式沿地勢線建立一系列車輛縱斷面。我們將使用 **Geometry.Transform** 節點，從車輛縱斷面圖塊定義來轉換幾何圖形。這是一個難以理解的概念，因此在我們查看節點之前，這裡有一個圖表顯示將要發生的情況。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>在座標系統之間轉換幾何圖形的視覺圖像。</p></figcaption></figure>

因此，我們基本上是從_單一_圖塊定義中取得 Dynamo 幾何圖形，然後移動/旋轉它，同時沿地勢線建立一個陣列。好酷的東西！以下是節點序列的外觀。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. 這會從文件中取得圖塊定義。
> 2. 這些節點會取得圖塊內物件的 Dynamo 幾何圖形。
> 3. 這些節點基本上是定義我們要從中轉換幾何圖形的_來源_座標系統。
> 4. 最後，此節點會實際執行轉換幾何圖形的工作。
> 5. 請注意此節點上_最長的_交織。

以下是我們在 Dynamo 中得到的結果。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>轉換後的車輛縱斷面圖塊幾何圖形</p></figcaption></figure>

### 產生實體

好消息！辛苦的工作已經完成。我們現在只需在輪廓之間產生實體。這可以透過 **Solid.ByLoft** 節點輕鬆完成。

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

以下是結果。請記住，這些是 Dynamo 實體 - 我們仍需要在 Civil 3D 中建立它們。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>斷面混成後的 Dynamo 實體</p></figcaption></figure>

### 將實體輸出至 Civil 3D

我們的最後一步是將產生的實體輸出至模型空間。我們也為它們塗上顏色，比較容易看清楚。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>將實體輸出至 Civil 3D</p></figcaption></figure>

### 結果

以下是使用 **Dynamo 播放器**執行圖表的範例。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>使用 Dynamo 播放器執行圖表，然後在 Civil 3D 中查看結果</p></figcaption></figure>

{% hint style="info" %} 如果您不熟悉 Dynamo 播放器，請查看 [dynamo-player.md](../../dynamo-player.md "mention")一節。{% endhint %}

> :tada：任務完成！

## 構想

以下是一些如何擴充此圖表功能的構想。

{% hint style="info" %} 加入針對每條軌道分別使用**不同樁號範圍**的功能。{% endhint %}

{% hint style="info" %} **將實體分割**為可單獨分析衝突的較小區段。{% endhint %}

{% hint style="info" %} 請檢查包絡線實體是否**與圖徵相交**，並為發生衝突的實體著色。 {% endhint %}
