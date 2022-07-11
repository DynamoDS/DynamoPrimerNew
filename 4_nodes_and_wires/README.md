# Blöcke und Drähte

## Blöcke

In Dynamo stellen **Blöcke** die Objekte dar, die zum Bilden eines visuellen Programms miteinander verbunden werden. Jeder **Block** führt einen Vorgang aus –  vom einfachen Speichern einer Zahl bis hin zu komplexen Aktionen wie das Erstellen oder Abfragen von Geometrie.

### Anatomie von Blöcken

In Dynamo setzen sich die meisten Blöcke aus fünf Teilen zusammen. Abgesehen von einigen Ausnahmen (z. B. Eingabeblöcke) kann die Anatomie eines jeden Blocks wie folgt beschrieben werden:

![](<images/nodes and wires - nodes anatomy.jpg>)

> 1. Name: Der Name des Blocks gemäß `Category.Name`-Benennungskonvention
> 2. Hauptkörper: Der Hauptkörper des Blocks. Durch Klicken mit der rechten Maustaste auf diesen Bereich werden Optionen für den gesamten Block angezeigt.
> 3. Anschlüsse (eingehend und ausgehend): Die Rezeptoren für Drähte, über die die eingegebenen Daten sowie die Ergebnisse von Blockaktionen an Blöcke geliefert werden.
> 4. Vorgabewert: Klicken Sie mit der rechten Maustaste auf einen Eingabeanschluss. Einige Blöcke verfügen über Vorgabewerte, die verwendet werden können, aber nicht verwendet werden müssen.
> 5. Symbol Vergitterung: Zeigt die Option [Vergitterung](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing) an, die für übereinstimmende Listeneingaben angegeben ist (mehr dazu später).

### Eingabe-/Ausgabeanschlüsse von Blöcken

Die Eingaben und Ausgaben für Blöcke werden als Anschlüsse bezeichnet. Sie fungieren als Kontakte für Drähte. Daten gelangen über die Anschlüsse auf der linken Seite in Blöcke und strömen auf der rechten Seite wieder aus den Blöcken hinaus, nachdem der entsprechende Vorgang ausgeführt wurde.

Anschlüsse erwarten Daten eines bestimmten Typs. Das Verbinden einer Zahl wie _2,75_ mit den Anschlüssen eines Point By Coordinates-Blocks führt beispielsweise dazu, dass ein Punkt erfolgreich erstellt wird. Wenn jedoch _"Rot"_ an denselben Anschluss geliefert wird, tritt ein Fehler auf.

{% hint style="info" %}
Tipp: Bewegen Sie den Cursor auf einen Anschluss, um eine QuickInfo mit dem erwarteten Datentyp aufzurufen.
{% endhint %}

![](<images/nodes and wires - nodes input and tooltip.jpg>)

> 1. Anschlussbezeichnung
> 2. QuickInfo
> 3. Datentyp
> 4. Vorgabewert

### Blockstatus

Dynamo gibt einen Hinweis auf den Status der Ausführung eines visuellen Programms aus, indem Blöcke mit unterschiedlichen Farbschemata basierend auf dem Status der einzelnen Blöcke gerendert werden. Die Hierarchie der Status ist wie folgt: Fehler > Warnung > Info > Vorschau.

Durch Bewegen des Cursors auf den Namen bzw. die Anschlüsse oder durch Klicken mit der rechten Maustaste darauf werden zusätzliche Informationen und Optionen angezeigt.

![](<images/nodes and wires - node states.jpg>)

> 1. Aktiv: Blöcke, deren Namen einen dunkelgrauen Hintergrund aufweisen, sind ordnungsgemäß angeschlossen, d. h., alle Eingaben konnten erfolgreich verbunden werden.
> 2. Fehlerstatus: Die rote Statusleiste unter dem Block gibt an, dass der Block einen Fehlerstatus aufweist.
> 3. Anhalten: Bei transparent dargestellten Blöcken ist Anhalten aktiviert, d. h., ihre Ausführung wurde unterbrochen.
> 4. Hintergrundvorschau: Die graue Statusleiste unter dem Block und das Augensymbol ![](<images/nodes and wires - preview off.jpg>) geben an, dass die Geometrievorschau deaktiviert ist.
> 5. Ausgewählt: Aktuell ausgewählte Blöcke weisen einen aquamarinblau hervorgehobenen Rand auf.
> 6. Warnung: Die gelbe Statusleiste unter dem Block zeigt einen Warnstatus an, d. h., es fehlen Eingabedaten oder es sind möglicherweise falsche Datentypen vorhanden.

#### Umgang mit Fehler- oder Warnungsblöcken

Wenn Ihr visuelles Programm Warnungen oder Fehler aufweist, gibt Dynamo zusätzliche Informationen zu dem Problem an. Alle Blöcke, die in gelb angezeigt werden, verfügen auch über eine QuickInfo über dem Namen. Bewegen Sie den Mauszeiger über das Symbol der QuickInfo für die Warnung ![](<images/nodes and wires - node warning icon.png>) oder den Fehler ![](<images/nodes and wires - node error icon.png>), um sie zu erweitern.

