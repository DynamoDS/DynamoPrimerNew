# Dalsze kroki z Zero-Touch

Po omówieniu tworzenia projektu Zero-Touch możemy teraz lepiej zapoznać się z zagadnieniami dotyczącymi tworzenia węzła, analizując przykład ZeroTouchEssentials w witrynie dodatku Dynamo w serwisie Github.

![Węzły Zero-Touch](images/ootbzerotouch.png)

> Wiele węzłów standardowych dodatku Dynamo to w zasadzie węzły Zero-Touch, takie jak większość węzłów Math, Color i DateTime powyżej.

Aby rozpocząć, pobierz projekt ZeroTouchEssentials stąd: [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials)

W programie Visual Studio otwórz plik rozwiązania `ZeroTouchEssentials.sln` i skompiluj to rozwiązanie.

![Projekt ZeroTouchEssentials w programie Visual Studio](images/vs-build-zte.jpg)

> Plik `ZeroTouchEssentials.cs` zawiera wszystkie metody, które zaimportujemy do dodatku Dynamo.

Otwórz dodatek Dynamo i zaimportuj plik `ZeroTouchEssentials.dll`, aby pobrać węzły, do których odwołujemy się w kolejnych przykładach.

Przykłady kodu są ściągane z pliku [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs) i na ogół są z nim zgodne. Dokumentację XML usunięto, aby były one zwięzłe. Każdy przykład kodu spowoduje utworzenie węzła pokazanego na ilustracji powyżej.

### Domyślna wartość wejściowa <a href="#default-input-values" id="default-input-values"></a>

Dodatek Dynamo obsługuje definiowanie wartości domyślnych dla portów wejściowych w węźle. Te wartości domyślne zostają dostarczone do węzła, jeśli porty nie mają połączeń. Wartości domyślne są wyrażane za pomocą mechanizmu języka C# do określania argumentów opcjonalnych omówionego w [Podręczniku programowania C#](https://msdn.microsoft.com/pl-pl/library/dd264739.aspx). Wartości domyślne określa się w następujący sposób:

* Ustaw domyślną wartość parametrów metody: `inputNumber = 2.0`

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Wartość domyślna](images/defaultval.jpg)

> 1. Po umieszczeniu kursora na porcie wejściowym węzła zostanie wyświetlona wartość domyślna

### Zwracanie wielu wartości <a href="#returning-multiple-values" id="returning-multiple-values"></a>

Zwracanie wielu wartości jest nieco bardziej złożone niż tworzenie wielu danych wejściowych. Aby to zrobić, należy użyć słownika. Pozycje słownika stają się portami po stronie wyjściowej węzła. Wiele portów wartości zwracanych tworzy się w następujący sposób:

* Dodaj instrukcję `using System.Collections.Generic;`, aby użyć słownika: `Dictionary<>`.
* Dodaj instrukcję `using Autodesk.DesignScript.Runtime;`, aby użyć atrybutu `MultiReturn`. Jest to odwołanie do pliku „DynamoServices.dll” z pakietu NuGet dodatku DynamoServices.
* Dodaj do metody atrybut `[MultiReturn(new[] { "string1", "string2", ... more strings here })]`. Te ciągi odwołują się do kluczy w słowniku i staną się one nazwami portów wyjściowych.
* Zwróć słownik `Dictionary<>` z funkcji z kluczami odpowiadającymi nazwom parametrów w atrybucie: `return new Dictionary<string, object>`

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> Zobacz przykład kodu w pliku [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70)

Węzeł zwracający wiele pozycji danych wyjściowych.

![Wiele pozycji danych wyjściowych](images/multipleoutputs.png)

> 1. Zwróć uwagę, że teraz istnieją dwa porty wyjściowe o nazwach zgodnych z ciągami wprowadzonymi dla kluczy słownika.

### Dokumentacja, etykiety narzędzi i wyszukiwanie <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

