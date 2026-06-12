# Cykl rozwojowy i częstotliwość aktualizacji usługi obliczeniowej dodatku Dynamo



W tym dokumencie opisano częstotliwość aktualizacji i politykę pomocy technicznej dla usługi Dynamo Cloud Compute. W niniejszym dokumencie może być ona również określana zamiennie jako „usługa”.

W tym dokumencie opisano sposób zarządzania wersjami silnika, informacje o czasie udostępniania aktualizacji oraz o tym, czego użytkownicy mogą oczekiwać w przypadku uruchamiania grafów dodatku Dynamo w chmurze.

---

## Częstotliwość aktualizacji

Aby sprostać różnym potrzebom użytkowników, dla usługi Dynamo Cloud Compute zachowywane są **dwie różne ścieżki silnika**. Każda ścieżka ma konkretny cel i własny harmonogram aktualizacji:

### Stabilny silnik (produkcja)

Stabilny silnik został zaprojektowany z myślą o niezawodności i spójności w środowiskach produkcyjnych. Jest oparty na najnowszej stabilnej wersji środowiska uruchomieniowego DynamoCore i aktualizowany wtedy, gdy oficjalne wersje dodatku Dynamo są dostępne dla użytkowników dodatku Dynamo na komputer. Początkowo będziemy trzymać się częstotliwości aktualizacji dodatku DynamoRevit.

Ta ścieżka jest przeznaczona dla obciążeń produkcyjnych, w przypadku których niezawodność i przewidywalność mają kluczowe znaczenie. Podczas korzystania ze stabilnego silnika można oczekiwać aktualizacji zgodnych z harmonogramem publicznych wydań dodatku Dynamo, co daje czas na przygotowanie się do zmian i przetestowanie grafów, zanim wpłyną one na procesy.

### Silnik w wersji Preview (Preview/codzienna piaskownica)

Silnik w wersji Preview daje wczesny dostęp do najnowszych funkcji dodatku Dynamo. Bazuje na najnowszej kompilacji środowiska uruchomieniowego DynamoCore i jest często aktualizowany w miarę scalania nowych funkcji i poprawek błędów.

Ta ścieżka jest idealna dla użytkowników, którzy chcą przetestować nadchodzące zmiany, poeksperymentować z nowymi funkcjami przed ich oficjalnym wydaniem lub sprawdzić, czy ich grafy będą nadal działać z przyszłymi wersjami dodatku Dynamo. Silnik w wersji Preview pozwala być na bieżąco ze zmianami i przekazywać opinie zespołowi dodatku Dynamo.

---


## Oś czasu pomocy technicznej

Zrozumienie, jak długo każda wersja silnika jest wspierana, pomaga w planowaniu okien konserwacji i aktualizacji grafów.

### Stabilny silnik

Stabilny silnik otrzymuje aktualizacje, gdy dodatek Dynamo publikuje nową stabilną wersję składnika Dynamo Core w programie Revit. Każda stabilna wersja pozostaje dostępna i wspierana do momentu wdrożenia w usłudze kolejnej stabilnej wersji.

Jeśli na przykład usługa korzysta obecnie z silnika Dynamo 3.6 (stabilnego), będzie z niej korzystać do momentu, gdy dodatek Dynamo 4.0 stanie się ogólnie dostępny dla użytkowników (zazwyczaj z chwilą dostarczenia w programie Revit). Wówczas usługa zostanie zaktualizowana do silnika Dynamo 4.0 (stabilnego).

Takie podejście zapewnia, że usługa w chmurze pozostaje zsynchronizowana ze środowiskiem, którego większość użytkowników używa na komputerach.

### Silnik w wersji Preview

Silnik w wersji Preview jest stale aktualizowany na podstawie najnowszej gałęzi programistycznej dodatku Dynamo. Wraz z postępem prac nad każdą wersją silnik w wersji Preview odzwierciedla te zmiany.

Na przykład gdy dodatek Dynamo 4.1 jest aktywnie opracowywany, silnik w wersji Preview może być oznaczony jako „Dynamo Cloud Compute Service 4.1”. Gdy zaczną się prace programistyczne nad wersją 4.2, silnik w wersji Preview zacznie odzwierciedlać te zmiany i może zostać oznaczony jako „Dynamo Cloud Compute Service 4.2”.

Ponieważ silnik w wersji Preview jest często aktualizowany, można spodziewać się sporadycznych zmian powodujących niezgodność lub funkcji eksperymentalnych. Najlepiej używać go do testowania i walidacji, a nie do procesów w środowisku produkcyjnym.

---

## Wybór odpowiedniego silnika

Przy podejmowaniu decyzji, której ścieżki silnika użyć:

- **Wybierz silnik stabilny**, jeśli potrzebujesz przewidywalnego, przetestowanego zachowania dla procesów w środowisku produkcyjnym lub jeśli wdrażasz grafy dla użytkowników końcowych, którzy oczekują spójnych wyników.

- **Wybierz silnik w wersji Preview**, jeśli chcesz wcześniej przetestować nowe funkcje, zweryfikować, czy Twoje grafy będą działać w nadchodzących wersjach, lub przekazać opinię na temat rozwoju dodatku Dynamo.

Oba silniki działają w tym samym podstawowym środowisku uruchomieniowym dodatku Dynamo. Różnica polega na tym, kiedy i jak często otrzymują aktualizacje. 

---

> **Uwaga: usługa w wersji beta**  
 Usługa Dynamo Cloud Compute jest obecnie w wersji beta. Terminy świadczenia pomocy technicznej i zasady aktualizacji opisane w tym dokumencie odzwierciedlają nasze bieżące intencje podczas eksperymentowania z usługą i jej udoskonalania. Nie są to gwarancje i mogą ulec zmianie w zależności od opinii użytkowników i doświadczenia operacyjnego.



