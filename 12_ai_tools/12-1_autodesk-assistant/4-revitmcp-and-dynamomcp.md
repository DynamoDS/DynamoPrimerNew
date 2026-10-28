# RevitMCP und DynamoMCP

Autodesk Assistant greift über MCP-Server auf Funktionen zu. Jeder Server stellt Assistant einen Satz von Werkzeugen zur Verfügung, und jedes Werkzeug erledigt eine Aufgabe in dem Produkt, zu dem es gehört.

Wenn Sie in Revit arbeiten, sind zwei dieser Server von Bedeutung: **RevitMCP** und **DynamoMCP**. Es handelt sich nicht um konkurrierende Optionen, und Sie müssen sich nicht für eine davon entscheiden. Sie funktionieren auf verschiedenen Ebenen, und Assistant verwendet die für Ihre Anforderung geeignete Ebene.

### RevitMCP: Direkte Modellvorgänge

RevitMCP, der als [öffentlicher MCP-Server von Revit](https://help.autodesk.com/cloudhelp/2027/DEU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) bereitgestellt wird, umfasst einen kuratierten Satz von Revit API-Vorgängen als MCP-Werkzeuge. Jedes Werkzeug ist einer definierten Aktion im Modell zugeordnet: Suchen von Elementen, Prüfen von Parametern und deren Anzahl, Massenbearbeitung von Parametern und Erfassen von Screenshots von Ansichten.

Dies ist der kürzeste Pfad für eine Aufgabe, die das Toolset bereits abdeckt. Aus einer Anfrage wird ein Aufruf, und das Modell ändert sich sofort. Es muss kein Diagramm erstellt und kein Element nachfolgend ausgeführt werden.

### DynamoMCP: Werkzeuge zum Erstellen des Vorgangs

DynamoMCP arbeitet eine Ebene höher. Seine Werkzeuge führen Modellierungsaktionen nicht direkt aus. Stattdessen erstellen sie ein Dynamo-Diagramm: Sie platzieren Blöcke, vernetzen sie, legen Werte fest und führen das Ergebnis aus. Eine vollständige Werkzeugliste finden Sie unter [Informationen zum Dynamo MCP-Server](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md).

Dies ist wichtig, weil es sich um einen Block handelt. Bei jedem Block in der Dynamo-Bibliothek handelt es sich selbst um einen oder mehrere API-Aufrufe, die bereits geschrieben, getestet und dokumentiert wurden. Paketblöcke und Python-Blöcke sind auf die gleiche Weise verfügbar. So erreicht ein kleiner Satz von Diagrammerstellungswerkzeugen die gesamte Dynamo-Bibliothek.

Dies hat zur Folge, dass die Werkzeuge von DynamoMCP neue Funktionen erzeugen, anstatt nur vorhandene Funktionen zu nutzen. Anstelle einer festen Liste von Aktionen stellt Assistant die Aktion, die Sie angefordert haben, aus Blöcken zusammen. In der Praxis verhalten sich MCP-Werkzeuge, die Diagramme schreiben, wie Werkzeuge, die weitere Werkzeuge erstellen.

### Der Unterschied in der Praxis

Angenommen, Sie fragen nach Türbeschriftungen auf einer Ebene.

- Die von RevitMCP abgewickelte Arbeit ist ein direkter Aufruf, und die Beschriftungen werden angezeigt.
- Assistant wird von DynamoMCP verwaltet und platziert die Blöcke, die die Türen sammeln, sie auf die von Ihnen benannte Ebene filtern, die Beschriftungen erstellen und dann das Diagramm ausführen.

Bei beiden Routen werden die Türen mit einer Beschriftung versehen. Die zweite Route hinterlässt auch etwas im Ansichtsbereich.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| Funktion eines Werkzeugs | Führt einen definierten Revit-Vorgang aus | Erstellt ein Dynamo-Diagramm und führt es aus |
| Reichweite | Die im Werkzeugsatz enthaltenen Vorgänge | Alle Blöcke in Ihrer Bibliothek, einschließlich Paket- und Python-Blöcke |
| Was Sie zurückbekommen | Die Änderung im Modell | Die Änderung im Modell sowie das Diagramm, aus dem es resultiert |
| Am besten geeignet für | Eine einzelne, klar definierte Aufgabe | Logik, die Sie prüfen, anpassen, wiederverwenden oder freigeben möchten |

### Warum sich das Diagramm lohnt

Ein Diagramm ist eine Aufzeichnung der Arbeit, nicht nur des Ergebnisses. Sobald es sich im Ansichtsbereich befindet, haben Sie folgende Möglichkeiten:

- Lesen Sie es, um festzustellen, was gemacht wurde.
- Ändern Sie einen Wert, und führen Sie es erneut aus, ohne die gesamte Aufgabe erneut abzufragen.
- Speichern Sie es, und führen Sie es im nächsten Projekt aus, oder geben Sie es an einen Kollegen weiter.
- Publizieren Sie es in Dynamo Player, damit es von Personen ausgeführt werden kann, die Dynamo nicht verwenden.

Die erste Anforderung ist zeitabhängig. Jede Ausführung danach ist beinahe kostenlos.

{% hint style="info" %}
In Dynamo Sandbox erreicht Assistant nur DynamoMCP, da kein Revit-Modell vorhanden ist, auf das reagiert werden kann. In Revit ist beides verfügbar.
{% endhint %}

### Entscheiden, wonach gefragt werden soll

Sie müssen im Prompt keinen Server nennen. Es genügt, das Ergebnis zu beschreiben. Assistant wählt daraufhin die Werkzeuge aus.

Wenn Sie speziell ein Diagramm wollen, sagen Sie es. Prompts wie *"Erstelle mir ein Diagramm, das die Türen auf Ebene 2 beschriftet"* weisen Assistant darauf hin, dass es um die wiederverwendbare Version geht und nicht nur um die beschrifteten Türen.

### Weiterführende Informationen

- [Autodesk Assistant (Technology Preview)](https://help.autodesk.com/cloudhelp/2027/DEU/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm): Funktionen von Assistant in Revit in der Revit 2027-Hilfe.
- [Öffentlicher MCP-Server in Revit (Technology Preview)](https://help.autodesk.com/cloudhelp/2027/DEU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm): die Revit-Seite von MCP in der Revit 2027-Hilfe.
- [Autodesk Assistant: Allgemeiner Überblick und häufig gestellte Fragen](https://www.autodesk.com/de/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html): welche Produkte Assistant enthalten und wie er verwendet wird.