Do najlepszych rozwiązań należy dodawanie do węzłów dodatku Dynamo dokumentacji opisującej funkcję węzła, jego dane wejściowe i wyjściowe, tagi wyszukiwania itp. Robi się to za pomocą tagów dokumentacji XML. Dokumentację XML tworzy się w następujący sposób:

* Każdy tekst komentarza poprzedzony trzema ukośnikami jest traktowany jako należący do dokumentacji
  * Na przykład: `/// Documentation text and XML goes here`
* Po trzech ukośnikach utwórz tagi XML powyżej metod, które dodatek Dynamo odczyta podczas importowania pliku .dll
  * Na przykład: `/// <summary>...</summary>`
* Włącz dokumentację XML w programie Visual Studio, wybierając opcję `Project > [Project] Properties > Build > Output` i zaznaczając opcję `Documentation file`

![Generowanie pliku XML](images/vs-xml.jpg)

> 1. Program Visual Studio wygeneruje plik XML w określonej lokalizacji

Typy tagów są następujące:

* Tag `/// <summary>...</summary>` określa dokumentację główną węzła i jego zawartość będzie wyświetlana jako etykieta narzędzi nad węzłem na pasku po lewej stronie paska wyszukiwania
* Tag `/// <param name="inputName">...</param>` tworzy dokumentację dla określonych parametrów wejściowych
* Tag `/// <returns>...</returns>` tworzy dokumentację dla parametru wyjściowego
* Tag `/// <returns name = "outputName">...</returns>` tworzy dokumentację dla wielu parametrów wyjściowych
* Tag `/// <search>...</search>` dopasowuje węzeł do wyników wyszukiwania na podstawie listy rozdzielonej przecinkami. Jeśli na przykład tworzymy węzeł, który dzieli siatkę, możemy dodać takie oznaczenia, jak „mesh” (siatka), „subdivision” (podpodział) i „catmull-clark”.

Poniżej przedstawiono przykładowy węzeł z opisami danych wejściowych i wyjściowych oraz podsumowanie, które będzie wyświetlane w bibliotece.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Zobacz przykład kodu w pliku [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80)

Należy zwrócić uwagę, że kod dla tego węzła przykładowego zawiera:

> 1. Podsumowanie węzła
> 2. Opis danych wejściowych
> 3. Opis danych wyjściowych

#### Opisy węzłów Dynamo — najlepsze praktyki 

Opisy węzłów to podsumowania funkcji węzła i danych wyjściowych. W dodatku Dynamo są one wyświetlane w dwóch miejscach:

- W etykiecie narzędzia węzła
- W przeglądarce dokumentacji

![Opis węzła](images/node-description.png)

Przestrzeganie tych wskazówek zapewnia spójność i pomaga zaoszczędzić czas podczas pisania oraz aktualizowania opisów węzłów.

##### Przegląd

Opisy powinny składać się z jednego lub dwóch zdań. Jeśli potrzebnych jest więcej informacji, należy podać je w obszarze Szczegółowe informacje w przeglądarce dokumentacji.

Stosuj wielkość liter jak w zdaniu (pierwsza litera pierwszego słowa zdania i pierwsza litera każdej nazwy własnej powinna być wielka). Nie stawiaj kropki na końcu.

Język powinien być tak jasny i prosty, jak to tylko możliwe. Zdefiniuj akronimy przy ich pierwszym użyciu, chyba że są one znane nawet użytkownikom niebędącym ekspertami.

Zawsze stawiaj na pierwszym miejscu jasność, nawet jeśli konieczne będzie odejście od tych wskazówek.

##### Wskazówki

