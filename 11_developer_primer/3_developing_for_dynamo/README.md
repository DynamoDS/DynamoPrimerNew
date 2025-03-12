# Sviluppo per Dynamo

Indipendentemente dal livello di esperienza, la piattaforma Dynamo è progettata per consentire a tutti gli utenti di contribuire. Esistono diverse opzioni di sviluppo che si rivolgono a diversi livelli di abilità e competenze, ciascuno con i suoi punti di forza e di debolezza a seconda dell'obiettivo. Di seguito illustreremo le diverse opzioni e come scegliere una piuttosto che un'altra.

![Tre ambienti di sviluppo](images/developing-for-dynamo.png)

> Tre ambienti di sviluppo: Visual Studio, l'editor Python Editor e la sintassi DesignScript di Code Block

### Quali sono le opzioni disponibili? <a href="#what-are-my-options" id="what-are-my-options"></a>

Le opzioni di sviluppo per Dynamo si suddividono principalmente in due categorie: _per_ Dynamo anziché _in_ Dynamo. Le due categorie possono essere considerate come segue: "in" Dynamo implica che il contenuto creato con l'IDE Dynamo deve essere utilizzato in Dynamo e "per" Dynamo implica che devono essere utilizzati strumenti esterni per creare il contenuto da importare in Dynamo. Sebbene questa guida si concentri sullo sviluppo _per_ Dynamo, le risorse per tutti i processi sono descritte qui di seguito.

### Per Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

Questi nodi consentono il massimo grado di personalizzazione. Molti pacchetti vengono creati con questo metodo ed è necessario per contribuire ai file di origine di Dynamo. Il processo di creazione sarà descritto in questa guida.

* Nodi zero-touch
* Nodi derivati da NodeModel
* Estensioni

> La Guida introduttiva contiene una guida sull'[importazione di librerie zero-touch](https://primer2.dynamobim.org/v/it/6_custom_nodes_and_packages/6-2_packages/5-zero-touch).

Per la discussione seguente, Visual Studio viene utilizzato come ambiente di sviluppo per i nodi zero-touch e NodeModel.

![Interfaccia di Visual Studio](images/vs-devenv.jpg)

> L'interfaccia di Visual Studio con un progetto che verrà sviluppato

### In Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

Sebbene questi processi esistano nell'area di lavoro di programmazione visiva e siano relativamente semplici, sono tutte opzioni valide per personalizzare Dynamo. La Guida introduttiva li tratta ampiamente e fornisce suggerimenti e pratiche ottimali di scripting nel capitolo [Strategie di scripting](../../9\_best\_practices/2-scripting-strategies.md).

