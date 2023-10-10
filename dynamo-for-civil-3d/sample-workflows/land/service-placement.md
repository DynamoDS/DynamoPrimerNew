# Posizionamento dei servizi

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

La progettazione ingegneristica di un tipico complesso residenziale prevede prevede la collaborazione con diversi impianti di pubblica utilità sotterranei, come le fognature, lo scarico delle acque piovane, l'acqua potabile o altri. In questo esempio si dimostrerà come Dynamo può essere utilizzato per disegnare le connessioni dei servizi da una conduttura di distribuzione ad un determinato lotto (ad esempio, una particella). È frequente che ogni lotto richieda una connessione dei servizi, il che comporta un notevole e noioso lavoro di posizionamento di tutti i servizi. Dynamo può accelerare il processo disegnando automaticamente la geometria necessaria con precisione, oltre a fornire input flessibili che possono essere adattati agli standard delle agenzie locali.

## Scopo

> :dart: Posizionare i riferimenti di blocco del contatore del servizio idrico in corrispondenza degli offset specificati da una linea del lotto e disegnare una linea per ogni connessione dei servizi perpendicolare alla conduttura di distribuzione.

## Concetti chiave

> * Utilizzo del nodo **Select Object** per l'input dell'utente
> * Utilizzo dei sistemi di coordinate
> * Utilizzo di operazioni geometriche quali **Geometry.DistanceTo** e **Geometry.ClosestPointTo**
> * Creazione di riferimenti di blocco
> * Controllo delle impostazioni di unione di oggetti

## Compatibilità delle versioni

{% hint style="success" %}\r\n Questo grafico verrà eseguito su **Civil 3D 2020** e versioni successive. \r\n{% endhint %}

## Set di dati

Iniziare scaricando i file di esempio riportati qui sotto, quindi aprendo il file DWG e il grafico di Dynamo.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Soluzione

Ecco una panoramica della logica di questo grafico.

> 1. Ottenere la geometria della curva per la conduttura di distribuzione
> 2. Ottenere la geometria della curva di una linea del lotto selezionata dall'utente, invertendola se necessario
> 3. Generare i punti di inserimento per i contatori
> 4. Ottenere i punti sulla conduttura di distribuzione più vicini alle posizioni dei contatori
> 5. Creare riferimenti di blocco e linee nello spazio modello

Procediamo!

### Recupero della geometria della conduttura di distribuzione

Il primo passaggio consiste nel caricare la geometria per la conduttura di distribuzione in Dynamo. Anziché selezionare singole linee o polilinee, si otterranno tutti gli oggetti su un determinato layer e li si unirà come PolyCurve di Dynamo.

