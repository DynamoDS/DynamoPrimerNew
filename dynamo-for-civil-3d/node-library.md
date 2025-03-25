# Blockbibliothek

Wie bereits erwähnt, sind **Blöcke** die wichtigsten Bausteine eines Dynamo-Diagramms. Sie werden in logische Gruppen in der **Bibliothek** organisiert. In Dynamo for Civil 3D gibt es zwei Kategorien (oder **Bereiche**) in der Bibliothek, die dedizierte Blöcke zum Arbeiten mit AutoCAD- und Civil 3D-Objekten enthalten, wie z. B. Achsen, Längsschnitte, 3D-Profilkörper, Blockreferenzen usw. Die restlichen Blöcke in der Bibliothek sind allgemeinere Blöcke, die in allen Dynamo-Versionen (z. B. Dynamo für Revit, Dynamo Sandbox usw.) gleich sind.

{% hint style="info" %}
 Weitere Informationen über die Struktur der Blöcke in der Dynamo-Hauptbibliothek finden Sie im Abschnitt [2-library.md](../3\_user\_interface/2-library.md "mention"). 
{% endhint %} 

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Die Blockbibliothek in Dynamo for Civil 3D</p></figcaption></figure>

> 1. Spezifische Blöcke für die Arbeit mit AutoCAD- und Civil 3D-Objekten
> 2. Allgemeine Blöcke
> 3. Blöcke aus **Paketen** von Drittanbietern, die Sie separat installieren können

{% hint style="warning" %} Durch die Verwendung der Blöcke in den Bereichen AutoCAD und Civil 3D funktioniert Ihr Dynamo-Diagramm nur in Dynamo for Civil 3D. Wenn ein Diagramm aus Dynamo for Civil 3D an einer anderen Stelle geöffnet wird (z. B. in Dynamo für Revit), werden diese Blöcke mit einer Warnung markiert und nicht ausgeführt. 
{% endhint %} 

{% hint style="info" %}
 **Warum gibt es zwei separate Bereiche für AutoCAD und Civil 3D?**

In dieser Struktur werden die Blöcke für programmeigene AutoCAD-Objekte (Linien, Polylinien, Blockreferenzen usw.) von den Blöcken für Civil 3D-Objekte (Achsen, 3D-Profilkörper, DGMs usw.) unterschieden. Aus technischer Sicht sind AutoCAD und Civil 3D zwei separate Dinge: AutoCAD ist die Basisanwendung, und Civil 3D basiert darauf. 
{% endhint %} 

## Blockhierarchie

Um mit den AutoCAD- und Civil 3D-Blöcken arbeiten zu können, ist es wichtig, dass Sie die Objekthierarchie in jedem Bereich verstehen. Dies lässt sich mit der Taxonomie in der Biologie vergleichen: Reich, Stamm, Klasse, Ordnung, Familie, Gattung, Art. AutoCAD- und Civil 3D-Objekte werden auf ähnliche Weise kategorisiert. Sehen wir uns einige Beispiele zur Erläuterung an.

### Civil-Objekte

Als Beispiel verwenden wir eine Achse.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Angenommen, Sie möchten den Namen der Achse ändern. Der nächste Block, den Sie von hier aus hinzufügen, ist ein **CivilObject.SetName**-Block.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

Zunächst scheint dies nicht sehr intuitiv. Was ist ein **CivilObject**, und warum enthält die Bibliothek keinen **Alignment.SetName**-Block? Die Antwort hängt mit der _Wiederverwendbarkeit_ und _Einfachheit_ zusammen. Denn: Der Prozess zum Ändern des Namens eines Civil 3D-Objekts ist gleich, unabhängig davon, ob es sich um eine Achse, einen 3D-Profilkörper, einen Längsschnitt oder etwas Anderes handelt. Anstatt sich wiederholende Blöcke zu verwenden, die im Wesentlichen alle denselben Zweck erfüllen (z. B. **Alignment.SetName, Corridor.SetName, Profile.SetName** usw.), ist es sinnvoll, diese Funktionen in einen einzigen Block zu vereinen. Genau das macht **CivilObject.SetName**!

Eine andere Betrachtungsweise dafür sind _Beziehungen_. Eine Achse und ein 3D-Profilkörper sind beide Arten von **Civil-Objekten**, genauso wie ein Apfel und eine Birne beide Obstsorten sind. Civil-Objekt-Blöcke können auf alle Typen von Civil-Objekten angewendet werden, so als ob Sie einen einzigen Schäler zum Schälen eines Apfels und einer Birne verwenden möchten. Ihre Küche wäre ziemlich vollgestopft, wenn Sie für jede Obstsorte einen eigenen Schäler verwenden würden! In diesem Sinne entspricht die Dynamo-Blockbibliothek Ihrer Küche.

### Objekte

Gehen wir nun einen Schritt weiter. Angenommen, Sie möchten den Layer der Achse ändern. Der Block, den Sie verwenden würden, ist der **Object.SetLayer**-Block.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Warum gibt es keinen Block mit dem Namen **CivilObject.SetLayer**? Hier gelten die gleichen Prinzipien der Wiederverwendbarkeit und Einfachheit, die wir bereits besprochen haben. Die Eigenschaft _Layer_ ist für alle Objekte in AutoCAD identisch, die gezeichnet oder eingefügt werden können, wie Linien, Polylinien, Text, Blockreferenzen usw. Civil 3D-Objekte wie Achsen und 3D-Profilkörper fallen in dieselbe Kategorie. Jeder Block, der einem **Objekt** entspricht, kann daher auch mit einem beliebigen **Civil-Objekt** verwendet werden.

