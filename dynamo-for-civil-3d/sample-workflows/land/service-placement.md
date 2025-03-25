# Platzieren von Hausanschlüssen

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

Beim technischen Entwurf eines typischen Wohngebäudes werden verschiedene unterirdische Versorgungssysteme wie Abwasserkanäle, Regenwasserabläufe, Trinkwasser usw. geplant. In diesem Beispiel wird gezeigt, wie Sie mit Dynamo die Hausanschlüsse von einer Hauptverteilung zu einem bestimmten Grundstück (d. h. einer Parzelle) zeichnen können. Für jedes Grundstück ist ein Hausanschluss erforderlich, was die Positionierung der gesamten Anschlüsse sehr mühsam macht. Dynamo kann den Prozess beschleunigen, indem die erforderliche Geometrie automatisch präzise gezeichnet wird und flexible Eingaben bereitgestellt werden, die an lokale behördliche Standards angepasst werden können.

## Ziel

> :dart: Platzieren Sie Wasseranschlusszähler-Blockreferenzen in angegebenen Abständen von einer Parzellenlinie, und zeichnen Sie eine Linie für jeden Hausanschluss lotrecht zur Hauptverteilung.

## Wichtige Konzepte

> * Verwenden des Blocks **Select Objects** für Benutzereingaben
> * Arbeiten mit Koordinatensystemen
> * Verwenden von geometrischen Operationen wie **Geometry.DistanceTo** und **Geometry.ClosestPointTo**
> * Erstellen von Blockreferenzen
> * Steuern der Einstellungen für Objektbindung

## Kompatibilität der Versionen

{% hint style="success" %} Dieses Diagramm wird in **Civil 3D 2020** und höher ausgeführt. 
{% endhint %} 

## Datensatz

Laden Sie zunächst die folgenden Beispieldateien herunter, und öffnen Sie dann die DWG-Datei und das Dynamo-Diagramm.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Lösung

Hier sehen Sie einen Überblick über die Logik in diesem Diagramm.

> 1. Ruft die Kurvengeometrie für die Hauptverteilung ab.
> 2. Ruft die Kurvengeometrie für eine vom Benutzer ausgewählte Parzellenlinie ab und kehrt sie bei Bedarf um
> 3. Generiert Einfügepunkte für die Hausanschlusszähler
> 4. Ruft die Punkte auf der Hauptverteilung ab, die den Zählerpositionen am nächsten sind
> 5. Erstellt Blockreferenzen und Linien im Modellbereich

Los gehts!

### Abrufen von Geometrie der Hauptverteilung

Der erste Schritt besteht darin, die Geometrie für die Hauptverteilung in Dynamo zu übernehmen. Anstatt einzelne Linien oder Polylinien auszuwählen, rufen wir stattdessen alle Objekte auf einem bestimmten Layer ab und führen sie als Dynamo-PolyCurve zusammen.

