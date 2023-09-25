# 物件併入

Dynamo for Civil 3D 包含功能非常強大的機制，可「記住」每個節點建立的物件。此機制稱為**物件併入**，它可讓 Dynamo 圖表在每次在同一個文件中執行時產生一致的結果。雖然在許多情況下這非常理想，但有時候，您可能想要對 Dynamo 的行為有更多控制。本節將協助您瞭解物件併入如何運作，以及如何利用此機制。

## 範例

考慮這個在模型空間中的目前圖層上建立圓的圖表。

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>一個建立圓的簡單圖表</p></figcaption></figure>

請注意半徑變更時會發生什麼情況。

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>在 Dynamo 中修改半徑輸入</p></figcaption></figure>

這是物件併入處於作用中的情況。Dynamo 的預設行為是_修改_圓的半徑，而不是每次半徑輸入變更時就建立一個新的圓。這是因為每次執行圖表時，**Object.ByGeometry** 節點會「記住」它建立了這個_特定的_圓。此外，Dynamo 會儲存此資訊，以便下次您開啟 Civil 3D 文件並執行圖表時，圖表就會有完全相同的行為。

## 另一個範例

我們來看一個您要變更 Dynamo 預設物件併入行為的範例。假設您要建置一個將文字放在圓中央的圖表。但是，您使用此圖表的目的是，可以一直重複執行圖表，而每次執行時無論選取哪個圓，都可以放置新的文字。這是圖表的外觀。

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>一個將文字放在所選圓中心的簡單圖表</p></figcaption></figure>

但是，以下是實際上選取不同圓時所發生的情況。

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>選取新的圓時，Dynamo 的預設行為</p></figcaption></figure>

看起來每次執行圖表後，文字就會刪除再重新建立。實際上，文字的位置是根據選取的圓做了_修改_。所以它是相同的文字，只是位置不同！為了每次都能建立新文字，我們需要修改 Dynamo 的物件併入設定為不要保留併入資料 (請參閱下面的[\#binding-settings](object-binding.md#binding-settings "mention"))。

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>物件併入設定</p></figcaption></figure>

進行這項變更後，我們會得到我們想要的行為。

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>停用物件併入後的行為</p></figcaption></figure>

## 併入設定

Dynamo for Civil 3D 允許透過**「Dynamo」**功能表中的**「併入資料儲存」**設定，修改預設的物件併入行為。

{% hint style="info" %} 請注意，「併入資料儲存」選項在 **Civil 3D 2022.1** 及更高版本中提供。{% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

預設會啟用所有選項。以下是每個選項的功能摘要。

### 選項 1：未保留併入資料

啟用此選項後，Dynamo 會「忘記」上次執行圖表時建立的物件。因此，圖表可在任何情況下在任何圖面中執行，並且每次都會建立新物件。

{% hint style="info" %} **使用時機**

當您希望 Dynamo「忘記」先前執行時所做的所有動作，並且每次都建立新物件時，請使用此選項。{% endhint %}

### 選項 2：儲存在 Dynamo 的圖表中

此選項表示在儲存物件併入中繼資料時，會將其序列化至圖表 (.dyn 檔)。如果您關閉/重新開啟圖表，並在**相同圖面**中執行該圖表，則所有作業應與您之前離開圖表時相同。如果您在**不同圖面**中執行圖表，則將從圖表中移除併入資料，並建立新物件。這表示如果您開啟原始圖面並再次執行圖表，將建立除舊物件之外的新物件。

{% hint style="info" %} **使用時機**

如果您希望 Dynamo「記住」上次在**特定圖面**中執行時建立的物件，請使用此選項。{% endhint %}

{% hint style="warning" %} 此選項最適合**特定圖面**與 Dynamo 圖表之間可以維持 1:1 關係的情況。選項 1 和 3 較適合設計為在多個圖面上執行的圖表。{% endhint %}

### 選項 3：儲存在 Dynamo 的圖面中

這與選項 2 類似，不同之處在於物件併入資料是在圖面中而不是在圖表 (.dyn 檔) 中序列化。如果您關閉/重新開啟圖表，並在**相同圖面**中執行該圖表，則所有作業應與您之前離開圖表時相同。如果您在**不同圖面**中執行圖表，則併入資料仍會保留在原始圖面中，因為併入資料是儲存在圖面中而不是圖表中。

{% hint style="info" %} **使用時機**

如果您要在**多個圖面**中使用同一個圖表，並讓 Dynamo「記住」它在每個圖面中所做的動作，請使用此選項。{% endhint %}

### 選項 4：儲存在 Dynamo 播放器的圖面中

使用此選項需要注意的第一點是，在透過 Dynamo 主介面執行圖表時，它不會影響圖表與圖面的互動方式。此選項_只_在使用 Dynamo 播放器執行圖表時適用。

{% hint style="info" %} 如果您不熟悉 Dynamo 播放器，請查看 [dynamo-player.md](../dynamo-player.md "mention")一節。{% endhint %}

如果您使用 Dynamo 主介面執行圖表，然後關閉主介面並使用 Dynamo 播放器執行同一個圖表，則會在之前建立的物件之上建立新物件。但是，Dynamo 播放器執行圖表一次後，就會序列化圖面中的物件併入資料。因此，如果您透過 Dynamo 播放器執行圖表多次，它會更新物件而不是建立新物件。如果您在**不同圖面**上透過 Dynamo 播放器執行圖表，則併入資料仍會保留在原始圖面中，因為併入資料是儲存在圖面中而不是圖表中。

{% hint style="info" %} **使用時機**

如果您要在多個圖面中使用 Dynamo 播放器執行圖表，並「記住」它在每個圖面中所做的動作，請使用此選項。{% endhint %}
