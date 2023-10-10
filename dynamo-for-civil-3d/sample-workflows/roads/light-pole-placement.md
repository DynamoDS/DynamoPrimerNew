# Lichtmastenplatzierung

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Einer der vielen praktischen Anwendungsfälle von Dynamo ist das dynamische Platzieren einzelner Objekte entlang eines 3D-Profilkörpermodells. Häufig müssen Objekte an Positionen platziert werden, die unabhängig von den eingefügten Querschnitten entlang des 3D-Profilkörpers sind. Dies manuell zu erledigen ist sehr mühsam. Wenn sich die horizontale oder vertikale Geometrie des 3D-Profilkörpers ändert, ist ein erheblicher Überarbeitungsaufwand erforderlich.

## Ziel

> :dart: Platzieren Sie Lichtmast-Blockreferenzen entlang eines 3D-Profilkörpers an Stationswerten, die in einer Excel-Datei angegeben sind. 

## Wichtige Konzepte

> * Lesen von Daten aus einer externen Datei (in diesem Fall Excel)
> * Organisieren von Daten in Wörterbüchern
> * Steuern von Position/Skalierung/Drehung mithilfe von Koordinatensystemen
> * Platzieren von Blockreferenzen
> * Visualisieren von Geometrie in Dynamo

## Kompatibilität der Versionen

{% hint style="success" %}\r\n Dieses Diagramm wird in **Civil 3D 2020** und höher ausgeführt. \r\n{% endhint %}

## Datensatz

Laden Sie zunächst die folgenden Beispieldateien herunter, und öffnen Sie dann die DWG-Datei und das Dynamo-Diagramm.

