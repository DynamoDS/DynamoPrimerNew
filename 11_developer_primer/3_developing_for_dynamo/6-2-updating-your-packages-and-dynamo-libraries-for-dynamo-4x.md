# Aktualisieren der Pakete und Dynamo-Bibliotheken für Dynamo 4.x

### Einführung: <a href="#introduction" id="introduction"></a>

Dieser Abschnitt enthält Informationen zu Problemen, die möglicherweise beim Migrieren Ihrer Diagramme, Pakete und Bibliotheken in Dynamo 4.x auftreten können. Neu in Dynamo 4.0:
* Erhebliche Leistungsverbesserungen
* Aktualisierungen bei Stabilität und Fehlerbehebung
* Modernisierung der Codebasis
* Entfernung von APIs, die in 1.x als veraltet markiert wurden
* Umfassendes Laufzeit-Update von .NET 8 auf .NET 10
* PythonNet3 ist jetzt die vorgegebene Python-Engine für alle neuen Python-Blöcke.

Das .NET 10-Migrationsverfahren stellt sicher, dass Dynamo weiterhin auf die Technologie-Roadmap von Microsoft abgestimmt ist, lange bevor die Unterstützung für .NET 8 im November 2026 eingestellt wird.

Wenn Sie Dynamo 4.0 starten, werden Sie aufgefordert, ein Update auf .NET 10 durchzuführen, falls Sie dies noch nicht getan haben. Paketersteller müssen ihre Projekte auf .NET 10 aktualisieren, um die vollständige Kompatibilität sicherzustellen.

Alle neuen Python-Blöcke, die ab Dynamo 4.0 erstellt werden, beginnen mit PythonNet3. Informationen zur Abwärtskompatibilität: Wenn Sie in Umgebungen mit mehreren Versionen arbeiten (z. B. Revit oder Civil 3D 2025/2026), installieren Sie das PythonNet3 Engine-Paket in Dynamo 3.3–3.6, um die Kompatibilität zu gewährleisten. Weitere Informationen finden Sie [hier](https://dynamobim.org/dynamo-core-4-0-release/).

APIs und Blöcke, die in Version 1.x als veraltet markiert waren, wurden in Dynamo 4.0 entfernt. Die vollständige Liste der Änderungen finden Sie [hier](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0).

### Paketkompatibilität <a href="#package-compatibility" id="package-compatibility"></a>

#### Verwenden von Dynamo 2.x- und 3.x-Paketen in Dynamo 4.x 
Da Dynamo 4.x jetzt unter der .NET 10-Laufzeitumgebung ausgeführt wird, kann die Funktionsweise von Paketen, die für Dynamo 2.x (*mit .NET 48*) und Dynamo 3.x (*mit .NET 8*) erstellt wurden, in Dynamo 4.x nicht garantiert werden. Wenn Sie versuchen, ein Paket in Dynamo 4.x herunterzuladen, das mit einer Dynamo-Version unter 4.0 veröffentlicht wurde, erhalten Sie eine Warnung, dass das Paket aus einer älteren Version von Dynamo stammt.

**Das bedeutet nicht, dass das Paket nicht funktioniert.** Es ist lediglich eine Warnung davor, dass Kompatibilitätsprobleme auftreten können. Im Allgemeinen ist es empfehlenswert, zu überprüfen, ob eine neuere Version speziell für Dynamo 4.x erstellt wurde.

Möglicherweise wird diese Art von Warnung auch in den Dynamo-Protokolldateien zur Paketladedauer angezeigt. Wenn alles ordnungsgemäß funktioniert, können Sie dies ignorieren.

#### Verwenden von Dynamo 4.x-Paketen in Dynamo 2.x 

Es ist sehr unwahrscheinlich, dass ein für Dynamo 4.x (*mit .NET 10*) erstelltes Paket mit Dynamo 2.x kompatibel ist. Die folgende Warnung wird auch angezeigt, wenn Sie versuchen, Pakete, die für Dynamo 4.x erstellt wurden, in Dynamo 2.x zu installieren.

![Warnung zur Paketkompatibilität](images/6-2-packages-new-version-compatibility-warning.png)


#### Verwenden von Dynamo 4.x-Paketen in Dynamo 3.x 

Das für Dynamo 4.x (*mit .NET 10*) erstellte Paket funktioniert möglicherweise unter Dynamo 3.x, solange alle im Paket verwendeten APIs in .NET 8 vorhanden sind. Die Funktionsweise kann aber nicht garantiert werden. Die folgende Warnung wird auch angezeigt, wenn Sie versuchen, Pakete, die für Dynamo 4.x erstellt wurden, in Dynamo 3.x zu installieren.

![Warnung zur Paketkompatibilität](images/6-2-packages-compatibility-warning.png)

#### Optimale Verfahren für Paketersteller 
Das optimale Verfahren besteht darin, Ihr Projekt sowohl mit .NET 8 als auch mit .NET 10 kompatibel zu machen, indem Sie Ihre CSPROJ-Datei ändern.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
Dadurch wird Folgendes sichergestellt:
* Weitere Unterstützung für in Revit gehostete Dynamo-Versionen unter .NET 8
* Kompatibilität mit der eigenständigen Dynamo 4.x-Version unter .NET 10