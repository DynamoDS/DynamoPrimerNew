# 点编组管理

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

在 Civil 3D 中使用几何空间点和点编组是许多现场完成过程的核心要素。Dynamo 在数据管理方面非常出色，我们将在本例中演示一个潜在用例。 

## 目标

> :dart: 为每个唯一的几何空间点描述创建点编组。

## 关键概念

> * 使用列表
> * 使用 **List.GroupByKey** 节点对类似对象进行分组
> * 在 Dynamo 播放器中显示自定义输出

## 版本兼容性

{% hint style="success" %}
此图形将在 **Civil 3D 2020** 及更高版本上运行。
{% endhint %}

## 数据集

首先下载下面的样例文件，然后打开 DWG 文件和 Dynamo 图形。

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## 解决方案

下面概述了此图形中的逻辑。

> 1. 获取文档中的所有几何空间点
> 2. 按描述对几何空间点进行编组
> 3. 创建点编组
> 4. 将摘要输出到 Dynamo 播放器

开始吧！

### 获取几何空间点

我们的第一步是获取文档中的所有点编组，然后获取每个编组中的所有几何空间点。这将为我们提供 _嵌套列表_ 或“列表的列表”（稍后如果我们使用 **List.Flatten** 节点将所有内容都向下展平为单个列表，将更易于使用它们）。

{% hint style="info" %}
如果您对操作列表不熟悉，请参见 [2-使用列表.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention")部分。
{% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>获取所有点编组和几何空间点 </p></figcaption></figure>

### 按描述对点进行分组

现在，我们已有所有几何空间点，需要根据这些几何空间点的描述将它们分成多个组。这正是 **List.GroupByKey** 节点的作用。它本质上是将共享相同键的任何项目分组在一起。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>按描述对几何空间点进行分组</p></figcaption></figure>

### 创建点编组

艰苦的工作已完成！最后一步是从分组的几何空间点创建新的 Civil 3D 点编组。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>创建新的点编组</p></figcaption></figure>

### 输出摘要

当您运行图形时，由于我们不在处理任何几何图形，因此在 Dynamo 后台预览中看不到任何内容。因此，查看图形是否正确执行的唯一方法是检查“工具空间”，或查看节点输出预览。但是，如果我们使用 **Dynamo 播放器**运行图形，则可以通过输出已创建的点编组的摘要来提供有关图形结果的更多反馈。您只需在节点上单击鼠标右键，然后将其设置为 _“为输出”(Is Output)_。在本例中，我们使用重命名的 **Watch** 节点来查看结果。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>将节点设置为<em>“是输出”(Is Output)</em> 将在 Dynamo 播放器输出中显示其内容</p></figcaption></figure>

### 结果

以下是一个使用 **Dynamo 播放器**运行图形的示例。

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>使用 Dynamo 播放器运行图形并在工具空间中查看结果</p></figcaption></figure>

{% hint style="info" %}
如果您对使用 Dynamo 播放器不熟悉，请参见 [Dynamo 播放器.md](../../dynamo-player.md "mention")部分。
{% endhint %}

> :tada: 任务完成！

## 想法

以下是一些有关如何扩展此图形功能的想法。

{% hint style="info" %}
将点编组修改为基于 **完整描述** 而非原始描述。
{% endhint %}

{% hint style="info" %}
按您选择的其他 **预定义类别**（例如，“地面快照”、“碑界”等）对点进行分组。
{% endhint %}

{% hint style="info" %}
为某些组中的点自动创建三角网曲面。
{% endhint %}
