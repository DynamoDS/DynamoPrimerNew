# Umieszczanie słupa oświetleniowego

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Jednym z wielu doskonałych przykładów zastosowań dodatku Dynamo jest dynamiczne umieszczanie odrębnych obiektów wzdłuż modelu korytarza. Często obiekty muszą być umieszczane w miejscach niezależnych od wstawionych zespołów wzdłuż korytarza, co jest bardzo żmudnym zadaniem w przypadku wykonywania ręcznego. Gdy zmienia się geometria pozioma lub pionowa korytarza, wprowadzana jest znaczna ilość poprawek.

## Cel

> :dart: Umieszczenie odniesień do bloków słupów oświetleniowych wzdłuż korytarza w wartościach pikiet określonych w pliku programu Excel. 

## Kluczowe pojęcia

> * Odczytywanie danych z pliku zewnętrznego (w tym przypadku programu Excel)
> * Organizowanie danych w słownikach
> * Sterowanie położeniem/skalą/obrotem za pomocą układów współrzędnych
> * Umieszczanie odniesień do bloków
> * Wizualizowanie geometrii w dodatku Dynamo

## Zgodność wersji

{% hint style="success" %} Ten wykres będzie działać w programie **Civil 3D 2020** i w nowszych wersjach. {% endhint %}

## Zestaw danych

Najpierw pobierz pliki przykładów poniżej, a następnie otwórz plik DWG i wykres dodatku Dynamo.

