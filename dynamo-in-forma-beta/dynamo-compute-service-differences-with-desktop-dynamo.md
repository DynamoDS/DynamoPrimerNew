# Unterschiede der Dynamo-Computing-Services bei Dynamo Desktop

Auf dieser Seite werden die Unterschiede erläutert, die Sie beim Schreiben von Dynamo-Programmen zur Ausführung im Cloud-Kontext des Dynamo-Computing-Services beachten sollten.

## Was ist DaaS?

DaaS, Dynamo as a Service, Dynamo-Computing-Service usw. beziehen sich alle auf dasselbe: die Dynamo Core Runtime, die in einem Cloud-Kontext ausgeführt wird. Dies bedeutet, dass das Diagramm nicht auf Ihrem Computer ausgeführt wird. Auf DaaS kann derzeit nur über die Dynamo Player Extension für Forma zugegriffen werden. Dort können Benutzer `.dyn`-Dateien, die in der Desktop-Umgebung erstellt wurden, hochladen und verwalten, `.dyn`-Dateien, die von Kollegen über die Erweiterung freigegeben werden, ausführen oder vorinstallierte `.dyn`-Routinen verwenden, die von Autodesk als Beispiele bereitgestellt werden.

Da Ihre Diagramme in diesem Cloud-Kontext und nicht auf Ihrem Computer ausgeführt werden, kann DaaS derzeit herkömmliche Dynamo-Host-Kontexte (Revit, Civil 3D usw.) nicht direkt verwenden. Wenn Sie Typen aus diesen Programmen in Ihrem Diagramm verwenden möchten, müssen Sie sie mit dem `Data.Remember`-Block oder anderen diagramminternen Serialisierungsverfahren im Diagramm serialisieren (speichern). Diese Arbeitsabläufe ähneln den Arbeitsabläufen, die Sie beim Schreiben von Diagrammen für Generative Design in Revit verwenden müssen.

## In welcher Version von Dynamo wird mein Code ausgeführt?

Die Version basiert auf Version 3.x und wird regelmäßig basierend auf der Open-Source-Hauptverzweigung von Dynamo aktualisiert.

## Welche Pakete/Blöcke sind in dieser Version von Dynamo verfügbar?

* Für die meisten Core-Blöcke finden Sie im nächsten Abschnitt spezifische Einschränkungen.
* `DynamoFormaBeta`-Paket für die Interaktion mit der Forma-API
* `VASA` für Voxelisierung/effiziente Analyse
* `MeshToolKit` für die Netzbearbeitung. Das Netz-Toolkit ist ab Dynamo 3.4 auch standardmäßig verfügbar.
* `RefineryToolkit` für nützliche Algorithmen, die Kollisionstests, Ansichtsabstand, kürzeste Strecke, Isovist usw. ermöglichen

## Was sollte ich beachten, wenn ich Diagramme für DaaS schreibe?

* Python-Blöcke funktionieren nicht. Diese werden _derzeit_ einfach nicht ausgeführt.
* Benutzerdefinierte Pakete können nicht verwendet werden.
* Der Benutzeroberflächen-/Ansichtslayer von Benutzeroberflächen-Blöcken wird nicht ausgeführt. Wir gehen nicht davon aus, dass dies die Kernfunktionalität beeinträchtigt, aber es ist gut, dies im Hinterkopf zu behalten, wenn Sie einen Fehler im Zusammenhang mit einem Block mit angepasster Benutzeroberfläche bemerken.
* Nur-Windows-Funktionen können nicht verwendet werden. Wenn Sie beispielsweise versuchen, die Windows-Registrierung oder WPF zu verwenden, schlägt dieser Vorgang fehl.
* Ansichtserweiterungen werden nicht geladen.
* Dateisystemblöcke werden Ihnen nicht sehr dienlich sein. Alle Dateien, die Sie auf Ihrem lokalen Computer referenzieren, sind beim Ausführen in DaaS nicht vorhanden.
* Interop-Blöcke für Excel/DSOffice funktionieren nicht. Open XML-Blöcke sollten funktionieren.
* Netzwerkanforderungen funktionieren generell nicht, Sie können jedoch die Forma-API aufrufen.

## Wie soll ich mir das alles merken? Was, wenn sich etwas ändert?

* In Zukunft wird es in Dynamo Werkzeuge für die Desktop-Version geben, mit denen Sie einfacher sicherstellen können, dass Ihr Diagramm in beiden Kontexten gleich ausgeführt werden kann.

## Wie viel kostet das?

* Während der Laufzeit der Beta-Version wird die Berechnungszeit nicht in Rechnung gestellt.

## Wie sehen die ersten Schritte aus?

Sehen Sie sich den [Blog-Post](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), die [YouTube-Reihe](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) oder die Beispiele in der Forma Extension an, um loszulegen. In diesen Ressourcen erhalten Sie eine Anleitung für Folgendes:

* Zugriff auf Autodesk Forma
* Installation von DynamoFormaBeta for Dynamo auf dem Desktop und der Dynamo Extension in Forma
* Schreiben Ihres ersten Diagramms

## Sicherheit

* Beachten Sie, dass Ihre freigegebenen Diagramme in Forma gespeichert werden.
* Die maximale Ausführungszeit für Diagramme beträgt derzeit weniger als 30 Minuten. Dieser Wert kann sich ändern.
* Ausführungsanfragen sind zeitlich beschränkt, sodass Fehler auftreten können, wenn Sie viele Berechnungsanfragen in einem zu kurzen Zeitraum stellen.
