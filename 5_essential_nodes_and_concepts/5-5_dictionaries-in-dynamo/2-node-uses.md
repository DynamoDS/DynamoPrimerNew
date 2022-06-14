# Węzły słownika

Dodatek Dynamo 2.0 udostępnia różne węzły słownika do wykorzystania. Obejmuje to węzły _tworzenia, operacji i zapytań_.

![](<../images/5-5/2/dictionary nodes - nodes.jpg>)

#### Tworzenie

1. Węzeł `Dictionary.ByKeysValues` tworzy słownik z określonymi wartościami i kluczami. _(Liczba pozycji będzie zgodna z liczbą pozycji na najkrótszej liście wejściowej)_

#### Action

2\. Węzeł `Dictionary.Components` tworzy składniki słownika wejściowego. _(Jest to operacja odwrotna do operacji węzła tworzenia)._

3\. Węzeł `Dictionary.RemoveKeys` tworzy nowy obiekt słownika z usuniętymi kluczami wejściowymi.

4\. Węzeł `Dictionary.SetValueAtKeys` tworzy nowy słownik na podstawie wejściowych kluczy i wartości zastępujących bieżące wartości dla odpowiednich kluczy.

5\. Węzeł `Dictionary.ValueAtKey` zwraca wartość dla klucza wejściowego.

#### Ilość

6\. Węzeł `Dictionary.Count` zwraca liczbę par wartości i kluczy w słowniku.

7\. Węzeł `Dictionary.Keys` zwraca aktualnie przechowywane w słowniku klucze.

8\. Węzeł `Dictionary.Values` zwraca aktualnie przechowywane w słowniku wartości.

Ogólnie powiązywanie danych ze słownikami stanowi świetną alternatywę dla starej metody pracy z indeksami i listami.
