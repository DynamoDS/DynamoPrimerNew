# Erwartungen beim Testen

Auf dieser Seite wird beschrieben, was wir beim Testen von neuem Code, der zu Dynamo hinzugefügt wird, erreichen möchten.

Also, ... Sie möchten einen neuen Block hinzufügen. Sehr gut. Es ist an der Zeit, einige Tests durchzuführen. Dafür gibt es zwei Gründe.

1. Tests helfen herauszufinden, an welchen Stellen der Block nicht funktioniert.
2. Wenn ein anderer Benutzer Änderungen vornimmt, wodurch Ihr Block beschädigt wird, sollte dies in Tests erkannt werden. Auf diese Weise muss die Person, die das Fehlschlagen des Tests verursacht hat, das Problem beheben. Wenn die Tests dabei nicht fehlschlagen, müssen Sie sich mit dem Problem und dem Benutzer auseinandersetzen, der für das Fehlschlagen eines Modells verantwortlich ist.

Es gibt zwei allgemeine Arten von Tests in Dynamo: Komponententests und Systemtests.

## Komponententests

Mit Komponententests sollte so wenig wie möglich getestet werden. Wenn Sie einen Block erstellt haben, der Quadratwurzeln über einen C#-Zero-Touch-Block berechnet, sollte der Test nur diese DLL-Datei umfassen, und es sollten C#-Aufrufe direkt für diesen Code ausgeführt werden. Die Komponententests sollten nicht einmal Dynamo einschließen.

Sie sollten Folgendes enthalten:

* Positive Tests (korrekte Ausführung)
* Negative Tests (fehlerhafte Eingaben haben keine negativen Auswirkungen)
* Regressionstests (wenn jemand einen Fehler in Ihrem Code findet, schreiben Sie einen Test, um sicherzustellen, dass dieser nicht erneut auftritt)

Sie sollten klein, schnell und zuverlässig sein. Der Großteil der Tests sollte Komponententests umfassen.

## Systemtests

Systemtests werden für mehrere Komponenten durchgeführt, und es wird getestet, wie diese zusammenpassen. Sie sollten mit Bedacht verwendet und erstellt werden. 

Was ist der Grund? Ihre Ausführung ist teuer. Wir müssen die Test-Suite mit so geringer Latenz wie möglich ausführen.

Wenn Sie einen Systemtest schreiben, sollten Sie sich zunächst [dieses](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) Diagramm ansehen und herausfinden, welche Teilsysteme Sie abdecken.

Im Idealfall gibt es eine progressive Reihe von Tests, die immer mehr Sätze der interagierenden Blöcke abdecken, sodass beim Fehlschlagen der Tests sehr schnell herausgefunden werden kann, wo das Problem liegt.

Beispiele für Elemente, für die Systemtests erforderlich sind:

* Neuer Revit-Blocktyp, der mehrere Elemente in Trace speichert, anstatt ein einzelnes Element
* Neuer Beobachtungsblock, der Daten unterschiedlich anzeigt

Beispiele für Elemente, für die keine Systemtests erforderlich sind:

* Neuer mathematischer Block
* Bibliothek für die Zeichenfolgenverarbeitung

Mit Systemtests sollte Folgendes sichergestellt werden:

* Korrektes Verhalten
* Nichtvorhandensein von pathologischen Verhaltensweisen, z. B. keine Ausnahmen