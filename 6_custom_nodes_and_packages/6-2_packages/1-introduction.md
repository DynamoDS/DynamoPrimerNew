# Pakiet — wprowadzenie

W skrócie: pakiet jest zbiorem węzłów niestandardowych. Dynamo Package Manager to portal dla społeczności umożliwiający pobranie dowolnego pakietu, który został opublikowany online. Te zestawy narzędzi zostały opracowane przez strony trzecie w celu rozszerzenia podstawowej funkcjonalności dodatku Dynamo i są dostępne dla wszystkich. Są gotowe do pobrania po kliknięciu przycisku.

![Witryna Menedżera pakietów](../images/6-2/1/dpm.jpg)

Projekt open-source, taki jak dodatek Dynamo, rozwija się dzięki takiemu zaangażowaniu społeczności. Dzięki zaangażowanym programistom zewnętrznym dodatek Dynamo może rozszerzyć zasięg na procesy robocze w różnych branżach. Z tego powodu zespół dodatku Dynamo podjął skoncentrowane wysiłki w celu usprawnienia opracowywania i publikowania pakietów (zostanie to omówione bardziej szczegółowo w kolejnych sekcjach).

### Instalowanie pakietu

Najprostszym sposobem instalacji pakietu jest użycie paska narzędzi Pakiety w interfejsie dodatku Dynamo. Przejdźmy od razu do rzeczy i zainstalujmy pakiet teraz. W tym szybkim przykładzie zainstalujemy popularny pakiet umożliwiający tworzenie paneli czworokątnych na siatce.

W dodatku Dynamo przejdź do obszaru _Pakiety > Wyszukaj pakiet_.

![](<../images/6-2/1/package introduction - installing a package 01.jpg>)

Na pasku wyszukiwania wyszukaj frazę „quads from rectangular grid”. Po kilku chwilach powinny pojawić się wszystkie pakiety zgodne z tym zapytaniem. Wybierzmy pierwszy pakiet z pasującą nazwą.

Kliknij przycisk Zainstaluj, aby dodać ten pakiet do biblioteki. Gotowe.

![](<../images/6-2/1/package introduction - installing a package 02.jpg>)

Zwróć uwagę, że w bibliotece Dynamo pojawiła się kolejna grupa o nazwie „buildz”. Ta nazwa odnosi się do programisty pakietu, a węzeł niestandardowy zostaje umieszczony w tej grupie. Możemy od razu zacząć z niego korzystać.

![](<../images/6-2/1/package introduction - installing a package 03.jpg>)

Użyj węzła **Code Block**, aby szybko zdefiniować prostokątną siatkę, zapisać wynik w węźle **Polygon.ByPoints**, a następnie użyj węzła **Surface.ByPatch**, aby wyświetlić listę właśnie utworzonych prostokątnych paneli.

![](<../images/6-2/1/package introduction - installing a package 04.jpg>)

### Instalowanie folderu pakietu — DynamoUnfold

W powyższym przykładzie skupiono się na pakiecie z jednym węzłem niestandardowym, ale ten sam proces jest używany do pobierania pakietów z kilkoma węzłami niestandardowymi i plikami danych pomocniczych. Zademonstrujmy to teraz z wszechstronniejszym pakietem: Dynamo Unfold.

Tak jak w przykładzie powyżej, rozpocznij od wybrania opcji _Pakiety > Wyszukaj pakiet_.

Tym razem poszukamy jednego słowa, _„DynamoUnfold”_, zwracając uwagę na wielkość liter. Po wyświetleniu pakietów pobierz je, klikając przycisk Zainstaluj, aby dodać składniki Dynamo Unfold do biblioteki Dynamo.

![](<../images/6-2/1/package introduction - installing package folder 01.jpg>)

W bibliotece Dynamo dostępna jest grupa _DynamoUnfold_ z wieloma kategoriami i węzłami niestandardowymi.

![](<../images/6-2/1/package introduction - installing package folder 02.jpg>)

Spójrzmy teraz na strukturę plików pakietu. Najpierw wybierz opcję Dynamo > Preferencje

