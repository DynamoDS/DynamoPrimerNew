# Sagoma dello spazio libero

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

Lo sviluppo di sagome cinematiche per la convalida dello spazio libero è una parte importante della progettazione ferroviaria. Dynamo può essere utilizzato per generare solidi per la sagoma anziché creare e gestire sottoassiemi complessi di modellatori per eseguire il processo.

## Scopo

> :dart: Utilizzare un blocco del profilo del veicolo per generare solidi 3D della sagoma dello spazio libero lungo un modellatore.

## Concetti chiave

> * Utilizzo delle linee caratteristiche del modellatore
> * Trasformazione della geometria tra sistemi di coordinate
> * Creazione di solidi tramite loft
> * Controllo del funzionamento dei nodi con le impostazioni di collegamento

## Compatibilità delle versioni

{% hint style="success" %}
Questo grafico verrà eseguito su **Civil 3D 2020** e versioni successive.
{% endhint %}

## Set di dati

Iniziare scaricando i file di esempio riportati qui sotto, quindi aprendo il file DWG e il grafico di Dynamo.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## Soluzione

Ecco una panoramica della logica di questo grafico.

> 1. Ottenere le linee caratteristiche dalla linea base del modellatore specificata
> 2. Generare sistemi di coordinate lungo la linea caratteristica del modellatore con la spaziatura desiderata
> 3. Trasformare la geometria del blocco del profilo in sistemi di coordinate
> 4. Eseguire il loft di un solido tra i profili
> 5. Creare i solidi in Civil 3D

Procediamo!

### Recupero di dati sul modellatore

Il primo passaggio consiste nell'ottenere i dati sul modellatore. Selezionare il modello di modellatore in base al nome, ottenere una linea base specifica all'interno del modellatore e ottenere una linea caratteristica all'interno della linea base secondo il relativo codice punto.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>Selezione del modellatore, della linea base e della linea caratteristica</p></figcaption></figure>

### Generazione di sistemi di coordinate

Ora si procederà alla generazione di **sistemi di coordinate** lungo le linee caratteristiche del modellatore tra una progressiva iniziale e una progressiva finale. Questi sistemi di coordinate verranno utilizzati per allineare la geometria del blocco del profilo del veicolo al modellatore.

{% hint style="info" %}
Se non si conoscono i sistemi di coordinate, consultare la sezione [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention").
{% endhint %}

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>Acquisizione di sistemi di coordinate lungo le linee caratteristiche del modellatore</p></figcaption></figure>

> 1. Notare il valore **XXX** nell'angolo inferiore destro del nodo. Ciò significa che le impostazioni di collegamento del nodo sono impostate su _Globale_, operazione che è necessaria per generare i sistemi di coordinate in corrispondenza degli stessi valori di progressiva per entrambe le linee caratteristiche.

{% hint style="info" %}
Se non si conosce il collegamento di nodi, consultare la sezione [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention").
{% endhint %}

### Trasformazione della geometria del blocco

Ora è necessario creare in qualche modo una serie di profili di veicoli lungo le linee caratteristiche. Si procederà alla trasformazione della geometria dalla definizione di blocco del profilo del veicolo utilizzando il nodo **Geometry.Transform**. Questo è un concetto complesso da visualizzare, quindi prima di osservare i nodi, ecco un grafico che mostra cosa succederà.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>Una visualizzazione della trasformazione della geometria tra sistemi di coordinate</p></figcaption></figure>

Quindi essenzialmente si tratta di "prendere" la geometria di Dynamo da una _singola_ definizione di blocco e di spostarla/ruotarla, creando al contempo una serie lungo la linea caratteristica. Forte! Ecco come appare la sequenza di nodi.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. In questo modo la definizione di blocco viene ottenuta dal documento.
> 2. Questi nodi ottengono la geometria di Dynamo degli oggetti all'interno del blocco.
> 3. Questi nodi definiscono essenzialmente il sistema di coordinate _da_ cui si sta trasformando la geometria.
> 4. Infine, questo nodo esegue il lavoro effettivo di trasformazione della geometria.
> 5. Notare il collegamento _Più lungo_ su questo nodo.

Ed ecco cosa si ottiene in Dynamo.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>La geometria del blocco del profilo del veicolo dopo la trasformazione</p></figcaption></figure>

### Generazione di solidi

Buone notizie! Il lavoro duro è finito. Ora è sufficiente generare solidi tra i profili. Ciò è facilmente possibile con il nodo **Solid.ByLoft**.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

Ed ecco il risultato. Tenere presente che questi sono solidi di Dynamo, ma è comunque necessario crearli in Civil 3D.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>I solidi di Dynamo dopo il loft</p></figcaption></figure>

### Output di solidi in Civil 3D

Il passaggio finale consiste nell'eseguire l'output dei solidi generati nello spazio modello. Verrà anche applicato del colore per renderli facilmente visibili.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Output dei solidi in Civil 3D</p></figcaption></figure>

### Risultato

Di seguito è riportato un esempio di esecuzione del grafico mediante il **Lettore Dynamo**.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Esecuzione del grafico mediante il Lettore Dynamo e visualizzazione dei risultati in Civil 3D</p></figcaption></figure>

{% hint style="info" %}
Se non si conosce il Lettore Dynamo, consultare la sezione [dynamo-player.md](../../dynamo-player.md "mention").
{% endhint %}

> :tada: Missione compiuta!

## Idee

Ecco alcune idee su come espandere le funzionalità di questo grafico.

{% hint style="info" %}
Aggiungere la possibilità di utilizzare **intervalli di progressive differenti** separatamente per ogni binario.
{% endhint %}

{% hint style="info" %}
**Dividere i solidi** in segmenti più piccoli che possono essere analizzati singolarmente per ricercare eventuali interferenze.
{% endhint %}

{% hint style="info" %}
Verificare se i solidi della sagoma **si intersecano con gli oggetti** e colorano quelli che incontrano.
{% endhint %}
