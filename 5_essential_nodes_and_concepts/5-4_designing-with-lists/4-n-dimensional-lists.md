# Listy n-wymiarowe

Idąc dalej, możemy dodać jeszcze więcej poziomów do hierarchii. Struktura danych może znacznie wykraczać poza dwuwymiarową listę list. Ponieważ same listy również są elementami w dodatku Dynamo, możemy tworzyć dane z dowolnie dużą liczbą wymiarów.

Można to porównać do rosyjskich matrioszek. Każdą listę można traktować jako jeden pojemnik zawierający wiele elementów. Każda lista ma określone właściwości i sama w sobie jest traktowana jako obiekt.

![Lalki](../images/5-4/4/145493363\_fc9ff5164f\_o.jpg)

> Zestaw matrioszek (autor zdjęcia: [Zeta](https://www.flickr.com/photos/beppezizzi/145493363)) to dobra analogia dla list n-wymiarowych. Każda warstwa oznacza listę, a każda lista zawiera elementy. W dodatku Dynamo każdy pojemnik może zawierać wiele pojemników (elementów listy).

Listy n-wymiarowe trudno przedstawić wizualnie, ale w tym rozdziale przygotowaliśmy kilka ćwiczeń dotyczących pracy z listami wykraczającymi poza dwa wymiary.

### Odwzorowywanie i kombinacje

Odwzorowywanie to zdecydowanie najbardziej złożona część zarządzania danymi w programie Dynamo, szczególnie istotna podczas pracy ze złożonymi hierarchiami list. W poniższych ćwiczeniach pokazano, kiedy używać odwzorowywania i kombinacji podczas pracy z wielowymiarowymi danymi.

Wstępne informacje o węzłach **List.Map** i **List.Combine** można znaleźć w poprzedniej sekcji. W ostatnim ćwiczeniu użyjemy tych węzłów w złożonej strukturze danych.

## Ćwiczenie — listy dwuwymiarowe — podstawowe

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-4/4/n-Dimensional-Lists.zip" %}

To pierwsze z serii trzech ćwiczeń, które skupia się na artykulacji zaimportowanej geometrii. W każdej części tej serii ćwiczeń zwiększy się złożoność struktury danych.

![Exercise](<../images/5-4/4/n-dimensional lists - 2d lists basic 01.jpg>)

> 1. Zacznijmy od pliku .sat w folderze plików ćwiczeniowych. Można przechwycić ten plik za pomocą węzła **File Path**.
> 2. Za pomocą węzła **Geometry.ImportFromSAT** geometria jest importowana do podglądu dodatku Dynamo jako dwie powierzchnie.

Aby uprościć to ćwiczenie, będziemy pracować z jedną z tych powierzchni.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 02.jpg>)

> 1. Wybierzmy indeks 1 , aby przechwycić górną powierzchnię. Służy do tego węzeł **List.GetItemAtIndex**.
> 2. Wyłącz podgląd geometrii w podglądzie **Geometry.ImportFromSAT**.

Kolejnym etapem jest podzielenie powierzchni na siatkę punktów.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 03.jpg>)

> 1\. Za pomocą węzła **Code Block** wstaw następujące dwa wiersze kodu: `0..1..#10;` `0..1..#5;`
>
> 2\. W węźle **Surface.PointAtParameter** połącz te dwie wartości z bloku kodu z elementami u i _v_. Zmień opcję _skratowania_ tego węzła na _„Iloczyn wektorowy”_.
>
> 3\. Wynik pokazuje strukturę danych, która jest również widoczna w podglądzie dodatku Dynamo.

Następnie użyto punktów z ostatniego kroku, aby wygenerować dziesięć krzywych wzdłuż powierzchni.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 04.jpg>)

> 1. Aby zobaczyć, jak zorganizowana jest struktura danych, połączmy węzeł **NurbsCurve.ByPoints** z elementem wyjściowym węzła **Surface.PointAtParameter**.
> 2. Podgląd można na razie wyłączyć z poziomu węzła **List.GetItemAtIndex**, aby uzyskać bardziej czytelny wynik.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 05.jpg>)

> 1. Podstawowa funkcja **List.Transpose** spowoduje zamianę kolumn i wierszy listy list.
> 2. Po połączeniu elementu wyjściowego węzła **List.Transpose** z węzłem **NurbsCurve.ByPoints** powstanie pięć krzywych biegnących poziomo na powierzchni.
> 3. Podgląd można wyłączyć z poziomu węzła **NurbsCurve.ByPoints** w poprzednim kroku, aby uzyskać ten sam wynik na obrazie.

## Ćwiczenie — listy dwuwymiarowe — zaawansowane

