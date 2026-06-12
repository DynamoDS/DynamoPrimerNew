# Dynamo-Computing-Service – Lebenszyklus und Aktualisierungsrhythmus



In diesem Dokument werden Aktualisierungsrhythmus und Support-Richtlinie für Dynamo Cloud Compute beschrieben. Die Lösung wird hier auch synonym als „der Service“ bezeichnet.

Das Dokument beschreibt, wie Engine-Versionen verwaltet werden, wann Aktualisierungen verfügbar sind und was Benutzer beim Ausführen von Dynamo-Diagrammen in der Cloud erwarten können.

---

## Aktualisierungsrhythmus

Um unterschiedlichen Benutzeranforderungen gerecht zu werden, gibt es **zwei unterschiedliche Engine-Pfade** für Dynamo Cloud Compute. Jeder Pfad dient einem bestimmten Zweck und folgt seinem eigenen Aktualisierungszeitplan:

### Stabile Engine (Produktion)

Die stabile Engine ist auf Zuverlässigkeit und Konsistenz in Produktionsumgebungen ausgelegt. Sie basiert auf der neuesten stabilen Version der Dynamo Core-Laufzeit und wird aktualisiert, wenn offizielle Dynamo-Versionen für Dynamo Desktop-Benutzer verfügbar sind. Zunächst orientiert sich der Aktualisierungsrhythmus an DynamoRevit.

Dieser Pfad ist für Produktions-Workloads gedacht, bei denen Zuverlässigkeit und Planbarkeit entscheidend sind. Bei der stabilen Engine können Sie davon ausgehen, dass Aktualisierungen an den Zeitplan für öffentliche Dynamo-Versionen angepasst werden. Somit haben Sie Zeit, sich auf Änderungen vorzubereiten und Ihre Diagramme zu testen, bevor sie sich auf Ihre Arbeitsabläufe auswirken.

### Vorschau-Engine (Vorschau/tägliche Sandbox)

Die Vorschau-Engine bietet frühzeitigen Zugriff auf die neuesten Entwicklungen in Dynamo. Sie basiert auf dem neuesten Entwicklungs-Build der Dynamo Core-Laufzeit und wird regelmäßig aktualisiert, wenn neue Funktionen und Fehlerbehebungen zusammengeführt werden.

Dieser Pfad eignet sich ideal für Benutzer, die bevorstehende Änderungen testen, mit neuen Funktionen vor der offiziellen Veröffentlichung experimentieren oder sicherstellen möchten, dass ihre Diagramme auch mit zukünftigen Versionen von Dynamo funktionieren. Die Vorschau-Engine ermöglicht es Ihnen, Änderungen stets einen Schritt voraus zu bleiben und dem Dynamo-Team Feedback zu geben.

---


## Unterstützungszeitraum

Wenn Sie wissen, wie lange die einzelnen Engine-Versionen unterstützt werden, können Sie Wartungsfenster und Diagrammaktualisierungen besser planen.

### Stabile Engine

Die stabile Engine erhält Updates, wenn Dynamo eine neue stabile Version von Dynamo Core in Revit veröffentlicht. Jede stabile Version bleibt verfügbar und wird unterstützt, bis die nächste stabile Version für den Service bereitgestellt wird.

Läuft der Service beispielsweise unter Dynamo 3.6 (stabil), wird diese Version so lange verwendet, bis Dynamo 4.0 allgemein für Benutzer zur Verfügung gestellt wird (entspricht in der Regel dem Bereitstellungszeitpunkt in Revit). Zu diesem Zeitpunkt wird der Service auf Dynamo 4.0 (stabil) aktualisiert.

So wird gewährleistet, dass der Cloud-Service mit dem synchron bleibt, was die meisten Benutzer in Desktop-Umgebungen vorfinden.

### Vorschau-Engine

Die Vorschau-Engine wird fortlaufend auf Basis des neuesten Entwicklungszweigs von Dynamo aktualisiert. Im Verlauf der Entwicklung jeder Version werden diese Änderungen von der Vorschau-Engine nachverfolgt.

Während sich zum Beispiel Dynamo 4.1 in der aktiven Entwicklung befindet, ist die Vorschau-Engine unter Umständen als Dynamo Cloud Compute Service 4.1 gekennzeichnet. Sobald die Entwicklung zu Version 4.2 übergeht, beginnt die Vorschau-Engine damit, diese Änderungen nachzuverfolgen, und wird möglicherweise in Dynamo Cloud Compute Service 4.2 umbenannt.

Da die Vorschau-Engine häufig aktualisiert wird, sollten Sie mit gelegentlichen inkompatiblen Änderungen oder experimentellen Funktionen rechnen. Sie eignet sich am besten für Tests und Validierungen, nicht für Produktionsabläufe.

---

## Auswahl der richtigen Engine

Beachten Sie Folgendes bei der Auswahl des Engine-Pfads:

- Entscheiden Sie sich für die **stabile Engine**, falls Sie ein planbares, getestetes Verhalten für Produktionsprozesse benötigen oder wenn Sie Diagramme für Endbenutzer bereitstellen, die konsistente Ergebnisse erwarten.

- Wählen Sie die **Vorschau-Engine** aus, um neue Funktionen frühzeitig zu testen, um sicherzustellen, dass Ihre Diagramme mit zukünftigen Versionen kompatibel sind, oder um Feedback rund um die Entwicklung von Dynamo zu geben.

Beide Engines führen dieselbe Dynamo-Kernlaufzeit aus – der Unterschied besteht darin, wann und wie oft sie Aktualisierungen erhalten. 

---

> **Anmerkung: Beta-Service**  
 Dynamo Cloud Compute befindet sich derzeit in der Beta-Phase. Die in diesem Dokument beschriebenen Unterstützungszeiträume und Update-Richtlinien spiegeln unsere aktuellen Absichten wider, während wir mit dem Service experimentieren und diesen weiterentwickeln. Sie stellen keine Garantien dar und können sich je nach Benutzerfeedback und Betriebserfahrung ändern.



