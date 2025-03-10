# Prośby o ściągnięcie (pull)

Platforma Dynamo polega na kreatywności i zaangażowaniu społeczności, więc zespół dodatku Dynamo zachęca współtwórców do badania możliwości, testowania pomysłów i zbierania opinii w społeczności. Mimo że zachęcamy do tworzenia innowacji, zmiany zostaną scalone tylko w przypadku, gdy ułatwiają korzystanie z dodatku Dynamo i spełniają wytyczne zdefiniowane w tym dokumencie. Zmiany zapewniające jedynie niewielkie korzyści nie zostaną scalone.

#### Oczekiwania dotyczące próśb o ściągnięcie (pull) <a href="#pull-request-expectations" id="pull-request-expectations"></a>

Zespół dodatku Dynamo oczekuje, że prośby o ściągnięcie (pull) będą zgodne z następującymi wskazówkami:

* Przestrzegaj naszych [standardów kodowania](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) i [standardów nazewnictwa węzłów](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards).
* Uwzględnij testy jednostkowe podczas dodawania nowych funkcji.
* Podczas naprawiania błędu dodaj test jednostkowy wskazujący, że bieżące zachowanie jest niewłaściwe.
* Skoncentruj się na jednym problemie. Jeśli pojawi się nowe lub powiązane zagadnienie, utwórz nowy problem.

Oto kilka wskazówek dotyczących tego, czego nie należy robić:

* Zaskakiwanie nas, prośbami o ściągnięcie (pull) zmian o dużych rozmiarach. Zamiast tego należy zgłosić problem i rozpocząć dyskusję, aby można było uzgodnić kierunek prac przed zainwestowaniem dużej ilości czasu.
* Zatwierdzanie (commit) kodu, którego nie napisano samodzielnie. W przypadku znalezienia kodu, który warto byłoby dodać do dodatku Dynamo, przed kontynuowaniem należy zgłosić problem i rozpocząć dyskusję.
* Przesyłanie próśb o ściągnięcie (pull), które zmieniają nagłówki lub pliki związane z licencjami. Jeśli uważasz, że jest z nimi jakiś problem, zgłoś to, a chętnie to omówimy.
* Dodawanie elementów w interfejsie API bez zgłaszania problemu i omówienia tego z nami.

#### Wypełnianie szablonu prośby o ściągnięcie (pull) <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

W przypadku przesyłania prośby o ściągnięcie (pull) należy użyć [domyślnego szablonu prośby o ściągnięcie](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Przed przesłaniem prośby o ściągnięcie (pull) należy się upewnić, że cel został jasno opisany i wszystkie poniższe stwierdzenia można uznać za prawdziwe:

* Po przesłaniu tej prośby o ściągnięcie (pull) baza kodu będzie w lepszym stanie
* Rozwiązanie udokumentowano zgodnie ze [standardami](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)
* Poziom przetestowania rozwiązania, którego dotyczy ta prośba o ściągnięcie (pull), jest odpowiedni
* Jeśli istnieją ciągi widoczne dla użytkownika, są one wyodrębnione do plików `*.resx`
* Wszystkie testy zostały zakończone pomyślnie z wykorzystaniem samoobsługowej ciągłej integracji
* Migawka zmian interfejsu użytkownika, jeśli takie zmiany istnieją
* Zmiany w interfejsie API są zgodne z [semantyczną obsługą wersji](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) i są opisane w [dokumencie zmian w interfejsie API](https://github.com/DynamoDS/Dynamo/wiki/API-Changes).

Zespół Dynamo przypisze do prośby o ściągnięcie (pull) odpowiedniego recenzenta.

#### Proces weryfikacji prośby o ściągnięcie (pull) <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Po przesłaniu prośby o ściągnięcie (pull) może być konieczne dalsze zaangażowanie w proces weryfikacji. Należy pamiętać o następujących kryteriach przeglądu:

* Zespół dodatku Dynamo spotyka się raz w miesiącu, aby przejrzeć prośby o ściągnięcie (pull), od najstarszych do najnowszych.
* Jeśli w przypadku przejrzanej prośby o ściągnięcie (pull) wymagane jest wprowadzenie zmian przez jej właściciela, ten właściciel ma 30 dni na odpowiedź. Jeśli dla danej prośby o ściągnięcie (pull) nie będzie żadnej aktywności do następnej sesji, zostanie ona zamknięta przez zespół lub w zależności od jej przydatności zostanie przejęta przez kogoś z zespołu.
* Do przesyłania próśb o ściągnięcie (pull) należy używać domyślnego szablonu prośby o ściągnięcie dodatku Dynamo.
* Prośby o ściągnięcie (pull), dla których nie wypełniono całkowicie szablonów próśb o ściągnięcie dodatku Dynamo z podaniem wszystkich deklaracji, nie zostaną przejrzane.

#### Wybieranie zatwierdzeń dodatku Dynamo Revit <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Ponieważ na rynku dostępnych jest wiele wersji programu Revit, może być konieczne wprowadzenie zmian w określonych gałęziach wersji dodatku DynamoRevit, tak aby różne wersje programu Revit mogły korzystać z nowych funkcji. Podczas procesu przeglądu współtwórcy odpowiadają za wybranie ich zweryfikowanych zatwierdzeń (commit) w innych gałęziach dodatku DynamoRevit zgodnie z wytycznymi zespołu Dynamo.
