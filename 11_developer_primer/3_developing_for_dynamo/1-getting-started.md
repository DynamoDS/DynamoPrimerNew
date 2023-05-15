# Pierwsze kroki

Przed rozpoczęciem prac nad rozwiązaniem należy opracować solidne podstawy dla nowego projektu. W społeczności programistów dodatku Dynamo dostępnych jest kilka szablonów projektów, które doskonale nadają się do rozpoczęcia pracy, ale jeszcze cenniejsza jest umiejętność rozpoczynania projektu od podstaw. Tworzenie projektu od podstaw pozwala lepiej zrozumieć proces opracowywania rozwiązania.

![Visual Studio](images/visual-studio.jpg)

#### Tworzenie projektu programu Visual Studio <a href="#creating-a-visual-studio-project" id="creating-a-visual-studio-project"></a>

Program Visual Studio to zaawansowane środowisko IDE, w którym można tworzyć projekty, dodawać odwołania, kompilować biblioteki `.dlls` i debugować. Podczas tworzenia nowego projektu program Visual Studio tworzy również rozwiązanie, czyli strukturę organizacyjną projektów. W jednym rozwiązaniu może istnieć wiele projektów i można je kompilować razem. Aby utworzyć węzeł ZeroTouch, należy rozpocząć nowy projekt programu Visual Studio, w którym zostanie napisana biblioteka klas języka C# i zostanie skompilowany plik `.dll`.

![Tworzenie nowego projektu w programu Visual Studio](images/vs-new-project-1.jpg)

![Konfigurowanie nowego projektu w programie Visual Studio](images/vs-new-project-2.jpg)

> Okno nowego projektu w programie Visual Studio
>
> 1. Zacznij od otwarcia programu Visual Studio i utworzenia nowego projektu: `File > New > Project`
> 2. Wybierz szablon projektu `Class Library`
> 3. Nadaj projektowi nazwę (w tym przypadku nazwaliśmy projekt MyCustomNode)
> 4. Ustaw ścieżkę pliku dla projektu. W tym przykładzie pozostawimy go w położeniu domyślnym
> 5. Wybierz przycisk `Ok`

Program Visual Studio automatycznie utworzy i otworzy plik w języku C#. Należy nadać mu odpowiednią nazwę, skonfigurować obszar roboczy i zastąpić kod domyślny tą metodą mnożenia.

```
 namespace MyCustomNode
 {
     public class SampleFunctions
     {
         public static double MultiplyByTwo(double inputNumber)
         {
             return inputNumber * 2.0;
         }
     }
 }
```

![Korzystanie z Eksploratora rozwiązań](images/vs-edit-class.jpg)

> 1. Otwórz Eksplorator rozwiązań i okna danych wyjściowych z poziomu obszaru `View`.
> 2. W Eksploratorze rozwiązań po prawej stronie zmień nazwę pliku `Class1.cs` na `SampleFunctions.cs`.
> 3. Dodaj powyższy kod dla funkcji mnożenia. Szczegóły dotyczące tego, jak dodatek Dynamo będzie odczytywał klasy w języku C#, zostaną omówione później.
> 4. Eksplorator rozwiązań: umożliwia dostęp do wszystkich elementów w projekcie.
> 5. Okno danych wyjściowych: będzie potrzebne później, aby sprawdzić, czy kompilacja się powiodła.

Następnym krokiem jest skompilowanie projektu, ale wcześniej należy sprawdzić kilka ustawień. Najpierw upewnij się, że jako platformę docelową wybrano `Any CPU` lub `x64` i że opcja `Prefer 32-bit` nie jest zaznaczona we właściwościach projektu.

![Ustawienia kompilacji programu Visual Studio](images/vs-build-settings.jpg)

> 1. Otwórz właściwości projektu, wybierając opcję `Project > "ProjectName" Properties`
> 2. Wybierz stronę `Build`
> 3. Wybierz z menu rozwijanego opcję `Any CPU` lub `x64`
> 4. Upewnij się, że opcja `Prefer 32-bit` nie jest zaznaczona

Teraz możemy skompilować projekt, aby utworzyć plik `.dll`. Aby to zrobić, wybierz opcję `Build Solution` z menu `Build` lub użyj skrótu `CTRL+SHIFT+B`.

![Kompilowanie rozwiązania](images/vs-build.jpg)

> 1. Wybierz opcję `Build > Build Solution`
> 2. Aby ustalić, czy projekt został pomyślnie skompilowany, należy sprawdzić okno danych wyjściowych

