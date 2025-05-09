# Różnice między usługami obliczeniowymi dodatku Dynamo a dodatkiem Dynamo na komputerze

Na tej stronie przedstawiono różnice, o których należy pamiętać podczas pisania programów dodatku Dynamo do wykonywania w kontekście chmurowym usług obliczeniowych dodatku Dynamo.

## Czym jest DaaS?

DaaS (Dynamo as a Service), Dynamo jako usługa, usługa obliczeniowa dodatku Dynamo — wszystkie te określenia odnoszą się do tego samego: podstawowego środowiska wykonawczego dodatku Dynamo wykonywanego w kontekście chmury. Oznacza to, że wykres nie jest wykonywany na komputerze. Dostęp do usługi DaaS można obecnie uzyskać tylko za pośrednictwem rozszerzenia Dynamo Player dla programu Forma, w którym użytkownicy mogą przekazywać pliki `.dyn` utworzone w środowisku komputerowym i zarządzać nimi, uruchamiać pliki `.dyn` udostępnione przez współpracowników za pomocą tego rozszerzenia lub używać wstępnie wczytanych procedur `.dyn` dostarczonych przez firmę Autodesk jako przykładów.

Ponieważ wykresy są uruchamiane w tym kontekście chmury, a nie na komputerze, w usłudze DaaS nie można obecnie bezpośrednio używać tradycyjnych kontekstów programów nadrzędnych dodatku Dynamo (Revit, Civil 3D itp.). Aby użyć typów z tych programów na wykresie, należy je zserializować (zapisać) na wykresie za pomocą węzła `Data.Remember` lub innych technik serializacji na wykresu. Są one podobne do procesów roboczych, których należy użyć podczas pisania wykresów dla projektowania generatywnego w programie Revit.

## Która wersja dodatku Dynamo wykonuje mój kod?

Ta wersja jest oparta na wersji 3.x i jest często aktualizowana na podstawie gałęzi głównej open source dodatku Dynamo.

## Jakie pakiety/węzły są dostępne w tej wersji dodatku Dynamo?

* W przypadku większości węzłów podstawowych zapoznaj się z następną sekcją w celu poznania pewnych ograniczeń.
* Pakiet `DynamoFormaBeta` do interakcji z interfejsem API programu Forma.
* `VASA` do wokselizacji / wydajnej analizy.
* `MeshToolKit` do manipulowania siatkami. Od wersji 3.4 dodatku Dynamo zestaw narzędzi Mesh Toolkit jest również od razu dostępny w dodatku.
* `RefineryToolkit` na potrzeby przydatnych algorytmów, które umożliwiają testowanie pod kątem kolizji oraz obsługę odległości widoku, najkrótszej ścieżki, analizy widoczności itp.

## Na co należy zwrócić uwagę podczas tworzenia wykresów dla usługi DaaS?

* Węzły w języku Python nie będą działać. _Obecnie_ po prostu nie są one wykonywane.
* Nie można używać pakietów niestandardowych.
* Interfejs użytkownika/warstwa widoku węzłów interfejsu użytkownika nie są wykonywane. Nie przewidujemy, że spowoduje to problemy z podstawową funkcjonalnością, ale warto o tym pamiętać, jeśli pojawi się błąd z węzłem z niestandardowym interfejsem użytkownika.
* Funkcje charakterystyczne dla systemu Windows nie będą działać. Na przykład jeśli spróbujesz użyć rejestru systemu Windows lub platformy WPF, to się nie powiedzie.
* Rozszerzenia widoku nie zostaną wczytane.
* Węzły systemu plików nie będą zbyt przydatne. W przypadku uruchamiania w usłudze DaaS nie będą istnieć żadne pliki, do których odwołujesz się na komputerze lokalnym.
* Węzły zgodności operacyjnej programu Excel/DSOffice nie będą działać. Węzły Open XML powinny działać.
* Żądania sieciowe na ogół nie działają, ale można wysyłać wywołania do interfejsu API programu Forma.

## Jak mam to wszystko zapamiętać? A co, jeśli to się zmieni?

* W przyszłości zamierzamy udostępnić w dodatku Dynamo na komputerze narzędzia, które ułatwią zapewnienie takiego samego działania wykresu w obu kontekstach.

## Ile to kosztuje?

* W tej wersji beta nie pobieramy obecnie opłat za czas wykonywania obliczeń.

## Jak rozpocząć pracę?

Aby rozpocząć, zapoznaj się z [postem w blogu](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), [serią w serwisie YouTube](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) lub przykładami w rozszerzeniu programu Forma. Pomogą Ci one w następujących czynnościach:

* Uzyskiwanie dostępu do programu Autodesk Forma.
* Instalowanie dodatku DynamoFormaBeta dla dodatku Dynamo na komputerze i rozszerzenia Dynamo w programie Forma.
* Pisanie pierwszego wykresu.

## Zabezpieczenia

* Pamiętaj, że wykresy udostępnione są przechowywane w programie Forma.
* Maksymalny czas wykonywania wykresu jest obecnie krótszy niż 30 minut. Ta wartość może ulec zmianie.
* Żądania wykonania mają ograniczoną szybkość, więc mogą wystąpić błędy, jeśli wykonasz wiele żądań obliczeń w zbyt krótkim czasie.
