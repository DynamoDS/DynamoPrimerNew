# Zarządzanie grupami punktów

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Praca z punktami COGO i grupami punktów w programie Civil 3D jest podstawowym elementem wielu procesów realizowanych od pola do zakończenia. Dodatek Dynamo naprawdę sprawdza się w przypadku konieczności zarządzania danymi. W tym przykładzie zademonstrujemy jeden potencjalny przypadek zastosowania.  

## Cel

> :dart: Utworzenie grupy punktów dla każdego niepowtarzalnego opisu punktu COGO. 

## Kluczowe pojęcia

> * Praca z listami
> * Grupowanie podobnych obiektów za pomocą węzła **List.GroupByKey**
> * Wyświetlanie niestandardowych danych wyjściowych w Odtwarzaczu Dynamo

## Zgodność wersji

{% hint style="success" %}\r\n Ten wykres będzie działać w programie **Civil 3D 2020** i w nowszych wersjach. \r\n{% endhint %}

## Zestaw danych

Najpierw pobierz pliki przykładów poniżej, a następnie otwórz plik DWG i wykres dodatku Dynamo.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Rozwiązanie

Poniżej przedstawiono przegląd logiki na tym wykresie.

> 1. Pobieranie wszystkich punktów COGO w dokumencie
> 2. Grupowanie punktów COGO na podstawie opisu
> 3. Tworzenie grup punktów
> 4. Wyprowadzanie danych z podsumowaniem do Odtwarzacza Dynamo

Zacznijmy!

### Pobieranie punktów COGO

Pierwszym krokiem jest pobranie wszystkich grup punktów w dokumencie, a następnie pobranie wszystkich punktów COGO w każdej grupie. Dzięki temu otrzymamy _listę zagnieżdżoną_ lub „listę list”, z którą łatwiej będzie pracować później, jeśli spłaszczymy wszystko do pojedynczej listy za pomocą węzła **List.Flatten**.

{% hint style="info" %}\r\n Jeśli nie zdarzyło Ci się jeszcze pracować z listami, skorzystaj z sekcji [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Pobieranie wszystkich grup punktów i punktów COGO </p></figcaption></figure>

### Grupowanie punktów na podstawie opisu

Mamy już wszystkie punkty COGO. Teraz musimy rozdzielić je na grupy na podstawie ich opisów. Właśnie do tego służy węzeł **List.GroupByKey**. Zasadniczo grupuje on wszystkie elementy o tym samym kluczu.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Grupowanie punktów COGO na podstawie opisu</p></figcaption></figure>

### Tworzenie grup punktów

Najcięższą pracę mamy już za sobą. Ostatnią czynnością jest utworzenie nowych grup punktów programu Civil 3D na podstawie zgrupowanych punktów COGO.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Tworzenie nowych grup punktów</p></figcaption></figure>

### Podsumowanie danych wyjściowych

Po uruchomieniu wykresu w podglądzie tła dodatku Dynamo niczego nie ma, ponieważ nie pracujemy z żadną geometrią. Dlatego jedynym sposobem sprawdzenia, czy wykres jest wykonywany poprawnie, jest sprawdzenie obszaru narzędzi lub podglądów danych wyjściowych węzłów. Jeśli jednak wykres zostanie uruchomiony za pomocą **Odtwarzacza Dynamo**, można przekazać więcej informacji na temat wyników wykresu, drukując podsumowanie utworzonych grup punktów. Wystarczy kliknąć prawym przyciskiem myszy węzeł i skonfigurować dla niego ustawienie _Is Output_ (Dane wyjściowe). W tym przypadku użyjemy węzła **Watch** o zmienionej nazwie, aby wyświetlić wyniki.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>Skonfigurowanie węzła jako <em>Is Output</em> (Dane wyjściowe) spowoduje wyświetlenie jego zawartości w danych wyjściowych Odtwarzacza Dynamo</p></figcaption></figure>

### Wynik

Oto przykład uruchomienia wykresu za pomocą **Odtwarzacza Dynamo**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Uruchamianie wykresu za pomocą Odtwarzacza Dynamo i wyświetlanie wyników w obszarze narzędzi</p></figcaption></figure>

{% hint style="info" %}\r\n Jeśli nie znasz jeszcze Odtwarzacza Dynamo Player, skorzystaj z sekcji [dynamo-player.md](../../dynamo-player.md "mention"). \r\n{% endhint %}

> :tada: Misja wykonana!

## Pomysły

Oto kilka pomysłów na rozszerzenie możliwości tego wykresu.

{% hint style="info" %}\r\n Zmodyfikuj grupowanie punktów tak, aby było oparte na **pełnym opisie**, a nie na opisie nieprzetworzonym. \r\n{% endhint %}

{% hint style="info" %}\r\n Grupuj punkty na podstawie innych wybranych **wstępnie zdefiniowanych kategorii** (na przykład „Ground shots”, „Monuments” itp.) \r\n{% endhint %}

{% hint style="info" %}\r\n Automatycznie twórz powierzchnie TIN dla punktów w niektórych grupach. \r\n{% endhint %}