| Co robić      | Czego nie robić |
| ----------- | ----------- |
| Rozpocznij opis od czasownika w trzeciej osobie. <ul><li>Przykład: *Określa*, czy dany obiekt geometrii przecina się z innym</li></ul>      | Nie zaczynaj od czasownika w drugiej osobie ani od żadnego rzeczownika. <ul><li>Przykład: *Określ*, czy dany obiekt geometrii przecina się z innym</li></ul>       |
| Zamiast słowa „uzyskuje” (gets) używaj słowa „zwraca” (returns), słowa „tworzy” (creates) lub innego czasownika opisowego. <ul><li>Przykład: *Zwraca* reprezentację Nurbs powierzchni</li></ul>   | Nie używaj słów „uzyskaj” ani „uzyskuje” (get, gets). Te słowa są mniej konkretne (a słowo „get” ma kilka możliwych tłumaczeń). <ul><li>Przykład: *Uzyskuje* reprezentację Nurbs powierzchni</li></ul>        |
| Odnosząc się do danych wejściowych, używaj słowa „dane” (given) albo „dane wejściowe” lub „wejściowe” (input) zamiast „określone” (specified) czy innych terminów. Jeśli to możliwe, pomijaj słowa „dane” (given) albo „dane wejściowe” lub „wejściowe” (input), aby uprościć opis i zmniejszyć liczbę słów. <ul><li>Przykład: Usuwa *dany* plik</li><li>Przykład: Rzutuje krzywą wzdłuż *danego* kierunku rzutowania na *daną* geometrię bazową</li></ul>Możesz używać słowa „określone” (specified), jeśli nie odnosi się ono bezpośrednio do danych wejściowych. <ul><li>Przykład: Zapisuje zawartość tekstową w pliku *określonym* za pomocą danej ścieżki</li></ul>       | Odnosząc się do danych wejściowych, aby zapewnić spójność, nie używaj słowa „określone” (specified) ani innych terminów z wyjątkiem „dane” (given) albo „dane wejściowe” lub „wejściowe” (input). Nie mieszaj słów „dane” (given) i„dane wejściowe” lub „wejściowe” (input) w tym samym opisie, chyba że jest to konieczne dla zapewnienia przejrzystości. <ul><li>Przykład: Usuwa *określony* plik</li><li>Przykład: Rzutuje krzywą *wejściową* wzdłuż *danego* kierunku rzutowania na *określoną* geometrię bazową</li></ul>      |
| W języku angielskim w pierwszym odniesieniu do danych wejściowych używaj słowa „a” lub „an”. Używaj słów „the given” lub „the input” zamiast „a” lub „an” zgodnie z potrzebami w celu zachowania przejrzystości.<ul><li>Przykład: Sweeps *a* curve along the path curve</li></ul>      | W języku angielskim w pierwszym odniesieniu do danych wejściowych nie używaj słowa „ten” (this). <ul><li>Przykład: Sweeps *this* curve along the path curve      |
| W języku angielskim w pierwszym odniesieniu do danych wejściowych lub innego rzeczownika, który jest elementem docelowym operacji węzła, używaj słowa „a” lub „an”. Słowa „the” używaj tylko razem ze słowami „given” lub „input”. <ul><li>Przykład: Copies *a* file</li><li>Przykład: Copies *the given* file</li></ul>      | W języku angielskim w pierwszym odniesieniu do danych wejściowych lub innego rzeczownika, który jest elementem docelowym operacji węzła, nie używaj samego słowa „the”. <ul><li>Przykład: Copies *the* file</li></ul>      |
| Pierwsza litera pierwszego słowa zdania i pierwsza litera każdej nazwy własnej, takiej jak nazwa czy rzeczownik tradycyjnie pisany wielkimi literami, powinna być wielka (w języku angielskim wszystkie pierwsze litery nazwy własnej złożonej z wielu słów powinny być wielkie). <ul><li>Przykład: Zwraca przecięcie dwóch elementów *BoundingBox*</li></ul>      | Nie rozpoczynaj wielkimi literami słów opisujących typowe obiekty i pojęciach geometryczne, chyba że jest to konieczne dla zachowania przejrzystości. <ul><li>Przykład: Skaluje niejednorodnie wokół danej *Płaszczyzny*      |
| Rozpoczynaj słowo Boolean wielką literą. Rozpoczynaj wielkimi literami wartości True (Prawda) i False (Fałsz) w odniesieniu do danych wyjściowych wartości logicznych (Boolean). <ul><li>Przykład: Zwraca wartość *True* (Prawda), jeśli dwie wartości są różne</li><li>Przykład: Konwertuje ciąg na ciąg napisany w całości wielkimi terami lub małymi literami na podstawie parametru w postaci wartości logicznej *Boolean*      | Nie rozpoczynaj słowa Boolean małą literą. Nie rozpoczynaj małymi literami wartości True (Prawda) ani False (Fałsz) w odniesieniu do danych wyjściowych wartości logicznych (Boolean). <ul><li>Przykład: Zwraca wartość *true* (prawda), jeśli dwie wartości są różne</li><li>Przykład: Konwertuje ciąg na ciąg napisany w całości wielkimi terami lub małymi literami na podstawie parametru w postaci wartości logicznej *boolean*</li></ul>

