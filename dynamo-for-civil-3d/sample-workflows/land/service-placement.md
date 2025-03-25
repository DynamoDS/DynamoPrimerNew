# Umieszczanie doprowadzeń usług komunalnych

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

Projekt inżynierski typowego osiedla mieszkaniowego obejmuje pracę z kilkoma instalacjami podziemnymi, takimi jak kanalizacja sanitarna, kanalizacja burzowa, doprowadzenie wody pitnej itp. W tym przykładzie pokazano, jak za pomocą dodatku Dynamo można narysować doprowadzenia usług komunalnych z systemu dystrybucji do danej działki. Zwykle każda działka wymaga połączenia z usługami komunalnymi, co powoduje, że opracowanie wszystkich doprowadzeń usług jest żmudnym procesem. Dodatek Dynamo może przyspieszyć ten proces dzięki automatycznemu rysowaniu niezbędnej geometrii z wysoką dokładnością, jak również udostępnianiu elastycznych danych wejściowych, które można dostosować do standardów agencji lokalnej.

## Cel

> :dart: Umieszczenie odniesień do bloków wodomierzy w określonych odsunięciach od linii działki i narysowanie linii dla każdego połączenia usług prostopadłego do głównego systemu dystrybucji.

## Kluczowe pojęcia

> * Zastosowanie węzła **Select Object** na potrzeby wprowadzania danych przez użytkownika
> * Praca z układami współrzędnych
> * Używanie operacji geometrycznych, takich jak **Geometry.DistanceTo** i **Geometry.ClosestPointTo**
> * Tworzenie odniesień do bloków
> * Sterowanie ustawieniami wiązań obiektów

## Zgodność wersji

{% hint style="success" %} Ten wykres będzie działać w programie **Civil 3D 2020** i w nowszych wersjach. 
{% endhint %} 

## Zestaw danych

Najpierw pobierz pliki przykładów poniżej, a następnie otwórz plik DWG i wykres dodatku Dynamo.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Rozwiązanie

Poniżej przedstawiono przegląd logiki na tym wykresie.

> 1. Pobieranie geometrii krzywej dla systemu dystrybucji
> 2. Pobieranie geometrii krzywej dla linii działki wybranej przez użytkownika z odwróceniem w razie potrzeby
> 3. Generowanie punktów wstawiania dla mierników
> 4. Pobieranie najbliższych położeniom mierników punktów na systemie dystrybucji
> 5. Tworzenie odniesień do bloków i linii w obszarze modelu

Zacznijmy!

### Pobieranie geometrii systemu dystrybucji

Pierwszym krokiem jest pobranie do dodatku Dynamo geometrii systemu dystrybucji. Zamiast wybierać pojedyncze linie lub polilinie, pobierzemy wszystkie obiekty na określonej warstwie i połączymy je w krzywą PolyCurve dodatku Dynamo.

