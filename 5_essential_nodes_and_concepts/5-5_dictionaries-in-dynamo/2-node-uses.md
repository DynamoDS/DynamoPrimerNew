# 词典节点

Dynamo 2.0 带有各种词典节点供我们使用。这包括 _创建、操作和查询_ 节点。

![](../images/5-5/2/dictionarynodes-nodes.jpg)

#### 创建

1.`Dictionary.ByKeysValues` 会使用提供的值和键创建词典。_（条目数将是最短列表输入内容）_

#### 操作

2\. `Dictionary.Components` 会生成输入词典的组件。_（这与创建节点相反。）_

3\. `Dictionary.RemoveKeys` 会生成一个新的词典对象，其中输入键已删除。

4\. `Dictionary.SetValueAtKeys` 会根据输入键和值生成新词典，以替换相应键处的当前值。

5\. `Dictionary.ValueAtKey` 会返回输入键处的值。

#### 合计

6. `Dictionary.Count` 会告诉您词典中有多少键值对。

7. `Dictionary.Keys` 会返回当前存储在词典中的键。

8. `Dictionary.Values` 会返回当前存储在词典中的值。

与词典建立整体关联数据是对使用索引和列表的旧方法的重大替代。