{% hint style="info" %} Najlepiej jest, jeśli plik programu Excel jest zapisany w tym samym katalogu co wykres dodatku Dynamo. {% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Rozwiązanie

Poniżej przedstawiono przegląd logiki na tym wykresie.

> 1. Odczytywanie pliku programu Excel i importowanie danych do dodatku Dynamo
> 2. Pobieranie linii charakterystycznych z określonej linii bazowej korytarza
> 3. Generowanie układów współrzędnych wzdłuż linii charakterystycznej korytarza w żądanych pikietach
> 4. Umieszczanie odniesień do bloków w obszarze modelu za pomocą układów współrzędnych

Zacznijmy!

### Pobieranie danych programu Excel

W tym przykładowym wykresie użyjemy pliku programu Excel do przechowywania danych, za pomocą których dodatek Dynamo umieści odniesienia do bloków słupów oświetleniowych. Tabela wygląda tak.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Struktura tabeli w pliku programu Excel</p></figcaption></figure>

{% hint style="info" %} Odczytanie danych z pliku zewnętrznego (np. pliku programu Excel) za pomocą dodatku Dynamo jest doskonałą strategią, zwłaszcza gdy dane muszą być współdzielone z innymi członkami zespołu. {% endhint %}

Dane programu Excel są importowane do dodatku Dynamo w ten sposób. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Importowanie danych programu Excel do dodatku Dynamo</p></figcaption></figure>

Mamy już dane. Teraz musimy je podzielić według kolumn (_Corridor_, _Baseline_, _PointCode_ itp.), aby móc wykorzystać je w pozostałej części wykresu. Typowym sposobem wykonania tej operacji jest użycie węzła **List.GetItemAtIndex** i określenie numeru indeksu każdej odpowiedniej kolumny. Na przykład kolumna _Corridor_ ma indeks 0, kolumna _Baseline_ ma indeks 1 itd.

Wygląda to dobrze, prawda? Jednak z tym podejściem wiąże się potencjalny problem. Co jeśli w przyszłości kolejność kolumn w pliku Excel ulegnie zmianie? Lub jeśli między dwiema kolumnami zostanie dodana nowa kolumna? Wykres nie będzie wtedy działał poprawnie i będzie wymagał aktualizacji. Wykres można zabezpieczyć przed przyszłymi zmianami, umieszczając dane w słowniku, **Dictionary**, z nagłówkami kolumn programu Excel jako kluczami, _keys_, i pozostałymi danymi jako wartościami, _values_.

{% hint style="info" %} Jeśli pierwszy raz masz do czynienia ze słownikami, skorzystaj z sekcji [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Umieszczanie danych programu Excel w słowniku</p></figcaption></figure>

Dzięki temu wykres jest bardziej niezawodny, ponieważ umożliwia elastyczne zmienianie kolejności kolumn w programie Excel. Dopóki nagłówki kolumn pozostają takie same, dane można po prostu pobrać ze słownika za pomocą jego klucza, _key_, czyli nagłówka kolumny, co jest naszym kolejnym krokiem.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Pobieranie danych ze słownika</p></figcaption></figure>

### Pobieranie linii charakterystycznych korytarza

Dane programu Excel są już zaimportowane i gotowe do użycia. Zacznijmy używać ich do pobierania informacji z programu Civil 3D na temat modeli korytarzy.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Wybierz model korytarza na podstawie jego nazwy.
> 2. Pobierz określoną linię bazową (Baseline) w korytarzu.
> 3. Pobierz linię charakterystyczną w linii bazowej na podstawie jej kodu punktu.

### Generowanie układów współrzędnych

Teraz wygenerujemy **układy współrzędnych** wzdłuż linii charakterystycznych korytarza przy wartościach pikiet określonych w pliku programu Excel. Te układy współrzędnych posłużą do zdefiniowania położenia, obrotu i skali odniesień do bloków słupów oświetleniowych.

{% hint style="info" %} Jeśli pierwszy raz masz do czynienia z układami współrzędnych, skorzystaj z sekcji [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Pobieranie układów współrzędnych wzdłuż linii charakterystycznych korytarza</p></figcaption></figure>

Zwróć uwagę, że zastosowano tu węzeł Code Block do obracania układów współrzędnych w zależności od tego, po której stronie linii bazowej się one znajdują. Można to też zrealizować za pomocą sekwencji kilku węzłów, ale jest to dobry przykład sytuacji, w której łatwiej jest po prostu napisać kod.

{% hint style="info" %} Jeśli pierwszy raz masz do czynienia z węzłami Code Block, skorzystaj z sekcji [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention"). {% endhint %}

### Tworzenie odniesień do bloków

Już prawie gotowe! Mamy wszystkie informacje, których potrzebujemy, aby móc umieścić odniesienia do bloków. Pierwszą czynnością jest pobranie definicji bloków, których użyjemy, za pomocą kolumny _BlockName_ w pliku programu Excel.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Pobieranie żądanych definicji bloków z dokumentu</p></figcaption></figure>

Na tym etapie ostatnim krokiem jest utworzenie odniesień do bloków.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Tworzenie odniesień do bloków w obszarze modelu</p></figcaption></figure>

### Wynik

Po uruchomieniu wykresu powinny być widoczne nowe odniesienia do bloków w obszarze modelu wzdłuż korytarza. Oto najlepsza część — jeśli ustawiono automatyczny tryb wykonywania wykresu, to po edytowaniu pliku programu Excel odniesienia do bloków zostaną zaktualizowane automatycznie.

{% hint style="info" %} Więcej informacji na temat trybów wykonywania wykresów można znaleźć w sekcji [3_user_interface](../../../3\_user\_interface/ "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Wprowadzanie aktualizacji w pliku programu Excel i szybkie wyświetlanie wyników w programie Civil 3D</p></figcaption></figure>

Oto przykład uruchomienia wykresu za pomocą **Odtwarzacza Dynamo**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Uruchamianie wykresu za pomocą Odtwarzacza Dynamo i wyświetlanie wyników w programie Civil 3D</p></figcaption></figure>

{% hint style="info" %} Jeśli nie znasz jeszcze Odtwarzacza Dynamo Player, skorzystaj z sekcji [dynamo-player.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Misja wykonana!

### Bonus: wizualizowanie w dodatku Dynamo

Pomocne może być zwizualizowanie geometrii korytarza w dodatku Dynamo w celu zapewnienia kontekstu. Ten konkretny model zawiera już wyodrębnione bryły korytarza w obszarze modelu, więc przenieśmy je do dodatku Dynamo. 

Jest jednak coś jeszcze, co musimy rozważyć. Bryły są stosunkowo „ciężkimi” typami geometrii, co oznacza, że ta operacja spowolni działanie wykresu. Przydałby się prosty sposób _wyboru_, czy chcemy wyświetlać bryły, czy nie. Oczywistym rozwiązaniem jest odłączenie węzła **Corridor.GetSolids**, ale spowoduje to wyświetlenie ostrzeżeń dla wszystkich węzłów znajdujących się za nim, co nie jest eleganckie. Jest to sytuacja, w której naprawdę przydaje się węzeł **ScopeIf**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Zwróć uwagę, że węzeł **Object.Geometry** ma szary pasek na dole. Oznacza to, że podgląd węzła jest wyłączony (dostępny po kliknięciu węzła prawym przyciskiem myszy), co pozwala na uniknięcie „konkurowania” z inną geometrią o priorytet wyświetlania w podglądzie tła w węźle **GeometryColor.ByGeometryColor**.
> 2. Węzeł **ScopeIf** zasadniczo umożliwia selektywne uruchamianie całej gałęzi węzłów. Jeśli wartość wejściowa _test_ ma wartość fałsz (false), nie zostanie uruchomiony żaden węzeł połączony z węzłem **ScopeIf**.

Oto wynik w podglądzie tła dodatku Dynamo.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Wizualizowanie geometrii korytarza w dodatku Dynamo</p></figcaption></figure>

## Pomysły

Oto kilka pomysłów na rozszerzenie możliwości tego wykresu.

{% hint style="info" %} Dodaj do pliku programu Excel kolumnę obrotu, **rotation**, i za jej pomocą steruj obrotem układów współrzędnych. {% endhint %}

{% hint style="info" %} Dodaj do pliku programu Excel **odsunięcia poziome lub pionowe**, tak aby słupy oświetleniowe mogły w razie potrzeby odbiegać od linii charakterystycznej korytarza. {% endhint %}

{% hint style="info" %} Zamiast używać pliku programu Excel z wartościami pikiet, wygeneruj wartości pikiet **bezpośrednio w dodatku Dynamo**, używając pikiety początkowej i typowego odstępu. {% endhint %}
