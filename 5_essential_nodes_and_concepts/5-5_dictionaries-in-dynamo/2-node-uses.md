# 词典节点

Dynamo 2.0 带有各种词典节点供我们使用。这包括_创建、操作和查询_节点。

![]

#### 创建

`Dictionary.ByKeysValues`Dictionary.ByKeysValues 将使用提供的值和键创建词典。 _（条目数将是最短列表输入内容）_

#### 操作

2\. `Dictionary.Components` Dictionary.Components 将生成输入词典的组件。 _（这与创建节点相反。）_

3\. `Dictionary.RemoveKeys` Dictionary.RemoveKeys 将生成一个新的词典对象，其中输入键已删除。

4\. `Dictionary.SetValueAtKeys` Dictionary.SetValueAtKeys 将根据输入键和值生成新词典，以替换相应键上的当前值。

5\. `Dictionary.ValueAtKey` Dictionary.ValueAtKey 将返回输入键处的值。

#### Count

6\. `Dictionary.Count` Dictionary.Count 会告诉您词典中有多少键值对。

7\. `Dictionary.Keys` Dictionary.Keys 将返回当前存储在词典中的键。

8\. `Dictionary.Values` Dictionary.Values 将返回当前存储在词典中的值。

与词典建立整体关联数据是对使用索引和列表的旧方法的重大替代。
