# Wybieranie

### Wybierz elementy programu Revit

Program Revit to środowisko bogate w dane. Daje to wiele możliwości wybierania, znacznie wykraczających poza metodę „wskaż i kliknij”. Można wykonać zapytanie do bazy danych programu Revit i dynamicznie połączyć elementy programu Revit z geometrią dodatku Dynamo podczas wykonywania operacji parametrycznych.

Biblioteka programu Revit w interfejsie użytkownika zawiera kategorię „Selection”, która umożliwia wybieranie geometrii na wiele sposobów.

![](../.gitbook/assets/select\_revit\_elements\_01.jpg)

### Hierarchia programu Revit

Aby prawidłowo wybrać elementy programu Revit, należy w pełni rozumieć hierarchię elementów programu Revit. Chcesz wybrać wszystkie ściany w projekcie? Wybierz według kategorii. Chcesz wybrać wszystkie fotele Eamesa w holu urządzonym w stylu z połowy XX wieku? Wybierz według rodziny.

Omówimy pokrótce hierarchię programu Revit.

![](images/2/hierarchy.png)

Pamiętasz systematykę organizmów z biologii? Królestwo, typ, gromada, rząd, rodzina, rodzaj, gatunek? Elementy programu Revit są skategoryzowane podobnie. Na poziomie podstawowym hierarchię programu Revit można podzielić na kategorie, rodziny, typy* i wystąpienia. Wystąpienie to pojedynczy element modelu (z unikatowym identyfikatorem), a kategoria definiuje ogólną grupę (na przykład „ściany” czy „stropy”). Dzięki uporządkowaniu bazy danych programu Revit w ten sposób można wybrać jeden element, a następnie wybrać wszystkie podobne elementy na podstawie określonego poziomu w hierarchii.

{% hint style="warning" %} *Uwaga — typy w programie Revit definiuje się inaczej niż typy w programowaniu. Typ w programie Revit oznacza gałąź hierarchii, a nie „typ danych”. {% endhint %}

### Nawigacja w bazach danych za pomocą węzłów Dynamo

Trzy poniższe ilustracje przedstawiają główne kategorie wyboru elementów programu Revit w dodatku Dynamo. To narzędzia, które można doskonale łączyć, a w kolejnych ćwiczeniach omówimy niektóre z nich.

Najprostszym sposobem bezpośredniego wybrania elementu programu Revit jest _wskazanie i kliknięcie go_. Można wybrać cały element modelu lub części jego topologii (na przykład ścianę lub krawędź). Pozostają one dynamicznie połączone z tym obiektem programu Revit, więc gdy w pliku programu Revit zmieni się jego położenie lub parametry, odpowiedni element dodatku Dynamo zostanie zaktualizowany na wykresie.

![](../.gitbook/assets/selecting\_database\_navigation\_with\_dynamo\_nodes\_01.jpg)

_Menu rozwijane_ zawierają listę wszystkich dostępnych elementów w projekcie programu Revit. Można ich użyć, aby tworzyć odniesienia do elementów programu Revit, które niekoniecznie są widoczne w widoku. Jest to doskonałe narzędzie do wykonywania zapytań dotyczących istniejących elementów oraz tworzenia nowych w projekcie programu Revit lub edytorze rodzin.

\![](../.gitbook/assets/selecting _database_navigation_with_dynamo_nodes_02.png)

Można również wybrać element programu Revit na podstawie określonych poziomów w _hierarchii programu Revit_. Jest to zaawansowana opcja umożliwiająca dostosowywanie dużych zestawów danych w przygotowaniu do tworzenia dokumentacji lub generacyjnego tworzenia i dostosowywania wystąpień.

![Interfejs użytkownika](../.gitbook/assets/allelements.jpg)

Pamiętając o trzech powyższych ilustracjach, przejdźmy do ćwiczenia polegającego na wybieraniu elementów z podstawowego projektu programu Revit w przygotowaniu do zastosowań parametrycznych, które będziemy tworzyć w pozostałych sekcjach tego rozdziału.

## Ćwiczenie

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="datasets/2/Revit-Selecting.zip" %}

Ten plik przykładowy programu Revit zawiera trzy typy elementów prostego budynku. Użyjemy tego przykładu do wybierania elementów programu Revit w kontekście hierarchii programu Revit.

![](../.gitbook/assets/selecting\_exercise\_01.jpg)

> 1. Bryła budynku
> 2. Belki (ramy konstrukcyjne)
> 3. Kratownice (komponenty adaptacyjne)

