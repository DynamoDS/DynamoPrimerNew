# Kompilowanie pakietu z programu Visual Studio 

Jeśli opracowujesz zespoły, które mają być publikowane jako pakiet dla dodatku Dynamo, projekt można skonfigurować tak, aby zgrupować wszystkie niezbędne zasoby i umieścić je w strukturze katalogów zgodnej z pakietem. Dzięki temu projekt zostanie szybko przetestowany jako pakiet i będzie mógł symulować środowisko użytkownika.

#### Jak kompilować bezpośrednio w folderze pakietu <a href="#how-to-build-directly-to-the-package-folder" id="how-to-build-directly-to-the-package-folder"></a>

Istnieją dwie metody kompilowania pakietu w programie Visual Studio:

* Dodanie zdarzeń po kompilacji za pomocą okna dialogowego ustawień projektu, w których niezbędne pliki są kopiowane za pomocą polecenia xcopy lub skryptów w języku Python
* Utworzenie zadań kopiowania plików i katalogów za pomocą obiektu docelowego kompilacji „AfterBuild” w pliku `.csproj`

Opcja „AfterBuild” jest metodą preferowaną w przypadku tych typów operacji (i to ją opisano w tym podręczniku), ponieważ nie jest oparta na kopiowaniu plików, które może nie być dostępne na komputerze kompilacji.

#### Kopiowanie plików pakietu metodą AfterBuild <a href="#copy-package-files-with-the-afterbuild-method" id="copy-package-files-with-the-afterbuild-method"></a>

Skonfiguruj strukturę katalogów w repozytorium, tak aby pliki źródłowe były oddzielone od plików pakietu. Używając analizy przypadku CustomNodeModel, umieść projekt programu Visual Studio i wszystkie skojarzone pliki w nowym folderze `src`. Wszystkie pakiety wygenerowane przez projekt będą zapisywane w tym folderze. Struktura folderów powinna teraz wyglądać tak:

```
CustomNodeModel
> src
  > CustomNodeModel
  > CustomNodeModelFunction
  > packages
  > CustomNodeModel.sln
```

![Przenoszenie plików projektu](images/fe-proj-directory.jpg)

> 1. Przenieś pliki projektu do nowego folderu `src`

Pliki źródłowe znajdują się już w oddzielnym folderze. Dodaj obiekt docelowy `AfterBuild` do pliku `CustomNodeModel.csproj` w programie Visual Studio. Powinno to spowodować skopiowanie potrzebnych plików do nowego folderu pakietu. Otwórz plik `CustomNodeModel.csproj` w edytorze tekstu (użyliśmy edytora [Atom](https://atom.io)) i umieść obiekt docelowy kompilacji przed tagiem zamykającym `</Project>`. Ten obiekt docelowy AfterBuild spowoduje skopiowanie wszystkich plików .dll, .pbd, .xml i .config do nowego folderu bin oraz utworzenie folderów dyf i extra.

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

![Umieszczanie obiektu docelowego AfterBuild](images/atom-afterbuild.jpg)

> Musimy upewnić się, że obiekt docelowy dodano do pliku `CustomNodeModel.csproj` (a nie innego pliku projektu) i że projekt nie ma żadnych istniejących ustawień po kompilacji (Post-Build).
>
> 1. Umieść element AfterBuild przed tagiem zamykającym `</Project>`.

W sekcji `<ItemGroup>` zdefiniowano szereg zmiennych reprezentujących określone typy plików. Na przykład zmienna `Dll` reprezentuje wszystkie pliki w katalogu wyjściowym mające rozszerzenie `.dll`.

```
<ItemGroup>
  <Dlls Include="$(OutDir)*.dll" />
</ItemGroup>
```

Zadanie `Copy` polega na skopiowaniu wszystkich plików `.dll` do katalogu, a konkretnie do folderu kompilowanego pakietu.

```
<Copy SourceFiles="@(Dlls)" DestinationFolder="$(SolutionDir)..\packages\CustomNodeModel\bin\" />
```

Pakiety dodatku Dynamo zazwyczaj zawierają foldery `dyf` i `extra` na węzły niestandardowe dodatku Dynamo i inne zasoby, takie jak obrazy. Aby utworzyć te foldery, należy użyć zadania `MakeDir`. To zadanie utworzy folder, jeśli on nie istnieje. Pliki można dodać do tego folderu ręcznie.

```
<MakeDir Directories="$(SolutionDir)..\packages\CustomNodeModel\extra" />
```

W przypadku kompilowania projektu folder projektu powinien teraz zawierać folder `packages` obok utworzonego wcześniej folderu `src`. W katalogu `packages` znajduje się folder zawierający wszystkie elementy potrzebne do utworzenia pakietu. Musimy również skopiować plik `pkg.json` do folderu pakietu, aby dodatek Dynamo wiedział, że ma wczytać pakiet.

![Kopiowanie plików](images/fe-proj-directory-package.jpg)

> 1. Nowy folder packages utworzony przez obiekt docelowy AfterBuild
> 2. Istniejący folder src z projektem
> 3. Foldery `dyf` i `extra` utworzone z obiektu docelowego AfterBuild
> 4. Ręcznie skopiuj plik `pkg.json`.

Teraz można opublikować pakiet za pomocą Menedżera pakietów dodatku Dynamo lub bezpośrednio skopiować go do katalogu pakietu dodatku Dynamo: `<user>\AppData\Roaming\Dynamo\1.3\packages`.