{% hint style="info" %}
Tipp: Untersuchen Sie vor dem Hintergrund dieser QuickInfo die vorgelagerten Blöcke, um zu sehen, ob der erforderliche Datentyp oder die erforderliche Datenstruktur fehlerhaft ist.
{% endhint %}

![](<images/nodes and wires - nodes with warning tooltip.jpg>)

> 1. QuickInfo zu Warnung: "Null" oder keine Daten können nicht als Double verstanden werden, d. h. eine Zahl.
> 2. Verwenden Sie den Watch-Block, um die Eingabedaten zu untersuchen.
> 3. Der vorgelagerte Number-Block speichert "Rot", keine Zahl.

## Drähte

Drähte verbinden Blöcke miteinander, um Beziehungen zu erstellen und den Ablauf eines visuellen Programms festzulegen. Sie können sie sich buchstäblich als elektrische Drähte vorstellen, die Datenimpulse von einem Objekt zum nächsten transportieren.

### Programmablauf <a href="#program-flow" id="program-flow"></a>

Drähte verbinden den Ausgabeanschluss eines Blocks mit dem Eingabeanschluss eines anderen Blocks. Diese Direktionalität legt den **Datenfluss** im visuellen Programm fest.

Die Eingabeanschlüsse befinden sich auf der linken Seite, die Ausgabeanschlüsse auf der rechten Seite der Blöcke. Daher kann man allgemein sagen, dass der Programmablauf von links nach rechts verläuft.

![](<images/nodes and wires - flow of data.jpg>)

### Drähte erstellen <a href="#creating-wires" id="creating-wires"></a>

Erstellen Sie einen Draht, indem Sie mit der linken Maustaste auf einen Anschluss und anschließend mit der linken Maustaste auf den Anschluss eines anderen Blocks klicken, um eine Verbindung zu erstellen. Während der Herstellung der Verbindung wird der Draht gestrichelt angezeigt. Nachdem die Verbindung erfolgreich hergestellt wurde, erscheint er als durchgezogene Linie.

Die Daten fließen immer von Ausgabe zu Eingabe durch diesen Draht. Sie können den Draht jedoch in beliebiger Richtung erstellen, die dadurch definiert wird, in welcher Reihenfolge Sie auf die Anschlüsse klicken.

![](<images/nodes and wires - creating a wire.gif>)

### Drähte bearbeiten <a href="#editing-wires" id="editing-wires"></a>

Es kommt häufig vor, dass Sie den Programmablauf in Ihrem visuellen Programm anpassen müssen, indem Sie die durch Drähte dargestellten Verbindungen bearbeiten. Um einen Draht zu bearbeiten, klicken Sie mit der linken Maustaste auf den Eingabeanschluss eines Blocks, der bereits verbunden ist. Sie haben jetzt zwei Möglichkeiten:

* Um die Verbindung zu einem Eingabeanschluss zu ändern, klicken Sie mit der linken Maustaste auf einen anderen Eingabeanschluss.

![](<images/nodes and wires - edit wire change port (2).gif>)

* Um den Draht zu entfernen, ziehen Sie den Draht weg und klicken Sie mit der linken Maustaste in den Arbeitsbereich.

![](<images/nodes and wires - edit wires remove.gif>)

* Erneutes Verbinden mehrerer Drähte mit UMSCHALT+Linksklick

![](<images/nodes and wires - edit multi ports.gif>)

* Duplizieren eines Drahts mit STRG+Linksklick

![](<images/nodes and wires - duplicate wire.gif>)

#### Vorgabemäßige und markierte Drähte im Vergleich <a href="#wire-previews" id="wire-previews"></a>

Standardmäßig werden Drähte in der Vorschau mit einem grauen Strich angezeigt. Wenn ein Block ausgewählt wird, werden alle Verbindungsdrähte wie der Block in aquamarinblau hervorgehoben.

![](<images/nodes and wires - default vs highlighted wires.jpg>)

> 1. Hervorgehobener Draht
> 2. Standarddraht

**Drähte vorgabemäßig ausblenden**

Falls Sie es vorziehen, die Drähte im Diagramm auszublenden, können Sie diese Option über Ansicht > Connectors > Connectors anzeigen deaktivieren.

Mit dieser Einstellung werden nur die ausgewählten Blöcke und die verbindenden Drähte in hellem Aquamarin hervorgehoben.

![](<images/nodes and wires - hide wires setting (1).gif>)

#### Nur einzelne Drähte ausblenden

Sie können auch nur ausgewählte Drähte ausblenden, indem Sie mit der rechten Maustaste auf die Blockausgabe klicken und Drähte ausblenden auswählen.

![](<images/nodes and wires - hide selected wire.gif>)
