# Wykonywanie skryptów w języku Python w węzłach Zero-Touch (C#)

### Wykonywanie skryptów w języku Python w węzłach Zero-Touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Jeśli użytkownik potrafi pisać skrypty w języku Python i chce uzupełnić węzły standardowe dodatku w języku Python Dynamo o więcej funkcji, może utworzyć własny za pomocą funkcji Zero-Touch. Zacznijmy od prostego przykładu umożliwiającego przekazanie skryptu w języku Python jako ciągu do węzła Zero-Touch, w którym wykonywany jest skrypt i zwracany jest wynik. Ta analiza przypadku jest oparta na przewodnikach i przykładach z sekcji „Pierwsze kroki”. Użytkownicy dopiero zaczynający tworzenie węzłów Zero-Touch powinni się z nimi zapoznać.

![Węzeł Zero-Touch wykonujący ciąg skryptu w języku Python](../../.gitbook/assets/python-case-study.png)

> Węzeł Zero-Touch wykonujący ciąg skryptu w języku Python

#### Silnik języka Python <a href="#python-engine" id="python-engine"></a>

PythonNet3 jest teraz domyślnym silnikiem, który zapewnia płynniejsze działanie w przypadku migracji z CPython. Wszystkie nowe węzły w języku Python tworzone w dodatku Dynamo od wersji 4.0 są uruchamiane przy użyciu silnika PythonNet3.

Ten węzeł bazuje na wystąpieniu silnika skryptów IronPython. Aby to obsłużyć, należy utworzyć odwołanie do kilku dodatkowych zespołów. Wykonaj poniższe czynności, aby skonfigurować szablon podstawowy w programie Visual Studio:

* Tworzenie nowego projektu klasy programu Visual Studio
* Dodaj odwołanie do pliku `IronPython.dll` znajdującego się tutaj: `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`
* Dodaj odwołanie do pliku `Microsoft.Scripting.dll` znajdującego się tutaj: `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`
* Dołącz do tej klasy instrukcje `IronPython.Hosting` i `Microsoft.Scripting.Hosting` `using`
* Dodaj pusty konstruktor prywatny, aby zapobiec dodaniu dodatkowego węzła do biblioteki Dynamo wraz z pakietem
* Utwórz nową metodę, która przyjmuje pojedynczy ciąg jako parametr wejściowy
* W ramach tej metody utworzymy wystąpienie nowego silnika języka Python i utworzymy pusty zakres skryptu. Ten zakres można sobie wyobrazić jako zmienne globalne w wystąpieniu interpretera języka Python
* Następnie wywołaj operację `Execute` dla silnika, przekazując ciąg wejściowy i zakres jako parametry
* Na koniec pobierz i zwróć wyniki skryptu, wywołując operację `GetVariable` dla zakresu i przekazując nazwę zmiennej ze skryptu w języku Python zawierającą wartość, którą próbujesz zwrócić. (Więcej informacji znajduje się w poniższym przykładzie)

