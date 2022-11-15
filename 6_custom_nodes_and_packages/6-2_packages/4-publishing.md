# Pubblicazione di un pacchetto

Nelle sezioni precedenti, sono stati analizzati i dettagli su come il pacchetto _MapToSurface_ è configurato con nodi personalizzati e file di esempio. Ma com'è possibile pubblicare un pacchetto che è stato sviluppato localmente? Questo case study mostra come pubblicare un pacchetto da un gruppo di file in una cartella locale.

![](../images/6-2/4/publishapackage-customnodes01.jpg)

Esistono diversi modi per pubblicare un pacchetto. Di seguito è riportato il processo consigliato: **pubblicare localmente, sviluppare localmente, quindi pubblicare in linea**. Si inizierà con una cartella contenente tutti i file del pacchetto.

### Disinstallazione di un pacchetto

Prima di passare alla pubblicazione del pacchetto MapToSurface, se il pacchetto è stato installato dalla lezione precedente, disinstallarlo in modo da non utilizzare pacchetti identici.

Iniziare accedendo a Dynamo > Preferenze > Package Manager > accanto a MapToSurface, fare clic sul menu con i puntini verticali > Elimina.

![](../images/6-2/4/publishapackage-deletepackage.jpg)

Quindi riavviare Dynamo. Alla riapertura, quando si seleziona la finestra _Gestisci pacchetti_, _MapToSurface_ non dovrebbe più essere presente. Ora si è pronti per cominciare dall'inizio.

### Pubblicazione locale di un pacchetto

{% hint style="warning" %} La pubblicazione del pacchetto di Dynamo è abilitata solo in Dynamo for Revit e Dynamo for Civil 3D. Dynamo Sandbox non dispone di funzionalità di pubblicazione.

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/6-2/4/MapToSurface.zip" %}

Questo è il primo invio per il pacchetto e tutti i file di esempio e i nodi personalizzati sono stati inseriti in una cartella. Una volta preparata questa cartella, è possibile caricarla in Dynamo Package Manager.

![](../images/6-2/4/publishapackage-publishlocally01.jpg)

> 1. Questa cartella contiene cinque nodi personalizzati (.dyf).
> 2. Contiene inoltre cinque file di esempio (.dyn) e un file vettoriale importato (.svg). Questi file fungeranno da esercizi introduttivi per mostrare all'utente come utilizzare i nodi personalizzati.

In Dynamo, iniziare facendo clic su _Pacchetti > Pubblica nuovo pacchetto_.

![](../images/6-2/4/publishapackage-publishlocally02.jpg)

Nella finestra _Pubblica un pacchetto di Dynamo_, sono stati compilati i moduli pertinenti a sinistra della finestra.

![](../images/6-2/4/publishapackage-publishlocally03.jpg)

> 1. Facendo clic su _Aggiungi file_, sono stati aggiunti anche i file della struttura delle cartelle sul lato destro dello schermo (per aggiungere i file diversi dal formato .dyf, assicurarsi di modificare il tipo di file nella finestra del browser in **Tutti i file (**_**.**_**)**. Notare che è stato aggiunto indiscriminatamente ogni file, il nodo personalizzato (.dyf) o il file di esempio (.dyn). Dynamo consentirà di suddividere questi elementi quando si pubblica il pacchetto.
> 2. Il campo "Gruppo" definisce in quale gruppo trovare i nodi personalizzati nell'interfaccia utente di Dynamo.
> 3. Pubblicare facendo clic su Pubblica localmente. Se si sta seguendo questa procedura, assicurarsi di fare clic su _Pubblica localmente_ e **non** _Pubblica in linea_. Non si desidera creare un gruppo di pacchetti duplicati in Package Manager.

Dopo la pubblicazione, i nodi personalizzati devono essere disponibili nel gruppo "DynamoPrimer" o nella libreria di Dynamo.

![](../images/6-2/4/publishapackage-publishlocally04.jpg)

Ora osservare la directory principale per vedere in che modo Dynamo ha formattato il pacchetto appena creato. A tale scopo, fare clic su Dynamo > Preferenze > Package Manager > accanto a MapToSurface, fare clic sul menu con i puntini verticali > selezionare Mostra directory principale.

![](../images/6-2/4/publishapackage-publishlocally05.jpg)

Si noti che la directory principale si trova nella posizione locale del pacchetto (tenere presente che il pacchetto è stato pubblicato "localmente"). Dynamo fa attualmente riferimento a questa cartella per la lettura di nodi personalizzati. È pertanto importante pubblicare la directory localmente in una posizione di cartella permanente (ad esempio, non sul desktop). Di seguito è riportata la suddivisione delle cartelle del pacchetto di Dynamo.

![](../images/6-2/4/publishapackage-publishlocally06.jpg)

> 1. La cartella _bin_ contiene i file .dll creati con le librerie C# o zero-touch. Non sono disponibili per questo pacchetto, pertanto questa cartella è vuota per questo esempio.
> 2. La cartella _dyf_ contiene i nodi personalizzati. Se si apre questa finestra, verranno visualizzati tutti i nodi personalizzati (file .dyf) per questo pacchetto.
> 3. La cartella extra contiene tutti i file aggiuntivi. Questi file probabilmente saranno file di Dynamo (.dyn) o eventuali file aggiuntivi richiesti (.svg, .xls, .jpeg, .sat, ecc.).
> 4. Il file pkg è un file di testo di base che definisce le impostazioni del pacchetto. Questa opzione è automatica in Dynamo, ma può essere modificata se si desidera accedere ai dettagli.

### Pubblicazione di un pacchetto in linea

{% hint style="warning" %} Nota Seguire questa procedura solo se si sta effettivamente pubblicando un pacchetto personalizzato. {% endhint %}

![](../images/6-2/4/publishapackage-publishonline01.jpg)

> 1. Quando si è pronti per la pubblicazione, nella finestra Preferenze > Package Manager, selezionare il pulsante a destra di MapToSurface e scegliere _Pubblica_.
> 2. Se si sta aggiornando un pacchetto già pubblicato, scegliere Pubblica versione. Dynamo aggiornerà il pacchetto in linea in base ai nuovi file nella directory principale del pacchetto. È semplicissimo.

### Pubblica versione...

Quando si aggiornano i file nella cartella principale del pacchetto pubblicato, è possibile pubblicare una nuova versione del pacchetto selezionando _Pubblica versione..._ nella finestra _Gestisci pacchetti_. Questo è un modo semplice per apportare gli aggiornamenti necessari al contenuto e condividerli con la comunità. _Pubblica versione_ funziona solo se si è il gestore del pacchetto.
