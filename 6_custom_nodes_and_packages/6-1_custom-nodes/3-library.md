# Publikowanie w bibliotece użytkownika

Właśnie utworzyliśmy węzeł niestandardowy i zastosowaliśmy go do określonego procesu na wykresie Dynamo. Ten węzeł podoba nam się tak bardzo, że chcemy pozostawić go w naszej bibliotece dodatku Dynamo, aby odwoływać się do niego na innych wykresach. Aby to zrobić, opublikujemy ten węzeł lokalnie. Jest to proces podobny do publikowania pakietu, który bardziej szczegółowo omówimy w następnym rozdziale.

Węzeł opublikowany lokalnie będzie dostępny w bibliotece dodatku Dynamo po otwarciu nowej sesji. Jeśli nie opublikujemy węzła, w folderze wykresu Dynamo, który odwołuje się do węzła niestandardowego, musi również znajdować się ten węzeł niestandardowy (lub węzeł niestandardowy musi być zaimportowany do dodatku Dynamo za pomocą polecenia _Plik > Importuj bibliotekę_).

{% hint style="warning" %}
Publikowanie węzła niestandardowego jest możliwe tylko w dodatku Dynamo dla programu Revit i dodatku Dynamo dla programu Civil 3d. Obszar izolowany (piaskownica) dodatku Dynamo nie ma funkcji publikowania.
{% endhint %}

## Ćwiczenie: publikowanie węzła niestandardowego lokalnie

> Pobierz plik przykładowy, klikając poniższe łącze.
>
> Pełna lista plików przykładowych znajduje się w załączniku.

{% file src="../datasets/6-1/3/PointsToSurface.dyf" %}

Kontynuujmy pracę z węzłem niestandardowym, który utworzyliśmy w poprzedniej sekcji. Po otwarciu węzła niestandardowego PointsToSurface widać wykres w Edytorze węzłów niestandardowych dodatku Dynamo. Węzeł niestandardowy można również otworzyć, klikając go dwukrotnie w Edytorze wykresu Dynamo.

![](<../images/6-1/3/publish custom node locally 01.jpg>)

Aby opublikować węzeł niestandardowy lokalnie, wystarczy kliknąć prawym przyciskiem myszy obszar rysunku i wybrać opcję _„Opublikuj ten węzeł niestandardowy...”_

![](<../images/6-1/3/publish custom node exercise - 02.jpg>)

Podaj odpowiednie informacje (podobnie, jak to zrobiono na ilustracji powyżej) i wybierz opcję _„Opublikuj lokalnie”._. Należy zwrócić uwagę, że pole Grupa definiuje element główny dostępny w menu dodatku Dynamo.

![](<../images/6-1/3/publish custom node exercise - 03.jpg>)

Wybierz folder, w którym będą przechowywane wszystkie węzły niestandardowe publikowane lokalnie. Ten folder będzie sprawdzany przy każdym wczytywaniu dodatku Dynamo, dlatego upewnij się, że jest to folder trwały. Przejdź do tego folderu i wybierz opcję _„Wybierz folder”._ Węzeł Dynamo jest teraz publikowany lokalnie i będzie dostępny na pasku narzędzi dodatku Dynamo po każdym wczytaniu programu.

![](<../images/6-1/3/publish custom node exercise - 04.jpg>)

Aby sprawdzić położenie folderu węzłów niestandardowych, przejdź do obszaru _Dynamo > Preferencje > Menedżer pakietów > Ścieżki do węzłów i pakietów_

![](<../images/6-1/3/publish custom node exercise - 05.jpg>)

W tym oknie jest widoczna lista ścieżek.

![](<../images/6-1/3/publish custom node exercise - 06.jpg>)

> 1. Ścieżka _Documents\DynamoCustomNodes..._ odnosi się do położenia węzłów niestandardowych, które zostały opublikowane lokalnie.
> 2. _AppData\Roaming\Dynamo..._ odnosi się do domyślnego położenia pakietów dodatku Dynamo instalowanych online.
> 3. Możesz przenieść ścieżkę folderu lokalnego w dół powyższej listy (wybierając ścieżkę folderu i klikając strzałkę skierowaną w dół po lewej stronie nazwy ścieżki). Folder na początku listy stanowi domyślną ścieżkę do instalacji pakietów. Dlatego jeśli domyślna ścieżka instalacji pakietów Dynamo zostanie zachowana jako folder domyślny, pakiety online będą oddzielone od węzłów opublikowanych lokalnie.

Zmieniliśmy kolejność nazw ścieżek, aby lokalizacją instalacji pakietu była domyślna ścieżka dodatku Dynamo.

![](<../images/6-1/3/publish custom node exercise - 07.jpg>)

Po przejściu do tego folderu lokalnego oryginalny węzeł niestandardowy znajdziemy w folderze _„.dyf”_ — ta nazwa stanowi rozszerzenie pliku węzła niestandardowego dodatku Dynamo (Dynamo Custom Node). Możemy edytować plik w tym folderze, a węzeł zostanie zaktualizowany w interfejsie użytkownika. Możemy także dodać więcej węzłów do głównego folderu _DynamoCustomNode_, a dodatek Dynamo doda je do biblioteki po ponownym uruchomieniu.

![](<../images/6-1/3/publish custom node exercise - 08.jpg>)

Dodatek Dynamo będzie teraz za każdym razem wczytywany z elementem „PointsToSurface” w grupie „DynamoPrimer” w bibliotece dodatku Dynamo.

![](<../images/6-1/3/publish custom node exercise - 09.jpg>)
