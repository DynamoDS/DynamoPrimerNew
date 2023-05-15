# Analiza przypadku pakietu — zestaw Mesh Toolkit

Zestaw Dynamo Mesh Toolkit zawiera narzędzia do importowania siatek z zewnętrznych formatów plików, tworzenia siatki z obiektów geometrii Dynamo oraz ręcznego tworzenia siatek na podstawie wierzchołków i indeksów. Ta biblioteka zawiera również narzędzia do modyfikowania siatek, naprawiania siatek i wyodrębniania warstw poziomych do użycia w produkcji.

\![](<../images/6-2/5/meshToolkitcasestudy01 (1).jpg>)

Zestaw narzędzi Dynamo Mesh Toolkit jest częścią nieustających badań firmy Autodesk nad siatką, więc w nadchodzących latach będzie się rozwijać. Należy spodziewać się częstego dodawania do zestawu nowych metod. Zachęcamy też do kierowania do zespołu Dynamo komentarzy, informacji o błędach i sugestii dotyczących nowych funkcji.

### Siatki a bryły

W poniższym ćwiczeniu przedstawiono niektóre podstawowe operacje na siatce przeprowadzane za pomocą zestawu Mesh Toolkit. W tym ćwiczeniu przetniemy siatkę serią płaszczyzn, co w przypadku używania brył może być kosztowne z punktu widzenia obliczeń. W przeciwieństwie do bryły siatka ma stałą „rozdzielczość” i nie jest zdefiniowana matematycznie, lecz topologicznie. Tę rozdzielczość można zdefiniować na podstawie bieżącego zadania. Aby uzyskać więcej informacji na temat relacji między siatką a bryłami, zapoznaj się z rozdziałem [Geometria w projektowaniu obliczeniowym](../../a-closer-look-at-dynamo-essential-nodes-and-concepts/5\_geometry-for-computational-design/) tego przewodnika. Bardziej dogłębną analizę zestawu Mesh Toolkit można znaleźć na [stronie wiki dodatku Dynamo](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Mesh-Toolkit). Przejdźmy do pakietu w ćwiczeniu poniżej.

### Instalowanie zestawu Mesh Toolkit

W dodatku Dynamo przejdź do obszaru _Pakiety > Wyszukaj pakiety_ na górnym pasku menu. W polu wyszukiwania wpisz _„MeshToolkit”_ (jedno słowo), pamiętając o wielkich literach. Kliknij przycisk Zainstaluj, aby rozpocząć pobieranie. To wystarczy.

![](../images/6-2/2/meshToolkitcasestudy-installpackage.jpg)

## Ćwiczenie: przecinająca się siatka

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/6-2/2/MeshToolkit.zip" %}

W tym przykładzie przyjrzymy się węzłowi Intersect w zestawie Mesh Toolkit. Zaimportujemy siatkę i przetniemy ją szeregiem płaszczyzn wejściowych, aby utworzyć warstwy. Jest to punkt wyjścia do przygotowania modelu do produkcji na przecinarce laserowej, wodnej lub frezarce CNC.

Rozpocznij od otwarcia pliku _Mesh-Toolkit_Intersect-Mesh.dyn w dodatku Dynamo._

![](../images/6-2/2/meshToolkitcasestudy-exercise01.jpg)

> 1. **File Path:** odszukaj plik siatki do zaimportowania (_stanford_bunny_tri.obj_). Obsługiwane typy plików to .mix i .obj
> 2. **Mesh.ImportFile:** połącz ścieżkę pliku w celu zaimportowania siatki.

![](../images/6-2/2/meshToolkitcasestudy-exercise02.jpg)

> 1. **Point.ByCoordinates:** utwórz punkt — będzie to środek łuku.
> 2. **Arc.ByCenterPointRadiusAngle:** utwórz łuk wokół punktu. Ta krzywa zostanie użyta do rozmieszczenia szeregu płaszczyzn. __ Znajdują się tu następujące ustawienia: __ `radius: 40, startAngle: -90, endAngle:0`

Utwórz szereg płaszczyzn zorientowanych wzdłuż łuku.

![](../images/6-2/2/meshToolkitcasestudy-exercise03.jpg)

> 1. **Code Block**: utwórz 25 liczb z zakresu od 0 do 1.
> 2. **Curve.PointAtParameter:** połącz łuk z wejściem _„curve”_ i wyjście węzła Code Block z wejściem _„param”_, aby wyodrębnić szereg punktów wzdłuż krzywej.
> 3. **Curve.TangentAtParameter:** połącz te same wejścia co w poprzednim węźle.
> 4. **Plane.ByOriginNormal:** połącz punkty z wejściem _„origin”_ i wektory z wejściem _„normal”_, aby utworzyć szereg płaszczyzn w każdym punkcie.

Następnie użyjemy tych płaszczyzn do przecięcia siatki.

![](../images/6-2/2/meshToolkitcasestudy-exercise04.jpg)

> 1. **Mesh.Intersect:** utwórz przecięcie płaszczyzn z zaimportowaną siatką, tworząc szereg konturów polikrzywej. Kliknij prawym przyciskiem myszy węzeł i ustaw skratowanie na najdłuższe
> 2. **PolyCurve.Curves:** rozbij polikrzywe na zakrzywione fragmenty.
> 3. **Curve.EndPoint:** wyodrębnij punkty końcowe każdej krzywej.
> 4. **NurbsCurve.ByPoints:** użyj punktów do utworzenia krzywej NURBS. Użyj węzła Boolean ustawionego na _True_, aby zamknąć krzywe.

Przed kontynuowaniem wyłącz podgląd niektórych węzłów, takich jak Mesh.ImportFile, Curve.EndPoint, Plane.ByOriginNormal i Arc.ByCenterPointRadiusAngle, aby lepiej uwidocznić wynik.

![](../images/6-2/2/meshToolkitcasestudy-exercise05.jpg)

> 1. **Surface.ByPatch:** utwórz ścieżki powierzchni dla każdego konturu, aby utworzyć „warstwy” siatki.

Dodaj drugi zestaw warstw, aby uzyskać efekt wafla.

![](../images/6-2/2/meshToolkitcasestudy-exercise06.jpg)

Można zauważyć, że operacje przecięcia są dla siatki wykonywane szybciej niż dla porównywalnej bryły. Procesy robocze, takie jak ten przedstawiony w tym ćwiczeniu, dobrze nadają się do stosowania siatek.
