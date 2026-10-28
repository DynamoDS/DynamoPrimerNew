# Czym jest serwer Dynamo MCP

**MCP** to skrót od Model Context Protocol. Jest to otwarty standard, który umożliwia modelom AI łączenie się z oprogramowaniem za pośrednictwem wspólnego protokołu. Serwer MCP udostępnia narzędzia i dane programu, dzięki czemu klient AI może odczytywać aktualny kontekst, podejmować działania i zwracać ustrukturyzowane wyniki.

**DynamoMCP** to wbudowany serwer MCP dodatku Dynamo. Łączy on narzędzie Autodesk Assistant z aktywną sesją dodatku Dynamo — grafem, węzłami, połączeniami i stanem obszaru roboczego — dzięki czemu pomoc AI jest oparta na tym, nad czym faktycznie pracujesz.

Dla użytkowników dodatku Dynamo oznacza to, że Assistant może zrobić więcej niż tylko odpowiadać na ogólne pytania. Może sprawdzić otwarty graf, pomóc w rozwiązywaniu problemów z węzłami i aktualizować obszar rysunku na podstawie rzeczywistego kontekstu sesji, dzięki czemu nie jest konieczne ręczne kopiowanie i wklejanie danych.

Serwer DynamoMCP jest dostępny w dodatku Dynamo 4.2.0 i jego nowszych wersjach.

### Jak korzystać z serwera DynamoMCP

Z serwera DynamoMCP korzysta się za pośrednictwem narzędzia Autodesk Assistant — nie ma osobnej konfiguracji ani oddzielnego interfejsu. Assistant jest punktem wejścia do zadawania pytań, uzyskiwania pomocy dotyczącej grafu oraz umożliwiania AI pracy z bieżącym kontekstem dodatku Dynamo.

Miejsce, w którym otwierasz narzędzie Assistant, zależy od tego, gdzie pracujesz:

- **Dynamo Sandbox** — narzędzia Dynamo MCP są dostępne w narzędziu Autodesk Assistant w dodatku Dynamo.
- **Revit** — narzędzia Dynamo MCP są dostępne w narzędziu Autodesk Assistant w programie Revit.

W obu przypadkach pozostajesz w bieżącej sesji i korzystasz z tego samego interfejsu narzędzia Assistant, aby uzyskać dostęp do serwera DynamoMCP.
### Co potrafi DynamoMCP

Serwer DynamoMCP sprawia, że Assistant ma dostęp do zestawu **narzędzi**. Każde narzędzie wykonuje jedno zadanie w dodatku Dynamo, takie jak odczytywanie obszaru rysunku, umieszczanie węzła lub uruchamianie grafu. Nigdy nie wywołujesz tych narzędzi samodzielnie. Językiem naturalnym opisujesz to, czego potrzebujesz, a Assistant wybiera potrzebne narzędzia.

Znajomość tych narzędzi nadal jest przydatna. Dzięki niej wiesz, co Assistant potrafi zrobić, i łatwiej formułujesz żądania, na podstawie których może działać.

{% hint style="info" %}
Narzędzia, których nazwy rozpoczynają się od `get_`, tylko odczytują informacje z sesji. Każde inne narzędzie służy do zmieniania grafu.
{% endhint %}

#### Odczytywanie bieżącego grafu

Przy użyciu tych narzędzi Assistant sprawdza, nad czym pracujesz, zanim udzieli odpowiedzi lub wprowadzi zmianę.

| Narzędzie | Działanie |
| --- | --- |
| `get_workspace_info` | Wyświetla listę elementów znajdujących się na obszarze rysunku: węzły, połączenia, grupy, uwagi i szczegóły obszaru roboczego. |
| `get_selection` | Zwraca informacje o zaznaczonych węzłach, połączeniach i grupach. |
| `get_node_connections` | Pokazuje, z czym jest połączony węzeł (port po porcie). |
| `get_node_property` | Zwraca ustawienia węzła, takie jak ustawienia podglądu, zablokowania, przypięcia podglądu i etykiet. |
| `get_node_lacing` | Zwraca ustawienie skratowania węzła. |
| `get_installation_info` | Zwraca informacje o wersji dodatku Dynamo i hoście, na którym ten dodatek działa, oraz łącze do informacji o wersji. |

*Przykładowy prompt: „Co teraz znajduje się w moim obszarze rysunku?”*

#### Znajdowanie odpowiedniego węzła

Dynamo zawiera tysiące węzłów, w tym węzły z zainstalowanych pakietów. Dzięki tym narzędziom Assistant może wybrać odpowiednią właściwość, zamiast zgadywać.

| Narzędzie | Działanie |
| --- | --- |
| `get_nodes_info` | Przeszukuje bibliotekę węzłów, w tym węzły z pakietów, i zwraca dokładny węzeł do umieszczenia. |
| `get_node_help` | Zwraca tekst pomocy dotyczącej węzła i ścieżkę do jego przykładowego grafu (jeśli istnieje). |
| `get_type_schema` | Zwraca oczekiwany format danych dla danego typu dodatku Dynamo, taki jak Point (punkt), Number (liczba) lub Color (kolor). |

*Przykładowy prompt: „Którego węzła należy użyć do pobrania pola powierzchni?”*

#### Tworzenie i edytowanie grafu

Te narzędzia wykonują działania na obszarze rysunku.

