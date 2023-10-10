# 節點和線路

## 節點

在 Dynamo 中，**節點**互相連接以形成視覺程式的物件。每個**節點**執行一項作業 - 有時可能是簡單作業 (例如儲存數字)，也可能是比較複雜的動作 (例如建立或查詢幾何圖形)。

### 剖析節點

Dynamo 中的大多數節點由五個部分組成。雖然有一些例外，例如輸入節點，但每個節點的剖析可說明如下：

![](images/nodesandwires-nodesanatomy.jpg)

> 1. 名稱：採用 `Category.Name` 命名慣例的節點名稱
> 2. 主體：即節點的主體，在此處按一下右鍵會顯示整個節點在該層級的選項
> 3. 埠 (輸入和輸出)：線路的接收器，向節點提供輸入資料，以及提供節點動作結果。
> 4. 預設值：對輸入埠按一下右鍵，某些節點具有可供使用或不可使用的預設值。
> 5. 交織圖示：註明為配對清單輸入內容所指定的[交織選項](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing) (之後會進一步討論)

### 節點輸入/輸出埠

節點的輸入與輸出稱為「埠」，可作為線路的接收器。資料從左側透過埠進入節點，在執行其作業後從右側流出節點。

埠應當接收特定類型的資料。例如，若將一個數字 (例如 _2.75_) 連接至座標點節點上的埠，會成功建立一個點；但是如果為同一埠提供 _「RED」_，則會導致錯誤。


{% hint style="info" %} 秘訣：將游標懸停在埠上可看到一個工具提示，其中包含預期的資料類型。
{% endhint %}

![](images/nodesandwires-nodesinputandtooltip.jpg)

> 1. 埠標示
> 2. 工具提示
> 3. 資料類型
> 4. 預設值

### 節點狀態

Dynamo 會根據每個節點的狀態，讓節點呈現不同的顏色外觀，標示出視覺程式的執行狀態。狀態階層依照下列順序：「錯誤」>「警告」>「資訊」>「預覽」。

懸停在名稱或埠上或對名稱或埠按一下右鍵，可呈現其他資訊和選項。

![](../.gitbook/assets/nodesandwires-nodestates.png)

> 1. 滿足條件的輸入：節點的輸入埠標有藍色直線，代表節點連接良好，且所有輸入都成功連接。
> 2. 未滿足條件的輸入：節點的一個或多個輸入埠標有紅色直線，表示必須讓這些輸入處於連接狀態。
> 3. 函數：輸出函數且在輸出埠標有灰色直線的節點，即是函數節點。
> 4. 選取狀態：目前選取的節點邊界會用水藍色醒目顯示。
> 5. 凍結狀態：半透明藍色節點表示已凍結，暫停執行節點。
> 6. 關閉預覽：節點下方出現灰色狀態列和眼睛圖示 <img src="images/nodesandwires-previewoff.jpg" alt="" data-size="line">，即表示已關閉節點的幾何圖形預覽。
> 7. 警告：節點下方的黃色狀態列為「警告」狀態，表示節點可能缺少輸入資料，或者資料類型不正確。
> 8. 錯誤狀態：節點下方的紅色狀態列表示節點處於「錯誤」狀態。
> 9. 資訊：節點下方的藍色狀態列表示「資訊」狀態，會標示出節點的相關實用資訊。當節點以可能影響效能等等的方式達到節點支援的最大值時，可觸發此狀態。

#### 處理錯誤或警告節點

如果您的視覺程式包含警告或錯誤，Dynamo 會提供有關問題的其他資訊。任何顯示為黃色的節點會在其名稱上方顯示工具提示。將滑鼠懸停在警告 ![](images/nodesandwires-nodewarningicon.png) 或錯誤 ![](images/nodesandwires-nodeerroricon.png) 工具提示圖示上，可將其展開。


{% hint style="info" %} 秘訣：隨時使用此工具提示資訊檢查上游節點，可查看所需的資料類型或資料結構是否有錯誤。
{% endhint %}

![](images/nodesandwires-nodeswithwarningtooltip.jpg)

> 1. 警告工具提示 -「Null」(亦即沒有資料) 無法識別為 Double (即數字)
> 2. 使用 Watch 節點檢查輸入資料
> 3. 上游的 Number 節點儲存「Red」，不是數字

## 線路

線路連接兩個節點，以建立關係並建立視覺程式的流動。可以將這些按字面意思想成電線，用於將資料的脈衝從一個物件傳遞至另一個物件。

### 程式流 <a href="#program-flow" id="program-flow"></a>

線路將一個節點的輸出埠連接至另一個節點的輸入埠。此定向性建立視覺程式中的**資料流**。

輸入埠位於節點的左側，輸出埠位於節點的右側，因此我們通常可以說程式從左邊流向右邊。

![](images/nodesandwires-flowofdata.jpg)

### 建立線路 <a href="#creating-wires" id="creating-wires"></a>

在埠上按一下左鍵來建立線路，接著在另一個節點的埠上按一下左鍵來建立連接。建立連接的過程中，線路會顯示為虛線，成功連接後即會變成實線。

資料將一律透過此線路從輸出流向輸入；但是，我們可透過點按連接埠的順序，建立任何方向的線路。

![](images/nodesandwires-creatingawire.gif)

### 編輯線路 <a href="#editing-wires" id="editing-wires"></a>

我們會經常需要編輯線路的連接，來調整視覺程式中的程式流。若要編輯線路，請按一下已連接節點的輸入埠。您現在有兩個選項：

* 變更輸入埠的連接，請在另一個輸入埠上按一下左鍵

![](<images/nodesandwires-editwirechangeport(1)(1) (1) (1).gif>)

* 若要移除線路，請將線路移開，然後在工作區上按一下左鍵

![](images/nodesandwires-editwiresremove.gif)

* 按住 Shift 並按一下左鍵重新連接多條線路

![](images/nodesandwires-editmultiports.gif)

* 按住 Ctrl 並按一下左鍵複製線路

![](images/nodesandwires-duplicatewire.gif)

#### 預設與亮顯的線路 <a href="#wire-previews" id="wire-previews"></a>

依預設，線路將以灰色線條呈現預覽。選取節點後，將使用與節點相同的水藍色亮顯方法呈現任何連接的線路。

![](images/nodesandwires-defaultvshighlightedwires.jpg)

> 1. 亮顯的線路
> 2. 預設線路

**依預設隱藏線路**

如果您想要隱藏圖表中的線路，可以從「檢視」>「連接器」> 取消勾選「展示連接器」找到此選項。

使用此設定時，只有選取的節點及其接合線路會以水藍色亮顯展示。

![](images/nodesandwires-hidewiressetting\(1\).gif)

#### 只隱藏個別線路

您也可以在節點輸出上按一下右鍵 > 選取「隱藏線路」，只隱藏選取的線路

![](images/nodesandwires-hideselectedwire.gif)
