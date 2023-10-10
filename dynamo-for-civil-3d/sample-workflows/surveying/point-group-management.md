# Punktgruppenverwaltung

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Die Arbeit mit Koordinatenpunkten und Punktgruppen in Civil 3D ist ein Kernelement vieler Prozesse von der Feldvermessung bis zum grafischen Endergebnis. Dynamo eignet sich perfekt für die Datenverwaltung, und wir werden in diesem Beispiel einen potenziellen Anwendungsfall zeigen.  

## Ziel

> :dart: Erstellen Sie eine Punktgruppe für jede eindeutige Koordinatenpunktbeschreibung. 

## Wichtige Konzepte

> * Arbeiten mit Listen
> * Gruppieren ähnlicher Objekte mit dem Block **List.GroupByKey**
> * Anzeigen von benutzerdefinierten Ausgaben in Dynamo Player

## Kompatibilität der Versionen

{% hint style="success" %}\r\n Dieses Diagramm wird in **Civil 3D 2020** und höher ausgeführt. \r\n{% endhint %}

## Datensatz

Laden Sie zunächst die folgenden Beispieldateien herunter, und öffnen Sie dann die DWG-Datei und das Dynamo-Diagramm.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Lösung

Hier sehen Sie einen Überblick über die Logik in diesem Diagramm.

> 1. Alle Koordinatenpunkte im Dokument abrufen
> 2. Koordinatenpunkte nach Beschreibung gruppieren
> 3. Punktgruppen erstellen
> 4. Zusammenfassung in Dynamo Player ausgeben

Los gehts!

### Abrufen von Koordinatenpunkten

Der erste Schritt besteht darin, alle Punktgruppen im Dokument und dann alle Koordinatenpunkte in jeder Gruppe abzurufen. Dadurch erhalten wir eine _verschachtelte Liste_ bzw. "Liste von Listen", die später einfacher zu bearbeiten ist, wenn wir alles mithilfe des **List.Flatten**-Blocks auf eine einzige Liste reduzieren.

{% hint style="info" %}\r\n Wenn Listen neu für Sie sind, finden Sie im Abschnitt [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") weitere Informationen. \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Alle Punktgruppen und Koordinatenpunkte abrufen </p></figcaption></figure>

### Gruppieren von Punkten nach Beschreibung

Nachdem wir nun alle Koordinatenpunkte haben, müssen wir sie anhand ihrer Beschreibungen in Gruppen unterteilen. Dies entspricht genau der Funktion des Blocks **List.GroupByKey**. Im Prinzip werden alle Elemente, die denselben Schlüssel verwenden, in Gruppen zusammengefasst.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Gruppieren der Koordinatenpunkte nach Beschreibung</p></figcaption></figure>

### Erstellen von Punktgruppen

Die harte Arbeit ist getan! Der letzte Schritt besteht darin, neue Civil 3D-Punktgruppen aus den gruppierten Koordinatenpunkten zu erstellen.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Neue Punktgruppen erstellen</p></figcaption></figure>

### Ausgabezusammenfassung

Wenn Sie das Diagramm ausführen, ist in der Dynamo-Hintergrundvorschau nichts zu sehen, da wir nicht mit Geometrie arbeiten. Die einzige Möglichkeit, um zu sehen, ob das Diagramm korrekt ausgeführt wurde, besteht darin, den Projektbrowser zu überprüfen oder die Blockausgabe-Vorschau anzuzeigen. Wenn Sie das Diagramm jedoch mit **Dynamo Player** ausführen, wird mehr Feedback zu den Ergebnissen des Diagramms bereitgestellt, indem eine Zusammenfassung der erstellten Punktgruppen ausgegeben wird. Sie müssen nur mit der rechten Maustaste auf einen Block klicken und _Ist Ausgabe_ auswählen. In diesem Fall verwenden wir einen umbenannten **Watch**-Block, um die Ergebnisse anzuzeigen.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>Wenn Sie einen Block auf <em>Ist Ausgabe</em> setzen, werden die Inhalte in der Dynamo Player-Ausgabe angezeigt.</p></figcaption></figure>

### Ergebnis

Hier sehen Sie ein Beispiel für die Ausführung des Diagramms mit **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Ausführen des Diagramms mit Dynamo Player und Anzeigen der Ergebnisse im Projektbrowser</p></figcaption></figure>

{% hint style="info" %}\r\n Wenn Dynamo Player neu für Sie ist, finden Sie im Abschnitt [dynamo-player.md](../../dynamo-player.md "mention") weitere Informationen. \r\n{% endhint %}

> :tada: Mission erfüllt!

## Ideen

Im Folgenden finden Sie einige Anregungen, wie Sie die Funktionen dieses Diagramms erweitern können.

{% hint style="info" %}\r\n Ändern Sie die Punktgruppierung, sodass die **ausführliche Beschreibung** anstelle der Kurzbeschreibung verwendet wird. \r\n{% endhint %}

{% hint style="info" %}\r\n Gruppieren Sie die Punkte nach anderen **vordefinierten Kategorien**, die Sie auswählen (z. B. Geländeaufnahmen, Monumente usw.)  \r\n{% endhint %}

{% hint style="info" %}\r\n Erstellen Sie automatisch triangulierte DGMs für Punkte in bestimmten Gruppen. \r\n{% endhint %}