Teraz zwiększmy złożoność. Załóżmy, że chcemy wykonać operację na krzywych utworzonych w poprzednim ćwiczeniu. Możemy na przykład powiązać te krzywe z inną powierzchnią i wykonać pomiędzy nimi wyciągnięcie. To wymaga zwrócenia większej uwagi na strukturę danych, ale podstawowa logika jest taka sama.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 01.jpg>)

> 1. Rozpocznij od kroku z poprzedniego ćwiczenia, w którym wyizolowaliśmy górną powierzchnię zaimportowanej geometrii za pomocą węzła **List.GetItemAtIndex**.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 02.jpg>)

> 1. Używając węzła **Surface.Offset**, odsuń tę powierzchnię o wartość _10_.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 03.jpg>)

> 1. Tak samo jak w poprzednim ćwiczeniu, zdefiniuj węzeł _Code Block_ zawierający te dwa wiersze kodu: `0..1..#10;` `0..1..#5;`
> 2. Połącz te dane wyjściowe z dwoma węzłami **Surface.PointAtParameter**, każdy z opcją _skratowania_ ustawioną na _„Iloczyn wektorowy”_. Jeden z tych węzłów jest połączony z pierwotną powierzchnią, a drugi z powierzchnią odsuniętą.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 04.jpg>)

> 1. Wyłącz podgląd tych powierzchni.
> 2. Tak samo jak w poprzednim ćwiczeniu, połącz elementy wyjściowe z dwoma węzłami **NurbsCurve.ByPoints**. Wynik pokazuje krzywe odpowiadające dwóm powierzchniom.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 05.jpg>)

> 1. Używając węzła **List.Create**, można połączyć oba zestawy krzywych w jedną listę list.
> 2. Jak widać w wyniku, otrzymaliśmy dwie listy zawierające po dziesięć elementów, odpowiadające obu połączonym zestawom krzywych NURBS.
> 3. Wykonując operację **Surface.ByLoft**, możemy wizualnie zinterpretować tę strukturę danych. Ten węzeł wyciąga wszystkie krzywe na każdej podliście.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 06.jpg>)

> 1. Wyłącz podgląd z poziomu węzła **Surface.ByLoft** w poprzednim kroku.
> 2. Jak pamiętamy, używając węzła **List.Transpose**, zamieniamy wszystkie kolumny i wiersze. Ten węzeł umożliwia przekształcenie dwóch list po dziesięć krzywych w dziesięć list po dwie krzywe. Teraz każda krzywa NURBS jest powiązana z sąsiednią krzywą na drugiej powierzchni.
> 3. Po użyciu węzła **Surface.ByLoft** otrzymujemy żebrowaną strukturę.

Następnie zademonstrujemy alternatywny proces pozwalający osiągnąć ten wynik

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 07.jpg>)

> 1. Przed rozpoczęciem należy wyłączyć podgląd węzła **Surface.ByLoft** w poprzednim kroku, aby uniknąć pomyłek.
> 2. Alternatywą dla węzła **List.Transpose** jest użycie węzła **List.Combine**. Spowoduje to zastosowanie _„kombinatora”_ do każdej podlisty.
> 3. W tym przypadku używamy węzła **List.Create **jako _„kombinatora”_ w celu utworzenia listy dla każdego elementu na podlistach.
> 4. Po użyciu węzła **Surface.ByLoft** otrzymujemy takie same powierzchnie, jak w poprzednim kroku. W tym przypadku łatwiej jest użyć węzła Transpose, ale jeśli struktura danych jest jeszcze bardziej złożona, węzeł **List.Combine** będzie bardziej niezawodny.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 08.jpg>)

> 1. Cofając się o kilka kroków, aby zmienić orientację krzywych w żebrowanej strukturze, należy użyć węzła **List.Transpose** przed połączeniem z węzłem **NurbsCurve.ByPoints**. Spowoduje to zamianę kolumn i wierszy, dając 5 poziomych żeber.

## Ćwiczenie — listy trójwymiarowe

Teraz pójdziemy o krok dalej. W tym ćwiczeniu będziemy pracować z obiema zaimportowanymi powierzchniami, tworząc złożoną hierarchię danych. Będziemy jednak wykonywać tę samą operację z tą samą logiką.

Rozpocznij od zaimportowanego pliku z poprzedniego ćwiczenia.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 01.jpg>)

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 02.jpg>)

> 1. Tak samo jak w poprzednim ćwiczeniu, użyj węzła **Surface.Offset**, aby wykonać odsunięcie o wartość _10_.
> 2. Jak pokazuje wynik, przy użyciu węzła Offset utworzyliśmy dwie powierzchnie.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 03.jpg>)

