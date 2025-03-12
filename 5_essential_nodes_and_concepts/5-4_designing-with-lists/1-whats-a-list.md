# 什麼是清單

### 什麼是清單？

清單是元素 (即項目) 的集合。例如一束香蕉。每個香蕉都是清單 (即香蕉束) 中的項目。揀選一束香蕉比分別揀選每個香蕉更容易，依據資料結構中的參數式關係對元素進行分組也是如此。

![香蕉](../images/5-4/1/Bananas\_white\_background\_DS.jpg)

> 相片由 [Augustus Binu](https://commons.wikimedia.org/wiki/File:Bananas\_white\_background\_DS.jpg?fastcci\_from=11404890\&c1=11404890\&d1=15\&s=200\&a=list) 拍攝。

購買雜貨時，我們會將購買的所有商品放入袋中。這個袋子也是清單。如果要製作香蕉麵包，我們需要 3 束香蕉 (我們將製作 _大量_ 香蕉麵包)。袋子表示香蕉束的清單，而每束香蕉表示香蕉的清單。袋子是清單的清單 (二維)，而香蕉束是清單 (一維)。

在 Dynamo 中，清單資料具有順序，每個清單中第一個項目的索引都是「0」。以下我們將討論在 Dynamo 中如何定義清單，以及多個清單如何彼此相關。

### 從零開始的索引

起初有一點可能看起來很奇怪，那就是清單的第一個索引始終是 0，而不是 1。因此，在談到清單的第一個項目時，實際指的是索引 0 對應的項目。

例如，如果您數數右手手指的數量，很可能會從 1 數到 5。但是，如果將手指放在清單中，Dynamo 會為其指定從 0 至 4 的索引。雖然這對於程式設計的初學者而言可能有些奇怪，但從零開始的索引是多數運算系統中的標準做法。

請注意，我們的清單中仍有 5 個項目，清單恰好使用從零開始的計數系統。清單中正在儲存的項目不一定是數字。它們可以是 Dynamo 支援的任何資料類型，例如點、曲線、曲面、族群等。

![](../images/5-4/1/what'salist-zerobasedindices.jpg)

> a.索引
>
> b.點
>
> c.項目

通常，查看清單中所儲存資料類型的最簡單方法，是將觀看節點連接至另一個節點的輸出。依預設，觀看節點會在清單的左側自動展示所有索引，並在右側展示資料項目。

使用清單時，這些索引是非常重要的元素。

### 輸入與輸出

對清單而言，輸入與輸出視使用的 Dynamo 節點而有所不同。例如，接下來我們使用包含 5 個點的清單，並將此輸出連接至兩個不同的 Dynamo 節點：**PolyCurve.ByPoints** 與 **Circle.ByCenterPointRadius**：

![輸入範例](../images/5-4/1/what'salist-inputsandoutputs.jpg)

> 1. **PolyCurve.ByPoints** 的 _points_ 輸入是尋找 _「Point[]」_。這表示點清單
> 2. **PolyCurve.ByPoints** 的輸出是從一個包含五個點的清單建立的一條 PolyCurve。
> 3. **Circle.ByCenterPointRadius** 的 _centerPoint_ 輸入要求 _「Point」_。
> 4. **Circle.ByCenterPointRadius** 的輸出是一個包含五個圓的清單，其中圓的中心對應於點的原始清單。

**PolyCurve.ByPoints** 與 **Circle.ByCenterPointRadius** 的輸入資料相同，但是 **Polycurve.ByPoints** 節點的結果是一條 PolyCurve，而 **Circle.ByCenterPointRadius** 節點的結果是中心位於每個點的 5 個圓。以直觀方式很容易理解這一點：polycurve 繪製為連接 5 個點的曲線，而圓會在每個點建立不同的圓。資料出現什麼情況？

將游標懸停在 **Polycurve.ByPoints** 的 _points_ 輸入上方，可以看到輸入在尋找 _「Point[]」_。注意末尾的中括號。這表示點的清單，若要建立 polycurve，輸入需要是每個 polycurve 的清單。因此，此節點會將每個清單濃縮到一條 polycurve 中。

另一方面，**Circle.ByCenterPointRadius** 的 _centerPoint_ 輸入要求 _「Point」_。此節點會尋找一個點，做為項目以定義圓的中心點。因此輸入資料會產生五個圓。辨識 Dynamo 中這些輸入的差異可協助您更好地瞭解在管理資料時節點的作業方式。

### 交織

資料相符是沒有明確解決方案的問題。在節點對大小不同的輸入具有存取權時，會發生此問題。變更資料相符演算法會產生截然不同的結果。

想像在點之間建立直線段的節點 (**Line.ByStartPointEndPoint**)。它有兩個輸入參數，都提供點座標：

#### 最短清單

最簡單的方式是逐一連接輸入，直到其中一個串流結束為止。這稱為「最短清單」演算法。這是 Dynamo 節點的預設行為：

![](../images/5-4/1/what'salist-lacing-shortest.jpg)

#### 最長清單

「最長清單」演算法會保持連接輸入，重複使用元素，直到所有串流結束為止：

![](../images/5-4/1/what'salist-lacing-longest.jpg)

#### 笛卡兒積

最後，「笛卡兒積」方法會產生所有可能的連接：

![](../images/5-4/1/what'salist-lacing-cross.jpg)

您可以看到，可以採用不同方法在這組點之間繪製直線。在節點的中心按一下右鍵，然後選擇「交織」功能表，可以找到「交織」選項。

![](../images/5-4/1/what'salist-rightclicklacingopt.jpg)

### 什麼是複製？

想像一下您有一串葡萄。如果您想做葡萄汁，不會一顆一顆壓榨 - 您會把它們全部都放入榨汁機。Dynamo 中的「複製」採用類似方式：Dynamo 可以將某個作業一次套用到整個清單，而不是一次套用到一個項目。

Dynamo 節點會自動辨識何時使用清單，並將作業套用到多個元素。這表示您不必手動處理所有項目 - 它會自己發生。但是，當有多個清單時，Dynamo 如何決定處理清單的方式？

主要有兩種方式：

#### 笛卡兒複製
假設您在廚房裡做果汁。您有一個水果清單：`{apple, orange, pear}`，以及每種果汁的固定水量：`1 cup`。您想用每種水果做出相同水量的果汁。在這種情況下，「笛卡兒複製」開始發揮作用。

在 Dynamo 中，這表示您要將水果清單送入 Juice.Maker 節點的水果輸入中，而水量輸入固定為 1 杯。節點就會個別處理每種水果，與固定水量混合。結果為：

`apple juice with 1 cup of water` `orange juice with 1 cup of water` `pear juice with 1 cup of water`

每種水果都搭配相同水量。

#### Zip 複製
Zip 複製的運作方式稍有不同。如果您有兩個清單，一個是水果：`{apple, orange, pear}`，另一個是糖量：`{2 tbsp, 3 tbsp, 1 tbsp}`，「Zip 複製」會結合每個清單中的對應項目。例如：

`apple juice with 2 tablespoons of sugar` `orange juice with 3 tablespoons of sugar` `pear juice with 1 tablespoon of sugar`

每種水果都搭配對應的糖量。

如需更深入的運作方式，請查看[複製與交織指南](https://github.com/DynamoDS/Dynamo/wiki/Replication-and-Replication-Guide-Part-1)。

## 練習

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-4/1/Lacing.dyn" %}

為了示範下面的交織作業，我們將使用此基準檔案來定義最短清單、最長清單及笛卡兒積。

我們將變更 **Point.ByCoordinates** 的交織，但不會變更上述圖表的任何其他內容。

### 最短清單

選擇 _最短清單_ 做為交織選項 (也是預設選項)，我們會得到一條由五個點組成的基本對角線。五個點是較短清單的長度，因此最短清單交織會在到達一個清單的結尾後停止。

![輸入範例](../images/5-4/1/what'salist-lacingexercise01.jpg)

### **最長清單**

如果將交織變更為 _最長清單_，我們會得到一條垂直延伸的對角線。運用與概念圖相同的方法，含 5 個項目的清單中的最後一個項目將重複，以達到較長清單的長度。

![輸入範例](../images/5-4/1/what'salist-lacingexercise02.jpg)

### **笛卡兒積**

如果將交織變更為 _笛卡兒積_，我們會得到各個清單之間的每種組合，產生一個 5x10 的點格線。這個資料結構等同於上面的概念圖顯示的笛卡兒積，只是現在資料是一個清單的清單。如果連接 polycurve，我們可以看到每個清單都由其 X 值定義，因此產生一列垂直線。

![輸入範例](../images/5-4/1/what'salist-lacingexercise03.jpg)
