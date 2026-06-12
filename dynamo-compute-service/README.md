# Dynamo Cloud Compute



Usługa Dynamo Cloud Compute przenosi możliwości środowiska uruchomieniowego programowania wizualnego dodatku Dynamo do chmury. Zamiast uruchamiać grafy na komputerze lokalnym, usługa obliczeniowa wykonuje je w bezpiecznym środowisku w chmurze i zwraca wyniki.

## Co to jest dodatek Dynamo?

Dynamo to język programowania wizualnego i środowisko tworzenia. Umożliwia tworzenie programów przez łączenie węzłów w grafie. Środowisko uruchomieniowe dodatku Dynamo wykonuje te grafy, umożliwiając automatyzację złożonych zadań, generowanie geometrii i integrację z innym oprogramowaniem.

## Jak działa usługa obliczeniowa

W przypadku korzystania z dodatku Dynamo za pośrednictwem klienta bazującego na chmurze (takiego jak program Dynamo Player w usłudze Forma) pliki grafu `.dyn` są wysyłane do usługi obliczeniowej w celu wykonania. Usługa:

1. Odbiera graf i wszystkie parametry wejściowe.
2. Wykonuje graf w izolowanym środowisku w chmurze.
3. Zwraca wyniki z powrotem do aplikacji klienckiej.

To oparte na chmurze podejście oznacza, że można uruchamiać grafy dodatku Dynamo bez instalowania dodatku Dynamo lokalnie, a także można wykorzystać moc obliczeniową chmury do złożonych operacji.

## Dlaczego warto korzystać z usługi Dynamo Cloud Compute?

Usługa Dynamo Cloud Compute pozwala realizować scenariusze, w których chcesz:

**Uruchamiać grafy bez instalacji na komputerze**: grafy dodatku Dynamo można wykonywać bezpośrednio z aplikacji internetowych bez konieczności instalowania dodatku Dynamo na komputerze.

**Współpracować i udostępniać**: grafy można udostępniać członkom zespołu, którzy mogą je uruchamiać za pośrednictwem interfejsów internetowych, takich jak Forma, co ułatwia wdrażanie zautomatyzowanych procesów w całej organizacji.

**Wykorzystywać przetwarzanie w chmurze**: korzystaj z infrastruktury chmury przy wykonywaniu operacji wymagających dużej mocy obliczeniowej, które na komputerach lokalnych potrzebowałyby więcej czasu.

**Standaryzować środowisko uruchomieniowe**: zapewnij spójne zachowanie dla różnych użytkowników i komputerów, uruchamiając grafy w kontrolowanym środowisko w chmurze.

**Połączyć z usługą Forma**: wchodź w interakcję z interfejsem API usługi Forma za pomocą dodatku Dynamo. [Więcej informacji można znaleźć w tym wpisie w blogu.](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/)

## Najważniejsze cechy

**Wykonywanie w chmurze**: grafy są uruchamiane w chmurze, a nie na komputerze lokalnym. Oznacza to, że:
- Nie ma potrzeby instalowania dodatku Dynamo na komputerze w celu uruchamiania grafów.
- Ma się dostęp do zasobów obliczeniowych w chmurze.
- Korzysta się ze spójnego środowiska uruchomieniowego dla różnych użytkowników.

**Bezpieczeństwo**: usługa wykonuje grafy każdego użytkownik w odizolowanych środowiskach, aby zapewnić bezpieczeństwo danych i prywatność. Twoje grafy i dane są przechowywane oddzielnie względem grafów i danych innych użytkowników.

**Przetwarzanie asynchroniczne**: wykonywanie grafu odbywa się asynchronicznie, czyli klienci przesyłają zadanie i mogą sprawdzać jego stan do momentu jego ukończenia. Umożliwia to wykonywanie długotrwałych obliczeń bez blokowania procesu.

## Bieżąca dostępność

Usługa Dynamo Cloud Compute jest obecnie dostępny za pośrednictwem:
- **Programu Dynamo Player w otwartej wersji beta usługi Forma**: grafy dodatku Dynamo można przesyłać, udostępniać i wykonywać bezpośrednio w interfejsie internetowym usługi Autodesk Forma.

## Dowiedz się więcej

- [Różnice między usługą Dynamo Cloud Compute i dodatkiem Dynamo na komputer](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md) — ważne różnice, o których należy pamiętać podczas pisania grafów do wykonywania w chmurze.
- [Cykl życia silnika](engine-lifecycle.md) — informacje o obsługiwanych wersjach silnika i ich cyklu życia.

-----


> **Uwaga: usługa w wersji beta**  
 Usługa Dynamo Cloud Compute jest obecnie w wersji beta. Terminy świadczenia pomocy technicznej i zasady aktualizacji opisane w tym dokumencie odzwierciedlają nasze bieżące intencje podczas eksperymentowania z usługą i jej udoskonalania. Nie są to gwarancje i mogą ulec zmianie w zależności od opinii użytkowników i doświadczenia operacyjnego.