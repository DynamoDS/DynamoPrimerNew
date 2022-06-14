# Tworzenie węzła niestandardowego

Dodatek Dynamo oferuje kilka różnych metod tworzenia węzłów niestandardowych. Węzły niestandardowe można tworzyć od podstaw, z istniejącego wykresu lub bezpośrednio w języku C#. W tej części omówimy tworzenie węzła niestandardowego w interfejsie użytkownika dodatku Dynamo z istniejącego wykresu. Ta metoda jest idealna do czyszczenia obszaru roboczego, jak również do pakowania sekwencji węzłów do ponownego użycia w innym miejscu.

## Ćwiczenie: węzły niestandardowe dla odwzorowania UV

### Część I. Rozpoczynanie od wykresu

Na poniższej ilustracji odwzorowujemy punkt z jednej powierzchni na drugą za pomocą współrzędnych UV. Użyjemy tej koncepcji do utworzenia panelowanej powierzchni, która odwołuje się do krzywych na płaszczyźnie XY. Utworzymy tu panele czworokątne dla naszego panelowania, ale stosując tę samą logikę, możemy utworzyć szeroką gamę paneli z odwzorowaniem UV. Jest to świetna okazja do tworzenia węzłów niestandardowych, ponieważ w ten sposób łatwiej będzie powtórzyć podobny proces na tym wykresie lub w innych procesach roboczych Dynamo.

![](<../images/6-1/2/custom node for uv mapping pt I - 01.jpg>)

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/6-1/2/UV-CustomNode.zip" %}

Zacznijmy od utworzenia wykresu, który zagnieździmy w węźle niestandardowym. W tym przykładzie utworzymy wykres, który będzie odwzorowywać wieloboki z powierzchni bazowej na powierzchnię docelową, za pomocą współrzędnych UV. Ten proces odwzorowywania UV jest często używany, przez co jest to dobra opcja dla węzła niestandardowego. Aby uzyskać więcej informacji na temat powierzchni i przestrzeni UV, zobacz stronę [Powierzchnia](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/5-surfaces.md). Pełny wykres: _UVmapping\_Custom-Node.dyn_ z pliku .zip pobranego powyżej.

![](<../images/6-1/2/custom node for uv mapping pt I - 02.jpg>)

> 1. **Code Block:** użyj tego wiersza, aby utworzyć zakres 10 liczb od -45 do 45 `45..45..#10;`
> 2. **Point.ByCoordinates**: połącz wyjście węzła **Code Block** z wejściami „x” i „y” oraz ustaw skratowanie na odniesienie krzyżowe. Powinna teraz istnieć siatka punktów.
> 3. **Plane.ByOriginNormal:** połącz wyjście _„Point”_ z wejściem _„origin”_, aby utworzyć płaszczyznę w każdym z punktów. Zostanie użyty domyślny wektor normalny (0,0,1).
> 4. **Rectangle.ByWidthLength**: połącz płaszczyzny z poprzedniego kroku z wejściem „_plane_” i użyj węzła **Code Block** z wartością _10_ w celu określenia szerokości i długości.

Powinna teraz istnieć siatka prostokątów. Odwzorujmy te prostokąty na powierzchnię docelową za pomocą współrzędnych UV.

![](<../images/6-1/2/custom node for uv mapping pt I - 03.jpg>)

> 1. **Polygon.Points**: połącz wyjście **Rectangle.ByWidthLength** z poprzedniego kroku z wejściem „_polygon_”, aby wyodrębnić punkty narożne każdego prostokąta. Są to punkty, które odwzorujemy na powierzchnię docelową.
> 2. **Rectangle.ByWidthLength**: użyj węzła **Code Block** z wartością _100_, aby określić szerokość i długość prostokąta. Będzie to obwiednia powierzchni bazowej.
> 3. **Surface.ByPatch**: połącz węzeł **Rectangle.ByWidthLength** z poprzedniego kroku z wejściem „_closedCurve_”, aby utworzyć powierzchnię bazową.
> 4. **Surface.UVParameterAtPoint:** połącz wyjście _„Point”_ węzła **Polygon.Points** i wyjście _„Surface”_ węzła **Surface.ByPatch**, aby zwrócić parametr UV w każdym punkcie.

Teraz gdy mamy powierzchnię bazową i zbiór współrzędnych UV, możemy zaimportować powierzchnię docelową i odwzorować punkty między powierzchniami.

![](<../images/6-1/2/custom node for uv mapping pt I - 04.jpg>)

> 1. **Ścieżka pliku:** wybierz ścieżkę pliku dla powierzchni, którą chcesz zaimportować. Powinien to być plik typu .SAT. Kliknij przycisk _„Przeglądaj”_ i przejdź do pliku _UVmapping\_srf.sat_ z pliku .zip pobranego powyżej.
> 2. **Geometry.ImportFromSAT:** połącz ścieżkę pliku, aby zaimportować powierzchnię. W podglądzie geometrii powinna być widoczna zaimportowana powierzchnia.
> 3. **UV:** połącz wyjście parametru UV z węzłami _UV.U_ i _UV.V_.
> 4. **Surface.PointAtParameter:** połącz zaimportowaną powierzchnię oraz współrzędne u i v. Na powierzchni docelowej powinna być teraz widoczna siatka punktów 3D.

