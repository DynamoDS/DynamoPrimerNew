# Dynamo Cloud Compute



Dynamo Cloud Compute integriert die Leistungsfähigkeit der visuellen Dynamo-Programmierlaufzeit in die Cloud. Der Computing-Service führt Diagramme nicht auf Ihrem lokalen Computer, sondern in einer sicheren Cloud-Umgebung aus und stellt die Ergebnisse bereit.

## Was ist Dynamo?

Dynamo ist eine visuelle Programmiersprache und Erstellungsumgebung, mit der sich Programme entwickeln lassen, indem Sie Blöcke in einem Diagramm miteinander verbinden. Die Dynamo-Laufzeit führt diese Diagramme aus, sodass Sie komplexe Aufgaben automatisieren, Geometrie generieren und in andere Software integrieren können.

## Funktionsweise des Computing-Services

Wenn Sie Dynamo über einen cloudbasierten Client (z. B. Dynamo Player in Forma) nutzen, werden Ihre `.dyn`-Diagrammdateien zur Ausführung an den Computing-Service gesendet. Der Service:

1. empfängt das Diagramm und alle Eingabeparameter,
2. führt das Diagramm in einer isolierten Cloud-Umgebung aus,
3. meldet die Ergebnisse zurück an die Client-Anwendung.

Dieser cloudbasierte Ansatz ermöglicht es Ihnen, Dynamo-Diagramme auszuführen, ohne Dynamo lokal installieren zu müssen. Zudem können Sie sich das Potenzial des Cloud-Computings für komplexe Vorgänge zunutze machen.

## Gründe für die Verwendung von Dynamo Cloud Compute

Dynamo Cloud Compute unterstützt die folgenden Szenarien:

**Ausführung von Diagrammen ohne Desktop-Installation**: Führen Sie Dynamo-Diagramme direkt über Webanwendungen aus, ohne Dynamo Desktop auf Ihrem Computer installieren zu müssen.

**Zusammenarbeit und Freigabe**: Geben Sie Diagramme für Teammitglieder frei, die diese über Webschnittstellen wie Forma ausführen können. Dadurch wird es leichter, automatisierte Arbeitsabläufe in Ihrem Unternehmen zu verteilen.

**Cloud-Computing**: Nutzen Sie die Cloud-Infrastruktur für rechenintensive Vorgänge, die auf lokalen Computern möglicherweise mehr Zeit in Anspruch nehmen.

**Standardisierte Ausführungsumgebung**: Gewährleisten Sie ein konsistentes Verhalten für verschiedene Benutzer und Computer, indem Sie Diagramme in einer kontrollierten Cloud-Umgebung ausführen.

**Verbindung mit Forma**: Interagieren Sie mit der Forma-API über Dynamo. [Weitere Informationen finden Sie in diesem Blog-Beitrag.](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/)

## Wesentliche Merkmale

**Cloud-Ausführung**: Diagramme werden in der Cloud und nicht auf Ihrem lokalen Computer ausgeführt. Dies bedeutet:
- Dynamo Desktop muss nicht installiert werden, um Diagramme auszuführen.
- Sie haben Zugang zu Cloud-Computing-Ressourcen.
- Unterschiedliche Benutzer profitieren von einer konsistenten Ausführungsumgebung.

**Sicherheit**: Der Service führt die einzelnen Diagramme von Benutzern in isolierten Umgebungen aus, um Datensicherheit und -schutz aufrechtzuerhalten. Ihre Diagramme und Daten werden getrennt von anderen Benutzern verwaltet.

**Asynchrone Verarbeitung**: Die Diagrammausführung erfolgt asynchron: Clients übermitteln einen Auftrag und können seinen Status überprüfen, bis er abgeschlossen ist. Dies ermöglicht Berechnungen mit langer Laufzeit, ohne Ihren Arbeitsablauf zu blockieren.

## Aktuelle Verfügbarkeit

Dynamo Cloud Compute ist derzeit in folgendem Rahmen verfügbar:
- **Dynamo Player in Forma (offene Beta-Version)**: Laden Sie Dynamo-Diagramme direkt über die Webschnittstelle von Autodesk Forma hoch, geben Sie sie frei, und führen Sie sie aus.

## Weitere Informationen

- [Unterschiede bei Dynamo Cloud Compute mit Dynamo Desktop](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md): Wichtige Unterschiede, die Sie beim Schreiben von Diagrammen für die Cloud-Ausführung beachten sollten.
- [Engine-Lebenszyklus](engine-lifecycle.md): Informationen zu unterstützten Engine-Versionen und deren Lebenszyklus.

-----


> **Anmerkung: Beta-Service**  
 Dynamo Cloud Compute befindet sich derzeit in der Beta-Phase. Die in diesem Dokument beschriebenen Unterstützungszeiträume und Update-Richtlinien spiegeln unsere aktuellen Absichten wider, während wir mit dem Service experimentieren und diesen weiterentwickeln. Sie stellen keine Garantien dar und können sich je nach Benutzerfeedback und Betriebserfahrung ändern.