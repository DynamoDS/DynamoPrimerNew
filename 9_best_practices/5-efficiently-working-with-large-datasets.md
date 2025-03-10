# Wydajna praca z dużymi zestawami danych w dodatku Dynamo

Na tej stronie przedstawiono pewne praktyczne zasady wydajnej pracy z dużymi zestawami danych w dodatku Dynamo. Mamy nadzieję, że za pomocą tych wskazówek zidentyfikujesz wąskie gardła na wykresach, dzięki czemu wykres będzie wykonywany w ciągu kilku minut, a nie godzin.

Zawartość:
* Generowanie geometrii a mozaikowanie
* Wykorzystanie pamięci
* interfejs API programu Revit

### Generowanie geometrii a mozaikowanie

W dodatku Dynamo utworzenie fragmentu geometrii i rysowanie to dwa zupełnie różne zdarzenia. Na ogół tworzenie geometrii jest znacznie szybsze i zużywa mniej pamięci niż rysowanie obiektu. Możesz myśleć o geometrii jako o liście pomiarów potrzebnych do wykonania garnituru, podczas gdy mozaikowanie to sam garnitur. O garniturze można powiedzieć całkiem sporo na podstawie jego wymiarów: jak długie są rękawy, ile będzie kosztować itp. Jednak prawie zawsze trzeba zobaczyć i przymierzyć gotowy garnitur, aby ustalić, czy jest dobry, czy nie. Podobnie jest z geometrią niemozaikową, można określić jej ramkę ograniczającą, powierzchnię, objętość, przeciąć ją z inną geometrią i wyeksportować do pliku SAT lub do programu Revit. Jednak prawie zawsze trzeba mozaikować geometrię, aby przekonać się, czy jest poprawna, czy nie. 

Jeśli wykres Dynamo zawiera wiele obiektów, a jego działanie jest spowolnione, można usunąć z niego kroki mozaikowania, aby go przyspieszyć.  

Węzły geometrii w dodatku Dynamo są zawsze mozaikowane*. Pozostawia to dwie opcje pracy z geometrią niemozaikowaną: węzły Python i węzły ZeroTouch. Dopóki nie zostanie zwrócony obiekt geometrii z węzła Python lub ZeroTouch, geometria nie będzie mozaikowana. Jeśli na przykład wykres zawiera kilka węzłów punktów połączonych z kilkoma węzłami linii połączonych z kilkoma węzłami wyciągnięcia złożonego połączonych z kilkoma węzłami pogrubienia, geometria będzie mozaikowana na każdym z tych kroków. Zamiast tego można połączyć tę logikę z węzłem Python lub ZeroTouch i zwrócić tylko końcowy obiekt z węzła.

Więcej informacji na temat korzystania z węzłów ZeroTouch można znaleźć w sekcji [Opracowywanie rozwiązań dla dodatku Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) tego przewodnika Primer.

### Wykorzystanie pamięci

Jeśli geometria nie jest już mozaikowana, mogą występować wąskie gardła pamięci z powodu nadmiernego nagromadzenia geometrii. Obiekty geometrii tworzone w dodatku Dynamo zużywają niewielką, ale niezerową ilość pamięci. W przypadku pracy z setkami tysięcy lub milionami obiektów może się to sumować i powodować awarię dodatku Dynamo lub programu Revit. W dodatku Dynamo w wersji 2.5 i w nowszych wersjach jest to obsługiwane niejawnie przez usuwanie nieużywanych obiektów, ale w przypadku korzystania z wersji starszej niż 2.5 jednym ze sposobów uniknięcia tworzenia dużej ilości geometrii jest pozbywanie się obiektów po zakończeniu pracy nad nimi. Załóżmy na przykład, że tworzone są setki tysięcy krzywych NurbsCurve, z których każda wymaga kilkudziesięciu punktów (Point). Jednym ze sposobów na ich utworzenie jest przekazanie listy 2-wymiarowej w dodatku Dynamo do węzła NurbsCurve.ByPoints. Wymaga to jednak utworzenia milionów punktów. Innym sposobem jest użycie węzła Python lub ZeroTouch. W tym węźle można utworzyć kilkanaście punktów, wprowadzić je do węzła NurbsCurve.ByPoints, a następnie usunąć te kilkanaście punktów za pomocą wywołania metody .Dispose(). Więcej informacji na temat korzystania z węzłów ZeroTouch można znaleźć w sekcji [Opracowywanie rozwiązań dla dodatku Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) tego przewodnika Primer. W pewnych okolicznościach pozbywanie się obiektów geometrii po ich utworzeniu może znacznie zmniejszyć ilość używanej pamięci. Mimo że jest to obsługiwane automatycznie w przypadku użytkowników korzystających z dodatku Dynamo 2.5 lub nowszego, zaleca się, aby użytkownik nadal usuwał geometrię jawnie, jeśli przypadek użycia wymaga zmniejszenia ilości pamięci w określonym z góry czasie. Aby dowiedzieć się więcej na temat nowych funkcji zwiększających stabilność wprowadzonych w dodatku Dynamo 2.5, zobacz [Ulepszenia stabilności geometrii w dodatku Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297).

### interfejs API programu Revit

Jeśli agresywnie usuwasz obiekty w węźle ZeroTouch lub Python i nadal występują problemy z pamięcią lub wydajnością, może być konieczne całkowite pominięcie dodatku Dynamo i utworzenie obiektów programu Revit bezpośrednio za pomocą interfejsu API. Można na przykład przeanalizować plik programu Excel pod kątem informacji o punkcie i użyć tych informacji do utworzenia zestawu współrzędnych XYZ oraz innych elementów programu Revit za pomocą odpowiedniego interfejsu API. W tym momencie to program Revit stanie się ostatecznym wąskim gardłem, którego nie da się ominąć.