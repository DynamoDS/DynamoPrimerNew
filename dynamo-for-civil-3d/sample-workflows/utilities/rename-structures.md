# Ridenominazione di strutture

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

Quando si aggiungono tubi e strutture ad una rete di tubazioni, in Civil 3D viene utilizzato un modello per assegnare automaticamente i nomi. In genere, ciò è sufficiente durante il posizionamento iniziale, ma inevitabilmente i nomi dovranno cambiare in futuro man mano che il progetto si evolve. Inoltre, potrebbero essere necessari molti modelli di denominazione diversi, ad esempio nominare in modo sequenziale le strutture all'interno di un ramo a partire dalla struttura più a valle o seguire un modello di denominazione che sia allineato con lo schema di dati di un'agenzia locale. Questo esempio dimostrerà come Dynamo può essere utilizzato per definire qualsiasi tipo di strategia di denominazione che possa essere applicata in modo coerente.

## Scopo

> :dart: Rinominare le strutture della rete di tubi in ordine in base alla progressiva di un tracciato.

## Concetti chiave

> * Utilizzo delle caselle di delimitazione
> * Filtraggio dei dati mediante il nodo **List.FilterByBoolMask**
> * Ordinamento dei dati mediante il nodo **List.SortByKey**
> * Generazione e modifica di stringhe di testo

## Compatibilità delle versioni

{% hint style="success" %}
Questo grafico verrà eseguito su **Civil 3D 2020** e versioni successive.
{% endhint %}

## Set di dati

Iniziare scaricando i file di esempio riportati qui sotto, quindi aprendo il file DWG e il grafico di Dynamo.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## Soluzione

Ecco una panoramica della logica di questo grafico.

> 1. Selezionare le strutture in base al layer
> 2. Ottenere le posizioni delle strutture
> 3. Filtrare le strutture per offset, quindi ordinarle in base alla progressiva
> 4. Generare i nuovi nomi
> 5. Rinominare le strutture

Procediamo!

### Selezione delle strutture

La prima cosa da fare è selezionare tutte le strutture con cui si intende lavorare. A tale scopo, è sufficiente selezionare tutti gli oggetti su un determinato layer, ovvero le strutture di reti di tubi differenti (supponendo che condividano lo stesso layer).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>Selezione delle strutture su un determinato layer</p></figcaption></figure>

> 1. Questo nodo garantisce che non vengano recuperati accidentalmente eventuali tipi di oggetto indesiderati che potrebbero condividere lo stesso layer delle strutture.

### Recupero delle posizioni delle strutture

Ora che si hanno le strutture, occorre dove si trovano nello spazio in modo da poterle ordinare in base alla loro posizione. Per eseguire questa operazione, si utilizzerà la casella di delimitazione di ogni oggetto. La **casella di delimitazione** di un oggetto è il riquadro di dimensioni minime che contiene completamente le estensioni geometriche dell'oggetto. Calcolando il centro della casella di delimitazione, si ottiene un'approssimazione piuttosto buona del punto di inserimento della struttura.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>Utilizzo delle caselle di delimitazione per ottenere il punto di inserimento approssimativo di ogni struttura</p></figcaption></figure>

Questi punti verranno utilizzati per ottenere la progressiva e l'offset delle strutture rispetto ad un tracciato selezionato.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### Filtraggio e ordinamento

Ecco dove le cose iniziano a diventare un po' complicate. A questo punto, si ha un grande elenco di tutte le strutture sul layer specificato ed è stato scelto un tracciato in base al quale ordinarle. Il problema è che nell'elenco potrebbero essere presenti strutture che non si vuole rinominare. Ad esempio, potrebbero non far parte del tratto specifico a cui si è interessati.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. Il tracciato selezionato
> 2. Le strutture che si desidera rinominare
> 3. Le strutture da ignorare

Pertanto, è necessario filtrare l'elenco di strutture in modo da non considerare quelle che sono maggiori di un determinato offset rispetto al tracciato. Per ottenere questo risultato, è meglio utilizzare il nodo **List.FilterByBoolMask**. Dopo aver filtrato l'elenco di strutture, è possibile utilizzare il nodo **List.SortByKey** per ordinarle in base ai valori di progressiva.

{% hint style="info" %}
Se non si conoscono gli elenchi, consultare la sezione [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention").
{% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>Filtraggio e ordinamento delle strutture</p></figcaption></figure>

> 1. Verificare se l'offset della strutture è inferiore al valore di soglia.
> 2. Sostituire eventuali valori null con _false_.
> 3. Filtrare l'elenco di strutture e progressive.
> 4. Ordinare le strutture in base alle progressive

### Generazione di nuovi nomi

L'ultima operazione da eseguire è creare nuovi nomi per le strutture. Il formato che si utilizzerà è `<alignment name>-STRC-<number>`. Sono disponibili qui alcuni nodi aggiuntivi per aggiungere nuovi zeri ai numeri, se lo si desidera (ad esempio, "01" anziché "1").

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>Generazione di nuovi nomi delle strutture</p></figcaption></figure>

### Ridenominazione di strutture

Infine, rinominare le strutture.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>Impostazione dei nomi per le strutture</p></figcaption></figure>

### Risultato

Di seguito è riportato un esempio di esecuzione del grafico mediante il **Lettore Dynamo**.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Esecuzione del grafico mediante il Lettore Dynamo e visualizzazione dei risultati in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
Se non si conosce il Lettore Dynamo, consultare la sezione [dynamo-player.md](../../dynamo-player.md "mention").
{% endhint %}

> :tada: Missione compiuta!

### Attività extra: Visualizzazione in Dynamo

Può essere utile sfruttare l'anteprima di sfondo 3D di Dynamo per visualizzare gli output intermedi del grafico anziché solo il risultato finale. Una cosa semplice da fare è mostrare le caselle di delimitazione delle strutture. Inoltre, questo particolare set di dati include un modellatore nel documento, pertanto è possibile importare la geometria della linea caratteristica del modellatore in Dynamo per fornire un contesto per il punto in cui le strutture si trovano nello spazio. Se il grafico viene utilizzato in un set di dati che non dispone di modellatori, questi nodi non avranno alcuna funzione.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>Visualizzazione della geometria per strutture e linee caratteristiche del modellatore</p></figcaption></figure>

Ora è possibile capire meglio come funziona il processo di filtraggio delle strutture per offset.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Regolazione del valore della soglia di offset del tracciato e visualizzazione delle strutture interessate in Dynamo</p></figcaption></figure>

## Idee

Ecco alcune idee su come espandere le funzionalità di questo grafico.

{% hint style="info" %}
Rinominare le strutture in base al relativo **tracciato più vicino** anziché selezionare un tracciato specifico.
{% endhint %}

{% hint style="info" %}
**Rinominare i tubi** oltre alle strutture.
{% endhint %}

{% hint style="info" %}
**Impostare i layer** delle strutture in base al loro tratto.
{% endhint %}
