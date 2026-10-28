# Informationen zum Dynamo MCP-Server

**MCP** steht für Model Context Protocol. Es handelt sich um einen offenen Standard, mit dem KI-Modelle über ein gemeinsames Protokoll mit der Software verbunden werden können. Ein MCP-Server stellt die Werkzeuge und Daten eines Programms bereit, sodass ein KI-Client Live-Kontext lesen, Aktionen ausführen und strukturierte Ergebnisse zurückgeben kann.

**DynamoMCP** ist der integrierte MCP-Server von Dynamo. Er verbindet Autodesk Assistant mit Ihrer aktiven Dynamo-Sitzung – Ihrem Diagramm, Ihren Blöcken, Ihren Drähten und Ihrem Arbeitsbereich –, sodass die KI-Hilfe auf dem basiert, woran Sie tatsächlich arbeiten.

Für Benutzer von Dynamo bedeutet dies, dass Assistant mehr kann als nur allgemeine Fragen beantworten. Er kann Ihr geöffnetes Diagramm überprüfen, bei der Behebung von Problemen mit Blöcken helfen und den Ansichtsbereich basierend auf dem realen Sitzungskontext aktualisieren, anstatt manuell zu kopieren und einzufügen.

DynamoMCP ist in Dynamo 4.2.0 und höher verfügbar.

### So verwenden Sie DynamoMCP

Sie verwenden DynamoMCP über Autodesk Assistant – es gibt keine separate Einrichtung oder eigenständige Benutzeroberfläche. Assistant ist der Einstiegspunkt, um Fragen zu stellen, Hilfe zu Ihrem Diagramm zu erhalten und die KI mit dem Live-Kontext von Dynamo auszuführen.

Wo Sie Assistant öffnen, hängt davon ab, wo Sie arbeiten:

- **Dynamo Sandbox**: Dynamo MCP-Werkzeuge sind in Autodesk Assistant innerhalb von Dynamo verfügbar.
- **Revit**: Dynamo MCP-Werkzeuge sind in Autodesk Assistant innerhalb von Revit verfügbar.

In beiden Fällen bleiben Sie in der aktuellen Sitzung und verwenden denselben Assistant, um auf DynamoMCP zuzugreifen.
### Funktionen von DynamoMCP

DynamoMCP bietet Assistant eine Reihe von **Werkzeugen**. Jedes Werkzeug führt in Dynamo eine Aufgabe aus, z. B. Lesen des Ansichtsbereichs, Platzieren eines Blocks oder Ausführen des Diagramms. Sie rufen diese Werkzeuge nie selbst auf. Sie beschreiben Ihre Wünsche in einfacher Sprache, und Assistant wählt die benötigten Werkzeuge aus.

Es ist dennoch nützlich, über die Werkzeuge Bescheid zu wissen. Sie können daraus ableiten, welche Aktionen mithilfe des Assistant möglich sind, und Sie können Anforderungen so formulieren, dass er sie ausführen kann.

{% hint style="info" %}
Werkzeugnamen, die mit `get_` beginnen, lesen nur Informationen aus Ihrer Sitzung. Jedes andere Werkzeug kann Ihr Diagramm ändern.
{% endhint %}

#### Lesen des aktuellen Diagramms

Mit diesen Werkzeugen kann Assistant sehen, woran Sie gerade arbeiten, bevor er antwortet oder eine Änderung vornimmt.

| Werkzeug | Aktion |
| --- | --- |
| `get_workspace_info` | Listet auf, was sich im Ansichtsbereich befindet: Blöcke, Drähte, Gruppen, Anmerkungen und Details zum Arbeitsbereich. |
| `get_selection` | Zeigt die ausgewählten Blöcke, Drähte und Gruppen an. |
| `get_node_connections` | Zeigt Port für Port an, womit ein Block vernetzt ist. |
| `get_node_property` | Meldet die Einstellungen für den Block, z. B. Vorschau, Einfrieren, fixierte Vorschau und Beschriftungen. |
| `get_node_lacing` | Gibt die Vergitterungseinstellung eines Blocks an. |
| `get_installation_info` | Gibt Ihre Dynamo-Version, den aktiven Host sowie einen Link zu den Versionshinweisen an. |

