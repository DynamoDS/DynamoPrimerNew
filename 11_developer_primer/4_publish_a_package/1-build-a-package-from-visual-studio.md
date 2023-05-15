# Visual Studio からパッケージをビルドする

Dynamo 用のパッケージとしてパブリッシュするアセンブリを開発している場合は、必要なすべてのアセットをグループ化し、パッケージと互換性のあるフォルダ構造に配置するように、プロジェクトを設定できます。これにより、プロジェクトをパッケージとしてすばやくテストし、ユーザの操作をシミュレーションすることができます。

#### パッケージ フォルダに直接ビルドする方法 <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Visual Studio でパッケージをビルドするには、次の 2 つの方法があります。

* xcopy または Python スクリプトを使用して必要なファイルをコピーするビルド後のイベントを、プロジェクト設定ダイアログで追加します。
* `.csproj` ファイルで「AfterBuild」ビルド ターゲットを使用して、ファイルおよびフォルダのコピー タスクを作成します。

「AfterBuild」は、ビルド マシンでは使用できない可能性のあるファイル コピーに依存しないため、このような操作に適した方法であり、このガイドではこの方法について説明しています。

#### AfterBuild メソッドを使用してパッケージ ファイルをコピーする <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

ソース ファイルがパッケージ ファイルとは別のファイルになるように、リポジトリ内のフォルダ構造を設定します。CustomNodeModel ケース スタディで作業しているため、Visual Studio プロジェクトと関連するすべてのファイルを新しい `src` フォルダに配置します。プロジェクトによって生成されるすべてのパッケージをこのフォルダに保存することになります。フォルダ構造は次のようになります。

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![プロジェクト ファイルを移動する](images/fe-proj-directory.jpg)

> 1. プロジェクト ファイルを新しい `src` フォルダに移動します。

ソース ファイルは別のフォルダにあるため、Visual Studio で `AfterBuild` ターゲットを `CustomNodeModel.csproj` ファイルに追加します。これにより、必要なファイルが新しいパッケージ フォルダにコピーされます。`CustomNodeModel.csproj` ファイルをテキスト エディタ(この例では [Atom](https://atom.io) を使用しました)で開き、ビルド ターゲットを終了タグ `</Project>` の前に配置します。この AfterBuild ターゲットは、.dll、.pbd、.xml、および .config ファイルをすべて新しい bin フォルダにコピーし、dyf フォルダと extra フォルダを作成します。

```
  <Target Name="AfterBuild">
    <ItemGroup>
      <Dlls Include="$(OutDir)*.dll" />
      <Pdbs Include="$(OutDir)*.pdb" />
      <Xmls Include="$(OutDir)*.xml" />
      <Configs Include="$(OutDir)*.config" />
    </ItemGroup>
    <Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Pdbs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Xmls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <Copy SourceFiles="@(Configs)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\dyf" />
    <MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
  </Target>
```

![AfterBuild ターゲットを配置する](images/atom-afterbuild.jpg)

> ターゲットが(他のプロジェクト ファイルではなく) `CustomNodeModel.csproj` ファイルに追加されていること、およびプロジェクトに既存のビルド後の設定がないことを確認する必要があります。
>
> 1. AfterBuild ターゲットを終了タグ `</Project>` の前に配置します。

`<ItemGroup>` セクションでは、特定のファイル タイプを表す変数がいくつか定義されています。たとえば、変数 `Dll` は、出力フォルダ内にある、拡張子が `.dll` のすべてのファイルを表します。

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

`Copy` タスクは、すべての `.dll` ファイルをフォルダにコピーします。具体的にはビルド先のパッケージ フォルダにコピーします。

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Dynamo パッケージには通常、Dynamo カスタム ノード用の `dyf` フォルダ、およびイメージなどのその他のアセット用の `extra` フォルダがあります。これらのフォルダを作成するには、`MakeDir` タスクを使用する必要があります。このタスクは、フォルダが存在しない場合にそのフォルダを作成します。このフォルダにファイルを手動で追加できます。

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

プロジェクトをビルドすると、プロジェクト フォルダには、以前に作成された `src` フォルダの横に `packages` フォルダが作成されます。`packages` フォルダ内には、パッケージに必要なものがすべて格納されているフォルダがあります。また、`pkg.json` ファイルをパッケージ フォルダにコピーして、Dynamo でパッケージをロードできるようにする必要があります。

![ファイルをコピーする](images/fe-proj-directory-package.jpg)

> 1. AfterBuild ターゲットが作成した新しいパッケージ フォルダ
> 2. プロジェクトの既存の src フォルダ
> 3. AfterBuild ターゲットによって作成された `dyf` および `extra` フォルダ
> 4. `pkg.json` ファイルを手動でコピーします。

これで、Dynamo のパッケージ マネージャを使用してパッケージをパブリッシュしたり、Dynamo のパッケージ フォルダ `<user>\AppData\Roaming\Dynamo\1.3\packages` に直接コピーできるようになりました。
