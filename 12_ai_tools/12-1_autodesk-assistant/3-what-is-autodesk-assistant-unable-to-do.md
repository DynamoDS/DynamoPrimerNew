# Was Autodesk Assistant noch nicht kann

### Aktualisieren von Paketen

Autodesk Assistant kann Pakete noch nicht in Dynamo herunterladen und installieren.

![](../../.gitbook/assets/Aa_Update.jpg)

Wie in der Abbildung oben zu sehen, kann Autodesk Assistant externe Pakete, die mit Dynamo verknüpft sind, nicht ändern, installieren oder deinstallieren. Er kann Anweisungen bereitstellen, damit Sie sie selbst installieren können.


### Direktes Lesen und Schreiben von Dateien

Autodesk Assistant kann mit Dynamo-Diagrammdateien verwendet werden: Er kann ein Diagramm öffnen, in den Ansichtsbereich einfügen, es ausführen und Ihre Arbeit speichern. Er kann andere Dateien auf Ihrer Festplatte nicht allein lesen oder schreiben.

Im folgenden Beispiel wird Geometrie im Ansichtsbereich erstellt. Autodesk Assistant wird aufgefordert, die Punkte, aus denen die Geometrie besteht, als JSON-Datei zu exportieren. Autodesk Assistant kann diese JSON-Datei zwar nicht direkt generieren, schlägt jedoch vor, dass ein Dynamo-Skript dafür erstellt werden könnte.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

Diesen Lösungsvorschlag können Sie als Muster verwenden. Alles außerhalb des Diagramms wird über Blöcke angesteuert, nicht von Autodesk Assistant selbst.

### Ändern des Revit-Modells ohne Diagramm

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant für Dynamo hat keine direkte Verbindung zum Revit-Modell. Wenn Sie ihn auffordern, das Modell so zu bearbeiten, wie Sie dies beim Assistant in Revit tun würden, wird Ihnen mitgeteilt, dass dies nicht möglich ist.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

Er kann Ihr Revit-Modell dennoch ändern. Dies geschieht so, wie Sie auch vorgehen würden: Er platziert Revit-Blöcke, vernetzt sie, und das Modell ändert sich, wenn das Diagramm ausgeführt wird.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Aus dieser Methode ergeben sich zwei Konsequenzen:

- **Änderungen erfolgen nur, wenn das Diagramm ausgeführt wird.** Sie können das Diagramm lesen und die Werte zuerst anpassen. Das Modell wird nur bearbeitet, wenn das Diagramm entweder von Ihnen oder von Autodesk Assistant als Teil Ihrer Anforderung ausgeführt wird.
- **Die Änderung ist wiederholbar.** Das Diagramm bleibt im Ansichtsbereich. Speichern Sie es, und Sie können dieselbe Aufgabe in einem anderen Modell erneut ausführen, oder geben Sie es für Ihr Team frei.

Wenn Sie eine einzelne Direktbearbeitung ohne Einbeziehung des Diagramms wünschen, verwenden Sie Autodesk Assistant in Revit.
