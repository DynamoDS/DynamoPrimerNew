# Zero-Touch — importowanie

### Co to jest Zero-Touch?

Importowanie Zero-Touch to prosta metoda importowania bibliotek C# przez wskazanie i kliknięcie. Dodatek Dynamo odczyta publiczne metody z pliku _.dll_ i przekonwertuje je na węzły Dynamo. Można użyć funkcji Zero-Touch, aby tworzyć własne węzły i pakiety niestandardowe, a także importować biblioteki zewnętrzne do środowiska Dynamo.

![](../images/6-2/5/zero-touchimporting01.jpg)

> 1. Pliki .dll
> 2. Węzły Dynamo

Za pomocą funkcji Zero-Touch można nawet zaimportować bibliotekę, która nie została opracowana dla dodatku Dynamo, i utworzyć pakiet nowych węzłów. Obecne działanie funkcji Zero-Touch pokazuje międzyplatformowy charakter projektu Dynamo.

W tej sekcji przedstawiono sposób użycia funkcji Zero-Touch w celu zaimportowania biblioteki innej firmy. Aby uzyskać informacje na temat tworzenia własnej biblioteki Zero-Touch, zobacz [stronę wiki dodatku Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Zero-Touch-Plugin-Development).

### Pakiety Zero-Touch

Pakiety Zero-Touch stanowią dobre uzupełnienie węzłów niestandardowych zdefiniowanych przez użytkownika. W poniższej tabeli wymieniono kilka pakietów używających bibliotek C#. Aby uzyskać więcej informacji na temat pakietów, zobacz [sekcję Pakiety](../../a\_appendix/a-3\_packages.md) w Załączniku.