Poniższy kod stanowi przykład kroku opisanego powyżej. Skompilowanie rozwiązania spowoduje utworzenie nowego pliku `.dll` znajdującego się w folderze bin projektu. Ten plik `.dll` można teraz zaimportować do dodatku Dynamo jako część pakietu lub przechodząc do polecenia `File < Import Library...`

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Skrypt w języku Python zwraca zmienną `output`, co oznacza, że w skrypcie w języku Python będzie potrzebna zmienna `output`. Użyj tego skryptu przykładowego, aby przetestować węzeł w dodatku Dynamo. Jeśli ten węzeł języka Python był kiedykolwiek używany w dodatku Dynamo, poniższe czynności powinny wyglądać znajomo. Aby uzyskać więcej informacji, zobacz [sekcję dotyczącą języka Python w przewodniku](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Wiele pozycji danych wyjściowych<a href="#multiple-outputs" id="multiple-outputs"></a>

Jednym z ograniczeń standardowych węzłów języka Python jest to, że mają one tylko jeden port wyjściowy. Jeśli więc chcemy zwrócić wiele obiektów, musimy utworzyć listę i umieścić na niej poszczególne obiekty. Jeśli zmodyfikujemy powyższy przykład tak, aby zwracał słownik, możemy dodać dowolną liczbę portów wyjściowych. Więcej informacji na temat słowników znajduje się w temacie „Dalsze kroki z Zero-Touch” w sekcji dotyczącej zwracania wielu wartości.

![Ten węzeł umożliwia zwrócenie zarówno objętości prostopadłościanu, jak i jego centroidy.](../../.gitbook/assets/python-multi-case-study.png)

> Ten węzeł umożliwia zwrócenie zarówno objętości prostopadłościanu, jak i jego centroidy.

Zmodyfikujmy poprzedni przykład, wykonując następujące czynności:

* Dodaj odwołanie do pliku `DynamoServices.dll` z Menedżera pakietów NuGet
* Oprócz poprzednich zespołów dołącz zespoły `System.Collections.Generic` i `Autodesk.DesignScript.Runtime`
* Zmień typ zwracany w metodzie, aby zwracać słownik, który będzie zawierał dane wyjściowe
* Każda pozycja wyjściowa musi zostać osobno pobrana z zakresu (dla większych zestawów danych wyjściowych rozważ skonfigurowanie prostej pętli)

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

Do tego przykładowego skryptu w języku Python dodaliśmy również dodatkową zmienną wyjściową (`output2`). Należy pamiętać, że te zmienne mogą używać dowolnych konwencji nazewnictwa języka Python, dlatego określenia „output” (dane wyjściowe) użyto wyłącznie w celu zachowania przejrzystości w tym przykładzie.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### Znane ograniczenia silnika PythonNet3 i ich obejścia<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

Poniżej przedstawiono kilka znanych ograniczeń występujących podczas korzystania z silnika PythonNet3 i ich obejść.

* Kolekcje .NET nie są automatycznie konwertowane na listy języka Python
    * Przed użyciem ```len()```, indeksowania lub iteracji należy jawnie przekonwertować tablice lub kolekcje ```.NET``` przy użyciu ```list(...)```.
* Ogólne metody platformy .NET mogą wymagać jawnych parametrów typu
    * Niektóre metody (np. ```GroupBy```) zakończą się niepowodzeniem, chyba że ręcznie określisz typy ogólne, zamiast polegać na automatycznym wnioskowaniu.
* Metody rozszerzeń nie są wykrywalne za pomocą ```dir()``` czy automatycznego uzupełniania
    * Metody rozszerzeń mogą nadal działać, gdy są wywoływane jawnie, ale nie pojawiają się podczas introspekcji ani uzupełniania kodu.
* Metody rozszerzeń DataTable nie są obsługiwane
    * Importowanie ```System.Data.DataTableExtensions``` kończy się niepowodzeniem. Te metody pomocnicze nie mogą być używane bezpośrednio.
* Niektóre metody podstawowe dodatku Dynamo zachowują się inaczej w silniku PythonNet3
    * Niektóre funkcje (np. spłaszczanie listy) mogą nie działać zgodnie z oczekiwaniami z powodu bardziej rygorystycznej obsługi kolekcji.
* Klas języka Python nie można przekazywać między węzłami, jeśli dziedziczą z typów .NET
    * Klas pochodnych od interfejsów ani typów .NET nie można bezpiecznie przenosić między węzłami języka Python.
* Funkcja ```set()``` języka Python nie akceptuje niektórych obiektów .NET
    * Obiekty takie jak ```InvalidElementId``` muszą być odfiltrowywane lub obsługiwane przy użyciu kolekcji .NET.
* Częste wywołania ```print()``` mogą powodować wzrost użycia pamięci
    * Należy unikać częstego używania ```print()``` w pętlach lub długotrwałych skryptach.
* Zgodność operacyjna słowników między dodatkiem Dynamo i językiem Python jest ograniczona
    * Słowniki dodatku Dynamo i języka Python nie są w pełni zamienne i mogą wymagać ręcznej konwersji.
* Metoda ```Marshal.GetActiveObject()``` pobierania uruchomionego wystąpienia COM określonego obiektu nie jest już dostępna
    * Użyj ```BindToMoniker```, jeśli znasz ścieżkę używanego pliku.
    * Koduj bibliotekę w języku C# przy użyciu struktury klas ```Marshal.GetActiveObject()```

#### Migracja z silnika CPython3 do silnika PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dodatek Dynamo automatycznie przeprowadzi migrację węzłów silnika CPython do silnika PythonNet 3. Oto co się dzieje:

> 1. Kopia zapasowa oryginalnego pliku jest tworzona automatycznie.
> 2. Wszystkie węzły silnika CPython (w tym węzły niestandardowe używające silnika CPython) są konwertowane na węzły silnika PythonNet3. 
> 3. Wyskakujące powiadomienie informuje o liczbie węzłów poddanych migracji.
> 4. Podczas zapisywania zostanie wyświetlone przypomnienie, że węzły w języku Python będą teraz używać silnika PythonNet3. Ponownie powtarzamy, że nie trzeba martwić się o zgodność obliczeniową ze starszymi wersjami: użytkownicy pracujący w środowiskach z wieloma wersjami (np. Revit lub Civil 3D 2025/2026) powinni zainstalować pakiet silnika PythonNet3 w dodatku Dynamo 3.3–3.6, aby zachować zgodność. 

#### Migracja z silnika IronPython2 do silnika PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Jeśli wykres korzysta z silnika IronPython, automatyczna migracja nie zostaje przeprowadzona. 

Jeśli jest zainstalowany zgodny pakiet silnika IronPython, wykres będzie działał normalnie. Jeśli go brakuje, w rozszerzeniu Odniesienia obszaru roboczego zostanie wyświetlone ostrzeżenie o zależności z prośbą o pobranie pakietu. Możesz dalej korzystać z silnika IronPython, ponownie instalując pakiet. Ponieważ jednak silnik IronPython nie był aktualizowany od lat, a środowisko Dynamo nie obsługuje aktywnie tych silników w dodatku Dynamo już od dłuższego czasu, zdecydowanie zalecamy migrację do silnika PythonNet3, aby zapewnić, że wykresy będą działać niezawodnie w przyszłości. Komponenty DynamoIronPython2.7 i DynamoIronPython3 pozostaną dostępne jako pakiety w Menedżerze pakietów Dynamo, ale nie będą już obsługiwane przez zespół Dynamo. 

W tym przypadku dostępna jest opcja migracji poszczególnych węzłów z użyciem asystenta migracji (Migration Assistant) dostępnego w edytorze języka Python.  

Więcej informacji na temat migracji można znaleźć w tym [wpisie w blogu](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/)