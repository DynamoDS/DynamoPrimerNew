# Cos'è il server MCP di Dynamo

**MCP** è l'acronimo di Model Context Protocol. Si tratta di uno standard aperto che consente ai modelli IA di connettersi al software tramite un protocollo comune. Un server MCP rende accessibili gli strumenti e i dati di un programma, in modo che un client IA possa leggere il contesto in tempo reale, eseguire azioni e restituire risultati strutturati.

**DynamoMCP** è il server MCP incorporato di Dynamo. Collega Autodesk Assistant alla sessione attiva di Dynamo, ovvero al grafico, ai nodi, ai fili e allo stato dell'area di lavoro, in modo che l'assistenza fornita dall'IA si basi su ciò su cui si sta effettivamente lavorando.

Per gli utenti di Dynamo, ciò significa che Assistant è in grado di fare molto di più che rispondere a domande generiche. Può analizzare il grafico aperto, aiutare a risolvere i problemi relativi ai nodi e aggiornare l'area di lavoro in base al contesto reale della sessione, invece di ricorrere alla funzione di copia e incolla manuale.

DynamoMCP è disponibile in Dynamo 4.2.0 e versioni successive.

### Modalità di utilizzo di DynamoMCP

È possibile utilizzare DynamoMCP tramite Autodesk Assistant: non è prevista alcuna impostazione separata né un'interfaccia standalone. Assistant è il punto di accesso per porre domande, ottenere assistenza con il proprio grafico e consentire all'IA di operare nel contesto di Dynamo in tempo reale.

Il punto in cui si apre Assistant dipende dall'ambiente di lavoro in cui ci si trova:

- **Dynamo Sandbox**: gli strumenti di Dynamo MCP sono disponibili in Autodesk Assistant all'interno di Dynamo.
- **Revit**: gli strumenti di Dynamo MCP sono disponibili in Autodesk Assistant all'interno di Revit.

In entrambi i casi, si rimane nella sessione corrente e si utilizza la stessa esperienza di Assistant per accedere a DynamoMCP.
### Funzioni disponibili in DynamoMCP

DynamoMCP fornisce ad Assistant un set di **strumenti**. Ogni strumento svolge una specifica funzione in Dynamo, come leggere l'area di lavoro, posizionare un nodo o eseguire il grafico. Non è mai necessario chiamare questi strumenti manualmente. Basta descrivere ciò che si desidera in un linguaggio semplice e Assistant seleziona gli strumenti necessari.

Conoscere quali sono questi strumenti è comunque utile. Ciò consente di capire cosa è in grado di fare Assistant e aiuta a formulare richieste che possa elaborare.

{% hint style="info" %}
I nomi degli strumenti che iniziano con `get_` si limitano a leggere le informazioni presenti nella sessione. Ogni altro strumento può modificare il grafico.
{% endhint %}

#### Lettura del grafico corrente

Questi strumenti consentono ad Assistant di vedere su cosa si sta lavorando prima di rispondere o apportare una modifica.

| Strumento | Operazione svolta |
| --- | --- |
| `get_workspace_info` | Elenca gli elementi presenti nell'area di disegno: nodi, fili, gruppi, note e dettagli dell'area di lavoro. |
| `get_selection` | Riporta i nodi, i fili e i gruppi selezionati. |
| `get_node_connections` | Mostra a cosa è collegato un nodo, porta per porta. |
| `get_node_property` | Riporta le impostazioni dei nodi quali anteprima, congelamento, anteprima bloccata ed etichette. |
| `get_node_lacing` | Riporta l'impostazione di collegamento di un nodo. |
| `get_installation_info` | Riporta la versione di Dynamo, l'host in cui è in esecuzione e un collegamento alle note di rilascio. |

*Esempio di prompt: "Cosa c'è nella mia area di disegno in questo momento?"*

#### Individuazione del nodo giusto

Dynamo dispone di migliaia di nodi, compresi quelli dei pacchetti installati. Questi strumenti aiutano Assistant a scegliere quello corretto, invece di dover tirare a indovinare.

| Strumento | Operazione svolta |
| --- | --- |
| `get_nodes_info` | Esegue una ricerca nella libreria dei nodi, compresi i nodi dei pacchetti, e restituisce il nodo esatto da posizionare. |
| `get_node_help` | Restituisce la guida scritta relativa a un nodo e, se disponibile, il percorso del grafico di esempio corrispondente. |
| `get_type_schema` | Restituisce il formato dei dati previsto per un tipo di Dynamo, come Point, Number o Color. |

*Esempio di prompt: "Quale nodo devo usare per calcolare l'area di una superficie?"*

#### Creazione e modifica del grafico

Questi strumenti svolgono il lavoro nell'area di disegno.