Jeśli projekt został pomyślnie skompilowany, w folderze projektu `bin` będzie znajdować się plik `.dll` o nazwie `MyCustomNode`. W tym przykładzie pozostawiliśmy ścieżkę pliku projektu jako domyślną w programie Visual Studio: `c:\users\username\documents\visual studio 2015\Projects`. Przyjrzyjmy się strukturze plików projektu.

![Struktura plików projektu](images/folder-structure.jpg)

> 1. Folder `bin` zawiera plik `.dll` skompilowany w programie Visual Studio.
> 2. Plik projektu programu Visual Studio.
> 3. Plik klasy.
> 4. Ponieważ jako konfigurację rozwiązania ustawiono `Debug`, plik `.dll` zostanie utworzony w folderze `bin\Debug`.

Teraz możemy otworzyć dodatek Dynamo i zaimportować plik `.dll`. Za pomocą funkcji dodawania przejdź do położenia projektu `bin` i wybierz plik `.dll`, który chcesz otworzyć.

![Otwieranie pliku dll projektu](images/dyn-import-dll.jpg)

> 1. Wybierz przycisk Add (Dodaj), aby zaimportować plik `.dll`
> 2. Przejdź do położenia projektu. Projekt znajduje się w domyślnej ścieżce pliku programu Visual Studio: `C:\Users\username\Documents\Visual Studio 2015\Projects\MyCustomNode`
> 3. Wybierz plik `MyCustomNode.dll` do zaimportowania
> 4. Kliknij przycisk `Open`, aby wczytać plik `.dll`

Jeśli w bibliotece o nazwie `MyCustomNode` została utworzona kategoria, plik .dll został zaimportowany pomyślnie. Dodatek Dynamo utworzył jednak dwa węzły z tego, co powinno być jednym węzłem. W następnej sekcji wyjaśnimy, dlaczego tak się dzieje i jak dodatek Dynamo odczytuje plik .dll.

![Węzły niestandardowe](images/dyn-customnode.jpg)

> 1. Węzeł MyCustomNode w bibliotece dodatku Dynamo. Kategoria biblioteki jest określana przez nazwę pliku `.dll`.
> 2. Węzeł SampleFunctions.MultiplyByTwo w obszarze rysunku.

#### Jak dodatek Dynamo odczytuje klasy i metody <a href="#how-dynamo-reads-classes-and-methods" id="how-dynamo-reads-classes-and-methods"></a>

Gdy dodatek Dynamo wczytuje plik .dll, wszystkie publiczne metody statyczne zostają uwidocznione jako węzły. Konstruktory, metody i właściwości zostają przekształcone w węzły odpowiednio Create (tworzenia), Action (operacji) i Query (zapytań). W tym przykładzie z mnożeniem metoda `MultiplyByTwo()` staje się węzłem operacji w dodatku Dynamo. Dzieje się tak, ponieważ węzeł został nazwany na podstawie metody i klasy.

![Węzeł SampleFunction.MultiplyByTwo na wykresie](images/multiplybytwo.png)

> 1. Nazwa danych wejściowych to `inputNumber` na podstawie nazwy parametru metody.
> 2. Nazwa danych wyjściowych to domyślnie `double`, ponieważ jest to zwracany typ danych.
> 3. Węzeł ma nazwę `SampleFunctions.MultiplyByTwo`, ponieważ takie są nazwy klasy i metody.

W powyższym przykładzie utworzono dodatkowy węzeł tworzenia, `SampleFunctions`: nie udostępniliśmy konstruktora bezpośrednio, więc został on utworzony automatycznie. Można tego uniknąć, tworząc pusty konstruktor prywatny w klasie `SampleFunctions`.

