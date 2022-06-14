# 節點和線路

## 節點

在 Dynamo 中，**節點**是互相連結以形成視覺程式的物件。 每個**節點**執行一項作業 - 有時可能是簡單作業 (例如儲存數字)，也可能是比較複雜的動作 (例如建立或查詢幾何圖形)。

### 剖析節點

Dynamo 中的大多數節點由五個部分組成。雖然有一些例外，例如輸入節點，但每個節點的剖析可說明為如下：

![](<images/nodes and wires - nodes anatomy.jpg>)

> 1. 名稱 - 採用 `Category.Name` 命名慣例的節點名稱
> 2. 主體 - 節點的主體 - 在此處按一下右鍵會顯示整個節點在該層次的選項
> 3. 埠 (入埠和出埠) - 向節點提供輸入資料以及提供節點動作結果之線路的接收器
> 4. 預設值 - 對輸入埠按一下右鍵 - 某些節點具有可以使用或不可使用的預設值。
> 5. 交織圖示 - 指出指定的[交織選項](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing)以符合清單輸入 (之後還有其他目的)

### 節點輸入/輸出埠

節點的輸入與輸出稱為「埠」，可作為線路的接收器。資料從左側透過埠進入節點，在執行其作業後從右側流出節點。

埠預期接收特定類型的資料。例如，將一個數字 _2.75_ 連接至座標點節點上的埠將成功建立點；但是，如果我們為同一埠提供 _「RED」_，它會導致錯誤。

{% hint style="info" %}
秘訣：將游標懸停在埠上可看到一個工具提示，其中包含預期的資料類型。
{% endhint %}

![](<images/nodes and wires - nodes input and tooltip.jpg>)

> 1. 埠標籤
> 2. 工具提示
> 3. 資料類型
> 4. 預設值

### 節點狀態

Dynamo 會透過根據每個節點的狀態為節點呈現不同顏色外觀來指示視覺程式的執行狀態。狀態階層依照下列順序：「錯誤」>「警告」>「資訊」>「預覽」。

懸停在名稱或埠上或對名稱或埠按一下右鍵，可呈現其他資訊和選項。

![](<images/nodes and wires - node states.jpg>)

> 1. 作用中 - 具有深灰色背景名稱的節點已正確連結且其所有輸入已成功連接
> 2. 錯誤狀態 - 節點下方的紅色狀態列表示節點處於錯誤狀態
> 3. 凍結 - 透明節點表示已開啟「凍結」，該節點已終止執行
> 4. 背景預覽 - 節點下方的灰色狀態列和眼睛圖示 ![](<images/nodes and wires - preview off.jpg>) 表示幾何圖形預覽已關閉。
> 5. 已選取 - 目前所選節點在其邊界會以水藍色強調顯示。
> 6. 警告 - 節點下方的黃色狀態列是「警告」狀態，表示它們缺少輸入資料或可能有不正確的資料類型。

#### 處理錯誤或警告節點

如果您的視覺程式包含警告或錯誤，Dynamo 會提供有關問題的其他資訊。任何顯示為黃色的節點會在其名稱上方顯示工具提示。將滑鼠懸停在警告 ![](<images/nodes and wires - node warning icon.png>) 或錯誤 ![](<images/nodes and wires - node error icon.png>) 工具提示圖示上，可將其展開。

{% hint style="info" %}
秘訣：隨時使用此工具提示資訊檢查上游節點，可查看所需的資料類型或資料結構是否有錯誤。
{% endhint %}

![](<images/nodes and wires - nodes with warning tooltip.jpg>)

> 1. 警告工具提示 -「NULL」(或沒有資料) 無法識別為 Doubel，即數字
> 2. 使用 Watch 節點檢查輸入資料
> 3. 上游的 Number 節點儲存的「Red」不是數字

## 線路

線路連接兩個節點，以建立關係並建立視覺程式的流動。我們可以把它們按字面意思想成電線，用於將資料脈衝從一個物件傳遞至另一個物件。

### 程式流<a href="#program-flow" id="program-flow"></a>

線路將一個節點的輸出埠連接至另一個節點的輸入埠。此定向性建立視覺程式中的**資料流**。

輸入埠位於節點的左側，輸出埠位於節點的右側，因此我們通常可以說程式從左邊流向右邊。

![](<images/nodes and wires - flow of data.jpg>)

### 建立線路 <a href="#creating-wires" id="creating-wires"></a>

在埠上按一下左鍵來建立配線，接著在另一個節點的埠上按一下左鍵來建立連接。建立連接時，線路將顯示為虛線，當成功連結後，將變為實線。

資料將一律透過此線路從輸出流向輸入；但是，我們可透過改變點按所連結埠的順序，建立任何方向的線路。

![](<images/nodes and wires - creating a wire.gif>)

### 編輯線路 <a href="#editing-wires" id="editing-wires"></a>

我們會經常需要透過編輯線路表示的連線來調整視覺程式中的程式流。若要編輯線路，請按一下已連結節點的輸入埠。您現在有兩個選項：

* 變更輸入埠的連接，在另一個輸入埠上按一下左鍵

![](<images/nodes and wires - edit wire change port (2).gif>)

* 若要移除線路，請將線路移開，然後在工作區上按一下左鍵

![](<images/nodes and wires - edit wires remove.gif>)

* 按住 Shift 並按一下左鍵，重新連接多條線路

![](<images/nodes and wires - edit multi ports.gif>)

* 按住 Ctrl 並按一下左鍵以複製線路

![](<images/nodes and wires - duplicate wire.gif>)

#### 預設與亮顯的線路<a href="#wire-previews" id="wire-previews"></a>

依預設，線路將以灰色線條進行預覽。選取節點後，它將使用與節點相同的水藍色高亮顯示方法顯示任何連接線路。

![](<images/nodes and wires - default vs highlighted wires.jpg>)

> 1. 亮顯線路
> 2. 預設線路

**依預設隱藏線路**

如果您想要隱藏圖表中的線路，可以從「檢視」>「連接器」> 取消勾選「展示連接器」找到此選項。

使用此設定時，只有選取的節點及其接合線路會以水藍色亮顯展示。

![](<images/nodes and wires - hide wires setting (1).gif>)

#### 只隱藏個別線路

您也可以在節點輸出上按一下右鍵 > 選取「隱藏線路」，只隱藏選取的線路

![](<images/nodes and wires - hide selected wire.gif>)