| Strumento | Operazione svolta |
| --- | --- |
| `create_nodes` | Posiziona uno o più nodi nell'area di disegno, con valori iniziali facoltativi. |
| `set_node_value` | Imposta il valore dei nodi di input, ad esempio Number, String, Code Block ed elenchi a discesa. |
| `connect_nodes` | Collega una porta di output ad una porta di input. |
| `disconnect_nodes` | Rimuove i fili tra due nodi. |
| `delete_node` | Rimuove un nodo dall'area di disegno. |
| `rename_node` | Rinomina i nodi di input e Watch in modo che la loro funzione risulti chiara. I nodi di funzione mantengono i loro nomi originali. |
| `select_nodes` | Seleziona i nodi nell'area di disegno, in modo da poter vedere a quali si riferisce Assistant. |
| `set_node_property` | Attiva o disattiva le impostazioni dei nodi: anteprima, congelamento, anteprima bloccata, etichette e input o output. |
| `set_node_lacing` | Imposta il collegamento su Automatico, Più breve, Più lungo o Globale. |
| `set_node_port_list_at_level` | Imposta il livello di elenco su una porta di input. |

*Esempio di prompt: "Aggiungi un dispositivo di scorrimento numerico e collegalo all'input del raggio del mio cerchio."*

#### Organizzazione e documentazione del grafico

Un grafico di lavoro non è sempre leggibile. Questi strumenti consentono di gestire il layout, il raggruppamento, i colori e le note.

| Strumento | Operazione svolta |
| --- | --- |
| `create_group` | Crea un gruppo attorno ad un insieme di nodi. |
| `create_groups` | Crea diversi gruppi in un unico passaggio. |
| `ungroup_by_guid` | Rimuove un gruppo. I nodi al suo interno rimangono nell'area di disegno. |
| `ungroup_by_title` | Rimuove i gruppi in base al relativo titolo. I nodi al loro interno rimangono nell'area di disegno. |
| `get_available_group_styles` | Elenca gli stili di gruppo che è possibile utilizzare, compresi gli stili personalizzati definiti nelle preferenze. |
| `assign_group_styles` | Applica gli stili di gruppo, ad esempio Input, Azioni, Output e Revisione. |
| `get_notes` | Elenca le note nel grafico. |
| `create_notes` | Aggiunge note che illustrano parti del grafico. |
| `update_notes` | Modifica il testo di una nota o lo sposta. |
| `delete_notes` | Rimuove le note. |
| `set_note_state` | Blocca una nota ad un nodo in modo che i due si muovano insieme o la sblocca. |
| `auto_layout_workspace` | Ridispone i nodi utilizzando il layout automatico di Dynamo incorporato. |
| `set_workspace_description` | Scrive una descrizione del grafico nelle proprietà del file. |

*Esempio di prompt: "Raggruppa gli elementi di questo grafico e assegnagli un codice colore in base a input, azioni e output, poi aggiungi delle brevi note."*

#### Esecuzione del grafico e verifica dei risultati

| Strumento | Operazione svolta |
| --- | --- |
| `run_workspace` | Esegue il grafico corrente. In modalità automatica il grafico viene già rieseguito ad ogni modifica, quindi questo passaggio viene saltato. |
| `get_workspace_warnings` | Elenca i nodi che presentano avvisi o errori, il testo dei messaggi e i passaggi successivi consigliati. Prima occorre eseguire il grafico. |
| `get_node_output_values_shapes` | Riassume i valori di output di un nodo o di ogni nodo del grafico. |
| `get_node_output_value` | Legge l'output completo di un singolo nodo, una pagina alla volta, nel caso di risultati troppo voluminosi per essere riassunti. |

*Esempio di prompt: "Esegui il grafico e spiegami perché il nodo List.GetItemAtIndex mostra un avviso."*

#### Utilizzo dei file di grafici

Assistant può lavorare anche con i grafici presenti sul disco, non solo con quello attualmente aperto.

| Strumento | Operazione svolta |
| --- | --- |
| `new_workspace` | Apre un nuovo grafico vuoto. Se il grafico corrente contiene modifiche non salvate, viene richiesto di salvarle o eliminarle. |
| `save_workspace` | Salva il grafico corrente. Senza una posizione, viene salvato nella cartella di Dynamo in Documenti. |
| `edit_graph_from_path` | Apre un grafico salvato per la modifica, senza eseguirlo. |
| `insert_graph_from_path` | Unisce un grafico salvato all'area di disegno corrente. Questo è il modo in cui vengono importati i grafici di esempio dei nodi. |
| `run_graph_from_path` | Esegue un grafico salvato, con i valori di input facoltativi forniti dall'utente. |
| `run_graph_from_json` | Esegue un grafico fornito come file JSON di Dynamo, senza file sul disco. |
| `get_graph_info_from_path` | Legge gli input, gli output e i metadati di un grafico salvato senza aprirlo. |
| `get_graph_info_from_json` | Legge gli stessi dettagli da un grafico fornito come file JSON di Dynamo. |
| `get_graphs_info_from_path` | Esegue la scansione di una cartella di grafici e riporta il contenuto di ciascuno di essi. |
| `get_available_graphs_folders` | Elenca le cartelle note di Dynamo che possono contenere grafici. |

*Esempio di prompt: "Cerca nella mia cartella dei grafici di Dynamo e dimmi quali grafici posizionano i muri."*

{% hint style="warning" %}
Assistant può modificare il grafico. Salvare il lavoro prima di richiedere una modifica di ampia portata e controllare il risultato prima di sovrascrivere un file esistente.
{% endhint %}
