# Wykonywanie skryptów w języku Python w węzłach Zero-Touch (C#) 

### Wykonywanie skryptów w języku Python w węzłach Zero-Touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Jeśli użytkownik potrafi pisać skrypty w języku Python i chce uzupełnić węzły standardowe dodatku w języku Python Dynamo o więcej funkcji, może utworzyć własny za pomocą funkcji Zero-Touch. Zacznijmy od prostego przykładu umożliwiającego przekazanie skryptu w języku Python jako ciągu do węzła Zero-Touch, w którym wykonywany jest skrypt i zwracany jest wynik. Ta analiza przypadku jest oparta na przewodnikach i przykładach z sekcji „Pierwsze kroki”. Użytkownicy dopiero zaczynający tworzenie węzłów Zero-Touch powinni się z nimi zapoznać.

![Węzeł Zero-Touch wykonujący ciąg skryptu w języku Python](images/python-case-study.png)

> Węzeł Zero-Touch wykonujący ciąg skryptu w języku Python

#### Silnik języka Python <a href="#python-engine" id="python-engine"></a>

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

Skrypt w języku Python zwraca zmienną `output`, co oznacza, że w skrypcie w języku Python będzie potrzebna zmienna `output`. Użyj tego skryptu przykładowego, aby przetestować węzeł w dodatku Dynamo. Jeśli ten węzeł języka Python był kiedykolwiek używany w dodatku Dynamo, poniższe czynności powinny wyglądać znajomo. Aby uzyskać więcej informacji, zobacz [sekcję dotyczącą języka Python w przewodniku](http://dynamoprimer.com/en/09\_Custom-Nodes/9-4\_Python.html).

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

![Ten węzeł umożliwia zwrócenie zarówno objętości prostopadłościanu, jak i jego centroidy.](images/python-multi-case-study.png)

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