{% hint style="info" %}
 Jeśli pierwszy raz masz do czynienia z geometrią krzywej dodatku Dynamo, skorzystaj z sekcji [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Pobieranie obiektów z programu Civil 3D i łączenie wszystkiego w jedną krzywą PolyCurve</p></figcaption></figure>

### Pobieranie geometrii linii działki

Następnie musimy pobrać do dodatku Dynamo geometrię wybranej linii działki, aby można było z nią pracować. Właściwym narzędziem do tego zadania jest węzeł **Select Object**, który umożliwia użytkownikowi wykresu wybranie określonego obiektu w programie Civil 3D.

Musimy również dodać obsługę potencjalnego problemu. Linia działki ma punkt początkowy i punkt końcowy, co oznacza, że ma kierunek. Aby wykres mógł dawać spójne wyniki, wszystkie linie działki muszą mieć spójny kierunek. Warunek ten można uwzględnić bezpośrednio w logice wykresu, co zwiększy niezawodność wykresu. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Wybieranie linii działki i zapewnianie, że kierunek jest właściwy</p></figcaption></figure>

> 1. Pobierz punkt początkowy i punkt końcowy linii działki.
> 2. Zmierz odległość od każdego punktu do systemu dystrybucji, a następnie określ, która odległość jest większa.
> 3. Żądanym wynikiem jest sytuacja, w której to punkt początkowy linii znajduje się najbliżej systemu dystrybucji. Jeśli tak nie jest, kierunek linii działki zostanie odwrócony. W przeciwnym razie po prostu zwracamy oryginalną linię działki.

### Generowanie punktów wstawiania

Nadszedł czas, aby dowiedzieć się, gdzie zostaną umieszczone liczniki. Zazwyczaj położenie jest określane przez wymagania agencji lokalnej, dlatego wprowadzimy tylko wartości wejściowe, które można zmienić, aby odpowiadały różnym warunkom. Użyjemy **układu współrzędnych** wzdłuż linii działki jako odniesienia przy tworzeniu punktów. Ułatwi to zdefiniowanie odsunięć względem linii działki, bez względu na jej orientację.

{% hint style="info" %}
 Jeśli pierwszy raz masz do czynienia z układami współrzędnych, skorzystaj z sekcji [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Tworzenie punktów wstawiania dla mierników</p></figcaption></figure>

### Pobieranie punktów połączeń

Teraz musimy pobrać najbliższe położeniom mierników punkty na systemie dystrybucji. Pozwoli to narysować połączenia usług w obszarze modelu, tak aby były zawsze prostopadłe do systemu dystrybucji. Idealnym rozwiązaniem jest węzeł **Geometry.ClosestPointTo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Pobieranie punktów prostopadłych na systemie dystrybucji</p></figcaption></figure>

> 1. To krzywa PolyCurve systemu dystrybucji
> 2. To punkty wstawiania mierników

### Tworzenie obiektów

Ostatnią czynnością jest utworzenie obiektów w obszarze modelu. Użyjemy wygenerowanych wcześniej punktów wstawiania, aby utworzyć odniesienia do bloków, a następnie użyjemy punktów na systemie dystrybucji, aby narysować linie do połączeń usług.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Wynik

Po uruchomieniu wykresu powinny być widoczne nowe odniesienia do bloków i linie połączeń usług w obszarze modelu. Zmień niektóre dane wejściowe i obserwuj, jak wszystko jest aktualizowane automatycznie.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Dostosowywanie parametrów wejściowych w dodatku Dynamo i natychmiastowe wyświetlanie wyników w programie Civil 3D</p></figcaption></figure>

### Bonus: włączanie umieszczania sekwencyjnego

Można zauważyć, że po umieszczeniu obiektów dla jednej linii działki wybranie innej linii działki powoduje „przesunięcie” obiektów.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Zachowanie występujące, gdy włączono wiązanie obiektów</p></figcaption></figure>

Jest to domyślne zachowanie dodatku Dynamo, które jest bardzo przydatne w wielu przypadkach. Jednak może okazać się konieczne sekwencyjne umieszczenie kilku połączeń usług i wymuszenie, aby dodatek Dynamo utworzył nowe obiekty za każdym uruchomieniem, zamiast modyfikować oryginalne. Można sterować tym zachowaniem, zmieniając ustawienia wiązania obiektów.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Ustawienia wiązania obiektów dodatku Dynamo</p></figcaption></figure>

{% hint style="info" %}
 Aby uzyskać więcej informacji, skorzystaj z sekcji [object-binding.md](../../advanced-topics/object-binding.md "mention"). 
{% endhint %} 

Zmiana tego ustawienia spowoduje, że dodatek Dynamo będzie „zapominać” obiekty tworzone w poszczególnych uruchomieniach. Oto przykład uruchomienia wykresu z wyłączonym wiązaniem obiektów za pomocą **Odtwarzacza Dynamo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Uruchamianie wykresu za pomocą Odtwarzacza Dynamo i wyświetlanie wyników w programie Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Jeśli nie znasz jeszcze Odtwarzacza Dynamo Player, skorzystaj z sekcji [dynamo-player.md](../../dynamo-player.md "mention"). 
{% endhint %} 

> :tada: Misja wykonana!

## Pomysły

Oto kilka pomysłów na rozszerzenie możliwości tego wykresu.

{% hint style="info" %}
 Umieść **wiele połączeń usług** jednocześnie, zamiast zaznaczać każdą linię działki. 
{% endhint %} 

{% hint style="info" %}
 Dopasuj dane wejściowe, aby zamiast mierników wody umieszczać **odejścia czyszczące**. 
{% endhint %} 

{% hint style="info" %}
 **Dodaj przełącznik**, aby umożliwić umieszczenie pojedynczego połączenia usług po określonej stronie linii działki zamiast po obu stronach. 
{% endhint %} 
