# Zmienianie nazw konstrukcji

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

Podczas dodawania rur i konstrukcji do sieci rurociągów program Civil 3D używa szablonu do automatycznego przypisywania nazw. Jest to zwykle wystarczające podczas wstępnego umieszczania. Jednak w miarę rozwoju projektu nazwy będą musiały ulec zmianie. Ponadto może być wymaganych wiele różnych wzorów nazewnictwa, na przykład nadawanie konstrukcjom w rurociągu nazw sekwencyjnych od konstrukcji najdalszej w kolejności lub stosowanie wzoru nazewnictwa zgodnego ze schematem danych agencji lokalnej. W tym przykładzie pokazano, jak za pomocą dodatku Dynamo można definiować dowolnego typu strategię nazewnictwa, która ma być stosowana spójnie.

## Cel

> :dart: Zmiana nazw konstrukcji sieci rurociągów w kolejności opartej na pikietażu linii trasowania.

## Kluczowe pojęcia

> * Praca z ramkami ograniczającymi
> * Filtrowanie danych za pomocą węzła **List.FilterByBoolMask**
> * Sortowanie danych za pomocą węzła **List.SortByKey**
> * Generowanie i modyfikowanie ciągów tekstowych

## Zgodność wersji

{% hint style="success" %} Ten wykres będzie działać w programie **Civil 3D 2020** i w nowszych wersjach. {% endhint %}

## Zestaw danych

Najpierw pobierz pliki przykładów poniżej, a następnie otwórz plik DWG i wykres dodatku Dynamo.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Rozwiązanie

Poniżej przedstawiono przegląd logiki na tym wykresie.

> 1. Wybieranie konstrukcji na podstawie warstwy
> 2. Pobieranie lokalizacji konstrukcji
> 3. Filtrowanie konstrukcji na podstawie odsunięć, a następnie sortowanie ich na podstawie pikiet
> 4. Generowanie nowych nazw
> 5. Zmienianie nazw konstrukcji

Zacznijmy!

### Wybieranie konstrukcji

Najpierw musimy wybrać wszystkie konstrukcje, z którymi będziemy pracować. W tym celu wystarczy wybrać wszystkie obiekty na określonej warstwie, co oznacza, że można wybrać konstrukcje z różnych sieci rurociągów (przy założeniu, że mają one tę samą warstwę).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Wybieranie konstrukcji na danej warstwie</p></figcaption></figure>

> 1. Ten węzeł gwarantuje, że nie zostaną przypadkowo pobrane żadne niepożądane typy obiektów, które mogą mieć tę samą warstwę co konstrukcje.

### Pobieranie lokalizacji konstrukcji

Mamy już konstrukcje. Teraz musimy ustalić ich położenia w przestrzeni, tak aby można było je sortować według lokalizacji. W tym celu skorzystamy z ramek ograniczających poszczególnych obiektów. **Ramka ograniczająca** obiektu to ramka o minimalnym rozmiarze, która w pełni zawiera geometryczne zakresy obiektu. Obliczając środek ramki ograniczającej, otrzymujemy całkiem dobre przybliżenie punktu wstawiania konstrukcji.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Ustalanie przybliżonych punktów wstawiania poszczególnych konstrukcji za pomocą ramek ograniczających</p></figcaption></figure>

Za pomocą tych punktów ustalimy pikiety i odsunięcia konstrukcji względem wybranej linii trasowania.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filtrowanie i sortowanie

Tutaj zaczyna się robić trochę trudniej. Na tym etapie mamy dużą listę wszystkich konstrukcji na określonej warstwie i wybraliśmy linię trasowania, wzdłuż której mają być sortowane. Problem w tym, że na liście mogą znajdować się konstrukcje, których nazw nie chcemy zmieniać. Mogą one na przykład nie być częścią interesującego nas segmentu.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. Wybrana linia trasowania
> 2. Konstrukcje, których nazwy chcemy zmienić
> 3. Konstrukcje, które powinny zostać pominięte

Dlatego musimy przefiltrować listę konstrukcji, aby nie uwzględniać tych, które mają odsunięcie od linii trasowania większe niż określone. Najlepiej zrobić to za pomocą węzła **List.FilterByBoolMask**. Po przefiltrowaniu listy konstrukcji użyjemy węzła **List.SortByKey**, aby posortować je według wartości pikiet.

{% hint style="info" %} Jeśli nie zdarzyło Ci się jeszcze pracować z listami, skorzystaj z sekcji [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtrowanie i sortowanie konstrukcji</p></figcaption></figure>

> 1. Sprawdzanie, czy odsunięcie konstrukcji jest mniejsze niż wartość progowa
> 2. Zastąpienie wszelkich wartości null wartością _false_
> 3. Filtrowanie listy konstrukcji i pikiet
> 4. Sortowanie konstrukcji według pikiet

### Generowanie nowych nazw

Ostatnią czynnością, którą musimy wykonać, jest utworzenie nowych nazw konstrukcji. Użyjemy formatu `<alignment name>-STRC-<number>`. Dodano tu jeszcze kilka węzłów, aby w razie potrzeby uzupełnić liczby o dodatkowe zera (np. „01” zamiast „1”).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Generowanie nowych nazw konstrukcji</p></figcaption></figure>

### Zmienianie nazw konstrukcji

Wreszcie przechodzimy do zmieniania nazw konstrukcji.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Ustawianie nazw konstrukcji</p></figcaption></figure>

### Wynik

Oto przykład uruchomienia wykresu za pomocą **Odtwarzacza Dynamo**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Uruchamianie wykresu za pomocą Odtwarzacza Dynamo i wyświetlanie wyników w programie Civil 3D</p></figcaption></figure>

{% hint style="info" %} Jeśli nie znasz jeszcze Odtwarzacza Dynamo Player, skorzystaj z sekcji [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Misja wykonana!

### Bonus: wizualizowanie w dodatku Dynamo

Przydatne może być wykorzystanie podglądu tła 3D dodatku Dynamo do wizualizacji pośrednich danych wyjściowych wykresu zamiast tylko wyniku końcowego. Jednym z prostych rozwiązań jest wyświetlenie ramek ograniczających dla konstrukcji. Ponadto ten konkretny zestaw danych zawiera korytarz w dokumencie, dlatego można przenieść geometrię linii charakterystycznych korytarza do dodatku Dynamo, aby zapewnić kontekst dla lokalizacji konstrukcji w przestrzeni. Jeśli wykres zostanie użyty z zestawem danych bez żadnych korytarzy, węzły te po prostu nie wykonają żadnych działań.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Wizualizowanie geometrii konstrukcji i linii charakterystycznych korytarza</p></figcaption></figure>

Teraz możemy lepiej zrozumieć, jak działa proces filtrowania konstrukcji na podstawie odsunięć.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Dostosowywanie wartości progowej odsunięcia linii trasowania i wizualizowanie odpowiednich konstrukcji w dodatku Dynamo</p></figcaption></figure>

## Pomysły

Oto kilka pomysłów na rozszerzenie możliwości tego wykresu.

{% hint style="info" %} Zmień nazwy konstrukcji na podstawie ich **najbliższych linii trasowania**, zamiast wybierać określoną linię trasowania. {% endhint %}

{% hint style="info" %} **Zmień nazwy rur**, a nie tylko konstrukcji. {% endhint %}

{% hint style="info" %} **Ustaw warstwy** konstrukcji na podstawie ich segmentów.  {% endhint %}