{% hint style="info" %}\r\n Se non si conosce la geometria della curva di Dynamo, consultare la sezione [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Recupero di oggetti da Civil 3D e unione di tutti gli elementi in un'unica PolyCurve</p></figcaption></figure>

### Recupero della geometria della linea del lotto

Successivamente, è necessario caricare in Dynamo la geometria per una linea del lotto selezionata, in modo da potervi lavorare. Lo strumento giusto per il processo è il nodo **Select Object**, che consente all'utente del grafico di selezionare un oggetto specifico in Civil 3D.

Occorre anche gestire un potenziale problema che potrebbe sorgere. La linea del lotto ha un punto iniziale e un punto finale, il che significa che ha una direzione. Affinché il grafico produca risultati coerenti, è necessario che tutte le linee del lotto abbiano una direzione coerente. È possibile tenere conto di questa condizione direttamente nella logica del grafico, il che rende il grafico più resiliente. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Selezione di una linea del lotto e verifica della direzione corretta</p></figcaption></figure>

> 1. Ottenere i punti iniziale e finale della linea del lotto.
> 2. Misurare la distanza da ogni punto alla conduttura di distribuzione, quindi determinare quale distanza è maggiore.
> 3. Il risultato desiderato è che il punto iniziale della linea è più vicino alla conduttura di distribuzione. In caso contrario, si inverte la direzione della linea del lotto. Altrimenti, è sufficiente tornare alla linea del lotto originale.

### Generazione di punti di inserimento

È arrivato il momento di capire dove verranno posizionati i contatori. In genere, il posizionamento è determinato dai requisiti dell'agenzia locale, pertanto è sufficiente fornire valori di input che possono essere modificati in base alle diverse condizioni. Utilizzeremo un **sistema di coordinate** lungo la linea del lotto come riferimento per la creazione dei punti. In questo modo è molto semplice definire gli offset rispetto alla linea del lotto, indipendentemente dall'orientamento.

{% hint style="info" %}\r\n Se non si conoscono i sistemi di coordinate, consultare la sezione [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). \r\n{% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Creazione dei punti di inserimento per i contatori</p></figcaption></figure>

### Recupero dei punti di connessione

Ora è necessario individuare i punti sulla conduttura di distribuzione più vicini alle posizioni dei contatori. In questo modo, è possibile disegnare le connessioni dei servizi nello spazio modello in modo che siano sempre perpendicolari alla conduttura di distribuzione. Il nodo **Geometry.ClosestPointTo** è la soluzione perfetta.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Recupero di punti perpendicolari sulla conduttura di distribuzione</p></figcaption></figure>

> 1. Questa è la PolyCurve della conduttura di distribuzione.
> 2. Questi sono i punti di inserimento dei contatori.

### Creazione di oggetti

L'ultimo passaggio consiste nel creare effettivamente oggetti nello spazio modello. Si utilizzeranno i punti di inserimento generati in precedenza per creare i riferimenti di blocco, quindi si passerà ai punti presenti sulla conduttura di distribuzione per disegnare le linee sulle connessioni dei servizi.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Risultato

Quando si esegue il grafico, dovrebbero essere visualizzati nuovi riferimenti di blocco e nuove linee di connessione dei servizi nello spazio modello. Provare a modificare alcuni input e a controllare che tutto venga aggiornato automaticamente.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Regolazione dei parametri di input in Dynamo e visualizzazione immediata dei risultati in Civil 3D</p></figcaption></figure>

### Attività extra: Attivazione del posizionamento sequenziale

Si può notare che dopo aver posizionato gli oggetti per una linea del lotto, selezionando una diversa gli oggetti vengono "spostati".

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Funzionamento quando l'unione di oggetti è attivata</p></figcaption></figure>

Si tratta del funzionamento di default di Dynamo ed è molto utile in molti casi. Tuttavia, è possibile che si vogliano posizionare diverse connessioni dei servizi in sequenza e fare in modo che Dynamo crei nuovi oggetti ad ogni esecuzione anziché modificare quelle originali. È possibile controllare questo funzionamento modificando le impostazioni di unione di oggetti.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Impostazioni dell'unione di oggetti di Dynamo</p></figcaption></figure>

{% hint style="info" %}\r\n Per ulteriori informazioni, consultare la sezione [object-binding.md](../../advanced-topics/object-binding.md "mention"). \r\n{% endhint %}

La modifica di questa impostazione forzerà Dynamo a "dimenticare" gli oggetti che crea ad ogni esecuzione. Di seguito è riportato un esempio di esecuzione del grafico con l'unionee di oggetti disattivata utilizzando il **Lettore Dynamo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Esecuzione del grafico mediante il Lettore Dynamo e visualizzazione dei risultati in Civil 3D</p></figcaption></figure>

{% hint style="info" %}\r\n Se non si conosce il Lettore Dynamo, consultare la sezione [dynamo-player.md](../../dynamo-player.md "mention"). \r\n{% endhint %}

> :tada: Missione compiuta!

## Idee

Ecco alcune idee su come espandere le funzionalità di questo grafico.

{% hint style="info" %}\r\n Posizionare **più collegamenti dei servizi** contemporaneamente anziché selezionare ogni linea del lotto. {% endpoint %}

{% hint style="info" %}\r\n Regolare gli input per posizionare invece **sportelli di ispezione per fognature** anziché i contatori del servizio idrico. \r\n{% endhint %}

{% hint style="info" %}\r\n **Aggiungere un pulsante di commutazione** per consentire il posizionamento di una singola connessione dei servizi su un lato particolare della linea del lotto anziché su entrambi i lati. \r\n{% endhint %}
