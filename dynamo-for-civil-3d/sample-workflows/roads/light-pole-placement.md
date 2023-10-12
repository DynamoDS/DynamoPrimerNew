# Posizionamento di lampioni

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Uno dei molti casi di utilizzo importanti di Dynamo è il posizionamento dinamico di oggetti distinti lungo un modello di modellatore. Spesso è necessario posizionare gli oggetti in punti indipendenti dagli assiemi inseriti lungo il modellatore, un compito molto noioso da svolgere manualmente. E quando la geometria orizzontale o verticale del modellatore cambia, ciò comporta una notevole mole di rilavorazione.

## Scopo

> :dart: Posizionare i riferimenti di blocco dei lampioni lungo un modellatore in corrispondenza dei valori di progressiva specificati in un file Excel. 

## Concetti chiave

> * Lettura dei dati da un file esterno (in questo caso Excel)
> * Organizzazione dei dati nei dizionari
> * Utilizzo di sistemi di coordinate per controllare posizione/scala/rotazione
> * Inserimento di riferimenti di blocco
> * Visualizzazione della geometria in Dynamo

## Compatibilità delle versioni

{% hint style="success" %}
 Questo grafico verrà eseguito su **Civil 3D 2020** e versioni successive. 
{% endhint %}

## Set di dati

Iniziare scaricando i file di esempio riportati qui sotto, quindi aprendo il file DWG e il grafico di Dynamo.