{% hint style="info" %}\r\n Die Excel-Datei sollte im selben Verzeichnis wie das Dynamo-Diagramm gespeichert werden. \r\n{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Lösung

Hier sehen Sie einen Überblick über die Logik in diesem Diagramm.

> 1. Excel-Datei lesen und Daten in Dynamo importieren
> 2. Elementkanten aus der angegebenen 3D-Profilkörper-Basislinie abrufen
> 3. Koordinatensysteme entlang der 3D-Profilkörper-Elementkante an den gewünschten Stationen generieren
> 4. Koordinatensysteme zum Platzieren von Blockreferenzen im Modellbereich verwenden

Los gehts!

### Abrufen von Excel-Daten

In diesem Beispieldiagramm verwenden wir eine Excel-Datei, um die Daten zu speichern, die Dynamo zum Platzieren der Lichtmast-Blockreferenzen verwendet. Die Tabelle sieht so aus.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Struktur der Excel-Dateitabelle</p></figcaption></figure>

{% hint style="info" %}\r\n Das Lesen von Daten aus externen Dateien (z. B. Excel-Dateien) mit Dynamo ist praktisch, insbesondere dann, wenn auch andere Teammitglieder die Daten nutzen müssen. \r\n{% endhint %}

Die Excel-Daten werden wie folgt in Dynamo importiert. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Importieren der Excel-Daten in Dynamo</p></figcaption></figure>

Nachdem wir die Daten erstellt haben, müssen wir sie nach Spalten aufteilen (_Corridor_, _Baseline_, _PointCode_ usw.), damit sie im restlichen Diagramm verwendet werden können. Eine gängige Methode hierfür ist die Verwendung des **List.GetItemAtIndex**-Blocks und die Angabe der Indexnummer für jede gewünschte Spalte. Beispiel: Die Spalte _Corridor_ befindet sich bei Index 0, die Spalte _Baseline_ bei Index 1 usw.

Klingt gut, oder? Doch bei diesem Ansatz gibt es ein potenzielles Problem. Was geschieht, wenn sich die Reihenfolge der Spalten in der Excel-Datei in Zukunft ändert? Oder wenn eine neue Spalte zwischen zwei Spalten hinzugefügt wird? Das Diagramm funktioniert dann nicht ordnungsgemäß und muss aktualisiert werden. Sie können das Diagramm für zukünftige Aktionen absichern, indem Sie die Daten in ein **Wörterbuch** einfügen, wobei die Excel-Spaltenüberschriften die _Schlüssel_ und die übrigen Daten die _Werte_ sind.

{% hint style="info" %}\r\n Wenn Wörterbücher neu für Sie sind, finden Sie im Abschnitt [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention") weitere Informationen. \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Einfügen der Excel-Daten in ein Wörterbuch</p></figcaption></figure>

Dadurch wird das Diagramm stabiler, da es Flexibilität beim Ändern der Spaltenreihenfolge in Excel ermöglicht. Solange die Spaltenüberschriften gleich bleiben, können die Daten einfach mithilfe des _Schlüssels_ (d. h. der Spaltenüberschrift) aus dem Wörterbuch abgerufen werden. Dies ist die Vorgehensweise beim nächsten Schritt.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Abrufen von Daten aus dem Wörterbuch</p></figcaption></figure>

### Abrufen von 3D-Profilkörper-Elementkanten

Nachdem wir die Excel-Daten importiert haben und bereit für die Arbeit sind, können wir damit einige Informationen zu den 3D-Profilkörpermodellen aus Civil 3D abrufen.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Wählen Sie das 3D-Profilkörpermodell nach seinem Namen aus.
> 2. Rufen Sie eine bestimmte Basislinie innerhalb des 3D-Profilkörpers ab.
> 3. Rufen Sie eine Elementkante innerhalb der Basislinie anhand ihres Punktcodes ab.

### Erstellen von Koordinatensystemen

Als Nächstes generieren wir **Koordinatensysteme** entlang der 3D-Profilkörper-Elementkanten an den Stationswerten, die wir in der Excel-Datei angegeben haben. Diese Koordinatensysteme werden verwendet, um die Position, Drehung und Skalierung der Lichtmast-Blockreferenzen zu definieren.

{% hint style="info" %}\r\n Wenn Koordinatensysteme neu für Sie sind, finden Sie im Abschnitt [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") weitere Informationen. \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Abrufen von Koordinatensystemen entlang der 3D-Profilkörper-Elementkanten</p></figcaption></figure>

Beachten Sie, dass zum Drehen der Koordinatensysteme hier ein Codeblock verwendet wird, je nachdem, auf welcher Seite der Basislinie sie sich befinden. Dies könnte mithilfe einer Sequenz von mehreren Blöcken erreicht werden, ist aber ein gutes Beispiel für eine Situation, in der es einfacher ist, einfach den Code zu schreiben.

{% hint style="info" %}\r\n Wenn Codeblöcke neu für Sie sind, finden Sie im Abschnitt [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention") weitere Informationen. \r\n{% endhint %}

### Erstellen von Blockreferenzen

Gleich haben wir es geschafft! Wir verfügen über alle Informationen, die wir benötigen, um die Blockreferenzen tatsächlich platzieren zu können. Als Erstes müssen wir die gewünschten Blockdefinitionen mithilfe der Spalte _BlockName_ in der Excel-Datei abrufen.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Abrufen der gewünschten Blockdefinitionen aus dem Dokument</p></figcaption></figure>

Der letzte Schritt ist die Erstellung der Blockreferenzen.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Erstellen der Blockreferenzen im Modellbereich</p></figcaption></figure>

### Ergebnis

Beim Ausführen des Diagramms sollten neue Blockreferenzen im Modellbereich entlang des 3D-Profilkörpers angezeigt werden. Wenn der Ausführungsmodus des Diagramms auf Automatisch eingestellt ist und Sie die Excel-Datei bearbeiten, werden die Blockreferenzen automatisch aktualisiert.

{% hint style="info" %}\r\n Weitere Informationen über die Diagrammausführungsmodi finden Sie im Abschnitt [3_user_interface](../../../3\_user\_interface/ "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Aktualisieren der Excel-Datei und schnelles Anzeigen der Ergebnisse in Civil 3D</p></figcaption></figure>

Hier sehen Sie ein Beispiel für die Ausführung des Diagramms mit **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Ausführen des Diagramms mit Dynamo Player und Anzeigen der Ergebnisse in Civil 3D</p></figcaption></figure>

{% hint style="info" %}\r\n Wenn Dynamo Player neu für Sie ist, finden Sie im Abschnitt [dynamo-player.md](../../dynamo-player.md "mention") weitere Informationen. \r\n{% endhint %}

> :tada: Mission erfüllt!

### Bonus: Visualisierung in Dynamo

Es kann hilfreich sein, die 3D-Profilkörpergeometrie in Dynamo zu visualisieren, um Kontext bereitzustellen. In diesem speziellen Modell sind die 3D-Profilkörper-Volumenkörper bereits im Modellbereich extrahiert. Diese exportieren wir in Dynamo. 

Aber wir müssen noch etwas bedenken. Volumenkörper sind ein relativ "schwerer" Geometrietyp, was bedeutet, dass dieser Vorgang das Diagramm verlangsamt. Es wäre praktisch, wenn es eine einfache Möglichkeit gäbe, _auszuwählen_, ob wir die Volumenkörper anzeigen möchten oder nicht. Die offensichtliche Antwort ist, einfach den **Corridor.GetSolids**-Block zu trennen. Dadurch werden jedoch Warnungen für alle nachgeordneten Blöcke angezeigt, was zu einer chaotischen Ansicht führt. Dies ist eine Situation, in der der **ScopeIf**-Block hilfreich ist.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Beachten Sie, dass der **Object.Geometry**-Block unten einen grauen Balken aufweist. Dies bedeutet, dass die Blockvorschau deaktiviert ist (Zugriff durch Klicken mit der rechten Maustaste auf den Block). Dadurch muss der Block **GeometryColor.ByGeometryColor** in der Hintergrundvorschau nicht mit anderer Geometrie um die Anzeigepriorität konkurrieren.
> 2. Mit dem **ScopeIf**-Block können Sie einen ganzen Zweig von Blöcken selektiv ausführen. Wenn die _test_-Eingabe false ist, werden alle mit dem **ScopeIf**-Block verbundenen Blöcke nicht ausgeführt.

Hier sehen Sie das Ergebnis in der Dynamo-Hintergrundvorschau.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Visualisieren der 3D-Profilkörpergeometrie in Dynamo</p></figcaption></figure>

## Ideen

Im Folgenden finden Sie einige Anregungen, wie Sie die Funktionen dieses Diagramms erweitern können.

{% hint style="info" %}\r\n Fügen Sie eine **rotation**-Spalte zur Excel-Datei hinzu, die Sie zum Steuern der Drehung des Koordinatensystems verwenden können. \r\n{% endhint %}

{% hint style="info" %}\r\n Fügen Sie **horizontale oder vertikale Versätze** zur Excel-Datei hinzu, sodass die Lichtmasten bei Bedarf von der 3D-Profilkörper-Elementkante abweichen können. \r\n{% endhint %}

{% hint style="info" %}\r\n Anstatt eine Excel-Datei mit Stationswerten zu verwenden, generieren Sie die Stationswerte **direkt in Dynamo** mit einer Anfangsstation und typischem Abstand. \r\n{% endhint %}
