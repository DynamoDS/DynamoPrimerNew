# Kompilowanie dodatku DynamoRevit ze źródła

Pliki źródłowe dodatku DynamoRevit są również przechowywane w witrynie DynamoDS w serwisie GitHub dla programistów, którzy mogą współtworzyć dodatek i kompilować wersje beta. Kompilowanie dodatku DynamoRevit ze źródła zazwyczaj przebiega tak samo jak w przypadku dodatku Dynamo, z wyjątkiem kilku ważnych szczegółów:

* Dodatek DynamoRevit odwołuje się do zespołów dodatku Dynamo, dlatego należy go utworzyć za pomocą zgodnych pakietów NuGet. Na przykład dodatek DynamoRevit 2.x nie zostanie wczytany do dodatku Dynamo 1.3.
* Dodatek DynamoRevit jest powiązany z konkretną wersją programu Revit, na przykład: gałąź dodatku DynamoRevit 2018 powinna być uruchamiana w programie Revit 2018.

W tym podręczniku używamy następujących składników:

* Revit 2023
* Najnowsza kompilacja dodatku DynamoRevit w gałęzi `Revit2023`
* Najnowsza kompilacja dodatku Dynamo

Aby zapewnić pomyślną kompilację, sklonujemy i skompilujemy repozytoria dodatku Dynamo i dodatku DynamoRevit, które będą używane w tym przewodniku.