{% hint style="info" %}
 Wenn die Kurvengeometrie in Dynamo neu für Sie ist, finden Sie im Abschnitt [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention") weitere Informationen. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Abrufen der Objekte aus Civil 3D und Zusammenführen aller Objekte zu einer einzelnen PolyCurve</p></figcaption></figure>

### Abrufen der Parzellenliniengeometrie

Als Nächstes müssen wir die Geometrie für eine ausgewählte Parzellenlinie in Dynamo importieren, damit wir mit ihr arbeiten können. Das richtige Werkzeug dafür ist der Block **Select Object**, mit dem der Benutzer des Diagramms ein bestimmtes Objekt in Civil 3D auswählen kann.

An dieser Stelle tritt möglicherweise ein Problem auf, um das wir uns kümmern müssen. Die Parzellenlinie hat einen Start- und einen Endpunkt, d. h., sie hat eine Richtung. Damit das Diagramm konsistente Ergebnisse erzeugt, müssen alle Parzellenlinien eine konsistente Richtung aufweisen. Wir können diese Bedingung direkt in der Diagrammlogik berücksichtigen, wodurch das Diagramm zuverlässiger wird. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Auswählen einer Parzellenlinie und Sicherstellen der korrekten Richtung</p></figcaption></figure>

> 1. Rufen Sie die Start- und Endpunkte der Parzellenlinie ab.
> 2. Messen Sie den Abstand von jedem Punkt zur Hauptverteilung, und ermitteln Sie dann, welcher Abstand größer ist.
> 3. Das gewünschte Ergebnis ist, dass der Startpunkt der Linie der Hauptverteilung am nächsten ist. Wenn dies nicht der Fall ist, kehren wir die Richtung der Parzellenlinie um. Andernfalls geben wir einfach die ursprüngliche Parzellenlinie zurück.

### Generieren von Einfügepunkten

Jetzt überlegen wir, wo die Zähler platziert werden sollen. In der Regel wird die Platzierung durch die Anforderungen der lokalen Behörden bestimmt, daher geben wir einfach Eingabewerte an, die geändert werden können, um verschiedenen Bedingungen zu entsprechen. Wir verwenden ein **Koordinatensystem** entlang der Parzellenlinie als Referenz für die Erstellung der Punkte. Dadurch können Sie Versätze relativ zur Parzellenlinie ganz einfach definieren, unabhängig von der Ausrichtung.

{% hint style="info" %}
 Wenn Koordinatensysteme neu für Sie sind, finden Sie im Abschnitt [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") weitere Informationen. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Erstellen der Einfügepunkte für die Zähler</p></figcaption></figure>

### Abrufen von Verbindungspunkten

Jetzt müssen wir Punkte auf der Hauptverteilung abrufen, die den Zählerpositionen am nächsten sind. Dadurch können wir die Hausanschlüsse im Modellbereich so zeichnen, dass sie immer lotrecht zur Hauptverteilung sind. Dafür eignet sich der Block **Geometry.ClosestPointTo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Abrufen von lotrechten Punkten auf der Hauptverteilung</p></figcaption></figure>

> 1. Dies ist die Hauptverteilungs-PolyCurve.
> 2. Dies sind die Zählereinfügepunkte.

### Erstellen von Objekten

Der letzte Schritt besteht darin, Objekte im Modellbereich zu erstellen. Wir verwenden die zuvor generierten Einfügepunkte, um Blockreferenzen zu erstellen, und dann die Punkte auf der Hauptverteilung, um Linien zu den Hausanschlüssen zu zeichnen.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Ergebnis

Wenn Sie das Diagramm ausführen, sollten neue Blockreferenzen und Hausanschlusslinien im Modellbereich angezeigt werden. Ändern Sie einige der Eingaben, und beobachten Sie, wie alles automatisch aktualisiert wird.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Anpassen der Eingabeparameter in Dynamo und sofortiges Anzeigen der Ergebnisse in Civil 3D</p></figcaption></figure>

### Bonus: Aktivieren der sequenziellen Platzierung

Sie werden feststellen, dass nach dem Platzieren der Objekte für eine Parzellenlinie die Objekte bei Auswahl einer anderen Parzellenlinie "verschoben" werden.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Verhalten bei aktivierter Objektbindung</p></figcaption></figure>

Dies ist das Vorgabeverhalten von Dynamo und in vielen Fällen sehr nützlich. Möglicherweise möchten Sie jedoch mehrere Hausanschlüsse nacheinander platzieren und Dynamo neue Objekte mit jeder Ausführung erstellen lassen, anstatt die ursprünglichen zu ändern. Sie können dieses Verhalten steuern, indem Sie die Einstellungen für die Objektbindung ändern.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Einstellungen für Objektbindung in Dynamo</p></figcaption></figure>

{% hint style="info" %}
 Weitere Informationen finden Sie im Abschnitt [object-bound.md](../../advanced-topics/object-binding.md "mention") 
{% endhint %} 

Durch Ändern dieser Einstellung wird Dynamo gezwungen, die Objekte, die mit jedem Durchlauf erstellt werden, zu "vergessen". Hier sehen Sie ein Beispiel für die Ausführung des Diagramms mit deaktivierter Objektbindung in **Dynamo Player**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Ausführen des Diagramms mit Dynamo Player und Anzeigen der Ergebnisse in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Wenn Dynamo Player neu für Sie ist, finden Sie im Abschnitt [dynamo-player.md](../../dynamo-player.md "mention") weitere Informationen. 
{% endhint %} 

> :tada: Mission erfüllt!

## Ideen

Im Folgenden finden Sie einige Anregungen, wie Sie die Funktionen dieses Diagramms erweitern können.

{% hint style="info" %}
 Platzieren Sie **mehrere Hausanschlüsse** gleichzeitig, anstatt jede Parzellenlinie auszuwählen. 
{% endhint %} 

{% hint style="info" %}
 Passen Sie die Eingaben an, um **Kanalöffnungen** anstelle von Wasserzählern zu platzieren. 
{% endhint %} 

{% hint style="info" %}
 **Fügen Sie einen Schalter hinzu**, um das Platzieren eines einzelnen Hausanschlusses auf einer bestimmten Seite der Parzellenlinie anstatt auf beiden Seiten zu ermöglichen. 
{% endhint %} 
