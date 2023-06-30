# Visual Studio에서 패키지 빌드하기 

Dynamo용 패키지로 게시할 어셈블리를 개발하는 경우 필요한 모든 자산을 그룹화하여 패키지와 호환되는 디렉토리 구조에 배치하도록 프로젝트를 구성할 수 있습니다. 이렇게 하면 프로젝트를 패키지로 빠르게 테스트하고 사용자의 경험을 시뮬레이션할 수 있습니다.

#### 패키지 폴더에 직접 빌드하는 방법 <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Visual Studio에서 패키지를 빌드하는 방법에는 두 가지가 있습니다.

* xcopy 또는 Python 스크립트를 사용하는 프로젝트 설정 대화상자를 통해 빌드 후 이벤트를 추가하여 필요한 파일을 복사합니다.
* `.csproj` 파일에서 "AfterBuild" 빌드 대상을 사용하여 파일 및 디렉토리 복사 작업을 생성합니다.

"AfterBuild"는 빌드 컴퓨터에서 사용할 수 없는 파일 복사에 의존하지 않으므로 이러한 유형의 작업(및 이 안내서에서 다루는 작업)에 선호되는 방법입니다.

#### AfterBuild 방법을 사용하여 패키지 파일 복사하기 <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

소스 파일이 패키지 파일과 분리되도록 리포지토리의 디렉토리 구조를 설정합니다. CustomNodeModel 사례 연구를 사용하여 Visual Studio 프로젝트 및 연관된 모든 파일을 새 `src` 폴더에 배치합니다. 프로젝트에서 생성된 모든 패키지를 이 폴더에 저장할 예정입니다. 그러면 폴더 구조는 다음과 같게 됩니다.

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![프로젝트 파일 이동하기](images/fe-proj-directory.jpg)

> 1. 프로젝트 파일을 새 `src` 폴더로 이동합니다.

이제 소스 파일이 별도의 폴더에 있으므로 Visual Studio의 `CustomNodeModel.csproj` 파일에 `AfterBuild` 대상을 추가합니다. 이렇게 하면 필요한 파일이 새 패키지 폴더에 복사됩니다. 텍스트 편집기([Atom](https://atom.io))에서 `CustomNodeModel.csproj` 파일을 열고, 닫는 `</Project>` 태그 앞에 빌드 대상을 넣습니다. 이 AfterBuild 대상은 모든 .dll, .pbd, .xml 및 .config 파일을 새 bin 폴더에 복사하고 dyf 및 추가 폴더를 생성합니다.

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

![AfterBuild 대상 넣기](images/atom-afterbuild.jpg)

> 대상이 `CustomNodeModel.csproj` 파일(다른 프로젝트 파일 아님)에 추가되었고 프로젝트에 기존 빌드 후 설정이 없는지 확인해야 합니다.
>
> 1. 끝 `</Project>` 태그 앞에 AfterBuild 대상을 넣습니다.

`<ItemGroup>` 섹션에는 특정 파일 유형을 나타내는 여러 변수가 정의되어 있습니다. 예를 들어, `Dll` 변수는 확장자가 `.dll`인 출력 디렉토리의 모든 파일을 나타냅니다.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

`Copy` 작업은 모든 `.dll` 파일을 디렉토리, 특히 빌드 중인 패키지 폴더에 복사하는 것입니다.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Dynamo 패키지에는 일반적으로 Dynamo 사용자 지정 노드 및 이미지와 같은 기타 자산을 위한 `dyf` 및 `extra` 폴더가 있습니다. 이러한 폴더를 생성하려면 `MakeDir` 작업을 사용해야 합니다. 이 작업은 폴더가 없는 경우 폴더를 생성합니다. 이 폴더에 파일을 수동으로 추가할 수 있습니다.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

프로젝트를 빌드하면 이제 프로젝트 폴더에 이전에 생성한 `src` 폴더와 나란히 `packages` 폴더가 생성됩니다. `packages` 디렉토리 내에는 패키지에 필요한 모든 항목이 포함된 폴더가 있습니다. 또한 `pkg.json` 파일을 패키지 폴더에 복사하여 Dynamo가 패키지를 로드하도록 해야 합니다.

![파일 복사하기](images/fe-proj-directory-package.jpg)

> 1. AfterBuild 대상이 생성한 새 패키지 폴더
> 2. 프로젝트가 있는 기존 src 폴더
> 3. AfterBuild 대상에서 생성된 `dyf` 및 `extra` 폴더
> 4. `pkg.json` 파일을 수동으로 복사합니다.

이제 Dynamo의 패키지 관리자를 사용하여 패키지를 게시하거나 Dynamo의 패키지 디렉토리(`<user>\AppData\Roaming\Dynamo\1.3\packages`)에 직접 복사할 수 있습니다.
