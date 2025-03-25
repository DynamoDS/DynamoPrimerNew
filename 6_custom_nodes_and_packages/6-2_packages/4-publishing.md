# Publikowanie pakietu

W poprzednich sekcjach omówiono szczegółowo skonfigurowanie pakietu _MapToSurface_ za pomocą węzłów niestandardowych i plików przykładowych. Jak jednak opublikować pakiet, który został utworzony lokalnie? W tej analizie przypadku pokazano sposób publikowania pakietu z zestawu plików w folderze lokalnym.

![](<../images/6-2/3/develop package - custom nodes 01 (1) (1).jpg>)

Istnieje wiele sposobów na opublikowanie pakietu. Poniżej przedstawiono zalecany przez nas proces: **publikowanie lokalne, opracowywanie lokalne, a następnie publikowanie online**. Rozpoczniemy od folderu zawierającego wszystkie pliki w pakiecie.

### Odinstalowywanie pakietu

Zanim przejdziemy do publikowania pakietu MapToSurface, jeśli został on zainstalowany w ramach poprzedniej lekcji, należy go odinstalować, aby nie pracować z identycznymi pakietami.

Najpierw przejdź do obszaru Pakiety > Menedżer pakietów > karty Zainstalowane pakiety > obok pozycji MapToSurface kliknij menu w postaci pionowych kropek > Usuń.

<figure><img src="../../.gitbook/assets/delete-map-to-surface.png" alt=""><figcaption></figcaption></figure>

Następnie ponownie uruchom dodatek Dynamo. Po ponownym otwarciu w oknie _„Zarządzaj pakietami”_ nie powinno już być pakietu _MapToSurface_. Teraz można już zacząć od początku.

### Publikowanie pakietu lokalnie

{% hint style="warning" %} Pakiety i węzły niestandardowe można publikować w środowisku Dynamo Sandbox w wersji 2.17 lub nowszej, o ile nie mają one zależności od nadrzędnego interfejsu API. W starszych wersjach publikowanie pakietów i węzłów niestandardowych jest włączone tylko w dodatku Dynamo dla programu Revit i dodatku Dynamo dla programu Civil 3D. {% endhint %}

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

To jest pierwsze przesłanie pakietu i wszystkie pliki przykładowe i węzły niestandardowe zostały umieszczone w jednym folderze. Po przygotowaniu tego folderu można przekazać go do menedżera pakietów Dynamo.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Ten folder zawiera pięć węzłów niestandardowych (.dyf).
> 2. Ten folder zawiera także pięć przykładowych plików (.dyn) i jeden zaimportowany plik wektorowy (.svg). Te pliki będą służyły jako ćwiczenia wprowadzające, aby pokazać użytkownikowi, jak pracować z węzłami niestandardowymi.

W dodatku Dynamo najpierw kliknij kolejno opcje _Pakiety > Menedżer pakietów > kartę Opublikuj nowy pakiet_.

Na karcie _Publikowanie pakietu_ wypełnij odpowiednie pola po lewej stronie okna.

<figure><img src="../../.gitbook/assets/package-details.png" alt=""><figcaption></figcaption></figure>

Następnie dodamy pliki pakietu. Pliki można dodawać pojedynczo lub całymi folderami, wybierając opcję Dodaj katalog (1). Aby dodać pliki, które nie są plikami .dyf, należy zmienić typ pliku w oknie przeglądarki na **„Wszystkie pliki(**_._**)”**. Uwaga: dodamy wszystkie pliki bez rozróżniania typów: węzły niestandardowe (.dyf) i pliki przykładów (.dyn). Po opublikowaniu pakietu dodatek Dynamo skategoryzuje te elementy.

<figure><img src="../../.gitbook/assets/map-to-surface-contents.png" alt=""><figcaption></figcaption></figure>

Po wybraniu folderu MapToSurface w Menedżerze pakietów wyświetlana jest zawartość folderu. W przypadku przekazywania własnego pakietu ze złożoną strukturą folderów, gdy dodatek Dynamo nie powinien wprowadzać zmian w strukturze folderów, można włączyć przełącznik „Zachowaj strukturę folderów”. Ta opcja jest przeznaczona dla zaawansowanych użytkowników i jeśli pakiet nie jest celowo skonfigurowany w określony sposób, najlepiej wyłączyć ten przełącznik i pozwolić dodatkowi Dynamo na zorganizowanie plików zgodnie z potrzebami. Kliknij przycisk Dalej, aby kontynuować.

<figure><img src="../../.gitbook/assets/map-to-surface-contents-preview.png" alt=""><figcaption></figcaption></figure>

