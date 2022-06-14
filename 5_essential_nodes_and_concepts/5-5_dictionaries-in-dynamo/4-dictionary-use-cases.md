# Przypadki zastosowań w programie Revit

Czy kiedykolwiek zdarzyło Ci się wyszukiwać coś w programie Revit za pomocą zawartych w nim danych?

W takim przypadku zwykle robi się to jak w poniższym przykładzie.

Na poniższej ilustracji zbieramy wszystkie pomieszczenia w modelu programu Revit, uzyskujemy indeks żądanego pomieszczenia (według numeru pomieszczenia), a następnie pobieramy pomieszczenie o danym indeksie.

![](<../images/5-5/4/dictionary - collect room in revit model.jpg>)

> 1. Zbierz wszystkie pomieszczenia w modelu.
> 2. Numer pomieszczenia do znalezienia.
> 3. Pobierz numer pomieszczenia i znajdź indeks, w którym się ono znajduje.
> 4. Uzyskaj pomieszczenie o określonym indeksie.

## Ćwiczenie: słownik pomieszczeń

### Część I. Tworzenie słownika pomieszczeń

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/5-5/4/roomDictionary.dyn" %}

Teraz odtwórzmy ten pomysł, używając słowników. Najpierw musimy zebrać wszystkie pomieszczenia w modelu programu Revit.

![](<../images/5-5/4/dictionary - exercise I - 01.jpg>)

> 1. Wybieramy kategorię programu Revit, z którą chcemy pracować (w tym przypadku pracujemy z pomieszczeniami).
> 2. Zlecamy dodatkowi Dynamo zebranie wszystkich tych elementów.

Następnie musimy zdecydować, jakich kluczy użyjemy do wyszukiwania tych danych. (Informacje na temat kluczy można znaleźć w sekcji [Co to jest słownik?](9-1\_what-is-a-dictionary.md)).

![](<../images/5-5/4/dictionary - exercise I - 02.jpg>)

> 1. Dane, których użyjemy, to numer pomieszczenia.

Teraz utworzymy słownik z danymi kluczami i elementami.

![](<../images/5-5/4/dictionary - exercise I - 03.jpg>)

> 1. Węzeł **Dictionary.ByKeysValues** utworzy słownik na podstawie odpowiednich danych wejściowych.
> 2. `Keys` muszą być ciągami, a `values` mogą być różnymi typami obiektów.

Na koniec możemy pobrać pomieszczenie ze słownika za pomocą jego numeru.

![](<../images/5-5/4/dictionary - exercise I - 04.jpg>)

> 1. `String` będzie kluczem, który jest używany do wyszukania obiektu w słowniku.
> 2. Teraz węzeł **Dictionary.ValueAtKey** pobierze obiekt ze słownika.

### Część II. Wyszukiwanie wartości

Używając tej samej logiki słowników, można także tworzyć słowniki ze zgrupowanymi obiektami. Jeśli chcemy wyszukać wszystkie pomieszczenia na danym poziomie, możemy zmienić powyższy wykres w następujący sposób.

![](<../images/5-5/4/dictionary - exercise II - 01.jpg>)

> 1. Zamiast używać jako klucza numeru pomieszczenia, możemy teraz użyć wartości parametru (w tym przypadku użyjemy poziomu).

![](<../images/5-5/4/dictionary - exercise II - 02.jpg>)

> 1. Teraz możemy pogrupować pomieszczenia według poziomu, na którym się znajdują.

![](<../images/5-5/4/dictionary - exercise II - 03.jpg>)

> 1. Po pogrupowaniu elementów według poziomów możemy używać wspólnych (niepowtarzalnych) kluczy jako kluczy w słowniku, a list pomieszczeń jako elementów.

![](<../images/5-5/4/dictionary - exercise II - 04.jpg>)

> 1. Na koniec, korzystając z poziomów w modelu programu Revit, możemy sprawdzić w słowniku, które pomieszczenia znajdują się na danym poziomie. Węzeł `Dictionary.ValueAtKey` pobierze nazwę poziomu i zwróci obiekty pomieszczeń na tym poziomie.

Możliwości zastosowań słowników są naprawdę nieograniczone. Już sama możliwość powiązywania danych BIM w programie Revit z elementem zapewnia wiele różnych przypadków zastosowań.
