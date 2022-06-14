# Krótka składnia

### Krótka składnia

W bloku kodu dostępnych jest kilka podstawowych metod o krótkiej składni, które _znacznie_ ułatwiają zarządzanie danymi. Podstawy zostały szczegółowo omówione poniżej. Wyjaśniamy też, jak za pomocą tej krótkiej składni można tworzyć dane i stosować do nich zapytania.

| **Typ danych** | **Standard Dynamo** | **Odpowiednik w bloku kodu** |
| ---------------------- | -------------------------------------------------------- | ------------------------------------------------------------- |
| Liczby | ![](<../images/8-1/3/01 node - numbers.jpg>) | ![](<../images/8-1/3/01 codeblock - numbers.jpg>) |
| Ciągi | ![](<../images/8-1/3/02 node - string.jpg>) | ![](<../images/8-1/3/02 codeblock- string.jpg>) |
| Sekwencje | ![](<../images/8-1/3/03 node- sequence.jpg>) | ![](<../images/8-1/3/03 codeblock- sequence.jpg>) |
| Przedziały | ![](<../images/8-1/3/04 node- range.jpg>) | ![](<../images/8-1/3/04 codeblock - range.jpg>) |
| Pobierz element o indeksie | ![](<../images/8-1/3/05 node - list get item.jpg>) | ![](<../images/8-1/3/05 codeblock - list get item.jpg>) |
| Utwórz listę | ![](<../images/8-1/3/06 node - list create.jpg>) | ![](<../images/8-1/3/06 codeblock - list create.jpg>) |
| Scal ciągi | ![](<../images/8-1/3/07 node - string concat.jpg>) | ![](<../images/8-1/3/07 codeblock - string concat.jpg>) |
| Instrukcje warunkowe | ![](<../images/8-1/3/08 node - conditional.jpg>) | ![](<../images/8-1/3/08 codeblock - conditional.jpg>) |

### Dodatkowa składnia

|                                     |                           |                                                                                          |
| ----------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------- |
| **Węzły** | **Odpowiednik w bloku kodu** | **Uwagi** |
| Dowolny operator (+, &&, >=, Not itp.) | +, &&, >=, ! itp. | Uwaga: „Not” staje się „!”, ale węzeł nazywa się „Not”, aby odróżnić go od „Factorial” |
| Wartość logiczna True | true; | Uwaga: małe litera |
| Wartość logiczna False | false; | Uwaga: małe litera |

### Zakresy i sekwencje

Metoda definiowania zakresów i sekwencji może zostać zredukowana do krótkiej składni. Poniższa ilustracja przedstawia składnię „..”, która umożliwia definiowanie listy danych liczbowych za pomocą bloku kodu. Po zaznajomieniu się z tą notacją tworzenie danych liczbowych jest bardzo wydajnym procesem:

![](<../images/8-1/3/shorthand - ranges and sequences.jpg>)

> 1. W tym przykładzie zakres liczb zostaje zastąpiony podstawową składnią węzła **Code Block** definiującą `beginning..end..step-size;`. Liczbowo będzie to: `0..10..1;`
> 2. Warto zauważyć, że składnia `0..10..1;` odpowiada `0..10;` Wielkość kroku równa 1 jest domyślną wartością w krótkiej składni. Dlatego `0..10;` daje sekwencję od 0 do 10 o kroku 1.
> 3. Przykład _Sequence_ jest podobny, ale do ustawienia 15 wartości na liście używamy znaku „#” zamiast listy do 15. W tym przypadku definiujemy: `beginning..#ofSteps..step-size:` Rzeczywista składnia dla tej sekwencji to `0..#15..2`
> 4. Używając znaku _„#”_ z poprzedniego kroku, umieścimy go teraz w części _„rozmiar-kroku”_ składni. Teraz mamy _zakres liczb_ (Number Range) od początku _„beginning”_ do końca _„end”_ z ustalonym rozmiarem kroku _„step-size”_, co powoduje równomierne rozmieszczenie wartości między dwoma punktami: `beginning..end..#ofSteps`

### Zakresy zaawansowane

Tworzenie zakresów zaawansowanych pozwala na łatwe korzystanie z listy list. W poniższych przykładach wyodrębniamy zmienną z notacji zakresu głównego i tworzymy inny zakres tej listy.

