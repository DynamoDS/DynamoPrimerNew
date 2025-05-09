# Interfaccia della riga di comando di Dynamo

`-o, -O, --OpenFilePath`: indica a Dynamo di aprire un file dei comandi ed eseguire i comandi che contiene in questo percorso. Questa opzione è supportata solo se eseguita da DynamoSandbox.  

`-c, -C, --CommandFilePath`: indica a Dynamo di aprire un file dei comandi ed eseguire i comandi che contiene in questo percorso. Questa opzione è supportata solo se eseguita da DynamoSandbox.  

`-v, -V, --Verbose`: indica a Dynamo di generare tutte le valutazioni eseguite in un file XML nel percorso specificato.  

`-g, -G, --Geometry`: indica a Dynamo di generare la geometria di tutte le valutazioni in un file JSON in questo percorso.  

`-h, -H, --help`: consente di ottenere assistenza.  

`-i, -I, --Import`: indica a Dynamo di importare un assieme come libreria di nodi. Questo argomento dovrebbe essere un percorso di un singolo file `.dll`. Se si desidera importare più file `.dll`, elencarli separati da uno spazio: `-i 'assembly1.dll' 'assembly2.dll'`.  

`--GeometryPath`: è il percorso relativo o assoluto di una directory contenente ASM. Quando fornito, invece di cercare ASM nel disco rigido, tale modulo verrà caricato direttamente da questo percorso.  

`-k, -K, --KeepAlive`: in modalità keep-alive, lascia in esecuzione il processo di Dynamo fino a quando un'estensione caricata non lo arresta.  

`--HostName`: identifica la variazione di Dynamo associata all'host.  

`-s, -S, --SessionId`: identifica l'ID sessione di analisi host di Dynamo.  

`-p, -P, --ParentId`: identifica l'ID principale di analisi host di Dynamo.  

`-x, -X, --ConvertFile`: se utilizzato in combinazione con il flag `-O`, apre un file `.dyn` dal percorso specificato e lo converte in formato `.json`. Il file avrà l'estensione `.json` e si troverà nella stessa directory del file originale.  

`-n, -N, --NoConsole`: evita di utilizzare la finestra della console per interagire con la CLI della riga di comando in modalità keep-alive.  

`-u, -U, --UserData`: specifica la cartella dei dati dell'utente che deve essere utilizzata da PathResolver con la CLI.  

`--CommonData`: specifica la cartella dei dati comuni che deve essere utilizzata da PathResolver con la CLI.  

`--DisableAnalytics`: disattiva l'analisi in Dynamo per la durata del processo.  

`--CERLocation`: specifica lo strumento di segnalazione degli errori di arresto anomalo che si trova sul disco.  

`--ServiceMode`: specifica l'avvio della modalità di servizio.  



#### Perché 
 Potrebbe essere necessario controllare Dynamo dalla riga di comando per vari motivi, tra cui: 
 
 * Automazione di molte esecuzioni di Dynamo
 * Verifica dei grafici di Dynamo (osservare anche -c quando si utilizza Dynamo Sandbox)
 * Esecuzione di una sequenza di grafici di Dynamo in un ordine specifico
 * Scrittura di file batch per l'esecuzione di più righe di comando
 * Scrittura di un altro programma per controllare e automatizzare l'esecuzione dei grafici di Dynamo e svolgere operazioni complesse con i risultati di tali calcoli

#### Cosa
L'interfaccia della riga di comando (DynamoCLI) è un supplemento di DynamoSandbox. Si tratta di un'utilità della riga di comando DOS/terminale progettata per fornire tutta la comodità degli argomenti della riga di comando per eseguire Dynamo. Nella sua prima implementazione, non viene eseguita in modo indipendente, ma deve essere eseguita dalla cartella in cui risiedono i file binari di Dynamo, poiché dipende dagli stessi file DLL di base della sandbox. Non può interagire con altre build di Dynamo.

Esistono quattro modi per eseguire la CLI: da un prompt DOS, da file batch DOS e come collegamento sul desktop di Windows il cui percorso viene modificato in modo da includere i flag della riga di comando specificati. Le specifiche di file DOS possono essere complete o relative e sono supportate anche le unità mappate e la sintassi degli URL. Può anche essere compilata con Mono ed eseguita su Linux o Mac dal terminale.