{% hint style="info" %}
 È consigliabile salvare il file Excel nella stessa directory del grafico di Dynamo. 
{% endhint %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Soluzione

Ecco una panoramica della logica di questo grafico.

> 1. Leggere il file Excel e importare i dati in Dynamo
> 2. Ottenere le linee caratteristiche dalla linea base del modellatore specificata
> 3. Generare sistemi di coordinate lungo la linea caratteristica del modellatore in corrispondenza delle progressive desiderate
> 4. Utilizzare i sistemi di coordinate per posizionare i riferimenti di blocco nello spazio modello

Procediamo!

### Recupero di dati di Excel

In questo grafico di esempio, si utilizzerà un file Excel per memorizzare i dati impiegati da Dynamo per posizionare i riferimenti di blocco dei lampioni. La tabella ha questo aspetto.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>La struttura della tabella di file Excel</p></figcaption></figure>

{% hint style="info" %}
 L'utilizzo di Dynamo per la lettura dei dati da un file esterno (ad esempio un file Excel) è una strategia ottimale, soprattutto quando i dati devono essere condivisi con altri membri del team. 
{% endhint %}

I dati di Excel vengono importati in Dynamo in questo modo. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Importazione dei dati di Excel in Dynamo</p></figcaption></figure>

Ora che sono disponibili i dati, è necessario suddividerli per colonna (_Corridor_, _Baseline_, _PointCode_ e così via) in modo che possano essere utilizzati nel resto del grafico. Un metodo comune per eseguire questa operazione consiste nell'utilizzare il nodo **List.GetItemAtIndex** e specificare il numero di indice di ogni colonna desiderata. Ad esempio, la colonna _Corridor_ si trova all'indice 0, la colonna _Baseline_ all'indice 1 e così via.

Sembra a posto, giusto? Ma c'è un potenziale problema con questo approccio. Cosa succede se l'ordine delle colonne nel file Excel cambia in futuro? Oppure tra due colonne ne viene aggiunta una nuova? Il grafico non funzionerà correttamente e richiederà un aggiornamento. È proteggere il grafico in futuro inserendo i dati in un **dizionario**, con le intestazioni di colonna di Excel come _keys_ e gli altri dati come _values_.

{% hint style="info" %}
 Se non si conoscono i dizionari, consultare la sezione [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Inserimento dei dati di Excel in un dizionario</p></figcaption></figure>

In questo modo il grafico risulta più resiliente, perché consente di cambiare l'ordine delle colonne in Excel con una certa flessibilità. Finché le intestazioni di colonna rimangono invariate, è possibile recuperare i dati dal dizionario utilizzando _key_ (ovvero l'intestazione di colonna), che è la procedura successiva.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Recupero dei dati dal dizionario</p></figcaption></figure>

### Recupero di linee caratteristiche del modellatore

Ora che i dati di Excel sono stati importati e sono pronti, iniziare a utilizzarli per ottenere da Civil 3D alcune informazioni sui modelli di modellatore.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Selezionare il modello di modellatore in base al nome.
> 2. Ottenere una linea base specifica all'interno del modellatore.
> 3. Ottenere una linea caratteristica all'interno della linea base secondo il relativo codice punto.

### Generazione di sistemi di coordinate

Ora si procederà alla generazione di **sistemi di coordinate** lungo le linee caratteristiche del modellatore in corrispondenza dei valori di progressiva specificati nel file Excel. Questi sistemi di coordinate verranno utilizzati per definire la posizione, la rotazione e la scala dei riferimenti di blocco dei lampioni.

{% hint style="info" %}
 Se non si conoscono i sistemi di coordinate, consultare la sezione [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Acquisizione di sistemi di coordinate lungo le linee caratteristiche del modellatore</p></figcaption></figure>

Si noti che qui viene utilizzato un blocco di codice per ruotare i sistemi di coordinate in base al lato della linea base su cui si trovano. Si potrebbe ottenere questo risultato utilizzando una sequenza di più nodi, ma questo è un buon esempio di una situazione in cui è più facile scriverlo.

{% hint style="info" %}
 Se non si conoscono i blocchi di codice, consultare la sezione [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention"). 
{% endhint %}

### Creazione di riferimenti di blocco

È quasi fatta! Si hanno tutte le informazioni necessarie per poter collocare effettivamente i riferimenti di blocco. La prima cosa da fare è ottenere le definizioni di blocco che si desidera utilizzare nella colonna _BlockName_ del file Excel.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Recupero delle definizioni di blocco desiderate dal documento</p></figcaption></figure>

Da qui, l'ultimo passaggio consiste nella creazione dei riferimenti di blocco.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Creazione di riferimenti di blocco nello spazio modello</p></figcaption></figure>

### Risultato

Quando si esegue il grafico, dovrebbero essere visualizzati nuovi riferimenti di blocco nello spazio modello lungo il modellatore. Ed ecco la parte più interessante. Se la modalità di esecuzione del grafico è impostata su Automatica e si modifica il file Excel, i riferimenti di blocco vengono aggiornati automaticamente.

{% hint style="info" %}
 È possibile leggere ulteriori informazioni sulle modalità di esecuzione del grafico nella sezione [3_user_interface](../../../3\_user\_interface/ "mention"). 
{% endhint %}

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Esecuzione di aggiornamenti al file Excel e rapida visualizzazione dei risultati in Civil 3D</p></figcaption></figure>

Di seguito è riportato un esempio di esecuzione del grafico mediante il **Lettore Dynamo**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Esecuzione del grafico mediante il Lettore Dynamo e visualizzazione dei risultati in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Se non si conosce il Lettore Dynamo, consultare la sezione [dynamo-player.md](../../dynamo-player.md "mention"). 
{% endhint %}

> :tada: Missione compiuta!

### Attività extra: Visualizzazione in Dynamo

Può essere utile visualizzare la geometria del modellatore in Dynamo per fornire un contesto. In questo particolare modello i solidi del modellatore sono già stati estratti nello spazio modello, pertanto verranno importati in Dynamo. 

Ma c'è qualcos'altro che occorre considerare. I solidi sono di un tipo geometrico relativamente "pesante", il che significa che questa operazione rallenta il grafico. Sarebbe bello se esistesse un modo semplice per _scegliere_ se si desidera visualizzare o meno i solidi. La risposta ovvia è semplicemente scollegare il nodo **Corridor.GetSolids**, ma in questo modo verranno generati avvisi per tutti i nodi a valle, il che crea un po' di confusione. Questa è una situazione in cui il nodo **ScopeIf** ha un ruolo fondamentale.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Notare che il nodo **Object.Geometry** mostra una barra grigia nella parte inferiore. Ciò significa che l'anteprima del nodo è disattivata (accessibile facendo clic con il pulsante destro del mouse sul nodo), il che consente a **GeometryColor.ByGeometryColor** di evitare di "contendersi" con altra geometria la priorità di visualizzazione nell'anteprima di sfondo.
> 2. Il nodo **ScopeIf** consente in sostanza di eseguire un intero ramo di nodi in modo selettivo. Se l'input _test_ è false, non verrà eseguito alcun nodo connesso al nodo **ScopeIf**.

Di seguito è riportato il risultato dell'anteprima di sfondo di Dynamo.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Visualizzazione della geometria del modellatore in Dynamo</p></figcaption></figure>

## Idee

Ecco alcune idee su come espandere le funzionalità di questo grafico.

{% hint style="info" %}
 Aggiungere una colonna **rotation** al file Excel e utilizzarla per guidare la rotazione dei sistemi di coordinate. 
{% endhint %}

{% hint style="info" %}
 Aggiungere **offset orizzontali o verticali** al file Excel in modo che i lampioni possano deviare dalla linea caratteristica del modellatore, se necessario. 
{% endhint %}

{% hint style="info" %}
 Anziché utilizzare un file Excel con valori di progressiva, generare i valori di progressiva **direttamente in Dynamo** utilizzando una progressiva iniziale e una spaziatura tipica. 
{% endhint %}
