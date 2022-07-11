# 节点和导线

## 节点

在 Dynamo 中，**节点**是连接到可视化程序的对象。每个**节点**都执行一项操作 - 有时这可能与存储数字一样简单，或者可能是更复杂的操作，例如创建或查询几何体。

### 节点剖析

Dynamo 中的大多数节点由五个部分组成。虽然存在例外（如输入节点），但每个节点的剖析可描述如下：

![](<images/nodes and wires - nodes anatomy.jpg>)

> 1. 名称 - 采用 `Category.Name` 命名约定的节点名称
> 2. 主体 - 节点的主体 - 在此处单击鼠标右键可显示整个节点级别的选项
> 3. 端口（输入和输出）- 导线的接受器，它们向节点提供输入数据以及节点操作的结果
> 4. 默认值 - 在输入端口上单击鼠标右键 - 某些节点具有可以使用也可以不使用的默认值。
> 5. 连缀图标 - 表示为匹配列表输入指定的[“连缀”选项](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing)（稍后再做详细介绍）

### 节点输入/输出端口

节点的输入和输出称为端口，并充当导线的接受器。数据通过左侧的端口进入节点，并在节点执行右侧的操作后流出节点。

端口希望接收特定类型的数据。例如，将数字（如 _2.75_）连接到坐标点节点上的端口将成功创建点；但是，如果我们向同一端口提供_“Red”_，则会导致错误。

{% hint style="info" %}
提示：将光标悬停在端口上，可查看包含预期数据类型的工具提示。
{% endhint %}

![](<images/nodes and wires - nodes input and tooltip.jpg>)

> 1. 端口标签
> 2. 工具提示
> 3. 数据类型
> 4. 默认值

### 节点状态

Dynamo 通过基于每个节点的状态使用不同颜色方案渲染节点，来提供执行可视化程序的状态的指示。状态层次结构遵循以下顺序：“错误”>“警告”>“信息”>“预览”。

将光标悬停在名称或端口上或在其上单击鼠标右键，可显示其他信息和选项。

![](<images/nodes and wires - node states.jpg>)

> 1. 活动 - 背景为深灰色的节点连接良好，且已成功连接其所有输入
> 2. 错误状态 - 节点下方的红色状态栏指示节点处于“错误”状态
> 3. 冻结 - 透明节点已打开冻结，挂起节点的执行
> 4. 背景预览 - 节点和眼睛图标下方的灰色状态栏 ![](<images/nodes and wires - preview off.jpg>) 指示几何图形预览已关闭。
> 5. 已选定 - 当前选定节点在其边界上以浅绿色亮显
> 6. 警告 - 节点下方的黄色状态栏指示“警告”状态，这意味着它们缺少输入数据，也可能数据类型不正确。

#### 处理错误或警告节点

如果可视化程序包含警告或错误，Dynamo 将提供有关该问题的其他信息。任何黄色节点在名称上方也有工具提示。将光标悬停在警告 ![](<images/nodes and wires - node warning icon.png>) 或错误 ![](<images/nodes and wires - node error icon.png>) 工具提示图标上以将其展开。

{% hint style="info" %}
提示：掌握此工具提示信息后，检查上游节点以查看所需的数据类型或数据结构是否出错。{% endhint %}

![](<images/nodes and wires - nodes with warning tooltip.jpg>)

> 1. 警告工具提示 -“空”或无数据不能理解为双精度，即一个数字
> 2. 使用“Watch”节点检查输入数据
> 3. 上游“Number”节点将“Red”存储为非数字

## 导线

导线连接节点以创建关系并建立可视化程序的流。我们可以按照字面意思将其视为电线，用于将数据脉冲从一个对象传送到下一个对象。

### 程序流<a href="#program-flow" id="program-flow"></a>

导线将一个节点的输出端口连接到另一个节点的输入端口。此方向性将在可视化程序中建立**数据流**。

输入端口位于左侧，输出端口位于节点的右侧；因此，我们通常可以说程序流从左到右移动。

![](<images/nodes and wires - flow of data.jpg>)

### 创建导线<a href="#creating-wires" id="creating-wires"></a>

通过在一个端口上单击鼠标左键以创建导线，然后在另一个节点的端口上单击鼠标左键以创建连接。在建立连接的过程中，导线将显示为虚线，并在成功连接后进行捕捉以成为实线。

数据将始终通过此导线从输出流到输入；但是，我们可以按照单击连接端口的顺序沿任意方向创建导线。

![](<images/nodes and wires - creating a wire.gif>)

### 编辑导线<a href="#editing-wires" id="editing-wires"></a>

通常，我们要通过编辑导线表示的连接来调整可视化程序中的程序流。要编辑导线，请在已连接节点的输入端口上单击。现在有两种选择：

* 要更改到输入端口的连接，请在另一个输入端口上单击鼠标左键

![](<images/nodes and wires - edit wire change port (2).gif>)

* 要删除导线，请将导线移开并在工作空间上单击鼠标左键

![](<images/nodes and wires - edit wires remove.gif>)

* 使用 Shift+单击鼠标左键来重新连接多条导线

![](<images/nodes and wires - edit multi ports.gif>)

* 使用 Ctrl+单击鼠标左键来复制导线

![](<images/nodes and wires - duplicate wire.gif>)

#### 默认导线与亮显的导线<a href="#wire-previews" id="wire-previews"></a>

默认情况下，导线将通过灰色笔划进行预览。选择某个节点后，它将使用与该节点相同的浅绿色亮显渲染任何连接导线。

![](<images/nodes and wires - default vs highlighted wires.jpg>)

> 1. 亮显的导线
> 2. 默认导线

**默认隐藏导线**

如果您希望在图形中隐藏导线，可以从“视图”>“连接器”>“取消勾选显示连接器”中找到此选项。

使用此设置，只有选定的节点及其连接线将以淡绿色亮显。

![](<images/nodes and wires - hide wires setting (1).gif>)

#### 仅隐藏单条导线

还可以仅通过在节点输出上单击鼠标右键 > 选择“隐藏导线”来隐藏选定的导线

![](<images/nodes and wires - hide selected wire.gif>)
