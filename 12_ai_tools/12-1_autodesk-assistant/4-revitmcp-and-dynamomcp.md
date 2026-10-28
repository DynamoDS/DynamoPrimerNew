# RevitMCP i DynamoMCP

Autodesk Assistant pobiera swoje funkcje z serwerów MCP. Assistant od każdego serwera otrzymuje zestaw narzędzi. Każde narzędzie wykonuje jedno zadanie w produkcie, do którego należy.

Gdy pracujesz w programie Revit, istotne są dwa z tych serwerów: **RevitMCP** i **DynamoMCP**. Nie są to konkurencyjne opcje i nie musisz wybierać między nimi. Działają na różnych poziomach, a Assistant korzysta z tego, który pasuje do danego żądania.

### RevitMCP: bezpośrednie operacje na modelu

Serwer RevitMCP, dostarczany jako [Revit Public MCP Server](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm), udostępnia starannie dobrany zestaw operacji interfejsu API programu Revit w postaci narzędzi MCP. Każde narzędzie odpowiada określonemu działaniu w modelu: znajdowaniu elementów, sprawdzaniu parametrów i liczności, zbiorczemu edytowaniu parametrów oraz przechwytywaniu migawek widoków.

To najkrótsza droga do wykonania zadania, które dany zestaw narzędzi już obsługuje. Jedno żądanie staje się jednym wywołaniem, a model natychmiast ulega zmianie. Nie trzeba tworzyć grafu ani niczego później uruchamiać.

### DynamoMCP: narzędzia tworzące operację

DynamoMCP działa o jeden poziom wyżej. Jego narzędzia nie wykonują działań modelowania bezpośrednio. Zamiast tego tworzą graf Dynamo: umieszczają węzły, łączą je ze sobą, ustawiają wartości i uruchamiają uzyskany graf. Aby zapoznać się z pełną listą narzędzi, zobacz [Co to jest serwer Dynamo MCP](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md).

Ma to znaczenie ze względu na to, czym właściwie jest węzeł. Każdy węzeł w bibliotece dodatku Dynamo jest sam w sobie co najmniej jednym wywołaniem interfejsu API — już napisanym, przetestowanym i udokumentowanym. Węzły pakietów i węzły Python są dostępne w ten sam sposób. Dzięki temu niewielki zestaw narzędzi do tworzenia grafów daje dostęp do całej biblioteki dodatku Dynamo.

W efekcie narzędzia serwera DynamoMCP tworzą nowe funkcje, a nie tylko korzystają z już istniejących. Zamiast korzystać ze stałej listy działań, Assistant składa żądane działanie z węzłów. W praktyce narzędzia MCP, które tworzą grafy, zachowują się jak narzędzia tworzące kolejne narzędzia.

### Różnica w praktyce

Załóżmy, że pytasz o oznaczenia drzwi na jednej kondygnacji.

- Gdy zajmuje się tym RevitMCP, jest wykonywane bezpośrednie wywołanie i tagi po prostu się pojawiają.
- Gdy zajmuje się tym DynamoMCP, Assistant umieszcza węzły, które zbierają informacje o drzwiach, filtrują je z użyciem wskazanej kondygnacji i tworzą oznaczenia, a następnie uruchamia graf.

Obie ścieżki skutkują oznaczeniem drzwi. Druga ścieżka dodatkowo pozostawia coś na obszarze rysunku.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| Co robi narzędzie | Wykonuje określoną operację programu Revit | Tworzy i uruchamia graf Dynamo |
| Zasięg | Operacje zawarte w zestawie narzędzi | Dowolny węzeł w bibliotece, w tym węzły pakietów i węzły Python |
| Co otrzymujesz jako wynik | Zmiana w modelu | Zmiana w modelu i graf, który ją wprowadził |
| Polecane zastosowanie | Pojedyncze, dobrze zdefiniowane zadanie | Logika, którą chcesz sprawdzić, dostosować, ponownie wykorzystać lub udostępnić |

### Dlaczego warto mieć graf

Graf jest zapisem wykonanej pracy, a nie tylko jej wyniku. Gdy już znajduje się na obszarze rysunku, masz następujące możliwości:

- Zapoznaj się z grafem i dokładnie sprawdź, co zostało zrobione.
- Zmień wartość i ponownie uruchom graf, bez konieczności powtarzania całego zadania od nowa.
- Zapisz graf i uruchom go w kolejnym projekcie lub przekaż innemu członkowi zespołu.
- Opublikuj graf w odtwarzaczu Dynamo, aby mogły go uruchamiać osoby, które nie korzystają z dodatku Dynamo.

To pierwsze żądanie pochłania najwięcej czasu. Każde następne uruchomienie jest niemal bezkosztowe.

{% hint style="info" %}
Assistant w środowisku Dynamo Sandbox korzysta wyłącznie z serwera DynamoMCP, ponieważ nie ma modelu programu Revit, na którym mógłby pracować. W programie Revit dostępne są oba te serwery.
{% endhint %}

### Opisywanie, czego oczekujesz

W prompcie nie musisz podawać nazwy serwera. Wystarczy opisać oczekiwany wynik, a Assistant wybierze narzędzia.

Jeśli zależy Ci konkretnie na grafie, napisz to. Dzięki promptowi takiemu jak *„utwórz graf oznaczający drzwi na kondygnacji 2”* Assistant wie, że chodzi o wersję wielokrotnego użytku, a nie tylko o oznaczone drzwi.

### Więcej informacji

- [Autodesk Assistant (Tech Preview)](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) — informacje o tym, co Assistant robi w programie Revit, w pomocy do programu Revit 2027.
- [Revit Public MCP Server (Tech Preview)](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) — strona programu Revit w architekturze MCP, w pomocy do programu Revit 2027.
- [Autodesk Assistant: przegląd informacji i często zadawane pytania](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) — informacje o tym, w których produktach jest zawarte narzędzie Assistant i jak można z niego korzystać.
