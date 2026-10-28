# Autodesk Assistant で現在対応していない機能

### パッケージを更新する

Autodesk Assistant は、Dynamo へのパッケージのダウンロードおよびインストールには現在対応していません

![](../../.gitbook/assets/Aa_Update.jpg)

上の図に示すように、Autodesk Assistant は、Dynamo に関連付けられている外部パッケージの修正、インストール、アンインストールには対応していません。Autodesk Assistant は、お客様が自身でパッケージをインストールできるように、手順を案内することはできます。


### ファイルを直接読み書きする

Autodesk Assistant は Dynamo のグラフ ファイルを操作できます。グラフを開いたり、キャンバスに挿入したり、実行したり、作業内容を保存することができます。ハード ディスク上の他のファイルを単独で読み書きすることはできません。

次の例では、キャンバス上にいくつかのジオメトリが作成されています。Autodesk Assistant は、ジオメトリを構成する点を取得し、JSON として書き出すように指示されています。Autodesk Assistant は、この JSON を直接生成することはできませんが、これを実現する Dynamo スクリプトを作成することは可能であると示唆しています。

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

覚えておくべきなのは、この提案のパターンです。グラフ外部のものへのアクセスは、Assistant 自身ではなく、ノードを介してアクセスします。

### グラフを使用せずに Revit モデルを変更する

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Dynamo の Autodesk Assistant は、Revit モデルに直接接続されていません。Revit で Assistant に依頼する方法でモデルを編集するよう指示しても、編集できないと返答されます。

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

それでも、Revit モデルを変更することはできます。ユーザが自身で操作する時と同じように、Revit ノードを配置してそれらを接続し、そのグラフを実行するとモデルが変更されます。

![](../../.gitbook/assets/Aa_viewScript.jpg)

このような方法で動作するため、次の 2 つの特徴があります。

- **グラフを実行するまでは何も変更されません。**まず、グラフ内容を確認し、値を調整することができます。モデルが変更されるのは、ユーザがグラフを実行した時、あるいはユーザの要求に従って Assistant が実行した時のみです。
- **変更は繰り返し再現可能です。**グラフはキャンバス上に残ります。グラフを保存しておけば、別のモデルで同じタスクを再度実行したり、チームと共有することができます。

グラフを使わずに、単一の編集を直接行う場合は、Revit の Autodesk Assistant を使用してください。
