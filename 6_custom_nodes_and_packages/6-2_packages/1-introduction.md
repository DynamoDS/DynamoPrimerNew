# Introduzione ai pacchetti

Dynamo offre numerose funzionalità predefinite e dispone anche di un'ampia libreria di pacchetti in grado di estendere in modo significativo le sue potenzialità. Un pacchetto è una raccolta di nodi personalizzati o funzionalità aggiuntive. Dynamo Package Manager è un portale che consente alla comunità di scaricare qualsiasi pacchetto pubblicato online. Questi set di strumenti sono stati sviluppati da terze parti per estendere le funzionalità principali di Dynamo, sono accessibili a tutti e sono pronti per il download con un semplice clic.

![Sito di Package Manager](../images/6-2/1/dpm.jpg)

Un progetto open source, come Dynamo, si basa su questo tipo di coinvolgimento della comunità. Grazie a sviluppatori di terze parti dedicati, Dynamo è in grado di estendere il suo ambito di applicazione ai workflow di diversi settori. Per questo motivo, il team di Dynamo ha compiuto sforzi concertati per snellire lo sviluppo e la pubblicazione dei pacchetti (che saranno discussi in modo più dettagliato nelle sezioni seguenti).

### Installazione di un pacchetto

Il modo più semplice per installare un pacchetto consiste nell'utilizzare l'opzione di menu Pacchetti nell'interfaccia di Dynamo. Adesso si può passare direttamente al pacchetto e installarne uno. In questo esempio rapido, verrà installato un comune pacchetto per la creazione di pannelli quadrangolari su una griglia.

In Dynamo, accedere a _Pacchetti > Package Manager_.

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

Sulla barra di ricerca, cercare "quads from rettangular grid". Dopo alcuni minuti, si dovrebbero vedere tutti i pacchetti che corrispondono alla query di ricerca. Si desidera selezionare il primo pacchetto con il nome corrispondente.

Fare clic su Installa per aggiungere il pacchetto alla libreria, quindi accettare la conferma. Fatto.

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

Notare che ora è presente un altro gruppo nella libreria di Dynamo denominato "buildz". Questo nome si riferisce allo sviluppatore del pacchetto e il nodo personalizzato viene posizionato in questo gruppo. È possibile iniziare ad utilizzarlo immediatamente.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

Utilizzare **Code Block** per definire rapidamente una griglia rettangolare, generare il risultato in un nodo **Polygon.ByPoints**, quindi in un nodo **Surface.ByPatch** per visualizzare l'elenco di pannelli rettangolari appena creati.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### Installazione della cartella di pacchetti - DynamoUnfold

L'esempio riportato sopra si concentra su un pacchetto con un nodo personalizzato, ma si utilizza lo stesso processo per il download di pacchetti con diversi nodi personalizzati e file di dati di supporto. Ora si dimostrerà ciò con un pacchetto più completo: DynamoUnfold.

Come nell'esempio precedente, iniziare selezionando _Pacchetti > Package Manager_.

Questa volta, si cercherà _"DynamoUnfold"_, una parola. Quando vengono visualizzati i pacchetti, scaricarli facendo clic su Installa per aggiungere DynamoUnfold alla libreria di Dynamo.

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

Nella libreria di Dynamo, è presente un gruppo _DynamoUnfold_ con più categorie e nodi personalizzati.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

Ora, si può dare un'occhiata alla struttura dei file del pacchetto.

1. Innanzitutto, accedere a Pacchetti > Package Manager > Pacchetti installati.
2. Accanto a DynamoUnfold, selezionare il menu delle opzioni <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">.
3. Quindi, fare clic su Mostra directory principale per aprire la cartella principale per questo pacchetto.

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

Verrà aperta la directory principale del pacchetto. Notare che sono presenti 3 cartelle e un file.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. La cartella _bin_ contiene i file .dll. Questo pacchetto di Dynamo è stato sviluppato utilizzando la funzionalità zero-touch, pertanto i nodi personalizzati sono contenuti in questa cartella.
> 2. La cartella _dyf_ contiene i nodi personalizzati. Questo pacchetto non è stato sviluppato utilizzando nodi personalizzati di Dynamo, pertanto questa cartella è vuota per questo pacchetto.
> 3. La cartella extra contiene tutti i file aggiuntivi, inclusi i file di esempio.
> 4. Il file pkg è un file di testo di base che definisce le impostazioni del pacchetto. Lo si può ignorare per adesso.

Aprendo la cartella "extra", si notano alcuni file di esempio scaricati con l'installazione. Non tutti i pacchetti dispongono di file di esempio, ma è possibile trovarli se fanno parte di un pacchetto.

Aprire "SphereUnfold".

![](../images/6-2/1/rd2.jpg)

Dopo aver aperto il file e aver fatto clic su "Esegui" nel risolutore, è presente una sfera spiegata. File di esempio come questi sono utili per imparare ad utilizzare un nuovo pacchetto di Dynamo.

