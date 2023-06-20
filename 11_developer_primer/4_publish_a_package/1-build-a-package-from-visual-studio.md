# Vytvoření balíčku z aplikace Visual Studio 

Pokud vyvíjíte sestavy, které mají být publikovány jako balíček aplikace Dynamo, lze projekt nakonfigurovat tak, aby seskupil všechny potřebné komponenty a uložil je do adresářové struktury kompatibilní s balíčkem. To umožní rychlé testování projektu jako balíčku a simulaci uživatelského prostředí.

#### Jak provést sestavení přímo do složky balíčku <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

V aplikaci Visual Studio lze balíček sestavit dvěma způsoby:

* Můžete přidat události po sestavení prostřednictvím dialogu Nastavení projektu, které používají příkaz xcopy nebo skripty jazyka Python ke kopírování potřebných souborů.
* Pomocí cíle sestavení AfterBuild v souboru `.csproj` můžete vytvořit úlohy kopírování souborů a adresářů.

Metoda AfterBuild je upřednostňovanou metodou pro tyto typy operací (a metodou popsanou v této příručce), protože se nespoléhá na kopírování souborů, které nemusí být v počítači, ve kterém sestavení probíhá, k dispozici.

#### Kopírování souborů balíčku pomocí metody AfterBuild <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Nastavte strukturu adresáře v úložišti tak, aby zdrojové soubory byly odděleny od souborů balíčku. Při práci s případovou studií CustomNodeModel umístěte projekt aplikace Visual Studio a všechny související soubory do nové složky `src`. Všechny balíčky vygenerované projektem budou uloženy do této složky. Struktura složek by nyní měla vypadat takto:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Přesouvání souborů projektu](images/fe-proj-directory.jpg)

> 1. Přesuňte soubory projektu do nové složky `src`.

Nyní, když jsou zdrojové soubory v samostatné složce, přidejte v aplikaci Visual Studio do souboru `CustomNodeModel.csproj` cíl `AfterBuild`. Tím se potřebné soubory zkopírují do nové složky balíčku. Otevřete soubor `CustomNodeModel.csproj` v textovém editoru (použili jsme [Atom](https://atom.io)) a umístěte cíl sestavení před koncový štítek `</Project>`. Tento cíl AfterBuild zkopíruje všechny soubory DLL, PBD, XML a CONFIG do nové složky bin a vytvoří složky dyf a extra.

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

![Umístění cíle AfterBuild](images/atom-afterbuild.jpg)

> Je třeba zajistit, aby byl cíl přidán do souboru `CustomNodeModel.csproj` (nikoli do jiného souboru projektu) a aby projekt neměl žádná existující nastavení po sestavení.
>
> 1. Umístěte cíl AfterBuild před koncový štítek `</Project>`.

V části `<ItemGroup>` je definováno množství proměnných, které představují konkrétní typy souborů. Například proměnná `Dll` představuje všechny soubory ve výstupním adresáři, jejichž přípona je `.dll`.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

Úloha `Copy` zkopíruje všechny soubory `.dll` do adresáře, konkrétně do složky balíčku, do které provádíme sestavení.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Balíčky aplikace Dynamo obvykle obsahují složky `dyf` a `extra` pro vlastní uzly aplikace Dynamo a další komponenty, například obrázky. Chcete-li vytvořit tyto složky, je nutné použít úlohu `MakeDir`. Tato úloha vytvoří příslušnou složku, pokud neexistuje. Soubory můžete do této složky přidat ručně.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

Pokud projekt sestavíte, měla by se nyní ve složce projektu vedle dříve vytvořené složky `src` nacházet i složka `packages`. V adresáři `packages` je složka obsahující vše, co je pro balíček potřeba. Do složky balíčku je také nutné zkopírovat soubor `pkg.json`, aby aplikace Dynamo věděla, že má balíček načíst.

![Kopírování souborů](images/fe-proj-directory-package.jpg)

> 1. Nová složka packages vytvořená cílem AfterBuild.
> 2. Existující složka src s projektem.
> 3. Složky `dyf` a `extra` vytvořené z cíle AfterBuild.
> 4. Ručně zkopírujte soubor `pkg.json`.

Nyní můžete balíček publikovat pomocí nástroje Package Manager aplikace Dynamo nebo jej přímo zkopírovat do adresáře balíčku aplikace Dynamo: `<user>\AppData\Roaming\Dynamo\1.3\packages`.