*   I Code Block espongono DesignScript nell'ambiente di programmazione visiva, consentendo workflow flessibili per gli script di testo e i nodi. Una funzione in un Code Block può essere chiamata da qualsiasi elemento dell'area di lavoro.

    > Scaricare un esempio di Code Block (fare clic con il pulsante destro del mouse e salvare con nome) o vedere una simulazione dettagliata nella [Guida introduttiva](https://primer2.dynamobim.org/v/it/8_coding_in_dynamo/8-1_code-blocks-and-design-script/1-what-is-a-code-block).
*   I nodi personalizzati sono contenitori per raccolte di nodi o anche interi grafici. Sono un modo efficace per raccogliere le routine utilizzate più di frequente e condividerle con la comunità.

    > Scaricare un esempio di nodo personalizzato (fare clic con il pulsante destro del mouse e salvare con nome) o vedere una simulazione dettagliata nella [Guida introduttiva](https://primer2.dynamobim.org/v/it/6_custom_nodes_and_packages/6-1_custom-nodes/1-introduction).
*   I nodi Python sono un'interfaccia di scripting nell'area di lavoro di programmazione visiva, simile ai Code Block. Le librerie Autodesk.DesignScript utilizzano una notazione con punto simile a DesignScript.

    > Scaricare un esempio di nodo Python (fare clic con il pulsante destro del mouse e salvare con nome) o vedere una simulazione dettagliata nella [Guida introduttiva](https://primer2.dynamobim.org/v/it/8_coding_in_dynamo/8-3_python)

Lo sviluppo nell'area di lavoro di Dynamo è un potente strumento per ottenere un feedback immediato.

![Sviluppo nell'area di lavoro di Dynamo con il nodo Python](images/python-example.jpg)

> Sviluppo nell'area di lavoro di Dynamo con il nodo Python

### Quali sono i vantaggi/gli svantaggi di ciascuna opzione? <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Le opzioni di sviluppo per Dynamo sono state progettate per soddisfare la complessità di un'esigenza di personalizzazione. Sia che si tratti di scrivere uno script ricorsivo in Python o di creare un'interfaccia utente del nodo completamente personalizzata, esistono opzioni per l'implementazione del codice che comportano solo lo stretto necessario per essere operativi.

**Code Block, nodo Python e nodi personalizzati in Dynamo**

Si tratta di opzioni semplici per la scrittura di codice nell'ambiente di programmazione visiva Dynamo. L'area di lavoro di programmazione visiva di Dynamo consente di accedere a Python, DesignScript e alla possibilità di contenere più nodi all'interno di un nodo personalizzato.

![Code Block, script Python e nodo personalizzato](images/Development-Icons.png)

Con questi metodi è possibile:

* Iniziare a scrivere Python o DesignScript quasi senza alcuna impostazione.
* Importare le librerie di Python in Dynamo.
* Condividere Code Block, nodi Python e nodi personalizzati con la comunità Dynamo come parte di un pacchetto.

**Nodi zero-touch**

Il termine zero-touch si riferisce ad un semplice metodo di puntamento e clic per l'importazione di librerie C#. Dynamo consentirà di leggere i metodi pubblici di un file `.dll` e di convertirli in nodi di Dynamo. È possibile utilizzare il metodo zero-touch per sviluppare nodi e pacchetti personalizzati.

![Nodi zero-touch](images/ZTImport.png)

Con questo metodo è possibile:

* Importare una libreria non necessariamente sviluppata per Dynamo e creare automaticamente una suite di nuovi nodi, come l'[esempio AForge](../../6\_custom\_nodes\_and\_packages/6-2\_packages/5-zero-touch.md#case-study-importing-aforge) nella Guida introduttiva.
* Scrivere metodi C# e utilizzare facilmente i metodi come nodi in Dynamo.
* Condividere una libreria C# come nodi con la comunità Dynamo in un pacchetto.

**Nodi derivati da NodeModel**

Questi nodi sono un livello più basso nella struttura di Dynamo. Si basano sulla classe `NodeModel` e sono scritti in C#. Sebbene questo metodo offra la massima flessibilità e potenza, la maggior parte degli aspetti del nodo deve essere definita esplicitamente e le funzioni devono risiedere in un assieme separato.

![Nodi derivati da NodeModel](images/Development-Icons-NodeModel.png)

Con questo metodo è possibile:

* Creare un'interfaccia utente del nodo completamente personalizzabile con dispositivi di scorrimento, immagini, colori e così via (ad esempio, nodo ColorRange).
* Accedere a ciò che accade nell'area di disegno di Dynamo e intervenire.
* Personalizzare il collegamento.
* Caricare elementi in Dynamo come pacchetto.

### Informazioni sul controllo sulle modifiche all'API e delle versioni di Dynamo (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Poiché Dynamo viene aggiornato regolarmente, potrebbero essere apportate modifiche a parte dell'API utilizzata da un pacchetto. Il monitoraggio di queste modifiche è importante per garantire che i pacchetti esistenti continuino a funzionare correttamente.

Le modifiche all'API vengono registrate nella [pagina Wiki di Dynamo su GitHub](https://github.com/DynamoDS/Dynamo/wiki/API-Changes). Vengono descritte le modifiche apportate a DynamoCore, alle librerie e alle aree di lavoro.

![Documento delle modifiche all'API di Dynamo](images/api-changes.jpg)

Un esempio di modifica significativa imminente è la transizione dal formato di file XML a JSON nella versione 2.0. I nodi derivati da NodeModel ora richiedono un [costruttore JSON](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node), altrimenti non si apriranno in Dynamo 2.0.

La documentazione sull'API di Dynamo attualmente copre le funzionalità principali: [http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI).

![Documentazione sull'API](images/api-docs.jpg)

### Autorizzazione alla distribuzione di file binari in un pacchetto <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Tenere presente che i file .dll inclusi in un pacchetto vengono caricati nel gestore di pacchetti. Se l'autore del pacchetto non ha creato il file .dll, deve disporre dei diritti per condividerlo.

Se un pacchetto include file binari, al momento del download si deve richiedere conferma agli utenti che il pacchetto contiene file binari.

### Considerazioni sulle prestazioni dell'interfaccia utente di Dynamo
Al momento della stesura di questo articolo, Dynamo utilizza principalmente WPF (Windows Presentation Foundation) per eseguire il rendering dell'interfaccia utente. WPF è un sistema complesso e potente basato su oggetti elemento XAML/binding. Poiché Dynamo ha un'interfaccia utente complessa, è facile generare arresti anomali dell'interfaccia utente, perdite di memoria o wrapping dell'esecuzione del grafico e degli aggiornamenti dell'interfaccia utente con modalità che compromettono le prestazioni.

Fare riferimento alla pagina Wiki [Dynamo Performance Considerations](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-UI-Performance) che consente di evitare alcune insidie comuni quando si apportano modifiche al codice di Dynamo.