![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### Consultazione e visualizzazione delle informazioni sui pacchetti

In Package Manager, è possibile cercare i pacchetti utilizzando le opzioni di ordinamento e filtraggio disponibili nella scheda Cerca pacchetti. Sono disponibili diversi filtri per il programma host, lo stato (nuovo, obsoleto o non obsoleto) e la presenza o meno di dipendenze nel pacchetto.

Ordinando i pacchetti, è possibile identificare i pacchetti più votati o più scaricati oppure trovare i pacchetti con aggiornamenti recenti.

È inoltre possibile accedere a ulteriori dettagli su ciascun pacchetto facendo clic su Visualizza dettagli. Viene aperto un pannello laterale in Package Manager, in cui è possibile trovare informazioni quali il confronto delle versioni e le dipendenze, l'URL del sito Web o del repository, le informazioni sulla licenza e così via.

### Sito Web di Dynamo Package Manager

Un altro modo per scoprire i pacchetti di Dynamo consiste nell'esplorare il sito Web di [Dynamo Package Manager](http://dynamopackages.com). Qui è possibile trovare le dipendenze dei pacchetti e le informazioni sulla compatibilità tra host e versioni fornite dagli autori dei pacchetti. È inoltre possibile scaricare i file di pacchetto da Dynamo Package Manager, ma eseguire questa operazione direttamente da Dynamo è un processo più agevole.

![](../images/6-2/1/dpm2.jpg)

### Dove vengono memorizzati i file di pacchetto in locale?

Se si desidera vedere dove vengono mantenuti i file di pacchetto, nella parte superiore del browser fare clic su Dynamo > Preferenze > Impostazioni pacchetto > Posizioni di file di pacchetti e nodi; da qui è possibile trovare la directory della cartella principale corrente.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

Per default, i pacchetti vengono installati in una posizione simile a questo percorso delle cartelle: _C:/Utenti/[nome utente]/AppData/Roaming/Dynamo/[versione di Dynamo]_.

### Impostazione di una posizione condivisa per i pacchetti in un ufficio

Per gli utenti che chiedono se sia possibile distribuire Dynamo (in qualsiasi forma) con pacchetti preassociati: l'approccio che risolverà questo problema e consentirà il controllo in una posizione centrale per tutti gli utenti con installazioni di Dynamo è quello di aggiungere un percorso di pacchetti personalizzato a ogni installazione.

**Aggiungere una cartella di rete in una posizione che consenta al responsabile BIM o ad altri utenti di supervisionare l'archiviazione di pacchetti approvati dall'ufficio**

Nell'interfaccia utente di una singola applicazione, accedere a _Dynamo -> Preferenze -> Impostazioni pacchetto -> Posizioni di file di pacchetti e nodi_. Nella finestra di dialogo, premere il pulsante Aggiungi percorso e cercare nel percorso di rete la risorsa condivisa del pacchetto.

Come processo automatico, si tratta di aggiungere informazioni al file di configurazione installato con Dynamo:\
 `C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]\DynamoSettings.xml`

Per default, la configurazione di Dynamo for Revit è:

`<CustomPackageFolders>`

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`

`</CustomPackageFolders>`

L'aggiunta di una posizione personalizzata sarebbe simile alla seguente:

`<CustomPackageFolders>`

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`

`<string>N:\OfficeFiles\Dynamo\Packages_Limited</string>`

`</CustomPackageFolders>`

La gestione centrale di questa cartella può anche essere controllata semplicemente impostando la cartella di sola lettura.

### Caricamento di pacchetti con file binari da una posizione di rete

#### Situazione di design

Un'azienda potrebbe voler standardizzare i pacchetti installati da workstation e utenti diversi. Un modo per farlo potrebbe essere quello di installare questi pacchetti da _Dynamo -> Preferenze -> Impostazioni pacchetto -> Posizioni di file di pacchetti e nodi_, selezionando una cartella di rete come posizione di installazione e facendo in modo che le workstation aggiungano tale percorso a `Manage Node and Package Paths`.

#### Problema

Sebbene lo scenario funzioni correttamente per i pacchetti che contengono solo nodi personalizzati, potrebbe non funzionare per i pacchetti contenenti file binari, come i nodi zero-touch. Questo problema è causato dalle [misure di sicurezza](https://stackoverflow.com/questions/5328274/load-assembly-from-network-location) che .NET Framework adotta per il caricamento degli assembly quando provengono da una posizione di rete. Sfortunatamente, l'utilizzo dell'elemento di configurazione `loadFromRemoteSources`, come suggerito nel thread collegato, non è una soluzione possibile per Dynamo, perché è distribuito come un componente piuttosto che come un'applicazione.

#### Soluzione

Una possibile soluzione consiste nell'utilizzare un'unità di rete mappata che punta al percorso di rete e fare in modo che le workstation facciano riferimento a tale percorso. I passaggi per creare un'unità di rete mappata sono descritti [qui](https://support.microsoft.com/it-it/help/4026635/windows-10-map-a-network-drive).

### Ulteriori informazioni sui pacchetti

La comunità di Dynamo è in costante crescita e in continua evoluzione. Esplorando di tanto in tanto Dynamo Package Manager, si noteranno alcuni nuovi e interessanti miglioramenti. Nelle seguenti sezioni, verranno esaminati in modo più approfondito i pacchetti, dalla prospettiva dell'utente finale fino al programmatore del pacchetto di Dynamo.
