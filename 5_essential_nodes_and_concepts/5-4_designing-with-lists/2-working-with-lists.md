# Praca z listami

### Praca z listami

Ustaliliśmy już, czym jest lista. Omówmy teraz operacje, które możemy na niej wykonać. Wyobraź sobie listę jako talię kart do gry. Talia jest listą, a każda karta reprezentuje element.

![karty](../images/5-4/2/Playing_cards_modified.jpg)

> Autor zdjęcia: [Christian Gidlöf](https://commons.wikimedia.org/wiki/File:Playing_cards_modified.jpg)

### Zapytanie

Jakie **zapytania** możemy wykonać z poziomu listy? Umożliwia to dostęp do istniejących właściwości.

* Liczba kart w talii? 52\.
* Liczba kolorów? 4\.
* Materiał? Papier.
* Długość? 3,5" lub 89 mm.
* Szerokość? 2,5" lub 64 mm.

### Działanie

Jakie **działania** możemy wykonać na liście list? Umożliwia to zmianę listy w oparciu o daną operację.

* Możemy potasować talię.
* Możemy posortować talię według wartości.
* Możemy posortować talię według kolorów.
* Możemy podzielić talię.
* Możemy rozdać karty w talii.
* Możemy wybrać określoną kartę w talii.

Dla wszystkich operacji wymienionych powyżej istnieją analogiczne węzły Dynamo do pracy z listami danych ogólnych. W poniższych lekcjach przedstawiono niektóre z podstawowych operacji, które można wykonywać na listach.

## **Ćwiczenie**

### **Operacje na listach**

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/2/List-Operations.dyn" %}

Poniższy rysunek przedstawia wykres bazowy, na którym rysujemy linie między dwoma okręgami, aby przedstawić podstawowe operacje na listach. Przeanalizujemy sposób zarządzania danymi na liście i przedstawimy wyniki wizualne za pomocą poniższych operacji na liście.

![](../images/5-4/2/workingwithlist-listoperation.jpg)

> 1. Rozpocznij od węzła **Code Block** o wartości `500;`
> 2. Połącz wejście x z węzłem **Point.ByCoordinates**.
> 3. Podłącz węzeł z poprzedniego kroku do wejścia origin węzła **Plane.ByOriginNormal**.
> 4. Za pomocą węzła **Circle.ByPlaneRadius** podłącz węzeł z poprzedniego kroku do wejścia plane.
> 5. Używając węzła **Code Block**, oznacz wartość `50;` dla pozycji radius. To pierwszy okrąg, który utworzymy.
> 6. Za pomocą węzła **Geometry.Translate** przesuń okrąg w górę o 100 jednostek w kierunku Z.
> 7. Za pomocą węzła **Code Block** zdefiniuj zakres dziesięciu liczb z zakresu od 0 do 1 przy użyciu tego wiersza kodu: `0..1..#10;`
> 8. Wstaw blok kodu z poprzedniego kroku do wejścia _param_ dwóch węzłów **Curve.PointAtParameter**. Podłącz węzeł **Circle.ByPlaneRadius** do wejścia curve górnego węzła i węzeł **Geometry.Translate** do wejścia curve węzła poniżej.
> 9. Za pomocą węzła **Line.ByStartPointEndPoint** połącz dwa węzły **Curve.PointAtParameter**.

### List.Count

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/2/List-Count.dyn" %}

Węzeł _List.Count_ jest prosty: zlicza wartości na liście i zwraca ich liczbę. Jego działanie jest nieco bardziej złożone podczas pracy z listami list, ale zilustrujemy to w późniejszych sekcjach.

![Count](../images/5-4/2/workingwithlist-listoperation-listcount.jpg)

> 1. Węzeł **List.Count** zwraca liczbę linii w węźle **Line.ByStartPointEndPoint**. W tym przypadku wynosi ona 10, co odpowiada liczbie punktów utworzonych z oryginalnego węzła **Code Block**.

### List.GetItemAtIndex

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/2/List-GetItemAtIndex.dyn" %}

Węzeł **List.GetItemAtIndex** zapewnia podstawowy sposób stosowania zapytania dotyczącego elementu listy.

![Ćwiczenie](../images/5-4/2/workingwithlist-getitemindex01.jpg)

> 1. Najpierw kliknij prawym przyciskiem myszy węzeł **Line.ByStartPointEndPoint**, aby wyłączyć jego podgląd.
> 2. Za pomocą węzła **List.GetItemAtIndex** wybieramy indeks _„0”_, czyli pierwszy element na liście linii.

Zmień wartość suwaka na od 0 do 9, aby wybrać inny element za pomocą węzła **List.GetItemAtIndex**.

![](../images/5-4/2/workingwithlist-getitemindex02.gif)

### List.Reverse

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/2/List-Reverse.dyn" %}

Węzeł _List.Reverse_ odwraca kolejność wszystkich elementów na liście.

![Ćwiczenie](../images/5-4/2/workingwithlist-listreverse.jpg)

> 1. Aby poprawnie zwizualizować odwróconą listę linii, utwórz więcej linii, zmieniając węzeł **Code Block** na `0..1..#50;`
> 2. Powiel węzeł **Line.ByStartPointEndPoint** oraz wstaw węzeł List.Reverse między węzłem **Curve.PointAtParameter** i drugim węzłem **Line.ByStartPointEndPoint**
> 3. Użyj węzłów **Watch3D**, aby wyświetlić podgląd dwóch różnych wyników. Pierwszy pokazuje wynik bez odwróconej listy. Linie łączą się pionowo z sąsiednimi punktami. Natomiast odwrócona lista powoduje połączenie wszystkich punktów w kolejności odwrotnej na drugiej liście.

### List.ShiftIndices <a href="#listshiftindices" id="listshiftindices"></a>

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/2/List-ShiftIndices.dyn" %}

Węzeł **List.ShiftIndices** jest dobrym narzędziem do tworzenia skrętów lub wzorców śrubowych albo do innych podobnych manipulacji danymi. Ten węzeł przesuwa elementy na liście o podaną wartość indeksu.

![Ćwiczenie](../images/5-4/2/workingwithlist-shiftIndices01.jpg)

> 1. W tym samym procesie, w którym występuje odwrócona lista, wstaw węzeł **List.ShiftIndices** do węzłów **Curve.PointAtParameter** i **Line.ByStartPointEndPoint**.
> 2. Używając węzła **Code Block**, określ wartość „1”, aby przesunąć listę o jeden indeks.
> 3. Zauważmy, że zmiana jest subtelna, ale wszystkie linie w dolnym węźle **Watch3D** przesunęły się o jeden indeks podczas łączenia się z drugim zestawem punktów.

Po zmianie wartości w węźle **Code Block** na większą, na przykład _„30”_, zauważamy znaczną różnicę w liniach ukośnych. W tym przypadku przesunięcie działa jak obiektyw aparatu, tworząc skręt w oryginalnej formie walcowej.

![](../images/5-4/2/workingwithlist-shiftIndices02.jpg)

### List.FilterByBooleanMask <a href="#listfilterbybooleanmask" id="listfilterbybooleanmask"></a>

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/2/List-FilterByBooleanMask.dyn" %}

![](../images/5-4/2/ListFilterBool.png)

Węzeł **List.FilterByBooleanMask** usuwa niektóre elementy w oparciu o listę wartości logicznych lub wartości odczytywanych jako „true” lub „false”.

![Ćwiczenie](../images/5-4/2/workingwithlist-filterbyboolmask.jpg)

Aby utworzyć listę wartości odczytywanych jako „true” lub „false”, musimy wykonać nieco więcej pracy.

> 1. Używając węzła **Code Block**, zdefiniuj wyrażenie ze składnią: `0..List.Count(list);`. Połącz węzeł **Curve.PointAtParameter** z wejściem _list_. Przeanalizujemy tę konfiguracje dokładniej w rozdziale dotyczącym węzła Code Block, ale wiersz kodu w tym przypadku tworzy listę reprezentującą każdy indeks węzła **Curve.PointAtParameter**.
> 2. Używając węzła _**%**_** (moduł)**, połącz wyjście węzła _Code Block_ z wejściem _x_ oraz wartość _4_ z wejściem _y_. Spowoduje to zwrócenie reszty z dzielenia listy indeksów przez 4. Węzeł modułu jest bardzo przydatny podczas tworzenia szyku. Wszystkie wartości będą odczytywane jako możliwe reszty z dzielenia przez 4, czyli 0, 1, 2 i 3.
> 3. Na podstawie węzła _**%**_** (moduł)** wiemy, że wartość 0 oznacza, iż indeks jest podzielny przez 4 (0, 4, 8 itd...). Za pomocą węzła **==** możemy sprawdzić tę dzielność, testując tę pozycję pod kątem wartości _„0”_.
> 4. Węzeł **Watch** pokazuje tylko, że mamy wzorzec true/false, który wygląda następująco: _true,false,false,false..._.
> 5. Używając tego wzorca true/false, utwórz połączenie z wejściem mask dwóch węzłów **List.FilterByBooleanMask**.
> 6. Połącz węzeł **Curve.PointAtParameter** z każdym wejściem list węzła **List.FilterByBooleanMask**.
> 7. Wyjścia węzła **Filter.ByBooleanMask** to _„in”_ oraz _„out”_. Wyjście _„in”_ reprezentuje wartości, które miały wartość maski _„true”_, a wyjście _„out”_ — wartość maski _„false”_. Podłączając wyjścia _„in”_ do wejść _startPoint_ i _endPoint_ węzła **Line.ByStartPointEndPoint**, utworzyliśmy przefiltrowaną listę linii.
> 8. Węzeł **Watch3D** pokazuje, że mamy mniej linii niż punktów. Wybraliśmy tylko 25% węzłów, filtrując tylko wartości true.
