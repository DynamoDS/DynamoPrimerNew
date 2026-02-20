# Aktualisieren der Pakete und Dynamo-Bibliotheken für Dynamo 3.x und .NET 8

### Einführung: <a href="#introduction" id="introduction"></a>

Dieser Abschnitt enthält Informationen zu Problemen, die möglicherweise beim Migrieren Ihrer Diagramme, Pakete und Bibliotheken in Dynamo 3.x auftreten können.

Dynamo 3.0 ist eine Hauptversion, und einige APIs wurden geändert oder entfernt. Die größte Änderung, die Sie als Entwickler oder Benutzer von Dynamo 3.x wahrscheinlich betreffen wird, ist der Umstieg auf .NET 8.

Dotnet/.NET ist die Laufzeitumgebung, die die C#-Sprache unterstützt, in der Dynamo geschrieben ist. Wir haben zusammen mit der restlichen Autodesk-Umgebung auf eine moderne Version dieser Laufzeitumgebung aktualisiert.

Weitere Informationen finden Sie in [unserem Blog-Post](https://dynamobim.org/dynamo-on-net-8/).
***

### Paketkompatibilität <a href="#package-compatibility" id="package-compatibility"></a>

#### Verwenden von Dynamo 2.x-Paketen in Dynamo 3.x 
Da Dynamo 3.x jetzt unter der .NET 8-Laufzeitumgebung ausgeführt wird, kann die Funktionsweise von Paketen, die für Dynamo 2.x (*mit .NET 48*) erstellt wurden, in Dynamo 3.x nicht garantiert werden. Wenn Sie versuchen, ein Paket in Dynamo 3.x herunterzuladen, das mit einer Dynamo-Version unter 3.0 veröffentlicht wurde, erhalten Sie eine Warnung, dass das Paket aus einer älteren Version von Dynamo stammt. 

**Das bedeutet nicht, dass das Paket nicht funktioniert.** Es ist lediglich eine Warnung davor, dass Kompatibilitätsprobleme auftreten können. Im Allgemeinen ist es empfehlenswert, zu überprüfen, ob eine neuere Version speziell für Dynamo 3.x erstellt wurde.

Möglicherweise wird diese Art von Warnung auch in den Dynamo-Protokolldateien zur Paketladedauer angezeigt. Wenn alles ordnungsgemäß funktioniert, können Sie dies ignorieren.

#### Verwenden von Dynamo 3.x-Paketen in Dynamo 2.x 

Es ist sehr unwahrscheinlich, dass ein für Dynamo 3.x (*mit .NET 8*) erstelltes Paket mit Dynamo 2.x kompatibel ist. Außerdem wird eine Warnung angezeigt, wenn Sie Pakete herunterladen, die für neuere Versionen von Dynamo erstellt wurden, während Sie eine ältere Version verwenden.


