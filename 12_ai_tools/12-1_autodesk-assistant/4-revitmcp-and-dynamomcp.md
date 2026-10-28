# RevitMCP e DynamoMCP

Le funzionalità di Autodesk Assistant derivano dai server MCP. Ogni server fornisce ad Assistant una serie di strumenti e ogni strumento svolge una specifica funzione all'interno del prodotto a cui appartiene.

Quando si lavora in Revit, due di questi server sono particolarmente rilevanti: **RevitMCP** e **DynamoMCP**. Non si tratta di opzioni concorrenti e non è necessario scegliere tra l'una e l'altra. Operano a livelli diversi e Assistant utilizza quello più adatto alla richiesta dell'utente.

### RevitMCP: operazioni dirette sul modello

RevitMCP, fornito come [Revit Public MCP Server](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm), raggruppa una serie selezionata di operazioni API di Revit sotto forma di strumenti MCP. Ogni strumento corrisponde ad un'azione definita nel modello: individuare elementi, verificare parametri e conteggi, modificare parametri in blocco e acquisire istantanee delle viste.

Questo è il percorso più breve per un'attività già coperta dal set di strumenti. Una richiesta si traduce in una singola chiamata e il modello cambia immediatamente. Non occorre costruire alcun grafico né eseguire alcuna operazione successiva.

### DynamoMCP: strumenti che definiscono il funzionamento

DynamoMCP opera ad un livello superiore. I suoi strumenti non eseguono direttamente operazioni di modellazione. Piuttosto, creano un grafico di Dynamo: posizionano i nodi, li collegano tra loro, impostano i valori ed eseguono il risultato. Per l'elenco completo degli strumenti, vedere [Cos'è il server MCP di Dynamo](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md).

Questo aspetto è importante per via della natura stessa di un nodo. Ogni nodo della libreria di Dynamo è costituito da una o più chiamate API, già scritte, testate e documentate. I nodi dei pacchetti e i nodi Python sono disponibili allo stesso modo. Pertanto, un piccolo set di strumenti per la creazione di grafici consente di accedere all'intera libreria di Dynamo.

L'effetto è che gli strumenti di DynamoMCP generano nuove funzionalità anziché limitarsi ad utilizzare quelle esistenti. Anziché un elenco fisso di azioni, Assistant assembla l'azione richiesta dall'utente utilizzando i nodi. In pratica, gli strumenti MCP che scrivono grafici si comportano come strumenti che creano altri strumenti.

### La differenza nella pratica

Supponiamo di richiedere delle etichette per le porte su un determinato livello.

- Se gestito da RevitMCP, il processo avviene tramite una chiamata diretta e le etichette vengono visualizzate immediatamente.
- Se gestito da DynamoMCP, Assistant posiziona i nodi che raccolgono le porte, le filtra in base al livello specificato, crea le etichette e infine esegue il grafico.

Entrambi i metodi etichettano le porte. Il secondo metodo, inoltre, lascia qualcosa nell'area di disegno.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| Come funziona lo strumento | Esegue un'operazione di Revit definita | Crea ed esegue un grafico di Dynamo |
| Ambito di applicazione | Le operazioni incluse nel set di strumenti | Qualsiasi nodo presente nella libreria, compresi i nodi dei pacchetti e i nodi Python |
| Il risultato ottenuto | La modifica al modello | La modifica al modello, oltre al grafico che l'ha generata |
| Ideale per | Un'unica attività ben definita | La logica che si desidera esaminare, modificare, riutilizzare o condividere |

### Perché vale la pena avere un grafico

Un grafico è una traccia del lavoro svolto, non solo il suo risultato. Una volta visualizzato nell'area di disegno, è possibile:

- Leggerlo e vedere esattamente quali operazioni sono state eseguite.
- Modificare un valore ed eseguirlo di nuovo, senza richiedere di ripetere l'intera operazione.
- Salvarlo ed eseguirlo nel prossimo progetto oppure passarlo a un collega.
- Pubblicarlo sul Lettore Dynamo in modo che anche chi non utilizza Dynamo possa eseguirlo.

È la prima esecuzione quella che richiede tempo. Ogni esecuzione successiva è praticamente immediata.

{% hint style="info" %}
In Dynamo Sandbox, Assistant accede solo a DynamoMCP, poiché non è presente alcun modello di Revit su cui lavorare. In Revit, invece, sono disponibili entrambi.
{% endhint %}

### Che cosa richiedere

Non è necessario specificare il nome di un server nel prompt. È sufficiente descrivere il risultato desiderato e Assistant selezionerà gli strumenti adeguati.

Se si desidera specificatamente un grafico, occorre indicarlo. Prompt come *"creami un grafico che etichetti le porte del Livello 2"* indicano ad Assistant che l'obiettivo è ottenere una versione riutilizzabile, non solo le porte etichettate.

### Ulteriori approfondimenti

- [Autodesk Assistant (Novità della versione 2027)](https://help.autodesk.com/view/RVT/2027/ITA/?guid=GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B): le funzioni di Assistant in Revit, nella Guida di Revit 2027.
- [Revit Public MCP Server (Technology Preview) (Novità della versione 2027)](https://help.autodesk.com/view/RVT/2027/ITA/?guid=GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F): la sezione dedicata a Revit del server MCP, nella Guida di Revit 2027.
- [Autodesk Assistant: Domande frequenti e Panoramica di alto livello (Domande frequenti)](https://www.autodesk.com/it/support/technical/article/caas/sfdcarticles/sfdcarticles/ITA/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html): quali prodotti includono Assistant e come utilizzarlo.