Ostatnią czynnością jest użycie punktów 3D do utworzenia prostokątnych płatów powierzchni.

![](<../images/6-1/2/custom node for uv mapping pt I - 05.jpg>)

> 1. **PolyCurve.ByPoints:** połącz punkty na powierzchni, aby skonstruować krzywą PolyCurve przez punkty.
> 2. **Boolean**: dodaj węzeł **Boolean** do obszaru roboczego i połącz go z wejściem _„connectLastToFirst”_ oraz przełącz na wartość True, aby zamknąć krzywe PolyCurve. Powinny być teraz widoczne prostokąty odwzorowane na powierzchnię.
> 3. **Surface.ByPatch:** połącz krzywe PolyCurve z wejściem _„closedCurve”_, aby skonstruować płaty powierzchni.

### Część II. Od wykresu do węzła niestandardowego

Teraz wybierzmy węzły do zagnieżdżenia w węźle niestandardowym, uwzględniając to, jakie powinny być wejścia i wyjścia węzła. Chcemy, aby węzeł niestandardowy był możliwie najbardziej elastyczny, by umożliwiał odwzorowanie dowolnych wieloboków, a nie tylko prostokątów.

Wybierz następujące węzły (począwszy od węzła Polygon.Points), kliknij prawym przyciskiem myszy obszar roboczy i wybierz opcję „Utwórz węzeł niestandardowy”.

![](<../images/6-1/2/custom node for uv mapping pt II - 01.jpg>)

W oknie dialogowym Właściwości węzła niestandardowego przypisz nazwę, opis i kategorię do węzła niestandardowego.

![](<../images/6-1/2/custom node for uv mapping pt II - 02.jpg>)

> 1. Nazwa: MapPolygonsToSurface
> 2. Opis: odwzoruj wielokąty z powierzchni bazowej na powierzchnię docelową
> 3. Kategoria dodatków: Geometry.Curve

Węzeł niestandardowy znacznie wyczyścił obszar roboczy. Warto zauważyć, że wejścia i wyjścia zostały nazwane na podstawie oryginalnych węzłów. Zmodyfikujmy węzeł niestandardowy, zmieniając te nazwy na bardziej opisowe.

![](<../images/6-1/2/custom node for uv mapping pt II - 03.jpg>)

Kliknij dwukrotnie węzeł niestandardowy, aby go edytować. Spowoduje to otwarcie obszaru roboczego z żółtym tłem reprezentującym wnętrze węzła.

![](<../images/6-1/2/custom node for uv mapping pt II - 04.jpg>)

> 1. **Węzły Input:** zmień nazwy wejść na _baseSurface_ i _targetSurface_.
> 2. **Węzły Output:** dodaj kolejne wyjście dla odwzorowanych wieloboków.

Zapisz węzeł niestandardowy i wróć do głównego obszaru roboczego. Uwaga: węzeł **MapPolygonsToSurface** odzwierciedla wprowadzone zmiany.

![](<../images/6-1/2/custom node for uv mapping pt II - 05.jpg>)

Można również zwiększyć niezawodność węzła niestandardowego przez dodanie treści w sekcji **Komentarze niestandardowe**. Komentarze mogą pomóc w ustaleniu typów wejść i wyjść lub objaśnieniu funkcjonalności węzła. Komentarze pojawią się, gdy użytkownik ustawi kursor na wejściu lub wyjściu węzła niestandardowego.

Kliknij dwukrotnie węzeł niestandardowy, aby go edytować. Spowoduje to ponowne otwarcie żółtego obszaru roboczego.

![](<../images/6-1/2/custom node for uv mapping pt II - 06.jpg>)

> 1. Rozpocznij edycję wejściowego węzła **Code Block**. Aby rozpocząć komentarz, wpisz „//”, a następnie wpisz tekst komentarza. Wpisz wszelkie informacje, które mogą pomóc w objaśnieniu węzła — w tym miejscu opiszemy węzeł _targetSurface_.
> 2. Ustawmy również domyślną wartość dla węzła _inputSurface_ przez ustawienie typu wejścia równego wartości. W tym miejscu ustawimy wartość domyślną na oryginalny zestaw **Surface.ByPatch**.

Komentarze można również stosować do wyjścia.

![](<../images/6-1/2/custom node for uv mapping pt II - 07.jpg>)

> Edytuj tekst w wyjściowym węźle węzła Code Block. Wpisz „//”, a następnie wpisz tekst komentarza. W tym miejscu objaśnimy wyjścia _Polygons_ i _surfacePatches_ poprzez dodanie bardziej szczegółowego opisu.

![](<../images/6-1/2/custom node for uv mapping pt II - 08.jpg>)

> 1. Ustaw kursor na wejściach węzła niestandardowego, aby wyświetlić komentarze.
> 2. Po ustawieniu wartości domyślnej na _inputSurface_ można również uruchomić definicję bez wejścia powierzchni.