| **Logo/obraz**                                                                   | **Nazwa**                                                                    |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| \![](<../images/6-2/2/meshToolkitcasestudy01 (1).jpg>)                            | [Zestaw Mesh Toolkit](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit) |
| \![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (1).jpg>) | [Dynamo Unfold](http://dynamobim.com/dynamounfold/)                         |
| ![](../images/6-2/5/rhynamo.jpg)                                                 | [Rhynamo](http://www.case-inc.com/blog/what-is-rhynamo)                     |
| ![](../images/6-2/5/optimo.jpg)                                                  | [Optimo](https://github.com/BPOpt/Optimo)                                   |

## Analiza przypadku — importowanie biblioteki AForge

W tej analizie przypadku pokazano, jak zaimportować zewnętrzną bibliotekę _.dll_ [AForge](http://www.aforgenet.com). AForge to rozbudowana biblioteka, która zapewnia szeroką gamę funkcji, od przetwarzania obrazu po sztuczną inteligencję. Na potrzeby poniższych ćwiczeń dotyczących przetwarzania obrazu skorzystamy z klasy Imaging w bibliotece AForge.

Najpierw należy pobrać bibliotekę AForge. Na [stronie pobierania biblioteki AForge](http://www.aforgenet.com/framework/downloads.html) wybierz opcję _[Download Installer]_ (Pobierz instalator) i zainstaluj bibliotekę po zakończeniu pobierania.

W dodatku Dynamo utwórz nowy plik i wybierz kolejno opcje _Plik > Importuj bibliotekę..._

![](../images/6-2/5/casestudyaforge01.jpg)

Następnie odszukaj plik dll.

![](../images/6-2/5/casestudyaforge02.jpg)

> 1. W wyskakującym okienku przejdź do folderu Release w instalacji biblioteki AForge. Prawdopodobnie będzie on się znajdował w folderze podobnym do tego: _C:\\Program Files (x86)\\AForge.NET\\Framework\\Release._
> 2. **AForge.Imaging.dll:** w tej analizie przypadku użyjemy tylko tego pliku z biblioteki AForge. Wybierz ten plik _.dll_ i kliknij przycisk _„Otwórz”_.

W dodatku Dynamo grupa węzłów **AForge** powinna zostać dodana do paska narzędzi Biblioteka. Teraz mamy dostęp do biblioteki przetwarzania obrazów AForge z poziomu programu wizualnego.

![](../images/6-2/5/casestudyaforge03.jpg)

### Ćwiczenie 1 — wykrywanie krawędzi

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/6-2/5/ZeroTouchImages.zip" %}

Po zaimportowaniu biblioteki można wykonać pierwsze ćwiczenie (_01-EdgeDetection.dyn_). Wykonamy kilka podstawowych operacji na obrazie przykładowym, aby pokazać, w jaki sposób działają filtry obrazu biblioteki AForge. Użyjemy węzła „_Watch Image_”, aby wyświetlić wyniki i zastosować filtry w dodatku Dynamo, podobne do tych w programie Photoshop

Aby zaimportować obraz, dodaj węzeł **File Path** do obszaru rysunku i wybierz plik „soapbubbles.jpg” z folderu ćwiczeń (źródło zdjęcia: [flickr](https://www.flickr.com/photos/wwworks/667298782)).

![](../images/6-2/5/casestudyaforgeexercise1-01.jpg)

Węzeł File Path zawiera po prostu ciąg ścieżki do wybranego obrazu. Następnie należy przekształcić go w użyteczny plik obrazu w dodatku Dynamo.

![](../images/6-2/5/casestudyaforgeexercise1-02.jpg)

> 1. Użyj opcji **Plik ze ścieżki**, aby przekształcić element w ścieżce pliku w obraz w środowisku Dynamo.
> 2. Połącz węzeł **File Path** z węzłem **File.FromPath**.
> 3. Aby przekształcić ten plik na obraz, należy użyć węzła **Image.ReadFromFile**.
> 4. Teraz zobaczmy wynik. Upuść węzeł **Watch Image** na obszarze rysunku i połącz go z węzłem **Image.ReadFromFile**. Nie użyliśmy jeszcze biblioteki AForge, ale pomyślnie zaimportowaliśmy obraz do dodatku Dynamo.

W obszarze AForge.Imaging.AForge.Imaging.Filters (w menu nawigacji) można zauważyć, że dostępna jest szeroka gama filtrów. Użyjemy teraz jednego z tych filtrów, aby zmniejszyć nasycenie obrazu na podstawie wartości progowych.

![](../images/6-2/5/casestudyaforgeexercise1-03.jpg)

> 1. Upuść trzy suwaki na obszarze rysunku, zmień ich zakresy na 0–1, a ich wartości kroku na 0,01.
> 2. Dodaj węzeł **Grayscale.Grayscale** do obszaru rysunku. Jest to filtr biblioteki AForge, który stosuje filtr skali szarości do obrazu. Połącz trzy suwaki z kroku 1 z elementami „cr”, „cg” i „cb”. Ustaw wartość górnego i dolnego suwaka na 1, a wartość środkowego suwaka na 0.
> 3. Aby zastosować filtr skali szarości, należy wykonać czynność na obrazie. W tym celu użyjemy węzła **BaseFilter.Apply**. Połącz obraz z wejściem „image”, a węzeł **Grayscale.Grayscale** z wejściem „baseFilter”.
> 4. Po połączeniu z węzłem **Watch Image** otrzymujemy zdesaturowany obraz.

Możemy określać sposób zmniejszenia nasycenia obrazu za pomocą wartości progowych dla kolorów czerwonego, zielonego i niebieskiego. Są one definiowane przez elementy wejściowe węzła **Grayscale.Grayscale**. Można zauważyć, że obraz jest dość ciemny — jest to spowodowane tym, że suwak koloru zielonego jest ustawiony na wartość 0.

![](../images/6-2/5/casestudyaforgeexercise1-04.jpg)

> 1. Ustaw wartość górnego i dolnego suwaka na 0, a wartość środkowego suwaka na 1. W ten sposób można uzyskać bardziej czytelny zdesaturowany obraz.

Użyjemy teraz zdesaturowanego obrazu i zastosujemy na nim kolejny filtr. Obraz po zmniejszeniu nasycenia jest kontrastowy, więc przetestujemy wykrywanie krawędzi.

![](../images/6-2/5/casestudyaforgeexercise1-05.jpg)

> 1. Dodaj węzeł **SobelEdgeDetector.SobelEdgeDetector** do obszaru rysunku.
> 2. Połącz ten element z węzłem **BaseUsingCopyPartialFilter.Apply** i połącz zdesaturowany obraz z wejściem image tego węzła.
> 3. Detektor krawędzi Sobel wyróżnił krawędzie na nowym obrazie.

Po powiększeniu widać, że detektor krawędzi wyróżnił obrysy bąbelków pikselami. Biblioteka AForge zawiera narzędzia umożliwiające utworzenie geometrii dodatku Dynamo na podstawie takich wyników. Zobaczymy to w następnym ćwiczeniu.

![](../images/6-2/5/casestudyaforgeexercise1-06.jpg)

### Ćwiczenie 2 — tworzenie prostokątów

Po przedstawieniu podstawowych informacji o przetwarzaniu obrazu możemy użyć obrazu do utworzenia geometrii dodatku Dynamo. Na podstawowym poziomie w tym ćwiczeniu chcemy uzyskać efekt _„aktywnego obrysu”_ obrazu za pomocą biblioteki AForge i dodatku Dynamo. Aby uprościć to zadanie, wyodrębnimy prostokąty z obrazu referencyjnego, ale w bibliotece AForge dostępne są też narzędzia do bardziej złożonych operacji. Użyjemy pliku _02-RectangleCreation.dyn_ pobranego na potrzeby ćwiczenia.

![](../images/6-2/5/casestudyaforgeexercise2-01.jpg)

> 1. Za pomocą węzła File Path przejdź do pliku grid.jpg w folderze ćwiczenia.
> 2. Połącz pozostałe węzły powyżej, aby wyświetlić ćwiczeniową siatkę parametryczną.

W następnym kroku chcemy odwołać się do białych kwadratów na obrazie i przekonwertować je na rzeczywistą geometrię dodatku Dynamo. Biblioteka AForge zawiera wiele wydajnych narzędzi do przetwarzania obrazów, a tutaj użyjemy szczególnie ważnego narzędzia z tej biblioteki o nazwie [BlobCounter](http://www.aforgenet.com/framework/docs/html/d7d5c028-7a23-e27d-ffd0-5df57cbd31a6.htm).

![](../images/6-2/5/casestudyaforgeexercise2-02.jpg)

> 1. Dodaj węzeł BlobCounter do obszaru rysunku. Następnie potrzebny jest sposób na przetworzenie obrazu (podobnie jak w poprzednim ćwiczeniu z narzędziem **BaseFilter.Apply**).

Niestety węzeł „Process Image” nie jest widoczny w bibliotece Dynamo. Jest tak, ponieważ ta funkcja może nie być widoczna w kodzie źródłowym biblioteki AForge. Aby to naprawić, trzeba znaleźć obejście.

![](../images/6-2/5/casestudyaforgeexercise2-03.jpg)

> 1. Dodaj do obszaru rysunku węzeł w języku Python i dodaj do niego następujący kod. Ten kod umożliwia zaimportowanie biblioteki AForge, a następnie przetworzenie zaimportowanego obrazu.

```
import sys
import clr
clr.AddReference('AForge.Imaging')
from AForge.Imaging import *

bc= BlobCounter()
bc.ProcessImage(IN[0])
OUT=bc
```

Połączenie elementu wyjściowego „image” z elementem wejściowym węzła Python powoduje uzyskanie wyniku AForge.Imaging.BlobCounter z węzła Python.

![](../images/6-2/5/casestudyaforgeexercise2-04.jpg)

W następnych krokach zastosujemy pewne sposoby pokazujące znajomość [interfejsu AForge Imaging API](http://www.aforgenet.com/framework/docs/html/d087503e-77da-dc47-0e33-788275035a90.htm). Nie trzeba znać ich wszystkich, aby pracować z dodatkiem Dynamo. Jest to raczej pokaz pracy z zewnętrznymi bibliotekami, możliwej dzięki elastyczności środowiska Dynamo.

![](../images/6-2/5/casestudyaforgeexercise2-05.jpg)

> 1. Połącz element wyjściowy węzła Python Script z węzłem BlobCounterBase.GetObjectRectangles. Dzięki temu obiekty na obrazie zostaną odczytane na podstawie wartości progowej, a następnie obliczone prostokąty zostaną wyodrębnione z zakresu pikseli.

![](../images/6-2/5/casestudyaforgeexercise2-06.jpg)

> 1. Dodaj kolejny węzeł Python do obszaru rysunku, połącz go z węzłem GetObjectRectangles i wprowadź poniższy kod. Spowoduje to utworzenie uporządkowanej listy obiektów Dynamo.

```
OUT = []
for rec in IN[0]:
	subOUT=[]
	subOUT.append(rec.X)
	subOUT.append(rec.Y)
	subOUT.append(rec.Width)
	subOUT.append(rec.Height)
	OUT.append(subOUT)
```

![](../images/6-2/5/casestudyaforgeexercise2-07.jpg)

> 1. Zastosuj funkcję Transpose do wyniku węzła Python z poprzedniego kroku. Spowoduje to utworzenie 4 list przedstawiających parametry X, Y, szerokość i wysokość poszczególnych prostokątów.
> 2. Za pomocą węzła Code Block uporządkujemy te dane w strukturę odpowiednią dla węzła Rectangle.ByCornerPoints (kod poniżej).

```
recData;
x0=List.GetItemAtIndex(recData,0);
y0=List.GetItemAtIndex(recData,1);
width=List.GetItemAtIndex(recData,2);
height=List.GetItemAtIndex(recData,3);
x1=x0+width;y1=y0+height;
p0=Autodesk.Point.ByCoordinates(x0,y0);
p1=Autodesk.Point.ByCoordinates(x0,y1);
p2=Autodesk.Point.ByCoordinates(x1,y1);
p3=Autodesk.Point.ByCoordinates(x1,y0);
```

Mamy tutaj szyk prostokątów reprezentujących białe kwadraty z obrazu. Dzięki programowaniu uzyskaliśmy efekt (mniej więcej) podobny do funkcji Live Trace w programie Illustrator.

Wciąż jednak trzeba go trochę oczyścić. Po powiększeniu widzimy wiele małych, niepotrzebnych prostokątów.

![](../images/6-2/5/casestudyaforgeexercise2-08.jpg)

Następnie napiszemy kod, aby pozbyć się niechcianych prostokątów.

![](../images/6-2/5/casestudyaforgeexercise2-09.jpg)

> 1. Wstaw węzeł w języku Python między węzłem GetObjectRectangles a kolejnym węzłem w języku Python. Kod węzła, który znajduje się poniżej, umożliwia usunięcie wszystkich prostokątów poniżej danego rozmiaru.

```
rectangles=IN[0]
OUT=[]
for rec in rectangles:
 if rec.Width>8 and rec.Height>8:
  OUT.append(rec)
```

Po usunięciu niepotrzebnych prostokątów dla zabawy utworzymy powierzchnię z otrzymanych prostokątów i wyciągniemy je na odległość zależną od ich wielkości.

![](../images/6-2/5/casestudyaforgeexercise2-10.jpg)

Na koniec zmienimy element wejściowy both_sides na wartość „false”, aby uzyskać wyciągnięcie w jednym kierunku. Wystarczy zanurzyć tę bryłę w żywicy, aby powstał naprawdę niesamowity stolik.

![](../images/6-2/5/casestudyaforgeexercise2-11.jpg)

Są to podstawowe przykłady, ale przedstawione tutaj koncepcje można przełożyć na ciekawe rzeczywiste zastosowania. Przetwarzanie obrazów można wykorzystać w bardzo wielu procesach. Jako przykłady można przytoczyć czytniki kodów kreskowych, dopasowywanie perspektywy, [mapping przestrzenny](https://www.youtube.com/watch?v=XSR0Xady02o) i [rzeczywistość rozszerzoną](http://aforgenet.com/aforge/articles/gratf\_ar/). Bardziej zaawansowane tematy dotyczące biblioteki AForge, używanej w tym ćwiczeniu, zostały omówione w [tym artykule](http://aforgenet.com/articles/shape\_checker/).