_Uwaga: ręczne skompilowanie dodatku Dynamo przed rozpoczęciem kompilowania dodatku DynamoRevit jest wymagane tylko w przypadku kompilowania dodatków Dynamo 1.x i DynamoRevit 1.x — nowsze wersje repozytorium DynamoRevit używają Menedżera pakietów NuGet do obsługi zależności dodatku Dynamo wymaganych do skompilowania. Mimo że skompilowanie dodatku DynamoRevit 2.x nie wymaga ręcznego ściągnięcia (pull) dodatku Dynamo, nadal są potrzebne podstawowe biblioteki (`dlls`) w innym miejscu, aby faktycznie uruchomić składnik `addin` dodatku DynamoRevit — warto więc jednak ściągnąć i skompilować dodatek Dynamo. Zobacz więcej poniżej:_ [_Kompilowanie repozytorium za pomocą programu Visual Studio_](#building-the-repository-using-Visual-Studio)

#### Znajdowanie repozytorium DynamoRevit w serwisie Github <a href="#locating-the-dynamorevit-repository-on-github" id="locating-the-dynamorevit-repository-on-github"></a>

Kod projektu DynamoRevit znajduje się w serwisie Github w repozytorium oddzielnym od podstawowego kodu źródłowego dodatku Dynamo. To repozytorium zawiera pliki źródłowe dla węzłów charakterystycznych dla programu Revit i dodatek programu Revit, który wczytuje dodatek Dynamo. Kompilacje dodatku DynamoRevit dla różnych wersji programu Revit (na przykład 2016, 2017 lub 2018) są zorganizowane jako gałęzie w repozytorium.

Źródło dodatku DynamoRevit znajduje się tutaj: [https://github.com/DynamoDS/DynamoRevit](https://github.com/DynamoDS/DynamoRevit)

![DynamoRevit on GitHub](images/github-dynamorevit.jpg)

> 1. Klonowanie lub pobieranie repozytorium
> 2. Gałęzie dodatku DynamoRevit odnoszą się do wersji programu Revit

#### Klonowanie repozytorium przy użyciu narzędzia git <a href="#cloning-the-repository-using-git" id="cloning-the-repository-using-git"></a>

W procesie podobnym do ściągania (pull) repozytorium dodatku Dynamo użyjemy polecenia git clone, aby sklonować dodatek DynamoRevit i określić gałąź, która odpowiada używanej wersji programu Revit. Aby rozpocząć, otworzymy interfejs wiersza polecenia i ustawimy jako bieżący katalog, do którego mają zostać sklonowane pliki.

Polecenie `cd C:\Users\username\Documents\GitHub` zmienia katalog bieżący

> Zastąp ciąg `username` swoją nazwą użytkownika

![Używanie interfejsu wiersza polecenia](images/cli-cd-revit.jpg)

Teraz możemy sklonować repozytorium do tego katalogu. Mimo że musimy określić gałąź repozytorium, po sklonowaniu możemy przejść do tej gałęzi.

Polecenie `git clone https://github.com/DynamoDS/DynamoRevit.git` klonuje repozytorium ze zdalnego adresu URL i domyślnie przełącza do głównej gałęzi.

![Interfejs wiersza polecenia po sklonowaniu repozytorium](images/cli-clone-revit.jpg)

Po zakończeniu klonowania repozytorium zmień katalog bieżący na folder repozytorium i przełącz na gałąź odpowiadającą zainstalowanej wersji programu Revit. W tym przykładzie używamy programu Revit RC2.13.1_Revit2023. Wszystkie gałęzie zdalne można wyświetlić na stronie serwisu Github w menu rozwijanym Branch (Gałąź).

Polecenie `cd C:\Users\username\Documents\GitHub\DynamoRevit` zmienia katalog na DynamoRevit.\
Polecenie `git checkout RC2.13.1_Revit2023` ustawia bieżącą gałąź `RC2.13.1_Revit2023`.\
Polecenie `git branch` sprawdza, w której gałęzi pracujemy, i wyświetla inne istniejące lokalnie.

![Katalog po przełączeniu do gałęzi](images/cli-branch-revit.jpg)

> Gałąź z gwiazdką jest obecnie wyrejestrowana. Wyświetlana jest gałąź `Revit2018`, ponieważ wcześniej ją wyrejestrowano, więc istnieje ona lokalnie.

Ważne jest wybranie właściwej gałęzi repozytorium, aby zapewnić, że podczas kompilowania projektu w programie Visual Studio będzie on odwoływał się do zespołów we właściwej wersji katalogu instalacyjnego programu Revit, w szczególności do plików `RevitAPI.dll` i `RevitAPIUI.dll`.

#### Kompilowanie repozytorium za pomocą programu Visual Studio <a href="#building-dynamo-revit" id="building-dynamo-revit"></a>

Przed skompilowaniem repozytorium musimy przywrócić pakiety NuGet za pomocą pliku `restorepackages.bat` znajdującego się w folderze `src`. Ten plik bat wykorzystuje Menedżera pakietów [nuget](https://www.nuget.org) do ściągnięcia (pull) skompilowanych plików binarnych podstawowych elementów dodatku Dynamo wymaganych przez dodatek DynamoRevit. Można również zdecydować się na skompilowanie ich ręcznie, ale tylko w przypadku wprowadzania zmian w dodatku DynamoRevit, a nie w elementach podstawowych dodatku Dynamo. Dzięki temu można szybciej rozpocząć pracę. Ten plik należy uruchomić z uprawnieniami administratora.

![Uruchamianie z uprawnieniami administratora](images/fe-restorepackages.jpg)

> 1. Kliknij prawym przyciskiem myszy plik `restorepackages.bat` i wybierz polecenie `Run as administrator`

Jeśli pakiety zostaną pomyślnie przywrócone, do folderu `src` zostanie dodany folder `packages` z najnowszymi pakietami beta NuGet.

![Najnowsze pakiety NuGet dodatku Dynamo w wersji beta](images/fe-packages.jpg)

> 1. Najnowsze pakiety NuGet dodatku Dynamo w wersji beta

Po przywróceniu pakietów otwórz plik rozwiązania programu Visual Studio `DynamoRevit.All.sln` w folderze `src` i skompiluj rozwiązanie. Kompilacja może początkowo mieć problemy ze znalezieniem pliku `AssemblySharedInfo.cs`. W takim przypadku ponowne uruchomienie kompilacji rozwiąże ten problem.

![Kompilowanie rozwiązania](images/vs-build-dynamorevit.jpg)

> 1. Wybierz opcję `Build > Build Solution`
> 2. Sprawdź, czy kompilacja została zakończona pomyślnie w oknie danych wyjściowych. Komunikat powinien wyglądać tak: `===== Build: 13 succeeded, 0 failed, 0 up-to-date, 0 skipped =====`.

#### Uruchamianie kompilacji lokalnej dodatku DynamoRevit w programie Revit <a href="#running-a-local-build-of-dynamorevit-in-revit" id="running-a-local-build-of-dynamorevit-in-revit"></a>

Program Revit wymaga pliku dodatku w celu rozpoznania dodatku DynamoRevit. [Instalator](http://dynamobim.org/download/) tworzy go automatycznie. W trakcie opracowywania musimy ręcznie utworzyć plik dodatku, który wskazuje kompilację dodatku DynamoRevit, której chcemy użyć, a konkretnie zespół `DynamoRevitDS.dll`. Musimy również wskazać dodatkowi DynamoRevit kompilację dodatku Dynamo.

Utwórz plik `Dynamo.addin` w folderze dodatku programu Revit znajdującym się w katalogu `C:\ProgramData\Autodesk\Revit\Addins\2023`. Zainstalowano już wersję dodatku DynamoRevit, dlatego wystarczy edytować istniejący plik, aby wskazać nową kompilację.

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<RevitAddIns>
<AddIn Type="Application">
<Name>Dynamo For Revit</Name>
<Assembly>"C:\Users\username\Documents\GitHub\DynamoRevit\bin\AnyCPU\Debug\Revit\DynamoRevitDS.dll"</Assembly>
<AddInId>8D83C886-B739-4ACD-A9DB-1BC78F315B2B</AddInId>
<FullClassName>Dynamo.Applications.DynamoRevitApp</FullClassName>
<VendorId>ADSK</VendorId>
<VendorDescription>Dynamo</VendorDescription>
</AddIn>
</RevitAddIns>
```

* Określ ścieżkę pliku `DynamoRevitDS.dll` wewnątrz tagu `<Assembly>...</Assembly>`.

Ewentualnie można skonfigurować w dodatku wczytywanie selektora wersji zamiast określonego zespołu.

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<RevitAddIns>
<AddIn Type="Application">
<Name>Dynamo For Revit</Name>
<Assembly>"C:\Users\username\Documents\GitHub\DynamoRevit\bin\AnyCPU\Debug\Revit\DynamoRevitVersionSelector.dll"</Assembly>
<AddInId>8D83C886-B739-4ACD-A9DB-1BC78F315B2B</AddInId>
<FullClassName>Dynamo.Applications.VersionLoader</FullClassName>
<VendorId>ADSK</VendorId>
<VendorDescription>Dynamo</VendorDescription>
</AddIn>
</RevitAddIns>
```

* Ustaw ścieżkę pliku w tagu `<Assembly>...</Assembly>` na `DynamoRevitVersionSelector.dll`
* Tag `<FullClassName>...</FullClassName>` określa klasę, której wystąpienie należy utworzyć na podstawie zespołu wskazanego za pomocą powyższej ścieżki elementu Assembly. Ta klasa będzie punktem wejścia dla dodatku.

Ponadto musimy usunąć istniejący dodatek Dynamo dostarczany z programem Revit. Aby to zrobić, przejdź do folderu `C:\\Program Files\Autodesk\Revit 2023\AddIns ` i usuń dwa foldery zawierające dodatek **Dynamo** — `DynamoForRevit` i `DynamoPlayerForRevit`. Można je usunąć lub utworzyć ich kopię zapasową w oddzielnym folderze na wypadek późniejszej potrzeby odzyskania oryginalnego dodatku Dynamo dla programu Revit.

![Foldery DynamoForRevit i DynamoPlayerforRevit](images/fe-dynamo-folders-remove.jpg)

Drugim krokiem jest dodanie ścieżki pliku dla zespołów podstawowych dodatku Dynamo do pliku `Dynamo.config` w folderze `bin` dodatku DynamoRevit. Dodatek DynamoRevit wczyta je po otwarciu go w programie Revit. Ten plik konfiguracji umożliwia wskazanie dodatkowi DynamoRevit różnych wersji dodatku Dynamo na potrzeby opracowywania i testowania zmian zarówno w dodatku podstawowym, jak i w dodatku DynamoRevit.

Kod powinien wyglądać następująco:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
     <add key="DynamoRuntime" value="C:\Users\username\Documents\GitHub\Dynamo\bin\AnyCPU\Debug"/>
  </appSettings>
</configuration>
```

* Dodaj ścieżkę katalogu folderu `bin` do części `<add key/>`

> Tuż przed przystąpieniem do tego przewodnika sklonowaliśmy i skompilowaliśmy dodatek Dynamo, aby zapewnić jego sprawne współdziałanie z dodatkiem DynamoRevit. Ta ścieżka katalogu wskazuje tę kompilację.

Teraz po otwarciu programu Revit na karcie Zarządzaj powinien istnieć dodatek Dynamo.

![Dodatek Dynamo znajdujący się na karcie Zarządzaj](images/revit-dynamo.jpg)

> 1. Wybierz opcję `Manage`
> 2. Kliknij ikonę dodatku Dynamo
> 3. Wystąpienie dodatku DynamoRevit

Jeśli pojawia się okno dialogowe błędu brakujących zespołów, prawdopodobnie występuje niezgodność między wersjami dodatku DynamoCore, przy których wykonywano kompilację, a tymi, które są wczytywane w środowisku wykonywania. Na przykład dodatek DynamoRevit z najnowszymi pakietami beta w wersji 2.0 dodatku DynamoCore nie będzie działać w przypadku uruchomienia go za pomocą dodatku bibliotek dll dodatku Dynamo 1.3. Upewnij się, że oba repozytoria są w tej samej wersji, a dodatek DynamoRevit ściąga (pull) zgodną wersję zależności nuget. Są one zdefiniowane w pliku `package.json` repozytorium DynamoRevit.

#### Debugowanie dodatku DynamoRevit za pomocą programu Visual Studio <a href="#debugging-dynamorevit-using-visual-studio" id="debugging-dynamorevit-using-visual-studio"></a>

W poprzedniej sekcji, **Kompilowanie dodatku Dynamo ze źródła**, krótko omówiono debugowanie w programie Visual Studio i sposób dołączania programu Visual Studio do procesu. Używając wyjątku w węźle Wall.ByCurveAndHeight jako przykładu, omówimy sposób dołączania do procesu, ustawiania punktów przerwania, krokowe wykonywanie kodu i używania stosu wywołań w celu określenia źródła wyjątku. Te narzędzia debugowania mają ogólne zastosowanie do procesów roboczych opracowywania rozwiązań .net i warto zapoznać się z nimi w zakresie wykraczającym poza ten podręcznik.

* **Dołączenie do procesu** umożliwia połączenie uruchomionej aplikacji z programem Visual Studio w celu debugowania. Aby debugować zachowanie występujące w kompilacji dodatku DynamoRevit, można otworzyć pliki źródłowe dodatku DynamoRevit w programie Visual Studio i dołączyć proces `Revit.exe`, który jest procesem nadrzędnym dodatku DynamoRevit. Program Visual Studio używa [pliku symboli](https://msdn.microsoft.com/en-us/library/ms241613.aspx) (`.pbd`), aby utworzyć połączenie między zespołami wykonywanymi w dodatku DynamoRevit a kodem źródłowym.
* **Punkty przerwania** określają wiersze w kodzie źródłowym, w których działanie aplikacji zostanie wstrzymane przed dalszym wykonywaniem. Jeśli węzeł powoduje awarię dodatku DynamoRevit lub zwracanie przez niego nieoczekiwanego wyniku, można dodać do źródła węzła punkt przerwania, aby wstrzymać proces, przejść do kodu i sprawdzić wartości zmiennych na żywo aż do znalezienia źródła problemu.
* **Krokowe wykonywanie kodu** to przechodzenie przez źródło wiersz po wierszu. Możemy uruchamiać funkcje pojedynczo, wchodzić krokowo do wywołania funkcji lub wyskakiwać z aktualnie wykonywanej funkcji.
*   **Stos wywołań** pokazuje funkcję obecnie uruchomioną w procesie w odniesieniu do poprzednich wywołań funkcji, które wywołały tę funkcję. W programie Visual Studio jest to wyświetlane w oknie stosu wywołań. Jeśli na przykład dojdziemy do wyjątku poza kodem źródłowym, pojawi się ścieżka do kodu wywołującego w stosie wywołań.

    > [„2,000 Things You Should Know About C#” (2000 rzeczy, które należy wiedzieć o języku C#)](https://csharp.2000things.com/2013/05/20/847-how-the-call-stack-works/): tu znajdziesz bardziej szczegółowe wyjaśnienie stosów wywołań

Węzeł **Wall.ByCurveAndHeight** zgłasza wyjątek po przekazaniu mu krzywej PolyCurve jako krzywej wejściowej (curve) i zwraca komunikat: _„To BSPlineCurve Not Implemented”_ (Nie zaimplementowano do BSPlineCurve). Podczas debugowania możemy ustalić, dlaczego dokładnie węzeł nie akceptuje tego typu geometrii jako danych wejściowych dla parametru krzywej (curve). W tym przykładzie założono, że dodatek DynamoRevit pomyślnie skompilowano i można go uruchomić jako dodatek dla programu Revit.

![Węzeł Wall.ByCurbeAndHeight zgłasza wyjątek](images/dyn-wallbycurveandheight.jpg)

> 1. Węzeł Wall.ByCurveAndHeight zgłasza wyjątek

Rozpocznij od otwarcia pliku rozwiązania `DynamoRevit.All.sln`, uruchom program Revit i uruchom dodatek DynamoRevit. Następnie dołącz program Visual Studio do procesu programu Revit za pomocą okna `Attach to Process`.

![Okno dołączania do procesu](images/vs-debug-attachprocess.jpg)

> Program Revit i dodatek DynamoRevit muszą być uruchomione, aby były widoczne jako dostępne procesy
>
> 1. Otwórz okno `Attach to Process`, wybierając opcję `Debug > Attach to Process...`
> 2. Ustaw opcję `Transport` na wartość `Default`
> 3. Wybierz opcję `Revit.exe`
> 4. Wybierz opcję `Attach`

Po dołączeniu programu Visual Studio do programu Revit otwórz kod źródłowy węzła Wall.ByCurveAndHeight w pliku `Wall.cs`. Tę zawartość można znaleźć w Eksploratorze rozwiązań w części `Libraries > RevitNodes > Elements` w obszarze `Public static constructors` pliku. Ustaw punkt przerwania w konstruktorze typu wall, tak aby po uruchomieniu węzła w dodatku Dynamo proces został przerwany i można było krokowo wykonać poszczególne wiersze kodu. Nazwy konstruktorów typów Zero-Touch dodatku Dynamo zazwyczaj zaczynają się od `By<parameters>`.

![Ustawianie punktu przerwania](images/vs-debugging-breakpoint.jpg)

> 1. Plik klasy z konstruktorem dla węzła Wall.ByCurveAndHeight
> 2. Ustaw punkt przerwania, klikając po lewej stronie numeru wiersza lub klikając prawym przyciskiem myszy wiersz kodu i wybierając polecenie `Breakpoint > Insert Breakpoint`.

Po ustawieniu punktu przerwania należy zadbać o to, aby proces przebiegł przez funkcję Wall.ByCurveAndHeight. Funkcję można ponownie wykonać w dodatku Dynamo, ponownie łącząc przewód do jednego z portów węzła, co spowoduje ponowne uruchomienie tego węzła. W programie Visual Studio zostanie wyzwolony punkt przerwania.

![Punkt przerwania wyzwolony w programie Visual Studio](images/vs-breakpoint.jpg)

> 1. Ikona punktu przerwania zmienia się po jego wyzwoleniu
> 2. Okno stosu wywołań z następną metodą

Teraz krokowo wykonuj poszczególne wiersze w konstruktorze, aż zostanie zwrócony wyjątek. Kod wyróżniony na żółto to następna instrukcja do uruchomienia.

![Wykonywanie krokowe w programie Visual Studio](images/vs-stepover.jpg)

> 1. Narzędzia do debugowania do nawigowania po kodzie
> 2. Naciśnij opcję `Step Over`, aby uruchomić wyróżniony kod, a następnie wstrzymać wykonywanie po zwróceniu przez funkcję wyniku
> 3. Następna instrukcja do uruchomienia oznaczona żółtym wyróżnieniem i strzałką

Jeśli będziemy kontynuować wykonywanie krokowe, w końcu zostanie zwrócony wyjątek, który pojawił się wcześniej w oknie dodatku DynamoRevit. W oknie stosu wywołań widać, że wyjątek został pierwotnie zwrócony z metody o nazwie `Autodesk.Revit.CurveAPIUtils.CreateNurbsCurve`. Na szczęście wyjątek jest tutaj obsługiwany, więc dodatek Dynamo nie uległ awarii. Proces debugowania zapewnił kontekst dla problemu, przenosząc nas do innej metody w kodzie źródłowym.

Ponieważ nie jest to biblioteka open source, nie możemy tam wprowadzać zmian. Mamy teraz więcej informacji, więc możemy zgłosić problem z szerszym kontekstem, zgłaszając [problem](https://guides.github.com/features/issues/) w serwisie GitHub, lub zaproponować obejście tego problemu, wysyłając prośbę o ściągnięcie (pull).

![Wyjątek w programie Visual Studio](images/vs-exception.jpg)

> 1. Po dotarciu do instrukcji powodującej wyjątek w pliku `Walls.cs` proces debugowania maksymalnie przybliża nas do problemu źródłowego w kodzie użytkownika w pliku `ProtoToRevitCurve.cs`.
> 2. Instrukcja powodująca wyjątek w pliku `ProtoToRevitCurve.cs`
> 3. W oknie stosu wywołań widać, że wyjątek pochodzi z kodu innego niż kod użytkownika
> 4. Okno podręczne z informacjami o wyjątku

Ten proces można zastosować do wszystkich plików źródłowych, z którymi pracujemy. W przypadku tworzenia biblioteki węzłów Zero-Touch dla programu Dynamo Studio można otworzyć źródło biblioteki i dołączyć proces dodatku Dynamo w celu debugowania tej biblioteki węzłów. Nawet jeśli wszystko działa doskonale, debugowanie to doskonały sposób na przeglądanie kodu i analizowanie jego działania.

#### Ściąganie (pull) najnowszej kompilacji <a href="#pull-latest-build" id="pull-latest-build"></a>

Proces ten jest niemal identyczny jak w przypadku ściągania (pull) zmian dla dodatku Dynamo, ale należy upewnić się, że jest używana właściwa gałąź. Użyj polecenia `git branch` w repozytorium DynamoRevit, aby sprawdzić, które gałęzie są dostępne lokalnie i które są obecnie wyrejestrowane.

Polecenie `cd C:\Users\username\Documents\GitHub\DynamoRevit` ustawia repozytorium DynamoRevit jako katalog bieżący.\
 Polecenie `git branch` sprawdza, czy używana jest właściwa gałąź, czyli `RC2.13.1_Revit2023`.\
 Polecenie `git pull origin RC2.13.1_Revit2023` ściąga zmiany z gałęzi `RC2.13.1_Revit2023` zdalnej wersji oryginalnej.

Wersja oryginalna wskazuje po prostu adres URL sklonowanej wersji oryginalnej.

![Ustawianie katalogu w interfejsie wiersza polecenia](images/cli-pull-revit.jpg)

> Najlepiej jest pilnować tego, która gałąź jest używana i z której jest ściągana (pull) zawartość, aby na przykład uniknąć ściągnięcia zmian z gałęzi `RC2.13.1_Revit2023` do wersji `Revit2018`.

Jak wspomniano w temacie **Kompilowanie dodatku Dynamo ze źródła**, gdy wszystko będzie gotowe do przesłania zmiany do repozytorium DynamoRevit, można utworzyć prośbę o ściągnięcie zgodnie z wytycznymi zespołu dodatku Dynamo podanymi w sekcji „Prośby o ściągnięcie (pull)”.