| Narzędzie | Działanie |
| --- | --- |
| `create_nodes` | Umieszcza jeden lub więcej węzłów na obszarze rysunku (z opcjonalnymi wartościami początkowymi). |
| `set_node_value` | Ustawia wartość węzła wejściowego, takiego jak Number (liczba), String (ciąg), Code Block (blok kodu) lub lista rozwijana. |
| `connect_nodes` | Łączy port wyjściowy z portem wejściowym. |
| `disconnect_nodes` | Usuwa połączenia między dwoma węzłami. |
| `delete_node` | Usuwa węzeł z obszaru rysunku. |
| `rename_node` | Zmienia nazwy węzłów wejściowych i obserwacyjnych (Watch), aby ich przeznaczenie było jasne. Węzły funkcji (Function) zachowują oryginalne nazwy. |
| `select_nodes` | Zaznacza węzły na obszarze rysunku, dzięki czemu widać, do których z nich odnosi się Assistant. |
| `set_node_property` | Włącza lub wyłącza ustawienia węzła, takie jak ustawienia podglądu, zablokowania, przypięcia podglądu, etykiet oraz wejścia i wyjścia. |
| `set_node_lacing` | Ustawia skratowanie na Automatyczne, Najkrótsze, Najdłuższe lub Iloczyn wektorowy. |
| `set_node_port_list_at_level` | Ustawia poziom listy w porcie wejściowym. |

*Przykładowy prompt: „Dodaj suwak liczbowy i podłącz go do wejścia promienia mojego okręgu”.*

#### Organizowanie i dokumentowanie grafu

Działający graf nie zawsze jest czytelny. Te narzędzia obsługują układ, grupowanie, kolory i uwagi.

| Narzędzie | Działanie |
| --- | --- |
| `create_group` | Tworzy grupę obejmującą zestaw węzłów. |
| `create_groups` | Tworzy kilka grup w jednym kroku. |
| `ungroup_by_guid` | Usuwa grupę. Zawarte w niej węzły pozostają na obszarze rysunku. |
| `ungroup_by_title` | Usuwa grupy na podstawie ich tytułu. Zawarte w nich węzły pozostają na obszarze rysunku. |
| `get_available_group_styles` | Wyświetla listę dostępnych stylów grup, w tym style niestandardowe z Twoich preferencji. |
| `assign_group_styles` | Stosuje style grup, takie jak Wejścia, Działania, Wyjścia i Przegląd. |
| `get_notes` | Wyświetla uwagi w grafie. |
| `create_notes` | Dodaje uwagi objaśniające części grafu. |
| `update_notes` | Edytuje tekst uwagi lub ją przenosi. |
| `delete_notes` | Usuwa uwagi. |
| `set_note_state` | Przypina uwagę do węzła (aby oba elementy były przenoszone razem) lub odpina ją. |
| `auto_layout_workspace` | Zmienia rozmieszczenie węzłów za pomocą wbudowanej funkcji układu automatycznego dodatku Dynamo. |
| `set_workspace_description` | Zapisuje opis grafu we właściwościach pliku. |

*Przykładowy prompt: „Pogrupuj i pokoloruj ten graf według wejść, działań i wyjść, a następnie dodaj krótkie uwagi”.*

#### Uruchamianie grafu i sprawdzanie wyników

| Narzędzie | Działanie |
| --- | --- |
| `run_workspace` | Uruchamia bieżący graf. W trybie automatycznym graf jest ponownie uruchamiany po każdej zmianie, więc ten krok jest pomijany. |
| `get_workspace_warnings` | Wyświetla listę węzłów z ostrzeżeniami lub błędami, tekst komunikatu i sugerowane kolejne kroki. Graf musi zostać najpierw uruchomiony. |
| `get_node_output_values_shapes` | Podsumowuje wartości wyjściowe jednego węzła albo wszystkich węzłów w grafie. |
| `get_node_output_value` | Odczytuje pełne dane wyjściowe pojedynczego węzła, stronami, w przypadku wyników zbyt dużych, aby można było je podsumować. |

*Przykładowy prompt: „Uruchom graf i poinformuj mnie, dlaczego węzeł List.GetItemAtIndex wyświetla ostrzeżenie”.*

#### Praca z plikami grafów

Assistant potrafi też pracować z grafami zapisanymi na dysku, a nie tylko z grafem, który jest aktualnie otwarty.

| Narzędzie | Działanie |
| --- | --- |
| `new_workspace` | Otwiera nowy, pusty graf. Jeśli bieżący graf zawiera niezapisane zmiany, najpierw pojawia się prośba o ich zapisanie lub odrzucenie. |
| `save_workspace` | Zapisuje bieżący graf. Bez podania położenia zapisuje go w podfolderze Dynamo folderu Dokumenty. |
| `edit_graph_from_path` | Otwiera zapisany graf do edycji, bez jego uruchamiania. |
| `insert_graph_from_path` | Scala zapisany graf z bieżącym obszarem rysunku. W ten sposób są wprowadzane przykładowe grafy węzłów. |
| `run_graph_from_path` | Uruchamia zapisany graf (opcjonalnie z podanymi wartościami wejściowymi). |
| `run_graph_from_json` | Uruchamia graf dostarczony jako plik JSON dodatku Dynamo (bez pliku na dysku). |
| `get_graph_info_from_path` | Odczytuje wejścia, wyjścia i metadane zapisanego grafu bez jego otwierania. |
| `get_graph_info_from_json` | Odczytuje te same informacje z grafu dostarczonego jako plik JSON dodatku Dynamo. |
| `get_graphs_info_from_path` | Skanuje folder grafów i zwraca informacje o zawartości każdego z nich. |
| `get_available_graphs_folders` | Wyświetla listę folderów, które są znane dodatkowi Dynamo i mogą zawierać grafy. |

*Przykładowy prompt: „Przejrzyj folder grafów dodatku Dynamo i poinformuj mnie, które grafy umieszczają ściany”.*

{% hint style="warning" %}
Assistant może zmienić graf. Zapisz pracę, zanim poprosisz o dużą edycję, i sprawdź wynik, zanim nadpiszesz istniejący plik.
{% endhint %}
