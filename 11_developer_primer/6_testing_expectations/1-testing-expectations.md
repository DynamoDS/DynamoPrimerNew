# Oczekiwania dotyczące testowania

Na tej stronie opisano, czego oczekujemy, jeśli chodzi o testowanie nowego kodu dodawanego do dodatku Dynamo.

Dlatego też Masz nowy węzeł, który chcesz dodać. Wspaniale. Nadszedł czas, aby dodać kilka testów. Warto to zrobić z dwóch powodów.

1. Pomaga to ustalić, gdzie rozwiązanie nie działa
2. Gdy ktoś inny coś zmieni i spowoduje uszkodzenie węzła, powinno to też uszkodzić testy. W ten sposób osoba, która uszkodziła testy, musi naprawić powstały błąd. Jeśli nie spowoduje to uszkodzenia testów, to w dużej mierze Twoim problemem będzie obsługa użytkowników, których modele zostaną uszkodzone.

Rodzaje testowania w dodatku Dynamo dzielą się na dwa ogólne typy: testy jednostkowe i testy systemowe.

## Testy jednostkowe

Testy jednostkowe powinny obejmować możliwie najmniejszy obszar. Jeśli utworzono węzeł, który oblicza pierwiastki kwadratowe za pomocą węzła C# Zero Touch, test powinien sprawdzać tylko ten plik DLL i wykonywać wywołania języka C# bezpośrednio do tego kodu. Testy jednostkowe w ogóle nie powinny obejmować dodatku Dynamo.

Powinny obejmować:

* Testy pozytywne (rozwiązanie robi to, co powinno)
* Testy negatywne (rozwiązanie nie zachowuje się dziwnie w razie przekazania do niego błędnych danych wejściowych)
* Testy regresji (gdy ktoś znajdzie błąd w kodzie, napisz test, aby upewnić się, że ten błąd się nie powtórzy)

Powinny być małe, szybkie i niezawodne. Większość testów powinna być testami jednostkowymi.

## Testy systemowe

Testy systemowe działają na wielu komponentach i sprawdzają, jak są one do siebie dopasowane. Powinny być używane i opracowywane ostrożnie. 

Dlaczego? Ich przeprowadzanie jest kosztowne. Musimy zadbać o to, aby zestaw testów działał z jak najmniejszym opóźnieniem.

Pisząc test systemowy, pierwszą rzeczą, jaką należy zrobić, jest spojrzenie na [ten](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) diagram i ustalenie, które podsystemy są uwzględniane.

Idealnie byłoby, gdyby istniała progresywna seria testów obejmująca coraz więcej zestawów oddziałujących ze sobą bloków. Dzięki temu, gdy testy zaczną kończyć się niepowodzeniami, bardzo szybko będzie można ustalić, gdzie leży problem.

Przykłady rzeczy, które wymagają testów systemowych:

* Nowy typ węzła programu Revit przechowujący w śladzie wiele elementów, a nie tylko jeden element
* Nowy węzeł obserwacyjny (watch), który wyświetla dane w inny sposób

Przykłady rzeczy, które nie wymagają testów systemowych:

* Nowy węzeł matematyczny
* Biblioteka przetwarzania ciągów

Testy systemowe powinny:

* Potwierdzać, że zachowanie jest poprawne
* Potwierdzać brak zachowań patologicznych, np. brak wyjątków