W tym miejscu można wyświetlić podgląd zorganizowania plików pakietu przez dodatek Dynamo przed opublikowaniem. Kliknij przycisk Zakończ, aby kontynuować.

<figure><img src="../../.gitbook/assets/publish-locally.png" alt=""><figcaption></figcaption></figure>

Opublikuj, klikając przycisk „Opublikuj lokalnie” (1). Postępując zgodnie z tymi instrukcjami, należy koniecznie kliknąć przycisk _„Opublikuj lokalnie”_, a **nie** _„Opublikuj online”, aby uniknąć_ powielenia pakietów w Menedżerze pakietów.

Po opublikowaniu węzły niestandardowe powinny być dostępne w grupie „DynamoPrimer” lub w bibliotece Dynamo.

![](<../images/6-2/3/develop package - install package 02 (1) (1).jpg>)

Teraz spójrzmy na katalog główny, aby sprawdzić, w jaki sposób dodatek Dynamo sformatował utworzony właśnie pakiet. W tym celu przejdź do karty Zainstalowane pakiety > obok pozycji MapToSurface kliknij menu w postaci pionowych kropek > wybierz opcję Pokaż katalog główny.

<figure><img src="../../.gitbook/assets/show-root-directory.png" alt=""><figcaption></figcaption></figure>

Zwróć uwagę, że katalog główny znajduje się w lokalnym położeniu pakietu (pakiet został opublikowany lokalnie). Dodatek Dynamo aktualnie odwołuje się do tego folderu, aby odczytać węzły niestandardowe. Dlatego ważne jest, aby lokalnie opublikować katalog w trwałym położeniu folderu (czyli na przykład nie na pulpicie). Poniżej przedstawiono strukturę folderów pakietu Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. Folder _bin_ zawiera pliki .dll utworzone za pomocą bibliotek C# lub Zero-Touch. W tym pakiecie ich nie ma, więc ten folder jest pusty dla tego przykładu.
> 2. Folder _dyf_ zawiera węzły niestandardowe. Otwarcie tego folderu spowoduje wyświetlenie wszystkich węzłów niestandardowych (plików .dyf) dla tego pakietu.
> 3. W folderze dodatkowym („extra”) znajdują się wszystkie dodatkowe pliki. Będą to prawdopodobnie pliki dodatku Dynamo (.dyn) lub dowolne dodatkowe wymagane pliki (.svg, .xls, .jpeg, .sat itp.).
> 4. Plik pkg jest podstawowym plikiem tekstowym definiującym ustawienia pakietu. Jest to zautomatyzowane w dodatku Dynamo, ale możesz to edytować, jeśli chcesz przejść do szczegółów.

### Publikowanie pakietu online

{% hint style="warning" %} Uwaga: ten krok należy wykonać tylko w przypadku, gdy faktycznie publikuje się własny pakiet. {% endhint %}

<figure><img src="../../.gitbook/assets/publish-version.png" alt=""><figcaption></figcaption></figure>

1. Gdy wszystko jest gotowe do opublikowania, w oknie Pakiety > Menedżer pakietów > Zainstalowane pakiety wybierz przycisk znajdujący się po prawej stronie pakietu, który chcesz opublikować, i wybierz opcję Opublikuj.
2. Jeśli aktualizujesz pakiet, który już został opublikowany, wybierz opcję „Opublikuj wersję”, a dodatek Dynamo zaktualizuje pakiet online na podstawie nowych plików w katalogu głównym tego pakietu. To wystarczy.

#### Testowanie serwera Menedżera pakietów
Podczas testowania Menedżera pakietów nie wysyłaj pakietów testowych na serwer produkcyjny. Użyj serwera pomostowego. Zapobiega to zanieczyszczaniu rzeczywistych pakietów i działań za pomocą tych pakietów. Skonfigurowanie dodatku Dynamo do korzystania z serwera pomostowego jest łatwe. 

Aby uzyskać więcej informacji na ten temat, zobacz [stronę wiki dotyczącą testowania serwera Menedżera pakietów](https://github.com/DynamoDS/Dynamo/wiki/Testing-the-Package-Manager-Server).

### Opublikuj wersję...

W trakcie aktualizowania plików w folderze głównym opublikowanego pakietu można też opublikować nową wersję pakietu, wybierając opcję _„Opublikuj wersję”_ na karcie _Moje pakiety_. Jest to prosty sposób na wprowadzenie niezbędnych aktualizacji zawartości i udostępnienie ich społeczności. Polecenie _Opublikuj wersję_ działa tylko wtedy, gdy użytkownik jest administratorem pakietu.
