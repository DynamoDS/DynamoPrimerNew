# Publikowanie pakietu 

### Publikowanie pakietu <a href="#publish-a-package" id="publish-a-package"></a>

Pakiety to wygodny sposób przechowywania węzłów i udostępniania ich społeczności dodatku Dynamo. Pakiet może zawierać wszystko, od węzłów niestandardowych utworzonych w obszarze roboczym dodatku Dynamo po węzły pochodne od klasy NodeModel. Pakiety publikuje się i instaluje za pomocą Menedżera pakietów. Oprócz tej strony, także przewodnik [Primer](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) zawiera ogólne wskazówki dotyczące pakietów.

#### Czym jest Menedżer pakietów? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Menedżer pakietów dodatku Dynamo to rejestr oprogramowania (podobny do programu npm), do którego można uzyskać dostęp z poziomu dodatku Dynamo lub w przeglądarce internetowej. Menedżer pakietów umożliwia instalowanie, publikowanie, aktualizowanie i przeglądanie pakietów. Podobnie jak w przypadku programu npm, obsługuje on różne wersje pakietów. Pomaga również w zarządzaniu zależnościami projektu.

W przeglądarce wyszukaj pakiety i wyświetl statystyki: [https://dynamopackages.com/](https://dynamopackages.com)

* W dodatku Dynamo Menedżer pakietów umożliwia instalowanie, publikowanie i aktualizowanie pakietów.

![Wyszukiwanie pakietów](images/dynamopackagemanager.jpg)

> 1. Wyszukaj pakiety online: `Packages > Search for a Package...`
> 2. Wyświetl/edytuj zainstalowane pakiety: `Packages > Manage Packages...`
> 3. Opublikuj nowy pakiet: `Packages > Publish New Package...`

#### Publikowanie pakietu <a href="#publishing-a-package" id="publishing-a-package"></a>

Pakiety publikuje się z poziomu Menedżera pakietów w dodatku Dynamo. Zalecanym procesem jest opublikowanie pakietu lokalnie, przetestowanie go, a następnie opublikowanie go online w celu udostępnienia go społeczności. Korzystając z analizy przypadku NodeModel, wykonamy czynności niezbędne do opublikowania węzła RectangularGrid jako pakietu lokalnie, a następnie w trybie online.

Uruchom dodatek Dynamo i wybierz opcję `Packages > Publish New Package...`, aby otworzyć okno `Publish a Package`.

![Publikowanie pakietu](images/dyn-publish-package-add-files.jpg)

> 1. Wybierz opcję `Add file...`, aby wyszukać pliki, które mają zostać dodane do pakietu
> 2. Wybierz dwa pliki `.dll` z analizy przypadku NodeModel
> 3. Wybierz przycisk `Ok`

Po dodaniu plików do zawartości pakietu nadaj pakietowi nazwę oraz dodaj do niego opis i wersję. Opublikowanie pakietu przy użyciu dodatku Dynamo powoduje automatyczne utworzenie pliku `pkg.json`.

![Ustawienia pakietu](images/dyn-publish-package.jpg)

> Pakiet gotowy do opublikowania.
>
> 1. Podaj wymagane informacje dotyczące nazwy, opisu i wersji.
> 2. Opublikuj, klikając opcję „Opublikuj lokalnie” i wybierając folder pakietu dodatku Dynamo: `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages`, aby węzeł był dostępny w składniku Core. Dopóki pakiet nie będzie gotowy do udostępnienia, zawsze publikuj lokalnie.

Po opublikowaniu pakietu węzły będą dostępne w bibliotece dodatku Dynamo w kategorii `CustomNodeModel`.

![Pakiet w bibliotece dodatku Dynamo](images/dyn-publish-package-library.jpg)

> 1. Właśnie utworzony pakiet w bibliotece dodatku Dynamo

Gdy pakiet będzie gotowy do opublikowania w trybie online, otwórz Menedżera pakietów i wybierz opcję `Publish`, a następnie opcję `Publish Online`.

![Publikowanie pakietu w Menedżerze pakietów](images/dyn-publish-package-directory.jpg)

> 1. Aby sprawdzić, jak dodatek Dynamo sformatował pakiet, kliknij pionowy trzykropek po prawej stronie pozycji „CustomNodeModel” i wybierz opcję „Pokaż katalog główny”.
> 2. Wybierz opcję `Publish`, a następnie opcję `Publish Online` w oknie „Publikowanie pakietów Dynamo”.
> 3. Aby usunąć pakiet, wybierz opcję `Delete`.

#### Jak mogę zaktualizować pakiet? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

Aktualizowanie pakietu jest procesem podobnym do publikowania. Otwórz Menedżera pakietów i wybierz opcję `Publish Version...` dla pakietu wymagającego aktualizacji, a następnie wprowadź nowszą wersję.

![Publikowanie wersji pakietu](images/dyn-publish-package-version.jpg)

> 1. Wybierz opcję `Publish Version`, aby zaktualizować istniejący pakiet o nowe pliki w katalogu głównym, a następnie określ, czy ma on zostać opublikowany lokalnie, czy online.

#### Klient internetowy Menedżera pakietów <a href="#package-manager-web-client" id="package-manager-web-client"></a>

Klient internetowy Menedżera pakietów służy wyłącznie do wyszukiwania i wyświetlania danych pakietu, na przykład informacji o wersjach i statystyki pobierania.

Dostęp do klienta internetowego Menedżera pakietów można uzyskać za pomocą tego linku: [https://dynamopackages.com/](https://dynamopackages.com)

![Klient internetowy Menedżera pakietów ](images/packagemanager-browser.jpg)