![](<../images/8-1/3/shorthand - advance range 01.jpg>)

> 1\. Tworząc zakresy zagnieżdżone, porównaj notację z „#” z notacją bez tego znaku. Zastosowanie ma ta sama logika co w zakresach podstawowych, choć rozwiązanie jest nieco bardziej złożone.
>
> 2\. Możemy zdefiniować zakres podrzędny w dowolnym miejscu w zakresie głównym i możemy też używać dwóch zakresów podrzędnych.
>
> 3\. Sterując wartością „końca” w zakresie, tworzymy więcej zakresów o różnych długościach.

W ramach ćwiczenia logicznego porównaj dwie wersje krótkiej składni i spróbuj przeanalizować, w jaki sposób _zakresy podrzędne_ i notacja _#_ wpływają na wynik na wyjściu.

![](<../images/8-1/3/shorthand - advance range 02.jpg>)

### Tworzenie list i pobieranie elementów z listy

Poza tworzeniem list za pomocą krótkiej składni możemy również tworzyć listy na bieżąco. Te listy mogą zawierać szeroki zakres typów elementów i można stosować do nich zapytania (należy pamiętać, że listy to także obiekty). Podsumowując: blok kodu pozwala tworzyć listy i stosować zapytania o elementy z listy za pomocą nawiasów kwadratowych:

![](<../images/8-1/3/shorthand - list & get from list 01.jpg>)

> 1\. Szybko twórz listy za pomocą ciągów i stosuj do nich zapytania, korzystając z indeksu elementu.
>
> 2\. Twórz listy ze zmiennymi i stosuj do nich zapytania za pomocą notacji krótkiej składni zakresu.

Zarządzanie z listami zagnieżdżonymi jest podobnym procesem. Pamiętaj o kolejności listy i o korzystaniu z wielu zestawów nawiasów kwadratowych:

![](<../images/8-1/3/shorthand - list & get from list 02.jpg>)

> 1\. Zdefiniuj listę list.
>
> 2\. Zastosuj zapytanie do listy za pomocą notacji z jedną parą nawiasów kwadratowych.
>
> 3\. Zastosuj zapytanie do elementu za pomocą notacji z dwiema parami nawiasów kwadratowych.

## Ćwiczenie: powierzchnia sinusoidalna

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/8-1/3/Obsolete-Nodes_Sine-Surface.dyn" %}

W tym ćwiczeniu przećwiczymy nowe umiejętności dotyczące krótkiej składni, aby utworzyć „jajowatą” powierzchnię zdefiniowaną przez zakresy i formuły. W trakcie ćwiczenia zwróć uwagę na to, w jaki sposób używane są blok kodu i istniejące węzły Dynamo: blok kodu jest używany do złożonej obsługi danych, natomiast węzły Dynamo są rozmieszczone wizualnie w celu zapewnienia czytelności definicji.

Rozpocznij od utworzenia powierzchni przez połączenie powyższych węzłów. Zamiast używać węzła number do zdefiniowania szerokości i długości, kliknij dwukrotnie obszar rysunku i wpisz `100;` w bloku kodu.

![](<../images/8-1/3/shorthand - exercise 01.jpg>)

![](<../images/8-1/3/shorthand - exercise 02.jpg>)

> 1. Zdefiniuj zakres od 0 do 1 z 50 podziałami, wpisując `0..1..#50` w węźle **Code Block**.
> 2. Połącz ten zakres z węzłem **Surface.PointAtParameter**, który pobiera wartości u i v z zakresu od 0 do 1 na powierzchni. Pamiętaj, aby zmienić skratowanie na Iloczyn wektorowy, klikając prawym przyciskiem myszy węzeł **Surface.PointAtParameter**.

W tym kroku użyjemy pierwszej funkcji do przesunięcia siatki punktów w górę na osi Z. Ta siatka będzie sterować generowaną powierzchnią na podstawie funkcji źródłowej. Dodaj nowe węzły, jak pokazano na ilustracji poniżej

![](<../images/8-1/3/shorthand - exercise 03.jpg>)