I pacchetti di Dynamo sono supportati dall'utilità, tuttavia non è possibile caricare nodi personalizzati (.dyf), ma solo grafici indipendenti (.dyn).

Nei test preliminari, l'utilità CLI supporta le versioni localizzate di Windows ed è possibile specificare argomenti filespec con caratteri ASCII superiori.

È possibile accedere alla CLI tramite l'applicazione DynamoCLI.exe. Tale applicazione consente a un utente o ad un'altra applicazione di interagire con il modello di valutazione di Dynamo richiamando DynamoCLI.exe con una stringa di comando. Questa potrebbe essere simile alla seguente:
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
Questo comando indica a Dynamo di aprire il file specificato in *"C:\\someReallyCoolDynamoFile.Dyn"*, senza disegnare alcuna interfaccia utente, quindi di eseguirlo. Dynamo verrà quindi chiuso al termine dell'esecuzione del grafico. 

**Novità nella versione 2.1**: l'applicazione DynamoWPFCLI.exe. Questa applicazione supporta tutto ciò che l'applicazione DynamoCLI.exe supporta con l'aggiunta dell'opzione Geometry (-g). L'applicazione DynamoWPFCLI.exe è solo per Windows.

#### Note importanti

* Il metodo preferito per interagire con DynamoCLI è tramite un'interfaccia della riga di comando.
* A questo punto, sarà necessario eseguire DynamoCLI dalla posizione di installazione all'interno della cartella [Dynamo Version]. La CLI richiede l'accesso agli stessi file .dll di Dynamo, pertanto non deve essere spostata.
* Dovrebbe essere possibile eseguire grafici attualmente aperti in Dynamo, ma ciò potrebbe causare effetti collaterali indesiderati.
* Tutti i percorsi dei file sono completamente conformi a DOS, pertanto i percorsi relativi e completi dovrebbero funzionare, ma assicurarsi di racchiudere i percorsi tra virgolette *"C:percorso\\del\\file.dyn"*. 
* DynamoCLI è una nuova funzionalità e attualmente in evoluzione: al momento, la **CLI carica solo un sottogruppo** di librerie di Dynamo standard. Occorre tenerne conto se un grafico non viene eseguito correttamente. Queste librerie sono specificate [qui](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) 
* Attualmente non viene fornito alcun output std se non vengono rilevati errori; la CLI verrà semplicemente chiusa al termine dell'esecuzione.
 
#### Come

`-o` consente di aprire Dynamo puntando ad un file .dyn, in una modalità headless che eseguirà il grafico.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v` è utilizzabile quando Dynamo è in esecuzione in modalità headless (quando si è utilizzato `-o` per aprire un file). Questo flag itererà tutti i nodi nel grafico ed eseguirà il dump dei relativi valori di output in un semplice file XML. Poiché il flag `--ServiceMode` può forzare Dynamo ad eseguire più valutazioni del grafico, il file di output conterrà i valori per ogni valutazione che si verifica.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
Il formato del file XML di output è il seguente:
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` è utilizzabile quando Dynamo è in esecuzione in modalità headless (quando si è utilizzato `-o` per aprire un file). Questo flag genererà il grafico e quindi eseguirà il dump della geometria risultante in un file JSON. 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
Il formato del file della geometria JSON è il seguente:

 TBD - Lavori in corso

`-h` è utilizzabile per ottenere un elenco delle opzioni possibili:

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

Il flag -i può essere utilizzato più volte per importare più assiemi che il grafico che si sta tentando di aprire richiede per essere eseguito.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

Il flag -l può essere utilizzato per eseguire Dynamo con impostazioni internazionali diverse. Tuttavia, in genere, le impostazioni internazionali non influiscono sui risultati del grafico.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

Il flag --GeometryPath può essere utilizzato per far puntare DynamoSandbox o la CLI ad un gruppo specifico di file binari ASM. Utilizzarlo come riportato di seguito:

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

o

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

Il flag -k può essere utilizzato per lasciare in esecuzione il processo di Dynamo fino a quando un'estensione caricata non lo arresta.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

Il flag --HostName può essere utilizzato per identificare la variazione di Dynamo associata all'host.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

o

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

Il flag -s può essere utilizzato per identificare l'ID sessione di analisi host di Dynamo.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

Il flag -p può essere utilizzato per identificare l'ID principale di analisi host di Dynamo.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
