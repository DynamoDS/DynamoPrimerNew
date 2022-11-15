# Punkty przyciągania

Punkty przyciągania są przydatne podczas eksperymentowania ze wzorami geometrycznymi. Mogą one służyć do tworzenia stopniowych zmian obiektów na podstawie ich odległości.

Ten proces roboczy ilustruje:

* Tworzenie i edytowanie list oraz zarządzanie nimi.
* Przesuwanie punktów w podglądzie 3D za pomocą bezpośredniej manipulacji.
* Zmienianie trybu wykonywania.

![](../images/10-1/2/attractor1.gif)

## Definiowanie celów

W tym ćwiczeniu chcemy utworzyć okrąg (_cel_), w przypadku którego wartość wejściowa promienia jest zdefiniowana za pomocą odległości od pobliskiego punktu (_zależność_).

![Odręczny szkic okręgu](../images/10-1/2/00-Hand-Sketch-of-Circle.png)

> Punkt definiujący zależność na podstawie odległości jest powszechnie określany jako „punkt przyciągania” (lub „atraktor”). Tutaj odległość od punktu przyciągania będzie używana do określenia, jak duży będzie nasz okrąg.

## Następne kroki

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/10-1/2/DynamoSampleWorkflow-Attractors.dyn" %}

Po naszkicowaniu celów i zależności możemy rozpocząć tworzenie wykresu. Potrzebne są węzły reprezentujące sekwencję operacji wykonywanych przez dodatek Dynamo. Zacznijmy od dodania następujących węzłów: **Number**, **Number Slider**, **Point.ByCoordinates**, **Geometry.DistanceTo, Circle.ByCenterPointRadius**.

![](../images/10-1/2/attractor(2).png)

> 1. Dane wejściowe > Podstawowe > **Number**
> 2. Dane wejściowe > Podstawowe > **Number Slider**
> 3. Geometria > Punkty > Punkt > **By Coordinates(x,y,z)**
> 4. Geometria > Modyfikatory > Geometria > **DistanceTo**
> 5. Geometria > Krzywe > Okrąg > **ByCenterPointRadius**

### Łączenie węzłów z przewodami

Mamy już kilka węzłów, więc teraz musimy połączyć porty węzłów z przewodami. Połączenia te zdefiniują przepływ danych.

![](../images/10-1/2/attractor(3).png)

> 1. **Number** do **Point.ByCoordinates**
> 2. Węzły **Number Slider** do **Point.ByCoordinates**
> 3. **Point.ByCoordinates** (2) do **DistanceTo**
> 4. **Point.ByCoordinates** i **DistanceTo** do **Circle.ByCenterPointRadius**

### Wykonywanie programu

Po zdefiniowaniu przepływu programu wystarczy tylko wydać dodatkowi Dynamo polecenie wykonania go. Po uruchomieniu programu (automatycznie lub po kliknięciu przycisku Uruchom w trybie ręcznym) dane zostaną przekazane przez przewody. Wyniki powinny pojawić się w podglądzie 3D.

![](../images/10-1/2/attractor(4).png)

> 1. (Kliknij przycisk Uruchom) — jeśli pasek wykonywania jest w trybie ręcznym, aby uruchomić wykres, należy kliknąć przycisk Uruchom.
> 2. Podgląd węzła — umieszczenie kursora myszy na polu w prawym dolnym rogu węzła powoduje wyświetlenie wyskakującego pola wyników.
> 3. Podgląd 3D — jeśli dowolny z węzłów tworzy geometrię, zostanie ona wyświetlona w podglądzie 3D.
> 4. Geometria wyjściowa w węźle tworzenia.

### Dodawanie węzła **Code Block**

Jeśli nasz program działa, w podglądzie 3D powinniśmy zobaczyć okrąg, który przechodzi przez nasz punkt przyciągania. To doskonały wynik. Ale może zajść potrzeba dodania większej liczby szczegółów lub większej liczby elementów sterujących. Dopasujmy wejście węzła okręgu, tak aby można było kalibrować wpływ na promień. Dodaj kolejny węzeł **Number Slider** do obszaru roboczego, a następnie kliknij dwukrotnie puste miejsce obszaru roboczego, aby dodać węzeł bloku kodu — **Code Block**. Edytuj pole w bloku kodu, określając `X/Y`.

![](../images/10-1/2/attractor(5).png)

> 1. **Code Block**
> 2. **DistanceTo** i **Number Slider** do **Code Block**
> 3. **Code Block** do **Circle.ByCenterPointRadius**

### Korzystanie z sekwencji

Rozpoczynanie od czegoś prostego, a następnie zwiększanie złożoności jest skutecznym sposobem na stopniowe tworzenie programu. Po utworzeniu działającego rozwiązania dla jednego okręgu wykorzystajmy je do manipulowania więcej niż jednym okręgiem. Jeśli użyjemy siatki punktów zamiast jednego punktu środkowego i uwzględnimy tę zmianę w wynikowej strukturze danych, program będzie teraz tworzyć wiele okręgów — każdy z nich będzie miał unikalną wartość promienia definiowaną przez kalibrowaną odległość do punktu przyciągania.

![](../images/10-1/2/attractor(6).png)

> 1. Dodaj węzeł **Number Sequence** i zastąp wejścia węzła **Point.ByCoordinates** — kliknij prawym przyciskiem myszy węzeł Point.ByCoordinates i wybierz opcję Skratowanie > Odniesienie krzyżowe.
> 2. Dodaj węzeł **Flatten** po węźle Point.ByCoordinates. Aby całkowicie spłaszczyć listę, pozostaw domyślną wartość wejścia `amt` równą `-1`
> 3. Podgląd 3D zostanie zaktualizowany o siatkę okręgów.

### Dopasowywanie za pomocą manipulacji bezpośredniej

Czasami manipulacja liczbowa nie stanowi właściwego podejścia. Teraz można ręcznie popychać i pociągać geometrię punktu podczas nawigacji w podglądzie 3D tła. Można również sterować inną geometrią, która została utworzona przez punkt. Na przykład węzeł **Sphere.ByCenterPointRadius** również umożliwia manipulację bezpośrednią. Można sterować położeniem punktu za pomocą serii wartości X, Y i Z przy użyciu węzła **Point.ByCoordinates**. Jednak dzięki metodzie manipulacji bezpośredniej można aktualizować wartości suwaków, ręcznie przesuwając punkt w trybie **nawigacji w podglądzie 3D**. Zapewnia to bardziej intuicyjne podejście do sterowania zestawem wartości dyskretnych identyfikujących położenie punktu.

![](../images/10-1/2/attractor(7).png)

> 1. Aby zastosować **manipulację bezpośrednią**, wybierz panel punktu do przesunięcia — nad wybranym punktem pojawią się strzałki.
> 2. Przełącz do trybu **nawigacja w podglądzie 3D**.

![](../images/10-1/2/attractor\(8\).png)

> 1. Umieść kursor na punkcie, a pojawią się osie X, Y i Z.
> 2. Kliknij i przeciągnij kolorową strzałkę, aby przesunąć odpowiednią oś, a wartości **Number Slider** zostaną dynamicznie zaktualizowane zgodnie z ręcznie przesuniętym punktem.

![](../images/10-1/2/attractor(1).png)

> 1. Zwróć uwagę, że przed rozpoczęciem operacji **bezpośredniej manipulacji** do komponentu **Point.ByCoordinates** był podłączony tylko jeden suwak. Po ręcznym przesunięciu punktu w kierunku osi X dodatek Dynamo automatycznie wygeneruje nowy węzeł **Number Slider** dla wejścia X.