> 1. Zamiast używać węzła formuły, użyjemy węzła **Code Block** z wierszem: `(0..Math.Sin(x*360)..#50)*5;`. Krótkie objaśnienie: definiujemy zakres z formułą w jego wnętrzu. Ta formuła jest funkcją sinus. Funkcja sinus przyjmuje w dodatku Dynamo dane wejściowe w stopniach, więc aby uzyskać pełny kształt funkcji sinus, należy przemnożyć wartości x (jest to wejście zakresu od 0 do 1) przez 360. Następnie chcemy uzyskać taką samą liczbę podziałów, ile jest punktów siatki sterującej dla każdego wiersza, dlatego zdefiniujemy pięćdziesiąt podziałów podrzędnych za pomocą instrukcji #50. Na koniec: mnożnik 5 po prostu zwiększa amplitudę przekształcenia, dzięki czemu możemy zobaczyć efekt w podglądzie Dynamo.

![](<../images/8-1/3/shorthand - exercise 04.jpg>)

> 1. Mimo że poprzedni węzeł **Code Block** działał dobrze, nie był całkowicie parametryczny. Chcemy dynamicznie sterować jego parametrami, dlatego zastąpimy wiersz z poprzedniego kroku wierszem `(0..Math.Sin(x*360*cycles)..#List.Count(x))*amp;`. Daje to możliwość zdefiniowania tych wartości na podstawie wejść.

Zmieniając suwaki (w zakresie od 0 do 10), otrzymujemy interesujące wyniki.

![](<../images/8-1/3/shorthand - exercise 05.gif>)

![](<../images/8-1/3/shorthand - exercise 06.jpg>)

> 1. Transponując zakres liczb, odwrócimy kierunek fali kurtynowej: `transposeList = List.Transpose(sineList);`

![](<../images/8-1/3/shorthand - exercise 07.jpg>)

> 1. Po dodaniu wartości sineList i transposeList uzyskujemy zniekształconą „jajowatą” powierzchnię: `eggShellList = sineList+transposeList;`

Zmienimy wartości suwaków określone poniżej, aby zmniejszyć zniekształcenia tworzone przez ten algorytm.

![](<../images/8-1/3/shorthand - exercise 08.jpg>)

Na koniec zastosujmy zapytania do wyodrębnionych części danych za pomocą węzła Code Block. Aby ponownie wygenerować powierzchnię za pomocą określonego zakresu punktów, dodaj blok kodu powyżej między węzłami **Geometry.Translate** i **NurbsSurface.ByPoints**. Będzie on zawierać wiersz tekstu: `sineStrips[0..15..1];`. Spowoduje to wybranie pierwszych 16 wierszy punktów (spośród 50). Po ponownym utworzeniu powierzchni widać, że wygenerowaliśmy wyodrębnioną część siatki punktów.

![](<../images/8-1/3/shorthand - exercise 09.jpg>)

![](<../images/8-1/3/shorthand - exercise 10.jpg>)

> 1. W ostatnim kroku, aby uczynić ten węzeł **Code Block** bardziej parametrycznym, będziemy sterować zapytaniem za pomocą suwaka o zakresie od 0 do 1. W tym celu dodamy ten wiersz kodu: `sineStrips[0..((List.Count(sineStrips)-1)*u)];`. Może to wydawać się skomplikowane, ale ten wiersz kodu pozwala szybko przeskalować długość listy do mnożnika z zakresu od 0 do 1.

Wartość `0.53` na suwaku powoduje utworzenie powierzchni tuż za punktem środkowym siatki.

![](<../images/8-1/3/shorthand - exercise 11.jpg>)

Zgodnie z oczekiwaniami wartość `1` na suwaku tworzy powierzchnię z pełnej siatki punktów.

![](<../images/8-1/3/shorthand - exercise 12.jpg>)

Przyglądając się wykresowi wizualnemu, możemy wyróżnić bloki kodu i przejrzeć ich poszczególne funkcje.

![](<../images/8-1/3/shorthand - exercise 13.jpg>)

> 1\. Pierwszy węzeł **Code Block** zastępuje węzeł **Numer**.
>
> 2\. Drugi węzeł **Code Block** zastępuje węzeł **Number Range**.
>
> 3\. Trzeci węzeł **Code Block** zastępuje węzeł **Formula** (jak również węzły **List.Transpose**, **List.Count** i **Number Range**).
>
> 4\. Czwarty węzeł **Code Block** stosuje zapytania do listy list, zastępując węzeł **List.GetItemAtIndex**.
