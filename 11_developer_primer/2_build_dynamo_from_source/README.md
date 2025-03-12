# Compilazione di Dynamo dalla sorgente

La sorgente di Dynamo è ospitata su GitHub per consentire a chiunque di clonare e apportare contributi. In questo capitolo verrà illustrato come clonare il repository utilizzando git, compilare i file di origine con Visual Studio, eseguire una build locale e sottoporla a debug e infine effettuare eventuali nuove modifiche da GitHub.

### Individuazione dei repository di Dynamo su GitHub <a href="#locating-the-dynamo-repositories-on-github" id="locating-the-dynamo-repositories-on-github"></a>

GitHub è un servizio di hosting basato su [git](https://help.github.com/articles/git-and-github-learning-resources/), un sistema di controllo delle versioni per tenere traccia delle modifiche e coordinare il lavoro tra le persone. git è uno strumento che possiamo sfruttare per scaricare i file di origine di Dynamo e mantenerli aggiornati con alcuni comandi. Utilizzando questo metodo si evita il lavoro superfluo e inevitabilmente complicato di dover scaricare e sostituire manualmente i file di origine ad ogni aggiornamento. Il sistema di controllo delle versioni di git consente di rilevare le differenze tra un repository di codici locale e remoto.

La sorgente di Dynamo è ospitata nella pagina di DynamoDS su GitHub in questo repository: [https://github.com/DynamoDS/Dynamo](https://github.com/DynamoDS/Dynamo)

![I file di origine di Dynamo](images/github.jpg)
> I file di origine di Dynamo.
>
> 1. Clonare o scaricare l'intero repository.
> 2. Visualizza altri repository di DynamoDS.
> 3. File di origine di Dynamo.
> 4. File specifici di git.

### Pull del repository di Dynamo utilizzando git <a href="#pulling-the-dynamo-repository-using-git" id="pulling-the-dynamo-repository-using-git"></a>

Prima di poter clonare il repository, è necessario installare git. Seguire questa [breve guida](https://help.github.com/articles/set-up-git/#setting-up-git) per la procedura di installazione e per impostare un nome utente e un indirizzo e-mail di GitHub. In questo esempio, si utilizzerà git nella riga di comando. Questa guida presuppone che si utilizzi Windows, ma è possibile utilizzare git anche su Mac o Linux per clonare i file di origine di Dynamo.

È necessario un URL dal quale clonare il repository di Dynamo. Questo è disponibile nel pulsante Clone o Download nella pagina del repository. Copiare l'URL da incollare nella riga di comando.

![Clonazione di un repository](images/github-clone.png)
> 1. Selezionare Clone o Download.
> 2. Copiare l'URL.

Con git installato, è possibile clonare il repository di Dynamo. Iniziare aprendo la riga di comando. Quindi, utilizzare il comando di cambio della directory `cd` per individuare la cartella in cui si desidera clonare i file di origine. In questo caso, è stata creata una cartella denominata `Github` in `Documents`.

`cd C:\Users\username\Documents\GitHub`

> Sostituire "username" con il nome utente.

![Riga di comando](images/cli-1.jpg)

Nel passaggio successivo verrà eseguito un comando di git per clonare il repository di Dynamo nella posizione specificata. L'URL del comando si ottiene facendo clic sul pulsante Clone o Download su GitHub. Eseguire questo comando nel terminale di comando. Si noti che questa operazione consente di clonare il ramo principale del repository di Dynamo, che è il codice più aggiornato per Dynamo e che conterrà la versione più recente del codice di Dynamo. Questo ramo cambia ogni giorno.

`git clone https://github.com/DynamoDS/Dynamo.git`

![Risultati dell'operazione di clonazione di git](images/cli-2.jpg)

Sappiamo che git funziona se l'operazione di clonazione è stata completata correttamente. In Esplora file, individuare la directory in cui è stata eseguita la clonazione per vedere i file di origine. La struttura della directory dovrebbe essere identica al ramo principale del repository di Dynamo su GitHub.

![File di origine di Dynamo.](images/source-files.jpg)

> 1. File di origine di Dynamo.
> 2. File di git

### Compilazione del repository mediante Visual Studio <a href="#building-the-repository-using-visual-studio" id="building-the-repository-using-visual-studio"></a>

Con i file di origine ora clonati nel computer locale, è possibile creare un file eseguibile per Dynamo. A tale scopo, è necessario configurare l'IDE Visual Studio e verificare che siano installati .NET Framework e DirectX.

* Scaricare e installare [Microsoft Visual Studio Community 2015](https://my.visualstudio.com/Downloads/Results), un IDE (ambiente di sviluppo integrato) gratuito e completo (potrebbero funzionare anche le versioni successive).
* Scaricare e installare [Microsoft .NET Framework 4.5](https://www.microsoft.com/en-us/download/details.aspx?id=30653) o versione successiva.
* Installazione di Microsoft DirectX dal repository locale di Dynamo (`Dynamo\tools\install\Extra\DirectX\DXSETUP.exe`)

> È possibile che .NET e DirectX siano già installati.

Al termine dell'installazione, è possibile avviare Visual Studio e aprire la soluzione `Dynamo.All.sln` situata in `Dynamo\src`.

![Apertura del file della soluzione](images/vs-open-dynamo.jpg)

> 1. Selezionare `File > Apri > Progetto/Soluzione`.
> 2. Individuare il repository di Dynamo e aprire la cartella `src`.
> 3. Selezionare il file della soluzione `Dynamo.All.sln`.
> 4. Selezionare `Apri`.

Prima di poter creare la soluzione, è necessario specificare alcune impostazioni. È necessario innanzitutto creare una versione di debug di Dynamo in modo che Visual Studio possa raccogliere più informazioni durante il debug per facilitare lo sviluppo e si desidera dedicarsi a qualsiasi CPU.

![Impostazioni della soluzione](images/vs-dynamo-build-settings.jpg)

> Queste diventeranno cartelle all'interno della cartella `bin`.
>
> 1. In questo esempio è stata scelta `Debug` come configurazione della soluzione.
> 2. Impostare la piattaforma della soluzione su `Any CPU`.

Con il progetto aperto, è possibile creare la soluzione. Questo processo creerà un file DynamoSandbox.exe che è possibile eseguire.

![Creazione della soluzione](images/vs-build-dynamo.jpg)

> La compilazione del progetto ripristinerà le dipendenze NuGet.
>
> 1. Selezionare `Compilazione > Compila soluzione`.
> 2. Verificare che la build sia stata eseguita correttamente nella finestra Output. Il risultato dovrebbe essere simile a `==== Build: 69 succeeded, 0 failed, 0 up-to-date, 0 skipped ====`.

### Esecuzione di una build locale <a href="#running-a-local-build" id="running-a-local-build"></a>

Se Dynamo viene compilato correttamente, nel repository di Dynamo verrà creata una cartella `bin` con il file DynamoSandbox.exe. In questo caso, si sta eseguendo la compilazione con l'opzione Debug, in modo che il file eseguibile si trovi in `bin\AnyCPU\Debug`. L'esecuzione di questa operazione consente di aprire una build locale di Dynamo.

![File eseguibile di DynamoSandbox](images/ex-dynamosandbox.jpg)

> 1. Il file eseguibile di DynamoSandbox appena creato. Eseguire questo file per avviare Dynamo.

Ora siamo quasi completamente pronti per iniziare lo sviluppo per Dynamo.

Per istruzioni sulla compilazione di Dynamo per altre piattaforme (ad esempio, Linux o OS X), visitare questa [pagina Wiki](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-on-Linux,-Mac).

### Debug di una build locale mediante Visual Studio <a href="#debugging-a-local-build-using-visual-studio" id="debugging-a-local-build-using-visual-studio"></a>

Il debug è un processo di identificazione, isolamento e correzione di un bug o di un problema. Una volta che Dynamo è stato creato correttamente dalla sorgente, è possibile utilizzare diversi strumenti in Visual Studio per eseguire il debug di un'applicazione in esecuzione, ad esempio il modulo aggiuntivo DynamoRevit. È possibile analizzare il codice sorgente per individuare la causa di un problema o controllare il codice attualmente in esecuzione. Per una spiegazione più dettagliata su come eseguire il debug e spostarsi all'interno del codice in Visual Studio, consultare i [documenti su Visual Studio](https://docs.microsoft.com/en-us/visualstudio/debugger/navigating-through-code-with-the-debugger).

Per l'applicazione Dynamo indipendente, DynamoSandbox, verranno descritte due opzioni per il debug:

* Compilazione e avvio di Dynamo direttamente da Visual Studio
* Associazione di Visual Studio ad un processo in esecuzione di Dynamo.

Se si avvia Dynamo da Visual Studio, viene ricreata la soluzione per ogni sessione di debug, se necessario, pertanto se si apportano modifiche alla sorgente, queste verranno incorporate durante il debug. Con la soluzione `Dynamo.All.sln` ancora aperta, selezionare `Debug`, `AnyCPU` e `DynamoSandbox` dai menu a discesa, quindi fare clic su `Start`. In questo modo verrà compilato Dynamo e verrà avviato un nuovo processo (DynamoSandbox.exe), al quale viene associato il debugger di Visual Studio.

![Compilazione e avvio dell'applicazione da Visual Studio](images/vs-debug-options.jpg)

> Compilazione e avvio dell'applicazione direttamente da Visual Studio
>
> 1. Impostare la configurazione su `Debug`.
> 2. Impostare la piattaforma su `Any CPU`.
> 3. Impostare il progetto di avvio su `DynamoSandbox`.
> 4. Fare clic su `Start` per avviare il processo di debug.

In alternativa, è possibile eseguire il debug di un processo di Dynamo che è già in esecuzione per risolvere un problema con un determinato pacchetto o grafico aperto. A questo scopo, si aprono i file di origine del progetto in Visual Studio e si associano ad un processo di Dynamo in esecuzione utilizzando la voce di menu di debug `Attach to Process`.

![Finestra di dialogo Attach to Process](images/vs-attach-dynamosandbox.jpg)

> Associare un processo in esecuzione a Visual Studio.
>
> 1. Selezionare `Debug > Connetti a processo`.
> 2. Scegliere `DynamoSandbox.exe`.
> 3. Selezionare `Connetti`.

In entrambe le situazioni, il debugger viene associato ad un processo di cui si desidera eseguire il debug. È possibile impostare punti di interruzione nel codice prima o dopo l'avvio del debugger, in modo che il processo venga messo in pausa immediatamente prima di eseguire quella riga di codice. Se durante il debug viene generata un'eccezione non rilevata, Visual Studio passerà alla posizione in cui si è verificata nel codice sorgente. Si tratta di un metodo efficiente per individuare semplici arresti anomali, eccezioni non gestite e per comprendere il flusso di esecuzione di un'applicazione.

![Impostazione di un punto di interruzione](images/vs-debug-dynamocore.jpg)

> Durante il debug di DynamoSandbox, viene impostato un punto di interruzione nel costruttore del nodo Color.ByARGB che fa sì che il processo di Dynamo si metta in pausa quando viene creata l'istanza del nodo. Se questo nodo generava un'eccezione o causava l'arresto anomalo di Dynamo, si poteva eseguire ogni riga del costruttore per individuare il punto in cui si verificava il problema.
>
> 1. Il punto di interruzione
> 2. Lo stack di chiamate che mostra la funzione attualmente in esecuzione e le chiamate di funzione precedenti.

Nella sezione successiva, **Compilazione di DynamoRevit dalla sorgente**, verrà illustrato un esempio specifico di debug e verrà descritto come impostare punti di interruzione, eseguire il codice una riga alla volta e leggere lo stack di chiamate.

### Pull della build più recente <a href="#pulling-latest-build" id="pulling-latest-build"></a>

Poiché la sorgente di Dynamo è ospitata su GitHub, il modo più semplice per mantenere aggiornati i file di origine locali è quello di eseguire il pull delle modifiche utilizzando i comandi di git.

Utilizzando la riga di comando, impostare la directory corrente sul repository di Dynamo:

`cd C:\Users\username\Documents\GitHub\Dynamo`

> Sostituire `"username"` con il nome utente.

Utilizzare il seguente comando per eseguire il pull delle modifiche più recenti:

`git pull origin master`

![Repository locale aggiornato](images/cli-pull-changes.jpg)

> 1. Qui è possibile vedere che il repository locale è stato aggiornato con le modifiche da quello remoto.

Oltre al pull degli aggiornamenti, ci sono altri quattro workflow di git da conoscere.

* Eseguire il **fork** del repository di Dynamo per creare una copia separata dall'originale. Eventuali modifiche apportate qui non influiranno sul repository originale e gli aggiornamenti possono essere recuperati da o inviati con richieste pull. Il fork non è un comando di git, ma è un workflow che GitHub aggiunge: fork, il modello della richiesta pull, è uno dei workflow più comuni per contribuire ai progetti open source in linea. Vale la pena di imparare se si vuole contribuire a Dynamo.
* **Ramo**: si lavora a esperimenti o a nuove funzionalità isolate da altri lavori nei rami. In questo modo, è più semplice inviare richieste pull.
* Eseguire spesso i **commit** dopo aver completato un'unità di lavoro e dopo una modifica che potrebbe essere utile annullare. Un commit registra le modifiche apportate al repository e sarà visibile quando si esegue una richiesta pull nel repository di Dynamo principale.
* Creare **richieste pull** quando le modifiche sono pronte per essere ufficialmente proposte al repository di Dynamo principale.

Il team di Dynamo dispone di istruzioni specifiche per la creazione di richieste pull. Per informazioni più dettagliate sugli argomenti da trattare, fare riferimento alla sezione Richieste pull.

Vedere questa [pagina della documentazione](https://git-scm.com/docs) per un elenco di riferimento dei comandi di git.