*Beispiel-Prompt: "Was befindet sich gerade in meinem Ansichtsbereich?"*

#### Suchen des richtigen Blocks

Dynamo verfügt über Tausende von Blöcken, einschließlich Blöcken aus installierten Paketen. Mit diesen Werkzeugen kann Assistant die richtige Option auswählen, anstatt zu raten.

| Werkzeug | Aktion |
| --- | --- |
| `get_nodes_info` | Durchsucht die Blockbibliothek, einschließlich der Paketblöcke, und gibt den exakten Block zum Platzieren zurück. |
| `get_node_help` | Zeigt den Hilfetext für einen Block und den Pfad zu seinem Beispieldiagramm an, falls vorhanden. |
| `get_type_schema` | Gibt das erwartete Datenformat für einen Dynamo-Typ zurück, z. B. Punkt, Zahl oder Farbe. |

*Beispiel-Prompt: "Welchen Block verwende ich, um die Fläche einer Oberfläche abzurufen?"*

#### Erstellen und Bearbeiten des Diagramms

Diese Werkzeuge erledigen die Arbeit im Ansichtsbereich.

| Werkzeug | Aktion |
| --- | --- |
| `create_nodes` | Platziert einen oder mehrere Blöcke im Ansichtsbereich mit optionalen Startwerten. |
| `set_node_value` | Legt den Wert von Eingabeblöcken wie Nummer, Zeichenfolge, Codeblock und Dropdown-Listen fest. |
| `connect_nodes` | Vernetzt einen Ausgabeanschluss mit einem Eingabeanschluss. |
| `disconnect_nodes` | Entfernt Drähte zwischen zwei Blöcken. |
| `delete_node` | Entfernt einen Block aus dem Ansichtsbereich. |
| `rename_node` | Benennt Eingabe- und Überwachungsblöcke um, sodass ihr Zweck klar ist. Funktionsblöcke behalten ihre ursprünglichen Namen. |
| `select_nodes` | Wählt Blöcke im Ansichtsbereich aus, damit Sie sehen können, auf welche davon sich Assistant bezieht. |
| `set_node_property` | Aktiviert oder deaktiviert Blockeinstellungen: Vorschau, Einfrieren, fixierte Vorschau, Beschriftungen sowie Eingabe oder Ausgabe. |
| `set_node_lacing` | Legt die Vergitterung auf Auto, Kürzeste, Längste oder Kreuzprodukt fest. |
| `set_node_port_list_at_level` | Legt die Listenebene an einem Eingabeanschluss fest. |

*Beispiel-Prompt: "Füge einen Zahlen-Schieberegler hinzu und vernetze ihn mit der Radiuseingabe meines Kreises."*

#### Organisieren und Dokumentieren des Diagramms

Ein Arbeitsdiagramm ist nicht immer lesbar. Diese Werkzeuge sind für das Layout, die Gruppierung, Farben und Anmerkungen zuständig.

| Werkzeug | Aktion |
| --- | --- |
| `create_group` | Gruppiert einen Satz von Blöcken. |
| `create_groups` | Erstellt mehrere Gruppen in einem Schritt. |
| `ungroup_by_guid` | Entfernt eine Gruppe. Die darin enthaltenen Blöcke bleiben im Ansichtsbereich. |
| `ungroup_by_title` | Entfernt Gruppen nach Titel. Die darin enthaltenen Blöcke bleiben im Ansichtsbereich. |
| `get_available_group_styles` | Listet die Gruppenstile auf, die Sie verwenden können, einschließlich benutzerdefinierter Stile aus Ihren Voreinstellungen. |
| `assign_group_styles` | Wendet Gruppenstile wie Eingaben, Aktionen, Ausgaben und Überprüfung an. |
| `get_notes` | Listet die Anmerkungen im Diagramm auf. |
| `create_notes` | Fügt Anmerkungen hinzu, die Teile des Diagramms erläutern. |
| `update_notes` | Bearbeitet oder verschiebt den Text einer Anmerkung. |
| `delete_notes` | Entfernt Anmerkungen. |
| `set_note_state` | Fixiert eine Anmerkung an einem Block, sodass die beiden gemeinsam verschoben werden, oder hebt die Fixierung auf. |
| `auto_layout_workspace` | Ordnet die Blöcke mithilfe des integrierten automatischen Layouts von Dynamo neu an. |
| `set_workspace_description` | Schreibt eine Beschreibung des Diagramms in die Eigenschaften der Datei. |

