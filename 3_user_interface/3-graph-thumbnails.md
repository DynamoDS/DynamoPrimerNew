# Diagramm-Miniaturansichten für die Dynamo-Startseite

Auf der Dynamo-Startseite werden Diagramm-Miniaturansichten auf einer breiten Karte angezeigt. Das Kartenformat ist 16:9. Auf der Startseite wird das Bild zentriert und oben und unten zugeschnitten.

Eine quadratische Miniaturansicht kann in Generatives Design vollständig aussehen, wobei das vollständige Quadrat angezeigt wird. Für dasselbe Bild können Titel oder Logos auf der Startseite verloren gehen.

Dies gilt für alle Diagramme (`.dyn`), nicht nur für Diagramme in einem Paket. Legen Sie das Bild in den Diagrammeigenschaften fest. Das Bild wird in der Diagrammdatei gespeichert.

{% hint style="info" %}
Belassen Sie Titel, Logos und das Hauptbild im **sicheren Bereich** in der Mitte. Verwenden Sie die Ober- und Unterseite einer quadratischen Miniaturansicht nur als Hintergrund.
{% endhint %}

## Vorlagen für Miniaturansichten

Laden Sie eine Vorlage herunter, und erstellen Sie darüber einen Entwurf. Das dunklere Band in der Mitte ist das, was auf der Startseite angezeigt wird. Das hellere Band oben und unten wird auf der Startseite zugeschnitten. Generatives Design zeigt das vollständige Quadrat.

> Laden Sie die SVG-Vorlage herunter, um sie in einer Vektor-App zu bearbeiten, oder die PNG-Vorlage, um Grafiken in einem Bild-Editor zu platzieren.

{% file src="../.gitbook/assets/Template_Thumbnail.svg" %}

{% file src="../.gitbook/assets/DynamoThumbnailTemplate.png" %}

<figure><img src="../.gitbook/assets/DynamoThumbnailTemplate.png" alt="Quadratische Miniaturansicht-Vorlage. Das dunklere mittlere Band ist auf der Dynamo-Startseite sichtbar. Die helleren oberen und unteren Bänder werden auf der Startseite zugeschnitten und bleiben in Generatives Design sichtbar."><figcaption><p>Das dunklere mittlere Band ist auf der Startseite sichtbar. Die helleren oberen und unteren Bänder werden zugeschnitten.</p></figcaption></figure>

## Erstellen der Miniaturansicht

1. Öffnen Sie die SVG- oder PNG-Vorlage in einem Bild-Editor.
2. Platzieren Sie Ihre Grafiken auf dem rosafarbenen Quadrat in der Vorlage.
3. Behalten Sie Titel, Logo und Hauptbild im dunkleren mittleren Band bei.
4. Verwenden Sie das hellere obere und untere Band nur für Hintergrundfarbe oder zusätzliche Grafiken.
5. Blenden Sie die rosafarbene Hilfslinie aus, oder löschen Sie sie, und exportieren Sie dann eine **quadratische** PNG-Datei.

Verwenden Sie ein **quadratisches** Bild (1:1). Auf der Startseite wird das dunklere mittlere Band immer als 16:9-Zuschnitt angezeigt. Die Pixelgröße wirkt sich nicht auf den Zuschnitt aus. Wenn Sie mit einer leeren Datei anstelle der Vorlage beginnen, legen Sie sie quadratisch an. Die Vorlage der Miniaturansicht selbst hat 840 × 840 Pixel.

## Hinzufügen der Miniaturansicht zu einem Diagramm

Auf der Startseite wird die Miniaturansicht aus der Diagrammdatei (`.dyn`) gelesen, nicht aus dem Publizierungsformular von Package Manager.

1. Öffnen Sie das Diagramm in Dynamo.
2. Wechseln Sie zu **Datei > Diagrammeigenschaften anzeigen > Allgemein**.
3. Fügen Sie die exportierte PNG-Datei im Bildfeld hinzu.
4. Speichern Sie das Diagramm.

Wenn das Diagramm in einem Paket geliefert wird, schließen Sie diese `.dyn` beim Publizieren ein. Weitere Informationen finden Sie unter [Publizieren von Paketen](../6_custom_nodes_and_packages/6-2_packages/4-publishing.md).

## Überprüfen des Ergebnisses auf der Startseite

1. Öffnen Sie die Dynamo-Startseite.
2. Suchen Sie unter Zuletzt verwendet das Diagramm, das Sie gerade gespeichert haben.
3. Vergewissern Sie sich, dass der Titel und das Logo auf der Karte vollständig sichtbar sind.

Wenn Text abgeschnitten wird, verschieben Sie ihn in die Mitte, exportieren Sie das Bild erneut, und aktualisieren Sie die Diagrammeigenschaften. Verlassen Sie sich nicht nur auf Generatives Design. Generatives Design zeigt das vollständige Quadrat.