> 1. Tak samo jak w poprzednim ćwiczeniu, zdefiniuj węzeł **Code Block** zawierający te dwa wiersze kodu: `0..1..#20;` `0..1..#20;`
> 2. Połącz te dane wyjściowe z dwoma węzłami **Surface.PointAtParameter**, każdy z opcją skratowania ustawioną na _„Iloczyn wektorowy”_. Jeden z tych węzłów jest połączony z pierwotnymi powierzchniami, a drugi z powierzchniami odsuniętymi.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 04.jpg>)

> 1. Tak samo jak w poprzednim ćwiczeniu, połącz elementy wyjściowe z dwoma węzłami **NurbsCurve.ByPoints**.
> 2. W danych wyjściowych węzła **NurbsCurve.ByPoints** widzimy, że jest to lista dwóch list, czyli bardziej złożona struktura niż w poprzednim ćwiczeniu. Dane są kategoryzowane według bazowej powierzchni, a więc dodaliśmy do struktury danych kolejny poziom.
> 3. Należy zauważyć, że sytuacja w węźle **Surface.PointAtParameter** się skomplikowała. Mamy już listę list list.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 05.jpg>)

> 1. Przed przejściem dalej wyłącz podgląd z istniejących powierzchni.
> 2. Za pomocą węzła **List.Create** scalamy krzywe NURBS w jedną strukturę danych, tworząc listę list list.
> 3. Połączenie z węzłem **Surface.ByLoft** powoduje utworzenie nowej wersji pierwotnych powierzchni, z których każda pozostaje na osobnej liście utworzonej na podstawie początkowej struktury danych.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 06.jpg>)

> 1. W poprzednim ćwiczeniu można było użyć węzła **List.Transpose**, aby utworzyć żebrowaną strukturę. W tym przypadku to nie zadziała. Transpozycję można stosować do listy dwuwymiarowej, a ponieważ mamy listę trójwymiarową, nie można tak łatwo „zamienić kolumn i wierszy”. Należy pamiętać, że listy to obiekty, a więc węzeł **List.Transpose** spowodowałby zamianę list z podlistami, ale nie zamianę krzywych NURBS na kolejnym poziomie hierarchii list.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 07.jpg>)

> 1. W tym przypadku lepiej będzie działać węzeł **List.Combine**. Przechodząc do bardziej złożonych struktur danych, używamy węzłów **List.Map** i **List.Combine**.
> 2. Używając węzła **List.Create **jako _„kombinatora”_, tworzymy strukturę danych, która będzie lepiej przystosowana do naszego celu.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 08.jpg>)

> 1. Struktura danych wymaga jeszcze transpozycji na niższym poziomie hierarchii. W tym celu użyjemy węzła **List.Map**. Działa on podobnie jak węzeł **List.Combine**, ale wymaga jednej listy wejściowej, a nie dwóch lub więcej.
> 2. Do węzła **List.Map** zastosujemy funkcję **List.Transpose**, która spowoduje zamianę kolumn i wierszy podlist na głównej liście.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 09.jpg>)

> 1. Na koniec możemy wyciągnąć krzywe NURBS wraz z odpowiednią hierarchią danych, co daje żebrowaną strukturę.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 10.jpg>)

> 1. Teraz dodamy głębię do tej geometrii za pomocą węzła **Surface.Thicken** z pokazanymi ustawieniami wejść.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 11.jpg>)

> 1. Dobrze będzie dodać powierzchnię pokrywającą się z tą strukturą, dlatego dodaj kolejny węzeł **Surface.ByLoft** i jako wejścia użyj pierwszego wyjścia węzła **NurbsCurve.ByPoints** z wcześniejszego kroku.
> 2. Gdy podgląd staje się coraz mniej czytelny, wyłącz podgląd dla tych węzłów, klikając prawym przyciskiem myszy każdy z nich i anulując wybór pola podglądu („preview”), aby wyniki były czytelniejsze.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 12.jpg>)

> 1. Po pogrubieniu wybranych powierzchni artykulacja jest gotowa.

Nie jest to może najwygodniejszy fotel bujany, ale zawiera dużo danych.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 13.jpg>)

Na ostatnim etapie odwrócimy kierunek prążków. W poprzednim ćwiczeniu stosowaliśmy transpozycję. Tu zrobimy coś podobnego.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 14.jpg>)

> 1. Ponieważ hierarchia ma jeszcze jeden poziom, należy użyć węzła **List.Map** z funkcją **List.Tranpose**, aby zmienić kierunek krzywych NURBS.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 15.jpg>)

> 1. Jeśli chcemy zwiększyć liczbę stopni, możemy zmienić węzeł **Code Block** następująco: `0..1..#20;` `0..1..#30;`

Pierwsza wersja fotela bujanego była bardziej elegancka, a nasz drugi model to wersja dla miłośników sportów ekstremalnych.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 16.jpg>)
