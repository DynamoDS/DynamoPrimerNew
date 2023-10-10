# Umbenennen von Schächten/Bauwerken

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

Beim Hinzufügen von Haltungen und Schächten/Bauwerken zu einem Kanalnetz verwendet Civil 3D eine Vorlage, um automatisch Namen zuzuweisen. Dies ist während der ersten Platzierung in der Regel ausreichend, aber die Namen werden sich mit der Weiterentwicklung des Entwurfs unweigerlich ändern. Darüber hinaus sind möglicherweise viele verschiedene Benennungsmuster erforderlich, z. B. die sequenzielle Benennung von Schächten/Bauwerken in einem Kanalsystem ausgehend vom am weitesten stromabwärts gelegenen Schacht/Bauwerk oder die Verwendung eines Benennungsmusters, das dem Datenschema einer lokalen Behörde entspricht. In diesem Beispiel wird gezeigt, wie Dynamo zur Definition und konsistenten Anwendung jeder Art von Benennungsstrategie verwendet werden kann.

## Ziel

> :dart: Benennen Sie Kanalnetz-Schächte/Bauwerke basierend auf der Station einer Achse um.

## Wichtige Konzepte

> * Arbeiten mit Begrenzungsrahmen
> * Filtern von Daten mithilfe des Blocks **List.FilterByBoolMask**
> * Sortieren von Daten mithilfe des Blocks **List.SortByKey**
> * Erstellen und Ändern von Textzeichenfolgen

## Kompatibilität der Versionen

{% hint style="success" %}

 Dieses Diagramm wird in **Civil 3D 2020** und höher ausgeführt. 

{% endhint %}

## Datensatz

Laden Sie zunächst die folgenden Beispieldateien herunter, und öffnen Sie dann die DWG-Datei und das Dynamo-Diagramm.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Lösung

Hier sehen Sie einen Überblick über die Logik in diesem Diagramm.

> 1. Schächte/Bauwerke nach Layer auswählen
> 2. Positionen von Schächten/Bauwerken abrufen
> 3. Schächte/Bauwerke nach Versatz filtern und nach Station sortieren
> 4. Neue Namen generieren
> 5. Schächte/Bauwerke umbenennen

Los gehts!

### Auswählen von Schächten/Bauwerken

Als Erstes müssen wir alle Schächte/Bauwerke auswählen, mit denen wir arbeiten möchten. Dazu wählen wir einfach alle Objekte auf einem bestimmten Layer aus. Das bedeutet, dass wir Schächte/Bauwerke aus verschiedenen Kanalnetzen auswählen können (vorausgesetzt, sie haben den gleichen Layer).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Auswählen der Schächte/Bauwerke auf einem bestimmten Layer</p></figcaption></figure>

> 1. Dieser Block stellt sicher, dass wir nicht versehentlich unerwünschte Objekttypen abrufen, die möglicherweise den gleichen Layer wie die Schächte/Bauwerke verwenden.

### Abrufen von Schacht-/Bauwerkspositionen

Nachdem wir die Schächte/Bauwerke erstellt haben, müssen wir ihre Position im Raum herausfinden, damit wir sie nach ihrer Position sortieren können. Dazu nutzen wir den Begrenzungsrahmen der einzelnen Objekte. Der **Begrenzungsrahmen** eines Objekts ist ein Quader, dessen Mindestgröße die komplette Geometrie des Objekts umschließt. Durch die Berechnung des Mittelpunkts des Begrenzungsrahmens erhalten Sie eine ziemlich gute Annäherung an den Einfügepunkt des Schachts/Bauwerks.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Verwenden von Begrenzungsrahmen, um den ungefähren Einfügepunkt jedes Schachts/Bauwerks zu ermitteln</p></figcaption></figure>

Wir verwenden diese Punkte, um die Station und den Versatz der Schächte/Bauwerke relativ zu einer ausgewählten Achse abzurufen.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filtern und Sortieren

Jetzt wird es etwas schwieriger. Zu diesem Zeitpunkt haben wir eine große Liste aller Schächte/Bauwerke auf dem angegebenen Layer erstellt. Wir haben eine Achse ausgewählt, nach der wir sie sortieren möchten. Das Problem ist, dass die Liste möglicherweise Schächte/Bauwerke enthält, die nicht umbenannt werden sollen. Sie sind beispielsweise nicht Teil der Strecke, an der wir interessiert sind.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. Die ausgewählte Achse
> 2. Die Schächte/Bauwerke, die wir umbenennen möchten
> 3. Die Schächte/Bauwerke, die ignoriert werden sollen

