# Zero-Touch-Fallstudie - Rasterblock

Bei geöffnetem und gestartetem Visual Studio-Projekt werden Sie durch die Erstellung eines benutzerdefinierten Blocks geführt, mit dem ein rechteckiges Zellenraster erstellt wird. Auch wenn wir für die Erstellung mehrere Standardblöcke verwenden könnten, ist dies ein nützliches Werkzeug, das sich problemlos in einen Zero-Touch-Block integrieren lässt. Im Gegensatz zu Rasterlinien können Zellen um ihre Mittelpunkte skaliert, nach Eckscheitelpunkten abgefragt oder in Flächen integriert werden.

In diesem Beispiel werden einige der Funktionen und Konzepte behandelt, die Sie beim Erstellen eines Zero-Touch-Blocks beachten sollten. Nachdem wir den benutzerdefinierten Block erstellt und zu Dynamo hinzugefügt haben, informieren Sie sich auf der Seite Weitere Schritte mit Zero-Touch im Detail über die vorgegebenen Eingabewerte, das Zurückgeben mehrerer Werte, die Dokumentation, die Objekte, die Verwendung von Dynamo-Geometrietypen und Migrationen.

![Rechteckiges Rasterdiagramm](images/cover-image.jpg)

#### Benutzerdefinierter, rechteckiger Rasterblock <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

Um mit dem Erstellen des Rasterblocks zu beginnen, erstellen Sie ein neues Visual Studio-Klassenbibliotheksprojekt. Eine ausführliche exemplarische Vorgehensweise zum Einrichten eines Projekts finden Sie auf der Seite Erste Schritte.

![Erstellen eines neuen Projekts in Visual Studio](images/vs-new-project-1.jpg)

![Konfigurieren eines neuen Projekts in Visual Studio](images/vs-new-project-2.jpg)

> 1. Wählen Sie `Class Library` für den Projekttyp.
> 2. Geben Sie dem Projekt den Namen `CustomNodes`.

Da wir Geometrie erstellen, müssen wir das entsprechende NuGet-Paket referenzieren. Installieren Sie das ZeroTouchLibrary-Paket über den NuGet-Paket-Manager. Dieses Paket ist für die Anweisung `using Autodesk.DesignScript.Geometry;` erforderlich.

![ZeroTouchLibrary-Paket](images/vs-nugetpackage.jpg)

> 1. Suchen Sie nach dem ZeroTouchLibrary-Paket.
> 2. Wir verwenden diesen Block im aktuellen Build von Dynamo Studio (Version 1.3). Wählen Sie die Paketversion aus, die dieser Version entspricht.
> 3. Beachten Sie, dass wir auch die Klassendatei in `Grids.cs` umbenannt haben.

Als Nächstes müssen wir einen Namensbereich und eine Klasse einrichten, in denen die Methode RectangularGrid gespeichert wird. Der Block wird in Dynamo entsprechend den Methoden- und Klassennamen benannt. Wir müssen diesen Vorgang noch nicht in Visual Studio kopieren.

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

> `Autodesk.DesignScript.Geometry;` referenziert die Datei ProtoGeometry.dll im ZeroTouchLibrary-Paket. `System.Collections.Generic` ist zum Erstellen von Listen erforderlich.

Jetzt können wir die Methode zum Zeichnen der Rechtecke hinzufügen. Die Klassendatei sollte der folgenden Abbildung entsprechen und kann in Visual Studio kopiert werden.

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

Wenn das Projekt ähnlich aussieht wie hier gezeigt, versuchen Sie, die `.dll`-Datei zu erstellen.

![Erstellen einer DLL-Datei](images/vs-grids.jpg)

> 1. Wählen Sie Build > Projektmappe erstellen.

Prüfen Sie, ob im Ordner `bin` des Projekts eine `.dll`-Datei enthalten ist. Wenn der Build erfolgreich war, können wir die `.dll`-Datei zu Dynamo hinzufügen.

![Benutzerdefinierte Blöcke in Dynamo](images/RectangularGrid-Dynamo.jpg)

> 1. Der benutzerdefinierte Block RectangularGrids in der Dynamo-Bibliothek
> 2. Der benutzerdefinierte Block im Ansichtsbereich
> 3. Die Schaltfläche Hinzufügen zum Hinzufügen der `.dll`-Datei zu Dynamo

#### Benutzerdefinierte Blockänderungen <a href="#custom-node-modifications" id="custom-node-modifications"></a>

Im obigen Beispiel haben wir einen recht einfachen Block erstellt, der außerhalb der Methode `RectangularGrids` nicht viel mehr definiert hat. Es ist jedoch eventuell empfehlenswert, QuickInfos für Eingabeanschlüsse zu erstellen oder dem Block eine Zusammenfassung hinzuzufügen, wie bei den Dynamo-Standardblöcken. Das Hinzufügen dieser Elemente zu benutzerdefinierten Blöcken erleichtert deren Verwendung, insbesondere dann, wenn ein Benutzer in der Bibliothek nach ihnen suchen möchte.

![Eingabe-QuickInfo](images/nodemodification.png)

> 1. Ein Vorgabe-Eingabewert
> 2. Eine QuickInfo für die xCount-Eingabe

Der Block RectangularGrid benötigt einige dieser grundlegenden Funktionen. Im folgenden Code wurden Beschreibungen der Eingabe- und Ausgabeanschlüsse, eine Zusammenfassung und vorgabemäßige Eingabewerte hinzugefügt.

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

* Legen Sie Vorgabewerte für die Eingaben fest, indem Sie den Methodenparametern Werte zuweisen: `RectangularGrid(int xCount = 10, int yCount = 10)`
* Erstellen Sie QuickInfos für Ein- und Ausgabe, Suchbegriffe und eine Zusammenfassung mit XML-Dokumentation, der `///` vorangestellt ist.

Um QuickInfos hinzuzufügen, benötigen wir eine XML-Datei im Projektverzeichnis. Eine `.xml`-Datei kann automatisch von Visual Studio generiert werden, indem Sie die Option aktivieren.

![Aktivieren der XML-Dokumentation](images/vs-xml.jpg)

> 1. Aktivieren Sie hier die XML-Dokumentationsdatei, und geben Sie einen Dateipfad an. Dadurch wird eine XML-Datei erstellt.

Das ist alles. Wir haben einen neuen Block mit mehreren Standardelementen erstellt. Im folgenden Kapitel zu den Zero-Touch-Grundlagen wird ausführlicher auf die Zero-Touch-Blockentwicklung und die zu beachtenden Aspekte eingegangen.