*Beispiel-Prompt: "Gruppiere und codiere dieses Diagramm nach Eingaben, Aktionen und Ausgaben, und füge dann kurze Anmerkungen hinzu."*

#### Diagramm ausführen und Ergebnisse überprüfen

| Werkzeug | Aktion |
| --- | --- |
| `run_workspace` | Führt das aktuelle Diagramm aus. Im automatischen Modus wird das Diagramm bereits bei jeder Änderung erneut ausgeführt, sodass dieser Schritt übersprungen wird. |
| `get_workspace_warnings` | Listet die Blöcke mit Warnungen oder Fehlern, den Meldungstext und die vorgeschlagenen nächsten Schritte auf. Das Diagramm muss zuerst ausgeführt werden. |
| `get_node_output_values_shapes` | Fasst die Ausgabewerte eines einzelnen Blocks oder aller Blöcke im Diagramm zusammen. |
| `get_node_output_value` | Liest die gesamte Ausgabe eines einzelnen Blocks, Seite für Seite, für Ergebnisse, die zu groß sind, um sie zusammenzufassen. |

*Beispiel-Prompt: "Führe das Diagramm aus, und sage mir, warum der List.GetItemAtIndex-Block eine Warnung anzeigt."*

#### Arbeiten mit Diagrammdateien

Assistant kann auch mit Diagrammen auf der Festplatte arbeiten, nicht nur mit dem, das Sie geöffnet haben.

| Werkzeug | Aktion |
| --- | --- |
| `new_workspace` | Öffnet ein neues, leeres Diagramm. Wenn das aktuelle Diagramm nicht gespeicherte Änderungen enthält, werden Sie zunächst aufgefordert, diese zu speichern oder zu verwerfen. |
| `save_workspace` | Speichert das aktuelle Diagramm. Ohne einen Speicherort wird es in Ihrem Dynamo-Ordner unter Dokumente gespeichert. |
| `edit_graph_from_path` | Öffnet ein gespeichertes Diagramm zur Bearbeitung, ohne es auszuführen. |
| `insert_graph_from_path` | Führt ein gespeichertes Diagramm im aktuellen Ansichtsbereich zusammen. Auf diese Weise werden Beispieldiagramme für Blöcke eingefügt. |
| `run_graph_from_path` | Führt ein gespeichertes Diagramm mit optionalen Eingabewerten aus, die Sie angeben. |
| `run_graph_from_json` | Führt ein Diagramm aus, das als Dynamo-JSON-Datei bereitgestellt wird, ohne dass sich eine Datei auf der Festplatte befindet. |
| `get_graph_info_from_path` | Liest die Eingaben, Ausgaben und Metadaten eines gespeicherten Diagramms, ohne es zu öffnen. |
| `get_graph_info_from_json` | Liest dieselben Details aus einem Diagramm, das als Dynamo-JSON-Datei bereitgestellt wird. |
| `get_graphs_info_from_path` | Durchsucht einen Ordner mit Diagrammen und meldet, was jedes Diagramm enthält. |
| `get_available_graphs_folders` | Listet die Ordner auf, die Dynamo bekannt sind und die Diagramme enthalten können. |

*Beispiel-Prompt: "Gehe meinen Ordner mit den Diagrammen in Dynamo durch und teile mir mit, in welchen Diagrammen Wände platziert werden."*

{% hint style="warning" %}
Assistant kann Ihr Diagramm ändern. Speichern Sie Ihre Arbeit, bevor Sie eine umfangreiche Änderung anfordern, und überprüfen Sie das Ergebnis, bevor Sie eine vorhandene Datei überschreiben.
{% endhint %}
