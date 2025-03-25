# 间隙包络

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

为间隙验证开发运动包络是轨道设计的重要部分。Dynamo 可用于为包络生成实体，而不是创建和管理复杂的道路子部件来执行该作业。

## 目标

> :dart: 使用车辆轮廓块来沿道路生成间隙包络三维实体。

## 关键概念

> * 使用道路要素线
> * 在坐标系之间转换几何图形
> * 通过放样创建实体
> * 使用连缀设置控制节点行为

## 版本兼容性

{% hint style="success" %} 此图形将在 **Civil 3D 2020** 及更高版本上运行。 
{% endhint %} 

## 数据集

首先下载下面的样例文件，然后打开 DWG 文件和 Dynamo 图形。

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## 解决方案

下面概述了此图形中的逻辑。

> 1. 从指定的道路基准线获取要素线
> 2. 沿道路要素线以所需的间距生成坐标系
> 3. 将轮廓块几何图形转换为坐标系
> 4. 在轮廓之间放样实体
> 5. 在 Civil 3D 中创建实体

开始吧！

### 获取道路数据

我们的第一步是获取道路数据。我们将按名称选择道路模型、获取道路中的特定基准线，然后按点代码获取基准线中的要素线。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>选择道路、基准线和要素线</p></figcaption></figure>

### 生成坐标系

现在，我们将沿道路要素线在起点桩号和终点桩号之间生成**坐标系**。这些坐标系将用于将车辆轮廓块几何图形与道路对齐。

{% hint style="info" %}
 如果您对使用坐标系不熟悉，请参见 [2-向量.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") 部分。 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>沿道路要素线获取坐标系</p></figcaption></figure>

> 1. 请注意节点右下角的小 **XXX**。这意味着节点的连缀设置设为 _“叉积”_，这对于以相同桩号值为两条要素线生成坐标系而言是必要的。

{% hint style="info" %}
 如果您对使用节点连缀不熟悉，请参见 [1-什么是列表.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention") 部分。 
{% endhint %} 

### 转换块几何图形

现在，我们需要以某种方式创建沿要素线的车辆轮廓的阵列。我们将使用 **Geometry.Transform** 节点来基于车辆轮廓块定义转换几何图形。这是一个难以可视化的概念，因此在我们查看节点之前，这里有一张图显示了将要发生的情况。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>在坐标系之间转换几何图形的可视化。</p></figcaption></figure>

实际上，我们基于  _单个_ 块定义获取 Dynamo 几何图形，然后移动/旋转该几何图形，同时沿要素线创建阵列。酷炫！节点序列如下所示。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. 这将从文档中获取块定义。
> 2. 这些节点获取块中对象的 Dynamo 几何图形。
> 3. 这些节点本质上定义了我们将转换几何图形的 _来源_ 坐标系。
> 4. 最后，此节点执行转换几何图形的实际工作。
> 5. 注意此节点上的 _“最长”_ 连缀。

以下是我们在 Dynamo 中获取的内容。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>转换后的车辆轮廓块几何图形</p></figcaption></figure>

### 生成实体

好消息！艰苦的工作已完成。我们现在只需在轮廓之间生成实体。这可以通过 **Solid.ByLoft** 节点轻松完成。

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

结果如下所示。请记住，这些是 Dynamo 实体 - 我们仍需要在 Civil 3D 中创建它们。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>放样后的 Dynamo 实体</p></figcaption></figure>

### 将实体输出到 Civil 3D 中

我们的最后一步是将生成的实体输出到模型空间中。我们还会为这些实体赋予颜色，以使它们易于区分。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>将实体输出到 Civil 3D 中</p></figcaption></figure>

### 结果

以下是一个使用 **Dynamo 播放器**运行图形的示例。

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>使用 Dynamo 播放器运行图形并在 Civil 3D 中查看结果</p></figcaption></figure>

{% hint style="info" %}
 如果您对使用 Dynamo 播放器不熟悉，请参见 [Dynamo 播放器.md](../../dynamo-player.md "mention") 部分。 
{% endhint %} 

> :tada: 任务完成！

## 想法

以下是一些有关如何扩展此图形功能的想法。

{% hint style="info" %}
 添加为每个轨迹单独使用 **不同桩号范围** 的功能。 
{% endhint %} 

{% hint style="info" %}
 **拆分实体** 为可以单独分析其是否发生碰撞的较小段。 
{% endhint %} 

{% hint style="info" %}
 检查以查看包络实体是否 **与要素相交**，并为发生碰撞的实体标注颜色。 
{% endhint %} 
