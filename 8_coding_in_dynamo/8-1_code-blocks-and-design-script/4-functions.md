# Functions

W bloku kodu można tworzyć funkcje, a następnie można je ponownie wywoływać w innym miejscu w definicji dodatku Dynamo. Powoduje to utworzenie innej warstwy sterującej w pliku parametrycznym. Można to postrzegać jako wersję tekstową węzła niestandardowego. W tym przypadku „nadrzędny” blok kodu jest łatwo dostępny i może być umieszczony w dowolnym miejscu na wykresie. Nie są potrzebne żadne przewody.

### Parent

Pierwszy wiersz zawiera słowo kluczowe „def”, następnie nazwę funkcji, a następnie nazwy danych wejściowych w nawiasach. Klamry definiują treść funkcji. Wartość jest zwracana za pomocą instrukcji „return =”. Bloki kodu, które definiują funkcję, nie mają portów wejściowych ani wyjściowych, ponieważ są wywoływane z innych bloków kodu.

![](../images/8-1/4/functionsparentdef.jpg)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

### Podrzędne

Wywołaj funkcję w innym bloku kodu w tym samym pliku przez podanie nazwy i takiej samej liczby argumentów. Działa to tak jak w przypadku gotowych węzłów w bibliotece.

![](../images/8-1/4/functionschildrencalldef.jpg)

```
FunctionName(in1,in2);
```

## Ćwiczenie: kula według osi Z

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/8-1/4/Functions_SphereByZ.dyn" %}

W tym ćwiczeniu zostanie utworzona ogólna definicja, która utworzy sfery z wejściowej listy punktów. Promień tych sfer zależy od właściwości Z każdego punktu.

Zacznijmy od przedziału liczbowego dziesięciu wartości z zakresu od 0 do 100. Podłącz je do węzłów **Point.ByCoordinates**, aby utworzyć linię przekątną.

![](../images/8-1/4/functions-exercise-01.jpg)

Utwórz węzeł **Code Block** i wprowadź naszą definicję.

![](../images/8-1/4/functions-exercise-02.jpg)

> 1.  Użyj następujących wierszy kodu:
>
>     ```
>     def sphereByZ(inputPt)
>     {
>
>     };
>     ```
>
> _inputPt_ to nazwa, którą nadaliśmy reprezentacji punktów sterujących funkcją. Na razie funkcja niczego nie robi, ale w kolejnych krokach ją skonstruujemy.

![](../images/8-1/4/functions-exercise-03.jpg)

> 1. Dodając elementy do funkcji **Code Block**, umieścimy komentarz i zmienną _sphereRadius_, która wysyła zapytanie o położenie _Z_ każdego punktu. Pamiętaj, że _inputPt.Z_ nie wymaga nawiasów jak metoda. Jest to _zapytanie_ o właściwości istniejącego elementu, dlatego żadne dane wejściowe nie są konieczne:
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

![](../images/8-1/4/functions-exercise-04.jpg)

> 1. Teraz przypomnijmy sobie funkcję, którą utworzyliśmy w innym węźle **Code Block**. Jeśli dwukrotnie klikniemy obszar roboczy, aby utworzyć nowy węzeł _Code Block_, i wpiszemy _sphereB_, dodatek Dynamo zasugeruje zdefiniowaną przez nas wcześniej funkcję _sphereByZ_. Funkcja została dodana do biblioteki intellisense. To przydatne.

![](../images/8-1/4/functions-exercise-05.jpg)

> 1.  Teraz wywołamy tę funkcję i utworzymy zmienną o nazwie _Pt_, aby podłączyć punkty utworzone w poprzednich krokach:
>
>     ```
>     sphereByZ(Pt)
>     ```
> 2. Wszystkie wyjścia mają wartości null. Dlaczego tak jest? W definicji funkcji obliczamy zmienną _sphereRadius_, ale nie zdefiniowaliśmy, co funkcja powinna _zwracać_ na _wyjściu_. Możemy to naprawić w następnym kroku.

![](../images/8-1/4/functions-exercise-06.jpg)

> 1. Ważnym krokiem jest zdefiniowanie wyjścia funkcji przez dodanie wiersza `return = sphereRadius;` do funkcji _sphereByZ_.
> 2. Teraz na wyjściu węzła Code Block pojawiają się współrzędne Z każdego punktu.

Teraz utworzymy właściwe sfery, edytując funkcję _nadrzędną_.

![](../images/8-1/4/functions-exercise-07.jpg)

> 1. Najpierw zdefiniujemy sferę za pomocą wiersza kodu: `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`
> 2. Następnie zmienimy zwracaną wartość na _sphere_ zamiast _sphereRadius_: `return = sphere;` To pozwoli nam uzyskać kilka gigantycznych sfer w podglądzie Dynamo.

![](../images/8-1/4/functions-exercise-08.jpg)

> 1\. Aby zwiększyć rozmiar tych sfer, zaktualizuj wartość sphereRadius dodając dzielnik: `sphereRadius = inputPt.Z/20;` Teraz możemy dostrzec osobne sfery i zrozumieć związek między wartością promienia a wartością Z.

![](../images/8-1/4/functions-exercise-09.jpg)

> 1. W węźle **Point.ByCoordinates** tworzymy siatkę punktów, zmieniając skratowanie z Shortest List na Cross Product. Funkcja _sphereByZ_ nadal w pełni działa, dlatego wszystkie punkty tworzą sfery z promieniami na podstawie wartości Z.

![](../images/8-1/4/functions-exercise-10.jpg)

> 1. Aby przetestować rozwiązanie, podłączymy oryginalną listę liczb do wejść X węzła **Point.ByCoordinates**. Mamy teraz sześcian sfer.
> 2. Uwaga: jeśli obliczenia na komputerze trwają długo, spróbuj zmienić _\#10_ na wartość typu _\#5_.

Pamiętaj, że utworzona przez nas funkcja _sphereByZ_ to funkcja ogólna, więc możemy przywołać helisę z wcześniejszej lekcji i zastosować do niej tę funkcję.

![](../images/8-1/4/functions-exercise-11.jpg)

Ostatni krok: sterowanie współczynnikiem promienia za pomocą parametru zdefiniowanego przez użytkownika. Aby to zrobić, należy utworzyć nowe wejście dla tej funkcji, a także zastąpić dzielnik _20_ parametrem.

![](../images/8-1/4/functions-exercise-12.jpg)

> 1.  Zaktualizuj definicję funkcji _sphereByZ_ do postaci:
>
>     ```
>     def sphereByZ(inputPt,radiusRatio)
>     {
>     //get Z Value, use it to drive radius of sphere
>     sphereRadius=inputPt.Z/radiusRatio;
>     //Define Sphere Geometry
>     sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>     //Define output for function
>     return sphere;
>     };
>     ```
> 2. Zaktualizuj podrzędne węzły **Code Block**, dodając do wejścia zmienną ratio: `sphereByZ(Pt,ratio);`. Podłącz suwak do nowo utworzonego wejścia węzła **Code Block** i zmieniaj rozmiar promieni na podstawie współczynnika promienia.
