# Verwalten und Aktualisieren von Abhängigkeiten in Dynamo

### Wann gilt dieses Wiki?

Wenn Sie an einer neuen Funktion arbeiten oder einfach eine vorhandene Abhängigkeit aktualisieren, sollten Sie Folgendes prüfen, bevor Sie eine neue Abhängigkeit in das Dynamo-Repository aufnehmen.

### Hinweise

1. Welche Lizenz hat die neue oder aktualisierte Abhängigkeit? Nur bestimmte Open-Source-Lizenzen sind zulässig, ohne dass Sie mit der ADSK-Rechtsabteilung sprechen müssen.
   * Stellen Sie nach der Klärung der Lizenz sicher, dass die Abhängigkeit und die Version im internen Wiki aufgezeichnet werden.
   * Wenn die Lizenz `LGPL`, `GPL` oder `Apache` lautet, muss die Lizenzdatei in den Unterordner _Open Source Licenses_ des Dynamo-Builds kopiert werden.
   * Wenn die Lizenz `LGPL` lautet, muss der vollständige Quellcode für alle Komponenten von Drittanbietern zusammen mit Textkopien der entsprechenden Open-Source-Lizenzen auf [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution) hochgeladen werden.
2. Bei einem Update: Hat sich der Lizenztyp im Vergleich zur vorherigen Version geändert?
3. Ist die Abhängigkeit plattformübergreifend?
   * Verfügt sie über native Komponenten (wie `CEFSharp` oder `ImageMagick`)? Dadurch wird die plattformübergreifende Bereitstellung erschwert.
   * Weist sie Referenzen nur für Windows auf? Wenn dies der Fall ist, sollte sie nicht als Abhängigkeit von DynamoCore oder anderen plattformübergreifenden Teilen von Dynamo (dem Modell-Layer) verwendet werden.
4. Ist die Abhängigkeit mit allen erforderlichen zugehörigen Abhängigkeiten ordnungsgemäß im Ordner bin im Build gebündelt?
   * Werden bei der Aktualisierung einige Dateien infolge der Aktualisierung entfernt? Ist diese Version von Dynamo für eine Punktversion von Host-Produkten vorgesehen? Wenn dies der Fall ist, müssen Sie die alten Binärdateien bis zum Jahr einer globalen Veröffentlichung aufbewahren, um Patch-Installer zu unterstützen. Weitere Informationen finden Sie [hier](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. Steht die Abhängigkeit oder die zugehörige Abhängigkeitsstruktur im Konflikt mit anderen vorhandenen Abhängigkeiten in Dynamo?
6. Steht die Abhängigkeit oder die zugehörige Abhängigkeitsstruktur im Konflikt mit vorhandenen Abhängigkeiten in Produkten, die Dynamo in den Prozess integrieren (Revit, Civil usw.)? **Dies ist wichtig, da diese Probleme nur bei der Integration erkannt werden können, es sei denn, die Arbeit wird im Vorfeld erledigt.**