Wir müssen die Liste der Schächte/Bauwerke filtern, sodass diejenigen, die einen bestimmten Versatz von der Achse überschreiten, nicht berücksichtigt werden. Dies erreichen Sie am besten mit dem Block **List.FilterByBoolMask**. Nach dem Filtern der Schacht-/Bauwerksliste verwenden Sie den **List.SortByKey**-Block, um sie nach ihren Stationswerten zu sortieren.

{% hint style="info" %}

 Wenn Listen neu für Sie sind, finden Sie im Abschnitt [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") weitere Informationen. 

{% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtern und Sortieren der Schächte/Bauwerke</p></figcaption></figure>

> 1. Überprüfen, ob der Schacht-/Bauwerkversatz kleiner als der Schwellenwert ist.
> 2. Alle Nullwerte durch _false_ ersetzen
> 3. Liste der Schächte/Bauwerke und Stationen filtern
> 4. Schächte/Bauwerke nach Stationen sortieren

### Neue Namen generieren

Als Letztes müssen wir noch die neuen Namen für die Schächte/Bauwerke erstellen. Wir verwenden das Format `<alignment name>-STRC-<number>`. Hier gibt es einige zusätzliche Blöcke, um die Nummern ggf. mit zusätzlichen Nullen zu füllen (z. B. "01" statt "1").

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Generieren der neuen Namen für Schächte/Bauwerke</p></figcaption></figure>

### Umbenennen von Schächten/Bauwerken

Und nicht zuletzt benennen wir die Schächte/Bauwerke um.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Namen für die Schächte/Bauwerke festlegen</p></figcaption></figure>

### Ergebnis

Hier sehen Sie ein Beispiel für die Ausführung des Diagramms mit **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Ausführen des Diagramms mit Dynamo Player und Anzeigen der Ergebnisse in Civil 3D</p></figcaption></figure>

{% hint style="info" %}

 Wenn Dynamo Player neu für Sie ist, finden Sie im Abschnitt [dynamo-player.md](../../dynamo-player.md "mention") weitere Informationen. 

{% endhint %}

> :tada: Mission erfüllt!

### Bonus: Visualisierung in Dynamo

Es kann hilfreich sein, die 3D-Hintergrundvorschau von Dynamo zu nutzen, um die Zwischenausgaben des Diagramms anstatt nur das Endergebnis zu visualisieren. Eine einfache Möglichkeit besteht darin, die Begrenzungsrahmen für die Schächte/Bauwerke anzuzeigen. Darüber hinaus enthält dieses spezielle Dokument einen 3D-Profilkörper. Wir können die Geometrie der 3D-Profilkörper-Elementkante in Dynamo importieren, um den Kontext für die Position der Schächte/Bauwerke im Raum zu verdeutlichen. Wenn das Diagramm für einen Datensatz verwendet wird, der keine 3D-Profilkörper enthält, führen diese Blöcke einfach keine Aktionen aus.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Visualisieren der Geometrie für Schächte/Bauwerke und 3D-Profilkörper-Elementkanten</p></figcaption></figure>

Jetzt verstehen wir besser, wie der Prozess des Filterns der Schächte/Bauwerke nach Versatz funktioniert.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Anpassen des Grenzwerts für den Achsversatz und Visualisieren der betroffenen Schächte/Bauwerke in Dynamo</p></figcaption></figure>

## Ideen

Im Folgenden finden Sie einige Anregungen, wie Sie die Funktionen dieses Diagramms erweitern können.

{% hint style="info" %}

 Benennen Sie die Schächte/Bauwerke basierend auf der **nächstgelegenen Achse** um, anstatt eine bestimmte Achse auszuwählen. 

{% endhint %}

{% hint style="info" %}

 **Benennen Sie die Haltungen um** – zusätzlich zu den Schächten/Bauwerken. 

{% endhint %}

{% hint style="info" %}

 **Legen Sie die Layer der Schächte/Bauwerke fest** – basierend auf ihrem Verlauf. 

{% endhint %}
