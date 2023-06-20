# Analiza przypadku Zero-Touch — węzeł siatki 

Mamy już działający projekt programu Visual Studio, więc teraz omówimy tworzenie węzła niestandardowego, który tworzy prostokątną siatkę komórek. Mimo że można ją utworzyć za pomocą kilku węzłów standardowych, jest to przydatne narzędzie, które można łatwo umieścić w węźle Zero-Touch. Inaczej niż w przypadku linii siatki komórki można skalować względem punktów środkowych, można sprawdzać ich wierzchołki narożnikowe i można wbudowywać je w powierzchnie.

W tym przykładzie omówiono kilka funkcji i pojęć, które należy uwzględnić podczas tworzenia węzła Zero-Touch. Po utworzeniu węzła niestandardowego i dodaniu go do dodatku Dynamo należy przejrzeć stronę „Dalsze kroki z Zero-Touch”, aby uzyskać więcej informacji na temat domyślnych wartości wejściowych, zwracania wielu wartości, dokumentacji, obiektów, używania typów geometrii dodatku Dynamo i migracji.

![Wykres siatki prostokątnej](images/cover-image.jpg)

#### Węzeł niestandardowy siatki prostokątnej <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

Aby rozpocząć kompilowanie węzła siatki, utwórz nowy projekt biblioteki klas programu Visual Studio. Na stronie „Pierwsze kroki” można znaleźć szczegółowe omówienie sposobu konfigurowania projektu.

![Tworzenie nowego projektu w programu Visual Studio](images/vs-new-project-1.jpg)

![Konfigurowanie nowego projektu w programie Visual Studio](images/vs-new-project-2.jpg)

> 1. Jako typ projektu wybierz `Class Library`
> 2. Nadaj projektowi nazwę `CustomNodes`

Ponieważ będziemy tworzyć geometrię, musimy odwołać się do odpowiedniego pakietu NuGet. Zainstaluj pakiet ZeroTouchLibrary za pomocą Menedżera pakietów Nuget. Ten pakiet jest niezbędny dla instrukcji `using Autodesk.DesignScript.Geometry;`.

![Pakiet ZeroTouchLibrary](images/vs-nugetpackage.jpg)

> 1. Odszukaj pakiet ZeroTouchLibrary
> 2. Użyjemy tego węzła w bieżącej kompilacji programu Dynamo Studio, czyli 1.3. Wybierz wersję pakietu zgodną z tą wersją.
> 3. Zwróć uwagę, że zmieniliśmy również nazwę pliku klasy na `Grids.cs`

Następnie należy ustanowić przestrzeń nazw i klasę, w których będzie istnieć metoda RectangularGrid. Węzeł zostanie nazwany w dodatku Dynamo zgodnie z nazwami metody i klasy. Nie musimy tego jeszcze kopiować do programu Visual Studio.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
        //The method for creating a rectangular grid will live in here
        }
    }
}
```

> Część `Autodesk.DesignScript.Geometry;` odwołuje się do pliku ProtoGeometry.dll w pakiecie ZeroTouchLibrary `System.Collections.Generic`, który jest niezbędny do tworzenia list

Teraz możemy dodać metodę rysowania prostokątów. Plik klasy powinien wyglądać następująco i można go skopiować do programu Visual Studio.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                }
            }
            return pList;
        }
    }
}
```

Jeśli projekt wygląda podobnie do tego, spróbuj skompilować plik `.dll`.

![Kompilowanie biblioteki DLL](images/vs-grids.jpg)

> 1. Wybierz polecenie Build (Kompiluj) > Build Solution (Kompiluj rozwiązanie)

Sprawdź, czy w folderze `bin` projektu znajduje się plik `.dll`. Jeśli kompilacja się powiodła, można dodać plik `.dll` do dodatku Dynamo.

![Węzły niestandardowe w dodatku Dynamo](images/RectangularGrid-Dynamo.jpg)

> 1. Węzeł niestandardowy RectangularGrids w bibliotece Dynamo
> 2. Węzeł niestandardowy w obszarze rysunku
> 3. Przycisk Add (Dodaj) umożliwiający dodanie pliku `.dll` do dodatku Dynamo

#### Modyfikacje węzłów niestandardowych <a href="#custom-node-modifications" id="custom-node-modifications"></a>

W powyższym przykładzie utworzyliśmy dość prosty węzeł, który nie definiował wiele więcej oprócz metody `RectangularGrids`. Jednak może być konieczne utworzenie etykiet narzędzi dla portów wejściowych lub nadanie węzłowi podsumowania, jak w przypadku standardowych węzłów Dynamo. Dodanie tych elementów do węzłów niestandardowych ułatwia korzystanie z nich, zwłaszcza jeśli użytkownik chce wyszukiwać je w bibliotece.

![Etykieta narzędzia dla portu wejściowego](images/nodemodification.png)

> 1. Domyślna wartość wejściowa
> 2. Etykieta narzędzia dla danych wejściowych xCount

Węzeł RectangularGrid wymaga niektórych z tych podstawowych funkcji. W poniższym kodzie dodaliśmy opisy portów wejściowych i wyjściowych, podsumowanie oraz domyślne wartości wejściowe.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }
            return pList;
        }
    }
}
```

* Nadaj wartości domyślne danym wejściowym, przypisując wartości parametrom metody: `RectangularGrid(int xCount = 10, int yCount = 10)`
* Utwórz etykiety narzędzi dla portów wejściowych i wyjściowych, słowa kluczowe wyszukiwania i podsumowanie za pomocą dokumentacji XML poprzedzonej znakiem `///`.

Aby dodać etykiety narzędzi, potrzebujemy pliku xml w katalogu projektu. Plik `.xml` może zostać automatycznie wygenerowany przez program Visual Studio.

![Włączanie dokumentacji XML](images/vs-xml.jpg)

> 1. Włącz plik dokumentacji XML tutaj i określ ścieżkę pliku. Spowoduje to wygenerowanie pliku XML.

Gotowe! Utworzyliśmy nowy węzeł z kilkoma standardowymi elementami. W kolejnym rozdziale dotyczącym podstaw Zero-Touch podano więcej szczegółów na temat opracowywania węzłów Zero-Touch i problemów, o których należy pamiętać.
