# 服务设施放置

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

典型住宅开发的工程设计涉及使用多个地下公共设施（如生活污水管、雨水排水、饮用水等）。本例将演示如何使用 Dynamo 来绘制从配水总管到给定小块土地（即地块）的服务设施连接。每个地块都需要连接服务设施，这会导致放置所有服务设施的工作非常繁琐。Dynamo 可以通过自动精确绘制必要的几何图形，并提供可调整以符合当地机构标准的灵活输入，从而加快该过程。

## 目标

> :dart: 将水表块参照放置在距地块线的指定偏移处，并为垂直于配水总管的每个服务设施连接绘制一条线。

## 关键概念

> * 使用 **Select Object** 节点进行用户输入
> * 使用坐标系
> * 使用几何操作（如 **Geometry.DistanceTo** 和 **Geometry.ClosestPointTo**）
> * 创建块参照
> * 控制对象绑定设置

## 版本兼容性

{% hint style="success" %}
此图形将在 **Civil 3D 2020** 及更高版本上运行。
{% endhint %}

## 数据集

首先下载下面的样例文件，然后打开 DWG 文件和 Dynamo 图形。

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## 解决方案

下面概述了此图形中的逻辑。

> 1. 获取配水总管的曲线几何图形
> 2. 获取用户选定地块线的曲线几何图形，必要时反转
> 3. 生成服务设施计量表的插入点
> 4. 获取配水总管上距服务设施计量表位置最近的点
> 5. 在模型空间中创建块参照和线

开始吧！

### 获取配水总管几何图形

我们的第一步是将配水总管的几何图形输入到 Dynamo 中。我们将改为获取特定图层上的所有对象，并将这些对象一起连接为 Dynamo PolyCurve，而不是选择单条直线或多段线。

{% hint style="info" %}
如果您对使用 Dynamo 曲线几何图形不熟悉，请参见 [4-曲线.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "提及")部分。
{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>从 Civil 3D 获取对象并将所有对象一起连接为单个 PolyCurve</p></figcaption></figure>

### 获取地块线几何图形

接下来，我们需要将选定地块线的几何图形输入到 Dynamo 中，以便我们可以使用该几何图形。适合该作业的合适工具是 **Select Object** 节点，该节点使图形的用户能够拾取 Civil 3D 中的特定对象。

我们还需要处理可能会出现的潜在问题。地块线有起点和终点，这意味着它有方向。为了使图形能够生成一致的结果，我们需要所有地块线都有一致的方向。我们可以直接在图形逻辑中考虑此情况，这会使图形更具弹性。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>选择地块线并确保其方向正确</p></figcaption></figure>

> 1. 获取地块线的起点和终点。
> 2. 测量每个点到配水总管的距离，然后确定哪个距离更大。
> 3. 所需结果是该线的起点距离配水总管最近。如果不是这种情况的话，则我们反转地块线的方向。否则，我们只需返回原始地块线。

### 生成插入点

现在应该确定服务设施计量表将放置的位置。通常情况下，放置位置由当地机构的要求确定，因此我们只需提供可以更改以适应各种情况的输入值。我们会将沿地块线的**坐标系**用作创建点的参照。这使得定义相对于地块线的偏移非常容易，而无需考虑地块线的方向。

{% hint style="info" %}
如果您对使用坐标系不熟悉，请参见 [2-向量.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "提及")部分。
{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>创建服务设施计量表的插入点</p></figcaption></figure>

### 获取连接点

现在，我们需要获取配水总管上距服务设施计量表位置最近的点。这将使我们能够在模型空间中绘制服务设施连接，以便它们始终垂直于配水总管。**Geometry.ClosestPointTo** 节点是完美解决方案。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>获取配水总管上的垂直点</p></figcaption></figure>

> 1. 这是配水总管 PolyCurve
> 2. 这些是服务设施计量表插入点

### 创建对象

最后一步是在模型空间中实际创建对象。我们将使用之前生成的插入点来创建块参照，然后使用配水总管上的点来绘制到服务设施连接的线。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### 结果

当运行图形时，您应该会在模型空间中看到新的块参照和服务设施连接线。尝试更改某些输入，并观察所有内容自动更新！

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>在 Dynamo 中调整输入参数，并立即查看在 Civil 3D 中的结果</p></figcaption></figure>

### 小贴士：启用连续放置

您可能会注意到，在为一条地块线放置对象后，选择其他地块线会导致对象被“移动”。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>对象绑定时的行为处于启用状态</p></figcaption></figure>

这是 Dynamo 的默认行为，在许多情况下非常有用。但是，您可能会发现需要按顺序放置多个服务设施连接，并让 Dynamo 使用每个管路创建新对象，而不是修改原始对象。可以通过更改对象绑定设置来控制此行为。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Dynamo 的对象绑定设置</p></figcaption></figure>

{% hint style="info" %}
有关详细信息，请参见[对象绑定.md](../../advanced-topics/object-binding.md "提及")部分。
{% endhint %}

更改此设置会强制 Dynamo“忘记”它使用每个管路创建的对象。以下是一个使用 **Dynamo 播放器**运行图形（其对象绑定已关闭）的示例。

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>使用 Dynamo 播放器运行图形并在 Civil 3D 中查看结果</p></figcaption></figure>

{% hint style="info" %}
如果您对使用 Dynamo 播放器不熟悉，请参见 [Dynamo 播放器.md](../../dynamo-player.md "提及")部分。
{% endhint %}

> :tada: 任务完成！

## 想法

以下是一些有关如何扩展此图形功能的想法。

{% hint style="info" %}
同时放置 **多个服务设施连接**，而不是选择每条地块线。
{% endhint %}

{% hint style="info" %}
调整输入以改为放置 **污水管清扫口**，而不是放置水表。
{% endhint %}

{% hint style="info" %}
 **添加开关**，以允许在地块线的特定侧（而不是两侧）放置单个服务设施连接。
{% endhint %}
