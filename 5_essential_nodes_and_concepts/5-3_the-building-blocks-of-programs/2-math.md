# 數學

如果資料最簡單的形式是數字，則關聯這些數字最簡單的方式就是透過數學運算。從諸如除號等簡單運算子到三角函數，再到更複雜的公式，數學是開始探索數字關係與樣式的良好方式。

### 算術運算子

運算子是一組元件 (加、減、乘、除等)，使用代數函數與兩個數字輸入值，產生一個輸出值。在「運算子」>「動作」下可以找到這些運算子。

| 圖示                                                  | 名稱 (語法)     | 輸入                     | 輸出      |
| ----------------------------------------------------- | ----------------- | -------------------------- | ------------ |
| \![](<../images/5-1/addition(1)(1) (1) (1).jpg>)       | 加 (**+**)       | var[]...[]、var[]...[] | var[]...[] |
| \![](<../images/5-1/Subtraction(1)(1) (1) (1).jpg>)    | 減 (**-**)  | var[]...[]、var[]...[] | var[]...[] |
| \![](<../images/5-1/Multiplication(1)(1) (1) (1).jpg>) | 乘 (**\\***) | var[]...[]、var[]...[] | var[]...[] |
| \![](<../images/5-1/Division(1)(1) (1) (1).jpg>)       | 除 (**/**)    | var[]...[]、var[]...[] | var[]...[] |

## 練習：黃金螺旋線公式

> 按一下下方的連結下載範例檔案。
>
> 附錄中提供完整的範例檔案清單。

{% file src="../datasets/5-3/2/Building Blocks of Programs - Math.dyn" %}

### 第 I 部分：參數式公式

透過**公式**結合運算子和變數，以構成更複雜的關係。使用滑棒建立可透過輸入參數控制的公式。

1\. 建立表示參數式方程式中「t」的數字序列，因此，我們希望使用大到足以定義螺旋線的清單。

**Number Sequence：** 根據以下三項輸入定義數字序列：_start、amount_ 與 _step_。

![](../images/5-3/2/math-partI-01.jpg)

2\.上述步驟已建立用於定義參數範圍的數字清單。接下來，建立表示黃金螺旋線方程式的節點群組。

定義黃金螺旋線的方程式如下：

$$
x = r cos θ = a cos θ e^{bθ}
$$

$$
y = r sin θ = a sin θe^{bθ}
$$

以下影像以視覺程式設計形式表示黃金螺旋線。逐步檢查節點群組時，請盡可能注意視覺程式與書寫方程式之間的對應。

![](../images/5-3/2/math-partI-02.jpg)

> a.**Number Slider**：在圖元區加入兩個數字滑棒。這些滑棒代表參數式方程式中的 _a_ 與 _b_ 變數。這些表示彈性的常數，或表示我們可以針對所需結果進行調整的參數。
>
> b.**相乘 (*)**：相乘節點由星號表示。我們會重複使用此符號連接相乘的變數
>
> c.**Math.RadiansToDegrees**：「_t_」值需要轉換為度，才能在三角函數中演算。請記住，Dynamo 預設使用度來運算這些函數。
>
> d.**Math.Pow**：以「_t_」與數字「_e_」表示的函數，此函數會建立 Fibonacci 序列。
>
> e.**Math.Cos 與 Math.Sin**：這兩個三角函數將分別區分每個參數式點的 X 座標與 Y 座標。
>
> f.**Watch**：現在可以看到輸出是兩個清單，分別是產生螺旋線所用點的 _x_ 與 _y_ 座標。

### 第 II 部分：從公式到幾何圖形

現在，上一步的眾多節點都沒問題，但工作量很大。若要建立更有效率的工作流程，請參閱 [DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md) 將 Dynamo 表示式的字串定義為一個節點。在接下來的一系列步驟中，我們將瞭解使用參數式方程式來繪製 Fibonacci 螺旋線。

**Point.ByCoordinates：** 將上方的相乘節點連接到「_x_」輸入，將下方的節點連接到「_y_」輸入。我們現在可以在螢幕上看到點的參數式螺旋線。

![](../images/5-3/2/math-partII-01.gif)

**Polycurve.ByPoints：** 將上一步的 **Point.ByCoordinates** 連接到 _points_。我們可以保留 _connectLastToFirst_ 無輸入，因為不打算繪製封閉曲線。這會建立一條穿過上一步定義的每個點的螺旋線。

![](../images/5-3/2/math-partII-02.jpg)

我們現在完成了 Fibonacci 螺旋線！接下來進一步將此分為兩個單獨的練習，我們分別稱之為鸚鵡螺與向日葵。這些是自然系統的抽象名稱，但可以充分呈現 Fibonacci 螺旋線的兩種不同應用。

### 第 III 部分：從螺旋線到鸚鵡螺

**Circle.ByCenterPointRadius：** 我們在此處將使用圓節點，採用與上一步相同的輸入。半徑的預設值為 _1.0_，所以我們可以立即看到輸出的圓。它會立即清晰展示點如何進一步偏離原點。

![](../images/5-3/2/math-partIII-01.jpg)

**Number Sequence：** 這是「_t_」的原始陣列。將此序列插入 **Circle.ByCenterPointRadius** 的半徑值後，圓心仍會離原點越來越遠，但半徑會增加，因而產生很酷的 Fibonacci 圓形。

如果您使用 3D 製作會更酷！

![](../images/5-3/2/math-partIII-02.gif)

### 第 IV 部分：從鸚鵡螺到葉序

現在我們已經建立圓形的鸚鵡螺殼，接下來使用參數式格線。我們將對 Fibonacci 螺旋線使用基本旋轉，以建立 Fibonacci 格線，並在[向日葵種子長大後](https://blogs.unimelb.edu.au/sciencecommunication/2018/09/02/this-flower-uses-maths-to-reproduce/)對結果進行塑型。

一開始，我們先執行上一個練習中的相同步驟：使用 **Point.ByCoordinates** 節點建立點的螺旋線陣列。

\![](../images/5-3/2/math-part IV-01.jpg)

接下來，依照這些小步驟，以各種旋轉產生一系列螺旋線。

![](../images/5-3/2/math-partIV-02.jpg)

> a.**Geometry.Rotate：** 有幾個 **Geometry.Rotate** 選項，請確保選擇以 _geometry_、_basePlane_ 和 _degrees_ 為輸入的節點。將 **Point.ByCoordinates** 連接至 geometry 輸入。在此節點上按一下右鍵，並確保將交織設定為「笛卡兒積」
>
> <img src="../images/5-3/2/math-partIV-03crossproduct.jpg" alt="" data-size="original">
>
> b.**Plane.XY：** 連接至 _basePlane_ 輸入。我們將繞原點旋轉，此原點的位置與螺旋線的基準位置相同。
>
> c.**Number Range：** 對於角度輸入，我們希望建立多個旋轉。使用 **Number Range** 元件可以快速達成。將其連接至 _degrees_ 輸入。
>
> d.**Number：** 為了定義數字範圍，在圖元區以垂直順序加入三個數字節點。從上到下分別指定值為 _0.0、360.0_ 與 _120.0_。這些值將驅動螺旋線旋轉。請注意將三個數字節點連接至 **Number Range** 節點後的輸出結果。

輸出開始形成一個漩渦。接下來調整某些 **Number Range** 參數，並查看結果的變化。

將 **Number Range** 節點的步長大小從 _120.0_ 變更為 _36.0_。請注意，這會建立更多旋轉，因此會產生更密的格線。

![](../images/5-3/2/math-partIV-04.jpg)

將 **Number Range** 節點的步長大小從 _36.0_ 變更為 _3.6_。現在，這會產生密度大得多的格線，螺旋線的方向性變得不清楚。各位，我們產生了一朵向日葵。

![](../images/5-3/2/math-partIV-05.jpg)