Jakie wnioski można wyciągnąć z elementów aktualnie widocznych w widoku projektu programu Revit? Do jakiego poziomu hierarchii musimy dojść, aby wybrać odpowiednie elementy? To zadanie będzie oczywiście bardziej złożone podczas pracy nad dużym projektem. Dostępnych jest wiele opcji: można wybierać elementy według kategorii, poziomów, rodzin, wystąpień i tak dalej.

### Wybieranie brył i powierzchni

![](../.gitbook/assets/selecting\_exercise\_02.jpg)

> 1. Ponieważ pracujemy z podstawową konfiguracją, wybierzmy bryłę budynku, wybierając opcję _„Mass”_ z menu rozwijanego w węźle Categories. Można go znaleźć na karcie Revit>Selection.
> 2. Wynikiem kategorii Mass jest sama ta kategoria. Musimy wybrać elementy. W tym celu użyjemy węzła _„All Elements of Category”_.

Na tym etapie można zauważyć, że w dodatku Dynamo nie widać żadnej geometrii. Wybraliśmy element programu Revit, ale nie przekonwertowaliśmy go na geometrię dodatku Dynamo. To ważne rozróżnienie. Wybierając dużą liczbę elementów, nie chcesz wyświetlać ich wszystkich w dodatku Dynamo, ponieważ spowolniłoby to działanie programu. Dodatek Dynamo jest narzędziem umożliwiającym zarządzanie projektem programu Revit bez konieczności wykonywania operacji na geometrii. Przyjrzymy się temu w następnej sekcji tego rozdziału.

W tym przypadku pracujemy z prostą geometrią, dlatego włączymy jej podgląd w dodatku Dynamo. Obok elementu „BldgMass” w powyższym węźle Watch znajduje się zielony numer. Oznacza on identyfikator elementu i informuje, że jest to element programu Revit, a nie geometria dodatku Dynamo. Następnym krokiem jest przekształcenie tego elementu programu Revit w geometrię dodatku Dynamo.

![](../.gitbook/assets/selecting\_exercise\_03.jpg)

> 1. Przy zastosowaniu węzła _Element.Faces_ otrzymujemy listę powierzchni reprezentujących poszczególne ściany bryły programu Revit. Teraz można wyświetlić geometrię w rzutni Dynamo i odwoływać się do tych ścian w celu wykonania operacji parametrycznych.

Oto alternatywna metoda. W tym przypadku zamiast wybierać za pomocą hierarchii programu Revit _(„All Elements of Category”)_, będziemy jawnie wybierać geometrię w programie Revit.

![](../.gitbook/assets/selecting\_exercise\_04.jpg)

> 1. W węźle _„Select Model Element”_ kliknij przycisk *„select”*(lub _„change”_). W rzutni programu Revit wybierz żądany element. W tym przypadku wybieramy bryłę budynku.
> 2. Zamiast węzła _Element.Faces_ można użyć węzła _Element.Geometry_, aby wybrać całą bryłę jako jedną geometrię. Spowoduje to wybranie całej geometrii zawartej w tej bryle.
> 3. Używając węzła _Geometry.Explode_, można ponownie otrzymać listę powierzchni. Te dwa węzły działają tak samo jak węzeł _Element.Faces_, ale oferują alternatywne opcje wybierania geometrii elementu programu Revit.

Za pomocą podstawowych operacji listy można utworzyć zapytanie o interesującą nas ścianę.

\![](images/2/selecting - exercise 05.jpg)

> 1. Najpierw wyprowadź wybrane elementy z wcześniejszego węzła Element.Faces.
> 2. Następnie w węźle _List.Count_ widzimy, że pracujemy z 23 powierzchniami w ramach bryły.
> 3. Zgodnie z tą liczbą zmieniamy maksymalną wartość w węźle *Integer Slider *na _22_.
> 4. W węźle _List.GetItemAtIndex_ wprowadzamy listy oraz łączymy węzeł *Integer Slider *z elementem _index_. Na suwaku zmieniamy wybrany element, zatrzymując się po przejściu do _indeksu 9_ i wyizolowaniu głównej fasady z kratownicami.

Poprzedni krok był mało wydajny. Możemy to zrobić znacznie szybciej, korzystając z węzła _„Select face”_. Dzięki temu można wyizolować ścianę, która sama nie jest elementem w projekcie programu Revit. Wykonujemy takie samo działanie jak w przypadku węzła _„Select Model Element”_, z tym że wybieramy powierzchnię, a nie cały element.

![](../.gitbook/assets/selecting\_exercise\_06.jpg)

Załóżmy, że chcemy wyizolować główne ściany fasadowe budynku. W tym celu można użyć węzła _„Select Faces”_. Kliknij przycisk „Select”, a następnie wybierz cztery główne fasady w programie Revit.

![](../.gitbook/assets/selecting\_exercise\_07.jpg)

