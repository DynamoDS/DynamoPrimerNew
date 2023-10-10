# Gestione di gruppi di punti

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

L'utilizzo di punti COGO e gruppi di punti in Civil 3D è un elemento fondamentale di molti processi dall'inizio alla fine dei lavori. Dynamo si contraddistingue davvero quando si tratta di gestione dei dati; verrà illustrato un caso di utilizzo potenziale in questo esempio.  

## Scopo

> :dart: Creare un gruppo di punti per ogni descrizione di punto COGO univoca. 

## Concetti chiave

> * Utilizzo di elenchi
> * Raggruppamento di oggetti simili con il nodo **List.GroupByKey**
> * Visualizzazione dell'output personalizzato nel Lettore Dynamo

## Compatibilità delle versioni

{% hint style="success" %}\r\n Questo grafico verrà eseguito su **Civil 3D 2020** e versioni successive. \r\n{% endhint %}

## Set di dati

Iniziare scaricando i file di esempio riportati qui sotto, quindi aprendo il file DWG e il grafico di Dynamo.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Soluzione

Ecco una panoramica della logica di questo grafico.

> 1. Ottenere tutti i punti COGO nel documento
> 2. Raggruppare i punti COGO per descrizione
> 3. Creare gruppi di punti
> 4. Generare un riepilogo nel Lettore Dynamo

Procediamo!

### Recupero di punti COGO

Il primo passaggio consiste nell'ottenere tutti i gruppi di punti nel documento, quindi tutti i punti COGO all'interno di ciascun gruppo. In questo modo verrà fornito un _elenco nidificato_ o un "elenco di elenchi", che sarà più semplice utilizzare in un secondo momento se si riduce la nidificazione di tutto trasformandolo in un unico elenco con il nodo **List.Flatten**.

{% hint style="info" %}\r\n Se non si ha familiarità con gli elenchi, consultare la sezione [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Recupero di tutti i gruppi di punti e punti COGO </p></figcaption></figure>

### Raggruppamento di punti per descrizione

Ora che si dispone di tutti i punti COGO, occorre separarli in gruppi in base alle loro descrizioni. Questo è esattamente ciò che fa il nodo **List.GroupByKey**. In pratica raggruppa eventuali elementi che condividono la stessa chiave.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Raggruppamento dei punti COGO per descrizione</p></figcaption></figure>

### Creazione di gruppi di punti

Il lavoro duro è finito! Il passaggio finale consiste nella creazione di nuovi gruppi di punti Civil 3D dai punti COGO raggruppati.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Creazione di nuovi gruppo di punti</p></figcaption></figure>

### Generazione di un riepilogo

Quando si esegue il grafico, non c'è nulla da vedere nell'anteprima di sfondo di Dynamo perché non stiamo lavorando con la geometria. L'unico modo per vedere se il grafico viene eseguito correttamente è controllare l'Area strumenti o osservare le anteprime di output dei nodi. Tuttavia, se si esegue il grafico utilizzando il **Lettore Dynamo**, è possibile fornire ulteriori commenti sui risultati del grafico generando un riepilogo dei gruppi di punti creati. È sufficiente fare clic con il pulsante destro del mouse su un nodo e impostarlo su _È output_. In questo caso, per visualizzare i risultati viene utilizzato un nodo **Watch** rinominato.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>L'impostazione di un nodo su <em>È output</em> ne visualizzerà il contenuto nell'output del Lettore Dynamo</p></figcaption></figure>

### Risultato

Di seguito è riportato un esempio di esecuzione del grafico mediante il **Lettore Dynamo**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Esecuzione del grafico utilizzando il Lettore Dynamo e visualizzazione dei risultati nell'Area strumenti</p></figcaption></figure>

{% hint style="info" %}\r\n Se non si conosce il Lettore Dynamo, consultare la sezione [dynamo-player.md](../../dynamo-player.md "mention"). \r\n{% endhint %}

> :tada: Missione compiuta!

## Idee

Ecco alcune idee su come espandere le funzionalità di questo grafico.

{% hint style="info" %}\r\n Modificare il raggruppamento di punti in modo che si basi sulla **descrizione completa** anziché sulla descrizione non elaborata. \r\n{% endhint %}

{% hint style="info" %}\r\n Raggruppare i punti in altre **categorie predefinite** scelte (ad esempio, "Foto del suolo", "Monumenti" e così via). {% endpoint %}

{% hint style="info" %}\r\n Creare automaticamente superfici TIN per i punti in determinati gruppi. \r\n{% endhint %}