```
namespace MyCustomNode
{
    public class SampleFunctions
    {
        //The empty private constructor.
        //This will be not imported into Dynamo.
        private SampleFunctions() { }

        //The public multiplication method. 
        //This will be imported into Dynamo.
        public static double MultiplyByTwo(double inputNumber)
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Metoda zaimportowana jako węzeł tworzenia](images/private-constructor.jpg)

> 1. Dodatek Dynamo zaimportował metodę jako węzeł tworzenia

#### Dodawanie odwołań do pakietów NuGet dodatku Dynamo <a href="#adding-dynamo-nuget-package-references" id="adding-dynamo-nuget-package-references"></a>

Ten węzeł mnożenia jest bardzo prosty i nie są wymagane żadne odwołania do dodatku Dynamo. Aby uzyskać dostęp do dowolnej funkcji dodatku Dynamo na przykład w celu utworzenia geometrii, należy odwołać się do pakietów NuGet dodatku Dynamo.

* [ZeroTouchLibrary](https://www.nuget.org/packages/DynamoVisualProgramming.ZeroTouchLibrary/2.0.0-beta3026) — pakiet umożliwiający kompilowanie bibliotek węzłów Zero-Touch dla dodatku Dynamo, który zawiera następujące biblioteki: DynamoUnits.dll, ProtoGeometry.dll
* [WpfUILibrary](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) — pakiet umożliwiający kompilowanie bibliotek węzłów dla dodatku Dynamo z niestandardowym interfejsem użytkownika w pliku WPF, który zawiera następujące biblioteki: DynamoCoreWpf.dll, CoreNodeModels.dll, CoreNodeModelWpf.dll
* [DynamoServices](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) — biblioteka DynamoServices dla dodatku Dynamo
* [Core](https://www.nuget.org/packages/DynamoVisualProgramming.Core/2.0.0-beta3026) — infrastruktura testów jednostkowych i systemowych dla dodatku Dynamo, która zawiera następujące biblioteki: DSIronPython.dll, DynamoApplications.dll, DynamoCore.dll, DynamoInstallDetective.dll, DynamoShapeManager.dll, DynamoUtilities.dll, ProtoCore.dll, VMDataBridge .dll
* [Tests](https://www.nuget.org/packages/DynamoVisualProgramming.Tests/2.0.0-beta3026) — infrastruktura testów jednostkowych i systemowych dla dodatku Dynamo, która zawiera następujące biblioteki: DynamoCoreTests.dll, SystemTestServices.dll, TestServices.dll
* [DynamoCoreNodes](https://www.nuget.org/packages/DynamoVisualProgramming.DynamoCoreNodes/2.0.0-beta3026) — pakiet umożliwiający kompilowanie węzłów podstawowych dodatku Dynamo, który zawiera następujące biblioteki: Analysis.dll, GeometryColor.dll, DSCoreNodes.dll

Aby utworzyć odwołanie do tych pakietów w projekcie programu Visual Studio, należy pobrać pakiet z witryny NuGet za pomocą powyższych linków i ręcznie utworzyć odwołanie do plików .dll lub użyć Menedżera pakietów NuGet w programie Visual Studio. Najpierw omówimy sposób ich instalowania za pomocą menedżera NuGet w programie Visual Studio.

![Otwieranie Menedżera pakietów NuGet](images/vs-nuget-package-manager2.jpg)

> 1. Otwórz Menedżera pakietów NuGet, wybierając opcję `Tools > NuGet Package Manager > Manage NuGet Packages for Solution...`

To jest Menedżer pakietów NuGet. W tym oknie wyświetlane są pakiety zainstalowane dla projektu. Użytkownik może w nim też przeglądać inne pakiety. Jeśli zostanie wydana nowa wersja pakietu DynamoServices, w tym miejscu można zaktualizować pakiety lub przywrócić ich wcześniejszą wersję.

![Menedżer pakietów NuGet](images/vs-nuget-package-manager.jpg)

> 1. Wybierz opcję przeglądania i wyszukaj dodatek DynamoVisualProgramming, aby wywołać pakiety dodatku Dynamo.
> 2. Pakiety dodatku Dynamo. Wybranie jednego z nich spowoduje wyświetlenie bieżącej wersji i opisu zawartości.
> 3. Wybierz potrzebną wersję pakietu i kliknij przycisk instalowania. Spowoduje to zainstalowanie pakietu dla określonego projektu, w którym pracujesz. Używasz najnowszej stabilnej wersji dodatku Dynamo w wersji 1.3, więc wybierz odpowiednią dla niej wersję pakietu.

Aby ręcznie dodać pakiet pobrany z przeglądarki, otwórz Menedżera odnośników w Eksploratorze rozwiązań i wyszukaj pakiet.

![Menedżer odnośników](images/vs-manual-dynamo-package.jpg)

> 1. Kliknij prawym przyciskiem myszy opcję `References` i wybierz polecenie `Add Reference`.
> 2. Wybierz opcję `Browse`, aby przejść do lokalizacji pakietu.

Program Visual Studio jest teraz właściwie skonfigurowany i pomyślnie dodano plik `.dll` do dodatku Dynamo, więc mamy dobrze przygotowane środowisko do dalszej pracy. Jest to dopiero początek, dlatego postępuj zgodnie z instrukcjami, aby dowiedzieć się więcej na temat tworzenia węzła niestandardowego.