Po wybraniu czterech ścian pamiętaj, aby kliknąć przycisk „Finish” w programie Revit.

![](../.gitbook/assets/selecting\_exercise\_08.jpg)

Ściany zostały teraz zaimportowane do dodatku Dynamo jako powierzchnie.

![](../.gitbook/assets/selecting\_exercise\_09.jpg)

### Wybieranie belek

Spójrzmy teraz na belki nad atrium.

![](../.gitbook/assets/selecting\_exercise\_10.jpg)

> 1. Za pomocą węzła _„Select Model Element”_ wybierz jedną z belek.
> 2. Połącz element belki z węzłem _Element.Geometry_, aby wyświetlić belkę w rzutni dodatku Dynamo.
> 3. Można powiększyć geometrię za pomocą węzła _Watch3D_ (jeśli belka nie jest widoczna w widoku 3D, kliknij prawym przyciskiem myszy i wybierz polecenie „Dopasuj do okna”).

Częste pytanie w procesach roboczych Revit/Dynamo brzmi: jak wybrać jeden element i otrzymać wszystkie podobne elementy? Ponieważ wybrany element programu Revit zawiera wszystkie informacje o hierarchii, można użyć zapytania o typ rodziny i wybrać wszystkie elementy tego typu.

![](../.gitbook/assets/selecting\_exercise\_11.jpg)

> 1. Połącz element belki z węzłem _Element.ElementType_.
> 2. W węźle _Watch_ widać, że wynik to teraz symbol rodziny, a nie element programu Revit.
> 3. _Element.ElementType_ to proste zapytanie, więc możemy równie łatwo użyć bloku kodu z poleceniem `x.ElementType;`, aby uzyskać takie same wyniki.

![](../.gitbook/assets/selecting\_exercise\_12.jpg)

> 1. Aby wybrać pozostałe belki, należy użyć węzła _„All Elements of Family Type”_.
> 2. W węźle Watch widać, że wybraliśmy pięć elementów programu Revit.

![](../.gitbook/assets/selecting\_exercise\_13.jpg)

> 1. Można również przekonwertować wszystkie pięć elementów na geometrię dodatku Dynamo.

A co jeśli mamy 500 belek? Konwertowanie wszystkich tych elementów na geometrię dodatku Dynamo trwałoby bardzo długo. Jeśli w dodatku Dynamo obliczanie węzłów trwa zbyt długo, można użyć funkcji zablokowania węzła, aby wstrzymać wykonywanie operacji programu Revit podczas tworzenia wykresu. Aby uzyskać więcej informacji na temat blokowania węzłów, zobacz sekcję „[Blokowanie](../essential-nodes-and-concepts/5\_geometry-for-computational-design/5-6\_solids.md#freezing)” w rozdziale poświęconym bryłom.

Czy importując 500 belek, potrzebujemy wszystkich powierzchni do wykonania zamierzonej operacji parametrycznej? Czy możemy wyodrębnić podstawowe informacje z belek i wykonać zadania generacyjne z użyciem podstawowej geometrii? To pytanie, które będziemy mieć na uwadze w dalszej części tego rozdziału. Przyjrzyjmy się na przykład układowi kratownic.

### Wybieranie kratownic

Używając tego samego wykresu węzłów, wybierz element kratownicy zamiast elementu belki. Przed wykonaniem tej czynności usuń węzeł Element.Geometry z poprzedniego kroku.

![](../.gitbook/assets/selecting\_exercise\_14.jpg)

Następnie możemy wyodrębnić niektóre podstawowe informacje z typu rodziny kratownic.

![](../.gitbook/assets/selecting\_exercise\_15.jpg)

> 1. W węźle _Watch_ widać, że otrzymaliśmy listę komponentów adaptacyjnych wybranych z programu Revit. Chcemy wyodrębnić podstawowe informacje, dlatego zaczniemy od punktów adaptacyjnych.
> 2. Połącz węzeł _„All Elements of Family Type”_ z węzłem _„AdaptiveComponent.Location”_. Otrzymamy listę list, z których każda zawiera trzy punkty reprezentujące położenie punktów adaptacyjnych.
> 3. Po połączeniu z węzłem _„Polygon.ByPoints”_ otrzymamy krzywą PolyCurve. Widać to w rzutni dodatku Dynamo. Za pomocą tej metody zwizualizowaliśmy geometrię jednego elementu i wyabstrahowaliśmy geometrię pozostałych elementów (których może być więcej niż w tym przykładzie).

{% hint style="info" %} Wskazówka: po kliknięciu zielonego numeru elementu programu Revit w dodatku Dynamo ten element zostanie powiększony w rzutni programu Revit. {% endhint %}
