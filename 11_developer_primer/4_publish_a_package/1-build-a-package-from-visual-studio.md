# 從 Visual Studio 建置套件

如果您正在開發要發佈為 Dynamo 套件的組合，可以將專案規劃為群組所有必要資產，並將這些資產放在與套件相容的目錄結構中。這可讓專案以套件方式進行快速測試，並模擬使用者的體驗。

#### 如何直接建置到套件資料夾 <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

有兩種方法可以在 Visual Studio 中建置套件：

* 透過「專案設定」對話方塊，使用 xcopy 或 Python 指令碼複製必要檔案，加入建置後事件
* 在 `.csproj` 檔案中使用「AfterBuild」建置目標，來建立檔案和目錄複製工作

「AfterBuild」是這些類型作業 (以及本指南中介紹的作業) 的偏好方法，因為它不依賴在建置電腦上可能無法使用的檔案複製。

#### 使用 AfterBuild 方法複製套件檔案 <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

在儲存庫中設定目錄結構，讓原始碼檔案與套件檔案分開。使用 CustomNodeModel 案例研究，將 Visual Studio 專案及所有關聯的檔案放到新的 `src` 資料夾中。您將會在此資料夾中儲存專案產生的所有套件。資料夾結構現在應如下所示：

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![移動專案檔](images/fe-proj-directory.jpg)

> 1. 將專案檔移到新的 `src` 資料夾

由於原始碼檔案位於不同的資料夾，因此請在 Visual Studio 的 `CustomNodeModel.csproj` 檔案加入 `AfterBuild` 目標。這會將必要的檔案複製到新的套件資料夾中。在文字編輯器 (我們使用 [Atom](https://atom.io)) 中開啟 `CustomNodeModel.csproj` 檔案，在 `</Project>` 結束標籤之前放置建置目標。此 AfterBuild 目標會將所有 .dll、.pbd、.xml 和 .config 檔案複製到新的 bin 資料夾中，並建立 dyf 和 extra 資料夾。

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

![放置 AfterBuild 目標](images/atom-afterbuild.jpg)

> 我們需要確保目標已加到 `CustomNodeModel.csproj` 檔案 (而不是另一個專案檔) 中，而且專案沒有任何既有的「建置後」設定。
>
> 1. 在 `</Project>` 結束標籤前放置 AfterBuild 目標。

`<ItemGroup>` 區段中定義了許多變數來表示特定檔案類型。例如，`Dll` 變數表示輸出目錄中其副檔名為 `.dll` 的所有檔案。

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

`Copy` 工作是將所有 `.dll` 檔案複製到目錄，具體來說是要建置到的套件資料夾。

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Dynamo 套件通常會有 `dyf` 和 `extra` 資料夾，分別給 Dynamo 自訂節點和其他資產 (例如影像) 使用。若要建立這些資料夾，我們需要使用 `MakeDir` 工作。如果資料夾不存在，此工作會建立一個。您可以手動將檔案加到此資料夾中。

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

如果您建置專案，現在專案資料夾在先前建立的 `src` 資料夾旁邊應該會有一個 `packages` 資料夾。`packages` 目錄內有一個資料夾，其中包含套件所需的所有內容。我們也需要將 `pkg.json` 檔案複製到套件資料夾，讓 Dynamo 知道要載入套件。

![複製檔案](images/fe-proj-directory-package.jpg)

> 1. AfterBuild 目標建立的新套件資料夾
> 2. 專案既有的 src 資料夾
> 3. 從 AfterBuild 目標建立的 `dyf` 和 `extra` 資料夾
> 4. 手動複製 `pkg.json` 檔案。

現在，您可以使用 Dynamo 的 Package Manager 發佈套件，或直接將套件複製到 Dynamo 的套件目錄：`<user>\AppData\Roaming\Dynamo\1.3\packages`。