![](<../images/6-2/1/package introduction - installing package folder 03.jpg>)

W oknie podręcznym Preferencje otwórz Menedżera pakietów > obok opcji DynamoUnfold wybierz menu w postaci pionowych kropek ![](<../images/6-2/1/package introduction - vertical dots menu.jpg>) > Pokaż katalog główny, aby otworzyć folder główny dla tego pakietu.

![](<../images/6-2/1/package introduction - installing package folder 04.jpg>)

Spowoduje to przejście do katalogu głównego pakietu. Zwróć uwagę, że mamy 3 foldery i plik.

![](<../images/6-2/1/package introduction - installing package folder 05.jpg>)

> 1. Folder _bin_ zawiera pliki .dll. Ten pakiet Dynamo został opracowany przy użyciu narzędzia Zero-Touch, więc węzły niestandardowe są przechowywane w tym folderze.
> 2. Folder _dyf_ zawiera węzły niestandardowe. Ten pakiet nie został opracowany przy użyciu węzłów niestandardowych Dynamo, dlatego ten folder jest w przypadku tego pakietu pusty.
> 3. Ten dodatkowy folder zawiera wszystkie dodatkowe pliki, w tym pliki przykładowe.
> 4. Plik pkg jest podstawowym plikiem tekstowym definiującym ustawienia pakietu. Na razie możemy to zignorować.

Po otwarciu folderu „extra” zostaje wyświetlona seria plików przykładowych, które zostały pobrane wraz z instalacją. Nie wszystkie pakiety zawierają pliki przykładowe, ale jeśli są one częścią pakietu, można je znaleźć tutaj.

Otwórzmy plik „SphereUnfold”.

![](../images/6-2/1/rd2.jpg)

Po otwarciu pliku i naciśnięciu przycisku „Uruchom” w solwerze dostępna jest rozwinięta sfera. Pliki przykładowe są przydatne do nauki pracy z nowym pakietem Dynamo.

![](<../images/6-2/1/package introduction - installing package folder 07.jpg>)

### Menedżer pakietów Dynamo

Innym sposobem odkrywania pakietów Dynamo jest zapoznanie się z [Menedżerem pakietów Dynamo](http://dynamopackages.com) online. Jest to dobry sposób przeglądania w poszukiwaniu pakietów, ponieważ repozytorium sortuje pakiety w kolejności ich pobierania i popularności. Jest również łatwym sposobem na gromadzenie informacji o najnowszych aktualizacjach pakietów, ponieważ w przypadku niektórych pakietów Dynamo ma zastosowanie obsługa wersji i zależności kompilacji dodatku Dynamo.

Klikając opcję _„Quads from Rectangular Grid”_ w Menedżerze pakietów Dynamo, można wyświetlić odpowiednie opisy, wersje, informacje o programiście i możliwe zależności.

![](../images/6-2/1/dpm2.jpg)

Pliki pakietu można również pobrać z Menedżera pakietów Dynamo, ale robienie tego bezpośrednio z poziomu dodatku Dynamo stanowi płynniejszy proces.

### Gdzie lokalnie są przechowywane pliki pakietów?

Jeśli pobierasz pliki z Menedżera pakietów Dynamo lub chcesz sprawdzić, gdzie są przechowywane wszystkie pliki pakietu, kliknij opcje Dynamo > Menedżer pakietów > Ścieżki do węzłów i pakietów, a następnie odszukaj bieżący katalog główny folderu.

![](<../images/6-2/1/package introduction - installing package folder 08.jpg>)

Domyślnie pakiety są instalowane w położeniu podobnym do tej ścieżki folderu: _C:/Users/\[nazwa_użytkownika]/AppData/Roaming/Dynamo/\[wersja dodatku Dynamo]_.

### Dalsze kroki z pakietami

Społeczność dodatku Dynamo stale rośnie i ewoluuje. Przeglądając Menedżera pakietów Dynamo od czasu do czasu, znajdziesz nowe interesujące rozwiązania. W poniższych sekcjach przyjrzymy się dokładniej pakietom: od perspektywy użytkownika końcowego po perspektywę twórcy własnego pakietu Dynamo.