#### Ostrzeżenia i błędy węzła Dynamo

Ostrzeżenia i błędy węzłów informują użytkownika o problemie z wykresem. Powiadamiają one użytkownika o problemach, które zakłócają normalne działanie wykresu, wyświetlając nad węzłem ikonę i rozwinięty dymek tekstowy. Błędy i ostrzeżenia węzłów mogą mieć różną ważność: niektóre wykresy mogą działać wystarczająco dobrze mimo ostrzeżeń, podczas gdy inne blokują oczekiwane wyniki. We wszystkich przypadkach błędy i ostrzeżenia węzłów są ważnymi narzędziami umożliwiającymi informowanie użytkownika na bieżąco o problemach z wykresem.

Wskazówki dotyczące zapewniania spójności i oszczędzania czasu w przypadku pisania lub aktualizowania komunikatów o błędach i ostrzeżeń dotyczących węzłów można znaleźć na stronie wiki [Wzorzec zawartości: ostrzeżenia i błędy węzłów](https://github.com/DynamoDS/Dynamo/wiki/Content-Pattern:-Node-Warnings-and-Errors).

### Obiekty <a href="#objects" id="objects"></a>

W dodatku Dynamo nie ma słowa kluczowego `new`, dlatego obiekty należy konstruować przy użyciu statycznych metod konstrukcji. Obiekty konstruuje się w następujący sposób:

* Ustaw konstruktor jako wewnętrzny, `internal ZeroTouchEssentials()`, chyba że jest to niezgodne z wymaganiami
* Skonstruuj obiekt za pomocą metody statycznej, na przykład `public static ZeroTouchEssentials ByTwoDoubles(a, b)`

> Uwaga: w dodatku Dynamo używa się prefiksu „By”, aby wskazać, że metoda statyczna jest konstruktorem. Chociaż jest to opcjonalne, używanie prefiksu „By” ułatwia zapewnienie zgodności tworzonej biblioteki z istniejącym stylem dodatku Dynamo.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> Zobacz przykład kodu w pliku [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26)

Po zaimportowaniu biblioteki DLL przykładu ZeroTouchEssentials w bibliotece będzie znajdował się węzeł ZeroTouchEssentials. Ten obiekt można utworzyć za pomocą węzła `ByTwoDoubles`.

![Węzeł ByTwoDoubles](images/dyn-constructor.jpg)

### Używanie typów geometrii dodatku Dynamo <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

W bibliotekach Dynamo można używać własnych typów geometrii dodatku Dynamo jako danych wejściowych i tworzyć nową geometrię jako dane wyjściowe. Typy geometrii tworzy się w następujący sposób:

* Utwórz odwołanie do pliku „ProtoGeometry.dll” w projekcie, umieszczając instrukcję `using Autodesk.DesignScript.Geometry;` u góry pliku z kodem C# i dodając do projektu pakiet NuGet ZeroTouchLibrary.
* **Ważne:** zadbaj o zarządzanie zasobami geometrii, które nie są zwracane z funkcji — zobacz sekcję **Instrukcje Dispose/using** poniżej.

> Uwaga: obiekty geometrii dodatku Dynamo są używane tak samo jak wszystkie inne obiekty przekazywane do funkcji.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> Zobacz przykład kodu w pliku [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86)

Węzeł, który pobiera długość krzywej i podwaja ją.

![Dane wejściowe określające krzywą](images/doublelength.png)

> 1. Ten węzeł przyjmuje jako dane wejściowe typ geometrii Curve (krzywa).

### Instrukcje Dispose/using <a href="#disposeusing-statements" id="disposeusing-statements"></a>

Zasobami geometrii, które nie są zwracane z funkcji, należy zarządzać ręcznie, chyba że jest używany dodatek Dynamo w wersji 2.5 lub nowszej. W dodatku Dynamo 2.5 i wersjach późniejszych zasoby geometrii są obsługiwane wewnętrznie przez system, jednak w przypadku złożonego przypadku zastosowania geometrii może być konieczne ręczne usunięcie geometrii lub zmniejszenie pamięci w określonym z góry czasie. Silnik dodatku Dynamo obsługuje wszystkie zasoby geometrii zwracane z funkcji. Niezwrócone zasoby geometrii można obsłużyć ręcznie w następujący sposób:

*   Za pomocą instrukcji using:

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > Instrukcję using opisano [tutaj](https://msdn.microsoft.com/pl-pl/library/yh598w02.aspx)
    >
    > Aby dowiedzieć się więcej na temat nowych funkcji zwiększających stabilność wprowadzonych w dodatku Dynamo 2.5, zobacz [Ulepszenia stabilności geometrii w dodatku Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297).
*   Za pomocą ręcznych wywołań metody Dispose:

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

### Migracje <a href="#migrations" id="migrations"></a>

Wraz z opublikowaniem nowszej wersji biblioteki nazwy węzłów mogą ulec zmianie. Zmiany nazw można określić w pliku migracji, tak aby wykresy opracowane na podstawie poprzednich wersji biblioteki nadal działały poprawnie po aktualizacji. Migracje implementuje się w następujący sposób:

* Utwórz plik `.xml` w tym samym folderze, w którym znajduje się plik `.dll`, w następującym formacie: „Nazwa_podstawowego_pliku_DLL”.Migrations.xml
* W pliku `.xml` utwórz pojedynczy element `<migrations>...</migrations>`
* W tym elemencie migrations utwórz elementy `<priorNameHint>...</priorNameHint>` dla każdej zmiany nazwy
* Dla każdej zmiany nazwy dodaj element `<oldName>...</oldName>` i element `<newName>...</newName>`

![Plik migracji](images/vs-migrations-file.jpg)

> 1. Kliknij prawym przyciskiem myszy i wybierz polecenie `Add > New Item`
> 2. Wybierz opcję `XML File`
> 3. W przypadku tego projektu należy nadać plikowi migracji nazwę `ZeroTouchEssentials.Migrations.xml`

Ten kod przykładowy informuje dodatek Dynamo, że każdy węzeł o nazwie `GetClosestPoint` ma teraz nazwę `ClosestPointTo`.

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> Zobacz przykład kodu w pliku [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml)

### Typy ogólne <a href="#generics" id="generics"></a>

W projekcie Zero-Touch nie jest obecnie obsługiwane używanie typów ogólnych. Mogą one być używane, ale nie w kodzie, który jest bezpośrednio importowany, gdy nie ustawiono typu. Metod, właściwości lub klas, które są ogólne i nie mają ustawionego typu, nie można uwidocznić.

W poniższym przykładzie węzeł Zero-Touch typu `T` nie zostanie zaimportowany. Jeśli pozostała część biblioteki zostanie zaimportowana do dodatku Dynamo, pojawią się wyjątki wynikające z brakujących typów.

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

Użycie typu ogólnego z typem ustawionym w tym przykładzie spowoduje zaimportowanie do dodatku Dynamo.

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
