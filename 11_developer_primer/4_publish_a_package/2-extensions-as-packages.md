# パッケージとしての拡張機能

### パッケージとしての拡張機能 <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### 概要 <a href="#overview" id="overview"></a>

Dynamo 拡張機能は、通常の Dynamo ノード ライブラリと同様に、パッケージ マネージャに配置できます。インストールされたパッケージにビューの拡張機能が含まれている場合は、Dynamo をロードするとその拡張機能が実行時にロードされます。Dynamo コンソールで、拡張機能が正しくロードされていることを確認できます。

### パッケージ構造 <a href="#package-structure" id="package-structure"></a>

拡張パッケージの構造は、通常のパッケージと同じで次のようになっています。

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

既に拡張機能をビルドしたと仮定すると、(少なくとも) .NET アセンブリとマニフェスト ファイルがあります。アセンブリには、`IViewExtension` または `IExtension` を実装するクラスが含まれている必要があります。マニフェスト .XML ファイルによって、拡張機能を起動するために Dynamo でどのクラスがインスタンス化されるかが決まります。Package Manager が拡張機能を正しく見つけるために、マニフェスト ファイルはアセンブリの場所と名前に正確に対応している必要があります。

アセンブリ ファイルを `bin` フォルダに、マニフェスト ファイルを `extra` フォルダに配置します。追加のアセットをこのフォルダに配置することもできます。

マニフェスト .XML ファイルの例:

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### アップロードする <a href="#uploading" id="uploading"></a>

上記のようにサブフォルダを含むフォルダが作成されたら、パッケージ マネージャにプッシュ(アップロード)できます。現在、DynamoSandbox からパッケージをパブリッシュできないため、注意してください。つまり、Dynamo Revit を使用する必要があります。Dynamo Revit 内で [パッケージ] => [新しいパッケージをパブリッシュ]にナビゲートします。これにより、パッケージを関連付ける Autodesk Account にログインするよう求めるメッセージが表示されます。

ログインすると、パッケージをパブリッシュする通常のウィンドウが開くため、パッケージや拡張機能に関するすべての必須フィールドに入力します。**非常に重要な**追加の手順として、どのアセンブリ ファイルもノード ライブラリとしてマークされていないことを確認する必要があります。この確認を行うには、読み込んだファイル(上で作成したパッケージ フォルダ)を右クリックします。コンテキスト メニューが表示され、このオプションをオン(またはオフ)にできます。すべての拡張機能アセンブリをオフにする必要があります。

![パッケージをパブリッシュする](images/ViewExtension_Search.png)

パブリッシュして公開する前に、常にローカルにパブリッシュして、すべてが正しく機能していることを確認する必要があります。この確認が完了したら、パブリッシュを選択して公開できます。

### プルする <a href="#pulling" id="pulling"></a>

パッケージが正常にアップロードされたことを確認するには、パブリッシュの手順で指定した名前とキーワードを使用してパッケージを検索します。最後に注意する点として、同じ拡張機能を機能させる前に Dynamo を再起動する必要があります。通常、このような拡張機能では、Dynamo の起動時にパラメータの指定が必要です。

![パッケージを検索する](images/ViewExtension_Search.jpg)
