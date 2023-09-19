# 数学

如果最简单的数据形式是数字，则将这些数字关联起来的最简单方法是通过数学。从除法等简单的运算符到三角函数，再到更复杂的公式，Math 是开始探索数值关系和模式的好方法。

### 算术运算符

运算符是一组组件，这些组件使用具有两个数字输入值的代数函数，这些结果产生一个输出值（加、减、乘、除等）。这些可在“运算符”>“操作”下找到。

| 图标                                                  | 名称（语法）     | 输入(Inputs)                     | 输出      |
| ----------------------------------------------------- | ----------------- | -------------------------- | ------------ |
| \![](<../images/5-1/addition(1)(1) (1) (1).jpg>)       | 相加 (**+**)       | var[]...[], var[]...[] | var[]...[] |
| \![](<../images/5-1/Subtraction(1)(1) (1) (1).jpg>)    | 相减 (**-**)  | var[]...[], var[]...[] | var[]...[] |
| \![](<../images/5-1/Multiplication(1)(1) (1) (1).jpg>) | 相乘 ( ***** ) | var[]...[], var[]...[] | var[]...[] |
| \![](<../images/5-1/Division(1)(1) (1) (1).jpg>)       | 相除 (**/**)    | var[]...[], var[]...[] | var[]...[] |

## 练习：黄金螺旋公式

> 单击下面的链接下载示例文件。
>
> 可以在附录中找到示例文件的完整列表。

{% file src="../datasets/5-3/2/Building Blocks of Programs - Math.dyn" %}

### 第 I 部分：参数公式

通过**公式**将运算符和变量组合在一起以形成更复杂的关系。使用滑块来创建可由输入参数控制的公式。

1.创建表示参数方程中“t”的数字序列，因此我们要使用足够大的列表来定义螺旋。

**Number Sequence**：基于以下三个输入定义数字序列：_start、amount_ 和 _step_。

![](../images/5-3/2/math-partI-01.jpg)

2\.上述步骤已创建一列数字，来定义参数化域。接下来，创建表示黄金螺旋方程的节点组。

黄金螺旋定义为以下方程：

$$ x = r cos θ = a cos θ e^{bθ} $$

$$ y = r sin θ = a sin θe^{bθ} $$

下图以可视化编程形式呈现了黄金螺旋。在逐步查看节点组时，请尝试注意可视化程序和编写方程之间的平行性。

![](../images/5-3/2/math-partI-02.jpg)

> a.**Number Slider**：向画布添加两个数字滑块。这些滑块将表示参数方程的 _“a”_ 和 _“b”_ 变量。这些表示灵活的常量，或我们可以根据所需结果调整的参数。
>
> b.**相乘 (*)**：乘法节点由星号表示。我们将反复使用它来连接乘法变量
>
> c.**Math.RadiansToDegrees**：“_t_”值需要转换为度数，以便在三角函数中进行求值。请记住，Dynamo 默认使用度数来对这些函数求值。
>
> d.**Math.Pow**：作为“_t_”和数字“_e_”的函数，这将创建 Fibonacci 数列。
>
> e.**Math.Cos 和 Math.Sin**：这两个三角函数将分别区分每个参数点的 x 坐标和 y 坐标。
>
> f.**Watch**：现在，我们看到输出为两个列表，它们将是用于生成螺旋的点的 _x_ 和 _y_ 坐标。

### 第 II 部分：从公式到几何图形

现在，上一步中的大部分节点都可以正常工作，但这种工作量很大。要创建更高效的工作流，请查看[“DesignScript”](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md)，以将 Dynamo 表达式的字符串定义到一个节点中。在接下来的一系列步骤中，我们将了解如何使用参数方程绘制 Fibonacci 螺旋。

**Point.ByCoordinates**：将上乘法节点连接到 _“x”_ 输入，将下乘法节点连接到 _“y”_ 输入。现在，我们在屏幕上会看到点的参数化螺旋。

![](../images/5-3/2/math-partII-01.gif)

**Polycurve.ByPoints**：将上一步的 **“Point.ByCoordinates”** 连接到 _“points”_。我们可以不输入而保留 _“connectLastToFirst”_，因为我们不会绘制闭合曲线。这将创建穿过上一步中定义的每个点的螺旋。

![](../images/5-3/2/math-partII-02.jpg)

我们现在已完成 Fibonacci 螺旋！让我们从此处开始进一步研究两个独立的练习，我们称之为 Nautilus 和 Sunflower。这些是自然系统的抽象表示，但 Fibonacci 螺旋的两种不同应用将得到充分体现。

### 第 III 部分：从螺旋到 Nautilus

**Circle.ByCenterPointRadius**：我们将在此处使用“Circle”节点，其输入与上一步相同。半径值默认为 _“1.0”_，因此可以看到圆即时输出。这些点与原点之间的分离程度立即清晰可辩。

![](../images/5-3/2/math-partIII-01.jpg)

**Number Sequence**：这是 _“t”_ 的原始数组。通过将其连接到 **“Circle.ByCenterPointRadius”** 的半径值，圆心仍会与原点进一步偏离，但圆的半径不断增大，从而创建一个时髦的 Fibonacci 圆图形。

如果使其成为三维形式，可获得奖励积分！

![](../images/5-3/2/math-partIII-02.gif)

### 第 IV 部分：从 Nautilus 到 Phyllotaxis

现在的情况是，我们已创建一个圆形 Nautilus 壳，接下来我们转到参数化栅格。我们将在 Fibonacci 螺旋上使用基本旋转来创建 Fibonacci 栅格，然后在[向日葵种子生长](https://blogs.unimelb.edu.au/sciencecommunication/2018/09/02/this-flower-uses-maths-to-reproduce/)后对结果进行建模。

作为一个跳跃点，我们从上一练习的相同步骤开始：使用 **“Point.ByCoordinates”** 节点创建点的螺旋阵列。

\![](../images/5-3/2/math-part IV-01.jpg)

接下来，按照这些小步骤操作以生成一系列不同旋转的螺旋。

![](../images/5-3/2/math-partIV-02.jpg)

> a.**Geometry.Rotate**：有多个 **“Geometry.Rotate”** 选项；请务必选择将 _“geometry”_ 、 _“basePlane”_ 和 _“degrees”_ 作为其输入的节点。将 **“Point.ByCoordinates”** 连接到几何图形输入。在该节点上单击鼠标右键，并确保将连缀设置为“叉积”
>
> <img src="../images/5-3/2/math-partIV-03crossproduct.jpg" alt="" data-size="original">
>
> b.**Plane.XY**：连接到 _“basePlane”_ 输入。我们将绕原点旋转，该原点与螺旋的底部位置相同。
>
> c.**Number Range**：对于度数输入，我们要创建多个旋转。我们可以使用 **“Number Range”** 组件快速完成此操作。将其连接到 _“degrees”_ 输入。
>
> d.**Number**：要定义数字范围，请按垂直顺序将三个数字节点添加到画布。从上到下，分别指定值 _“0.0,360.0,”_ 和 _“120.0”_。这些驱动螺旋的旋转。将三个数字节点连接到相应节点后，请注意 **“Number Range”** 节点的输出结果。

我们的输出开始类似于旋涡。我们调整一些 **“Number Range”** 参数，看一看结果如何变化。

将 **“Number Range”** 节点的步长从 _“120.0”_ 更改为 _“36.0”_。请注意，这将创建更多旋转，因此会为我们提供更密集的栅格。

![](../images/5-3/2/math-partIV-04.jpg)

将 **“Number Range”** 节点的步长从 _“36.0”_ 更改为 _“3.6”_。现在，我们得到更密集的栅格，但螺旋的方向性尚不清楚。女士们，先生们：我们创建了一颗向日葵。

![](../images/5-3/2/math-partIV-05.jpg)
