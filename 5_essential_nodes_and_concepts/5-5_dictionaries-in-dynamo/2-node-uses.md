# [Dictionary]カテゴリのノード

Dynamo 2.0 では[Dictionary]カテゴリのさまざまなノードが公開されており、使用することができます。_作成、アクション、クエリー_ のノードがあります。

![](../images/5-5/2/dictionarynodes-nodes.jpg)

#### Create

1. `Dictionary.ByKeysValues` は、指定した値とキーでディクショナリを作成します。_(項目数は、最短のリストの入力によります)_

#### Action

2. `Dictionary.Components` は、入力ディクショナリのコンポーネントを作成します。_(これはノードの作成と逆の操作です)_。

3. `Dictionary.RemoveKeys` は、入力キーが削除された新しいディクショナリのオブジェクトを作成します。

4. `Dictionary.SetValueAtKeys` は、入力キーと値に基づいて新しいディクショナリを作成し、対応するキーで現在の値を置き換えます。

5. `Dictionary.ValueAtKey` は、入力キーに対する値を返します。

#### クエリー

6. `Dictionary.Count` は、ディクショナリに含まれるキーと値のペア数を示します。

7. `Dictionary.Keys` は、現在ディクショナリに格納されているキーを返します。

8. `Dictionary.Values` は、現在ディクショナリに格納されている値を返します。

ディクショナリに関するあらゆるデータは、インデックスとリストを操作する従来の方法に代わる優れた手段です。
