# Integrazione per Dynamo

È stata trovata la documentazione di integrazione per il linguaggio di programmazione visiva Dynamo.

In questa guida verranno illustrati vari aspetti di come ospitare Dynamo nell'applicazione in uso per consentire agli utenti di interagire con quest'ultima utilizzando la programmazione visiva.

Sommario:
* [Questa introduzione](#dynamo-integration): panoramica generale di ciò che questa guida include e di ciò che Dynamo rappresenta.
* [Punto di ingresso personalizzato di Dynamo](#dynamo-custom-entry-point): come creare DynamoModel e da dove iniziare.
* [Binding di elementi e traccia](#-element-binding-and-trace): utilizzo del meccanismo di traccia di Dynamo per unire i nodi nel grafico ai relativi risultati nell'host.
* [Nodi Selection di Dynamo for Revit](#-dynamo-revit-selection-nodes): come implementare nodi che consentono agli utenti di selezionare oggetti o dati dall'host e passarli come input al grafico di Dynamo. 
* [Panoramica dei pacchetti incorporati di Dynamo](#dynamo-built-in-packages-overview): cos'è la libreria di standard di Dynamo e come utilizzare il meccanismo sottostante per inviare i pacchetti con l'integrazione.


##### Un po' di terminologia:
In questi documenti verranno utilizzati i termini script, grafico e programma di Dynamo in modo intercambiabile per fare riferimento al codice creato dagli utenti in Dynamo.

## Punto di ingresso personalizzato di Dynamo
#### Esempio di Dynamo for Revit 

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

`DynamoModel` è il punto di ingresso per un'applicazione che ospita Dynamo; rappresenta un'applicazione di Dynamo. Il modello è l'oggetto principale di livello superiore che contiene riferimenti alle altre strutture di dati e agli oggetti importanti che costituiscono l'applicazione Dynamo e la macchina virtuale DesignScript. 

Un oggetto di configurazione viene utilizzato per impostare parametri comuni in `DynamoModel` quando viene costruito. 

Gli esempi contenuti in questo documento derivano dall'implementazione di DynamoRevit, ovvero un'integrazione in cui Revit ospita `DynamoModel` come modulo aggiuntivo (architettura di plug-in per Revit). Quando viene caricato questo modulo aggiuntivo, viene avviato `DynamoModel` che viene quindi visualizzato all'utente con `DynamoView` e `DynamoViewModel`. 

Dynamo è un progetto .NET in C# e, per utilizzarlo come processo nell'applicazione, è necessario essere in grado di ospitare ed eseguire il codice .NET.

DynamoCore è un motore di calcolo multipiattaforma e una raccolta di modelli di base, che possono essere compilati con .NET o Mono (in futuro .NET Core). Tuttavia, DynamoCoreWPF contiene i componenti dell'interfaccia utente di Dynamo solo per Windows e non verrà compilato su altre piattaforme.

### Passaggi per personalizzare il punto di ingresso di Dynamo 

Per inizializzare `DynamoModel`, gli integratori dovranno eseguire questi passaggi partendo da qualche punto del codice dell'host.

### Precaricare dall'host i file DLL di Dynamo condivisi.  

Attualmente l'elenco in Dynamo for Revit include solo `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.` Questa operazione viene eseguita per evitare conflitti di versioni della libreria tra Dynamo e Revit. Esempio: quando si verificano conflitti in `AvalonEdit`, la funzione del blocco di codice può essere completamente interrotta. Il problema è riportato in Dynamo 1.1.x alla paginahttps://github.com/DynamoDS/Dynamo/issues/7130 ed è riproducibile anche manualmente. Se gli integratori rilevano conflitti di libreria tra la funzione host e Dynamo, è consigliabile eseguire questa operazione come primo passaggio. Ciò è talvolta necessario per impedire ad altri plug-in o all'applicazione host stessa di caricare una versione incompatibile di una dipendenza condivisa. Una soluzione migliore consiste nel risolvere il conflitto di versioni allineando la versione o utilizzando un reindirizzamento del binding .NET nel file app.config dell'host, se possibile. 
 

### Caricamento di ASM 

#### Cosa sono ASM e LibG

ASM è la libreria della geometria di Autodesk su cui si basa Dynamo.

LibG è un wrapper intuitivo per.NET intorno al kernel di geometria ASM. LibG condivide il suo schema di controllo delle versioni con ASM: utilizza lo stesso numero di versione principale e secondaria di ASM per indicare che è il wrapper corrispondente di una particolare versione di ASM. Quando viene specificata una versione di ASM, la versione di LibG corrispondente dovrebbe essere la stessa. Nella maggior parte dei casi, LibG dovrebbe funzionare con tutte le versioni di ASM di una particolare versione principale. Ad esempio, LibG 223 dovrebbe essere in grado di caricare qualsiasi versione di ASM 223.


#### Caricamento di ASM in Dynamo Sandbox 

Dynamo Sandbox è progettato per funzionare con più versioni di ASM. A tale scopo, più versioni di LibG vengono raggruppate e fornite con le funzionalità principali. In Dynamo Shape Manager è disponibile una funzionalità incorporata per cercare i prodotti Autodesk forniti con ASM, in modo che Dynamo possa caricare ASM da questi prodotti e far funzionare i nodi della geometria senza che vengano caricati esplicitamente in un'applicazione host. Attualmente l'elenco dei prodotti è il seguente: 
```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```
Dynamo eseguirà una ricerca nel Registro di sistema di Windows per trovare se i prodotti Autodesk presenti in questo elenco sono installati nel computer dell'utente. Se uno di questi è installato, cercherà i file binari ASM, otterrà la versione e cercherà una versione di LibG corrispondente in Dynamo.  

Data la versione di ASM, la seguente API ShapeManager selezionerà la posizione del precaricatore di LibG corrispondente da caricare. Se c'è una corrispondenza esatta di versione, tale posizione verrà utilizzata, altrimenti verrà caricata la versione di LibG più vicina a quella riportata qui sotto, ma con la stessa versione principale.  

Esempio: se Dynamo è integrato con una build per sviluppatori di Revit in cui è presente una build 225.3.0 di ASM più recente, Dynamo tenterà di utilizzare LibG 225.3.0, se presente, altrimenti tenterà di utilizzare la versione principale più vicina e precedente rispetto alla prima scelta, ossia 225.0.0. 

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)` 

#### Caricamento di ASM dall'host con integrazione in-process di Dynamo 

Revit è la prima voce nell'elenco di ricerca dei prodotti ASM, il che significa che per default `DynamoSandbox.exe` tenterà di caricare ASM da Revit per primo. Occorre comunque assicurarsi che la sessione di lavoro Dynamo for Revit integrata carichi ASM dall'host di Revit corrente: ad esempio, se l'utente dispone sia di R2018 che di R2020 sul computer, quando si avvia Dynamo for Revit da R2020, Dynamo for Revit deve utilizzare ASM 225 da R2020 anziché ASM 223 da R2018. Per forzare il caricamento della versione specificata, gli integratori dovranno implementare chiamate simili a quelle indicate di seguito. 

```
internal static Version PreloadAsmFromRevit() 

{ 

     var asmLocation = AppDomain.CurrentDomain.BaseDirectory; 
     Version libGVersion = findRevitASMVersion(asmLocation); 
     var dynCorePath = DynamoRevitApp.DynamoCorePath; 
     var preloaderLocation = DynamoShapeManager.Utilities.GetLibGPreloaderLocation(libGVersion, dynCorePath); 
     Version preLoadLibGVersion = PreloadLibGVersion(preloaderLocation); 
     DynamoShapeManager.Utilities.PreloadAsmFromPath(preloaderLocation, asmLocation); 
     return preLoadLibGVersion; 

} 
```

#### Caricamento di ASM in Dynamo da un percorso personalizzato 

Recentemente è stata aggiunta la possibilità per `DynamoSandbox.exe` e `DynamoCLI.exe` di caricare una determinata versione di ASM. Per ignorare il normale comportamento di ricerca nel Registro di sistema, è possibile utilizzare il flag `�gp` per forzare Dynamo a caricare ASM da un determinato percorso. 

`DynamoSandbox.exe -gp �somePath/To/ASMDirectory/� `

  



### Creazione di StartConfiguration 

StartupConfiguration serve da parametro per l'inizializzazione di DynamoModel, il che indica che contiene quasi tutte le definizioni per come si desidera personalizzare le impostazioni della sessione di Dynamo. A seconda di come vengono impostate le seguenti proprietà, l'integrazione di Dynamo può variare tra i diversi integratori. Ad esempio, integratori differenti potrebbero impostare differenti percorsi di modelli Python o formati di numeri visualizzati. 

Comprende i seguenti elementi: 

* DynamoCorePath // Indica dove si trovano i file binari di DynamoCore in corso di caricamento.

* DynamoHostPath // Indica dove si trovano i file binari di integrazione di Dynamo.

* GeometryFactoryPath // Indica dove si trovano i file binari di LibG caricati.

* PathResolver // È l'oggetto che consente di risolvere vari file.

* PreloadLibraryPaths // Indica dove si trovano i file binari dei nodi precaricati, ad esempio DSOffice.dll. 

* AdditionalNodeDirectories // Indica dove si trovano i file binari di nodi aggiuntivi.
 
* AdditionalResolutionPaths // Indica i percorsi di risoluzione di assiemi aggiuntivi per altre dipendenze che potrebbero essere necessarie durante il caricamento delle librerie. 

* UserDataRootFolder // È la cartella dei dati utente, ad esempio `"AppData\Roaming\Dynamo\Dynamo Revit"`. 

* CommonDataRootFolder // È la cartella di default per il salvataggio di definizioni personalizzate, esempi e così via.

* Context // È il nome host dell'integratore + versione `(Revit<BuildNum>)`.

* SchedulerThread // È il thread dell'utilità di programmazione dell'integratore che implementa `ISchedulerThread`. Per la maggior parte degli integratori questo è il thread principale dell'interfaccia utente o qualsiasi thread da cui è possibile accedere all'API.

* StartInTestMode // Indica se la sessione corrente è una sessione di automazione di test; modifica un certo numero di comportamenti di Dynamo. Non utilizzare a meno che non si stiano scrivendo test.

* AuthProvider // È l'implementazione di IAuthProvider da parte dell'integratore, ad esempio l'implementazione di RevitOxygenProvider è in Greg.dll. Tale impostazione viene utilizzata per l'integrazione del caricamento di Package Manager. 

### Preferenze 

Il percorso di impostazione delle preferenze di default viene gestito da `PathManager.PreferenceFilePath`, ad esempio `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`. Gli integratori possono decidere se inviare anche un file di impostazione delle preferenze personalizzato in una posizione che deve essere allineata con la gestione dei percorsi. Di seguito sono riportate le proprietà di impostazione delle preferenze serializzate: 

* IsFirstRun // Indica se è la prima volta che viene eseguita questa versione di Dynamo, ad esempio, tale impostazione viene utilizzata per determinare se è necessario visualizzare il messaggio di consenso/rifiuto esplicito di GA. Viene anche utilizzata per determinare se è necessario eseguire la migrazione dell'impostazione delle preferenze di Dynamo preesistente quando si avvia una nuova versione di Dynamo, in modo che gli utenti abbiano un'esperienza coerente. 

* IsUsageReportingApproved // Indica se i rapporti sull'utilizzo sono approvati o meno. 

* IsAnalyticsReportingApproved // Indica se i rapporti sull'analisi sono approvati o meno. 

* LibraryWidth // È la larghezza del pannello della libreria sinistro di Dynamo. 

* ConsoleHeight // È l'altezza del display della console. 

* ShowPreviewBubbles // Indica se visualizzare i simboli circolari di anteprima. 

* ShowConnector // Indica se i connettori sono visualizzati. 

* ConnectorType //Indica il tipo di connettore: Bezier o Polilinea. 

* BackgroundPreviews // Indica lo stato attivo dell'anteprima dello sfondo specificata. 

* RenderPrecision // È il livello di precisione di rendering; un livello più basso genera mesh con meno triangoli. Un livello più alto genererà una geometria più uniforme nell'anteprima dello sfondo. 128 è un valore sufficiente per ottenere rapidamente un'anteprima della geometria.

* ShowEdges // Indica se verrà eseguito il rendering dei bordi di superfici e di solidi. 

* ShowDetailedLayout // Impostazione NON UTILIZZATA.

* WindowX, WindowY // È l'ultima coordinata X e Y della finestra di Dynamo. 

* WindowW, WindowH // È l'ultima larghezza e altezza della finestra di Dynamo. 

* UseHardwareAcceleration // Indica se Dynamo utilizza l'accelerazione hardware, se supportata. 

* NumberFormat // È la precisione decimale utilizzata per visualizzare i numeri nel simbolo circolare di anteprima toString(). 

* MaxNumRecentFiles // È il numero massimo di percorsi di file recenti da salvare. 

* RecentFiles // È un elenco di percorsi di file aperti di recente, che influisce direttamente sull'elenco dei file recenti nella pagina di avvio di Dynamo. 

* BackupFiles // È un elenco di percorsi di file di backup. 

* CustomPackageFolders // È un elenco di cartelle contenenti file binari zero-touch e percorsi di directory che verranno analizzati alla ricerca di pacchetti e nodi personalizzati.

* PackageDirectoriesToUninstall // È un elenco di pacchetti utilizzati da Package Manager per determinare quali pacchetti sono contrassegnati per l'eliminazione. Questi percorsi verranno eliminati, se possibile, durante l'avvio di Dynamo.

* PythonTemplateFilePath // Indica il percorso del file Python (.py) da utilizzare come modello iniziale durante la creazione di un nuovo nodo di PythonScript. Questa impostazione può essere utilizzata per configurare un modello di Python personalizzato per l'integrazione.

* BackupInterval // Indica ogni quanto tempo (in millisecondi) il grafico verrà salvato automaticamente. 

* BackupFilesCount // Indica il numero di backup che verranno eseguiti. 

* PackageDownloadTouAccepted // Indica se l'utente ha accettato le condizioni d'uso per il download di pacchetti da Package Manager. 

* OpenFileInManualExecutionMode // Indica lo stato di default della casella di controllo Apri in modalità di esecuzione manuale in OpenFileDialog. 

* NamespacesToExcludeFromLibrary // Indica quali (se presenti) spazi dei nomi non devono essere visualizzati nella libreria di nodi di Dynamo. Formato stringa: "[nome libreria]:[spazio dei nomi completo]" 

Esempio di impostazioni delle preferenze serializzate: 

``` xml 
<PreferenceSettings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> 

<IsFirstRun>false</IsFirstRun> 

<IsUsageReportingApproved>false</IsUsageReportingApproved> 

<IsAnalyticsReportingApproved>false</IsAnalyticsReportingApproved> 

<LibraryWidth>204</LibraryWidth> 

<ConsoleHeight>0</ConsoleHeight> 

<ShowPreviewBubbles>true</ShowPreviewBubbles> 

<ShowConnector>true</ShowConnector> 

<ConnectorType>BEZIER</ConnectorType> 

<BackgroundPreviews> 

<BackgroundPreviewActiveState> 

<Name>IsBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

<BackgroundPreviewActiveState> 

<Name>IsRevitBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

</BackgroundPreviews> 

<IsBackgroundGridVisible>true</IsBackgroundGridVisible> 

<RenderPrecision>128</RenderPrecision> 

<ShowEdges>false</ShowEdges> 

<ShowDetailedLayout>true</ShowDetailedLayout> 

<WindowX>553</WindowX> 

<WindowY>199</WindowY> 

<WindowW>800</WindowW> 

<WindowH>676</WindowH> 

<UseHardwareAcceleration>true</UseHardwareAcceleration> 

<NumberFormat>f3</NumberFormat> 

<MaxNumRecentFiles>10</MaxNumRecentFiles> 

<RecentFiles> 

<string></string> 

</RecentFiles> 

<BackupFiles> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\backup\backup.DYN</string> 

</BackupFiles> 

<CustomPackageFolders> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\2.5</string> 

</CustomPackageFolders> 

<PackageDirectoriesToUninstall /> 

<PythonTemplateFilePath /> 

<BackupInterval>60000</BackupInterval> 

<BackupFilesCount>1</BackupFilesCount> 

<PackageDownloadTouAccepted>true</PackageDownloadTouAccepted> 

<OpenFileInManualExecutionMode>false</OpenFileInManualExecutionMode> 

<NamespacesToExcludeFromLibrary> 

<string>ProtoGeometry.dll:Autodesk.DesignScript.Geometry.TSpline</string> 

</NamespacesToExcludeFromLibrary> 

</PreferenceSettings> 
``` 
 

* Extensions // È un elenco di estensioni che implementano IExtension; se è nullo, Dynamo caricherà le estensioni dal percorso di default (cartella `extensions` sotto la cartella Dynamo). 

* IsHeadless // Indica se Dynamo viene avviato senza interfaccia utente, con effetti sull'analisi. 

* UpdateManager // È l'implementazione di UpdateManager da parte dell'integratore; vedere la descrizione precedente. 

* ProcessMode // È equivalente a TaskProcessMode. La modalità è sincrona se in fase di test, altrimenti è asincrona. Controlla il comportamento dell'utilità di programmazione. Anche gli ambienti a thread singolo possono impostare questa opzione sulla modalità sincrona.

Utilizzo di StartConfiguration di destinazione per l'avvio di `DynamoModel`

Una volta passato StartConfig all'avvio di `DynamoModel`, DynamoCore supervisionerà le specifiche effettive per assicurarsi che la sessione di Dynamo venga inizializzata correttamente con i dettagli specificati. Dovrebbero esserci alcuni passaggi post-impostazione che i singoli integratori dovranno eseguire dopo l'inizializzazione di `DynamoModel`, ad esempio in Dynamo for Revit, la sottoscrizione di eventi per controllare le transazioni host di Revit o gli aggiornamenti dei documenti, la personalizzazione del nodo Python, ecc. 

 ### Programmazione visiva

Per inizializzare `DynamoViewModel` e `DynamoView`, sarà necessario innanzitutto creare `DynamoViewModel`, operazione che può essere eseguita utilizzando il metodo statico `DynamoViewModel.Start`. Vedere di seguito:

``` c#

    viewModel = DynamoViewModel.Start(
                    new DynamoViewModel.StartConfiguration()
                    {
                        CommandFilePath = commandFilePath,
                        DynamoModel = model,
                        Watch3DViewModel = 
                            HelixWatch3DViewModel.TryCreateHelixWatch3DViewModel(
                                null,
                                new Watch3DViewModelStartupParams(model), 
                                model.Logger),
                        ShowLogin = true
                    });
     
     var view = new DynamoView(viewModel);

```

`DynamoViewModel.StartConfiguration` ha molte meno opzioni rispetto al file .config del modello. Sono per lo più autoesplicative: `CommandFilePath` si può ignorare, a meno che non si stia scrivendo un caso di test.


Il parametro `Watch3DViewModel` controlla il modo in cui l'anteprima dello sfondo e i nodi Watch3D visualizzano la geometria 3D. È possibile utilizzare un'implementazione personalizzata se si implementano le interfacce richieste.

Per costruire `DynamoView`, tutto ciò che serve è `DynamoViewModel`. View è un controllo della finestra e può essere mostrato utilizzando WPF.

 ### Esempio con DynamoSandbox.exe:

 DynamoSandbox.exe è un ambiente di sviluppo per testare, utilizzare e sperimentare DynamoCore. È un ottimo esempio da consultare per vedere come vengono caricati e impostati i componenti `DynamoCore` e `DynamoCoreWPF`. È possibile visualizzare alcuni punti di ingresso [qui](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37).

 ## Binding di elementi e traccia

#### Panoramica

La *traccia* è un meccanismo di Dynamo Core in grado di serializzare i dati nel file .dyn (file di Dynamo). Fondamentalmente, questi dati vengono legati a CallSite di nodi all'interno del grafico di Dynamo.

Quando un grafico di Dynamo viene aperto dal disco, i dati di traccia salvati in esso vengono riassociati ai nodi del grafico.

#### Glossario:
* Meccanismo di traccia:
    
  * Implementa il binding di elementi in Dynamo.
  * Il meccanismo di traccia può essere utilizzato per garantire che gli oggetti vengano riportati alla geometria creata.
  * CallSite e meccanismo di traccia consentono di fornire un GUID persistente che l'implementatore del nodo può utilizzare per il ricollegamento.

* CallSite

  * Il file eseguibile contiene più CallSite. Queste classi CallSite vengono utilizzate per inviare l'esecuzione ai vari punti da cui devono essere inviate:
    * Libreria C#
    * Metodo incorporato
    * Funzione di DesignScript
    * Nodo personalizzato (funzione DS).

* TraceSerializer
  * Serializza le classi contrassegnate come `ISerializable` e `[Serializable]` nella traccia.
  * Gestisce la serializzazione e la deserializzazione dei dati nella traccia.
  * TraceBinder controlla il binding di dati deserializzati ad un tipo di runtime (crea l'istanza della classe reale).

#### Qual è il loro formato?
----

I dati di traccia vengono serializzati nel file .dyn all'interno di una proprietà denominata Bindings. Questa è una matrice di ID CallSite -> dati. Una CallSite è la posizione/istanza specifica in cui un nodo viene chiamato nella macchina virtuale DesignScript. È importante ricordare che i nodi in un grafico di Dynamo possono essere chiamati più volte e pertanto potrebbero essere create più CallSite per una singola istanza di nodo.

```json
"Bindings": [
    {
      "NodeId": "1e83cc25-7de6-4a7c-a702-600b79aa194d",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_1e83cc25-7de6-4a7c-a702-600b79aa194d":  "Base64 Encoded Data"
      }
    },
    {
      "NodeId": "c69c7bec-d54b-4ead-aea8-a3f45bea9ab2",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_c69c7bec-d54b-4ead-aea8-a3f45bea9ab2": "Base64 Encoded Data"
      }
    }
  ],

 
```

 Si consiglia di *NON* dipendere dal formato dei dati serializzati con codifica base64.


#### Quale problema si sta cercando di risolvere
----


Ci sono molte ragioni per cui si vorrebbero salvare dati arbitrari come risultato dell'esecuzione di una funzione, ma in questo caso la traccia è stata sviluppata per risolvere un problema specifico che gli utenti incontrano frequentemente quando compilano e iterano programmi software che creano elementi in applicazioni host.

Il problema è quello che abbiamo chiamato `Element Binding` e l'idea è questa:

Quando un utente sviluppa ed esegue un grafico di Dynamo, è probabile che generi nuovi elementi nel modello di applicazione host. Per questo esempio, supponiamo che l'utente abbia un piccolo programma che genera 100 porte in un modello architettonico. Il numero e la posizione di queste porte sono controllati dal suo programma.

La prima volta che l'utente esegue il programma, vengono generate queste 100 porte.

Successivamente, quando l'utente modifica un input per il programma e lo riesegue, il programma *(senza binding di elementi)* creerà 100 nuove porte; le porte precedenti continueranno ad esistere nel modello insieme a quelle nuove.

----

Poiché Dynamo è un ambiente di programmazione in tempo reale e dispone di una modalità di esecuzione `"Automatic"` in cui le modifiche apportate al grafico attivano una nuova esecuzione, ciò può sovraccaricare un modello con i risultati di molte esecuzioni del programma.


Abbiamo riscontrato che questo non è in genere ciò che gli utenti si aspettano; con il binding di elementi attivato, invece, i risultati precedenti di un'esecuzione del grafico vengono corretti ed eliminati o modificati. Quali siano le due opzioni (*eliminati o modificati*) dipende dalla flessibilità dell'API dell'host. Con il binding di elementi attivato, dopo la seconda, la terza o la cinquantesima esecuzione del programma Dynamo dell'utente, sono presenti solo 100 porte nel modello.

Ciò richiede più della semplice capacità di serializzare i dati nel file .dyn e, come si vedrà di seguito, in DynamoRevit sono disponibili meccanismi basati sulla traccia per supportare questi workflow di rebinding.

----

Questo è il momento appropriato per menzionare l'altro importante caso di utilizzo del binding di elementi per host come Revit. Poiché gli elementi creati quando è stato attivato il binding di elementi tenteranno di mantenere gli ID elemento esistenti (modificando gli elementi esistenti), la logica basata su questi elementi nell'applicazione host continuerà ad esistere dopo l'esecuzione di un programma Dynamo. Ad esempio:

Torniamo all'esempio del nostro modello architettonico.

Esaminiamo prima un esempio con il binding di elementi disattivato. Questa volta l'utente dispone di un programma che genera alcuni muri architettonici.

 Esegue il suo programma che genera alcuni muri nell'applicazione host. Esce quindi dal grafico di Dynamo e utilizza i normali strumenti di Revit per posizionare alcune finestre in tali muri. Le finestre vengono collegate a questi muri specifici come parte del modello di Revit.

L'utente avvia il backup di Dynamo ed esegue nuovamente il grafico; ora, come nell'ultimo esempio, sono presenti due serie di muri. Nella primo serie sono state aggiunte le finestre, ma non i nuovi muri.

Se è stato attivato il binding di elementi, è possibile mantenere il lavoro esistente eseguito manualmente nell'applicazione host senza Dynamo. Ad esempio, se il binding è stato attivato quando l'utente ha eseguito il programma per la seconda volta, i muri verrebbero modificati, non eliminati, e le modifiche a valle apportate nell'applicazione host rimarrebbero. Il modello conterrebbe muri con finestre, anziché due serie di muri con vari stati.

-----

![Creazione di muri](images/creates_walls.png)


#### Confronto di binding di elementi e traccia
----

La traccia è un meccanismo di Dynamo Core: utilizza una variabile statica di CallSite per associare i dati a CallSite di una funzione nel grafico, come descritto in precedenza.

Consente inoltre di serializzare dati arbitrari nel file .dyn durante la scrittura di nodi zero-touch di Dynamo. In genere questa procedura non è consigliabile, in quanto significa che il codice zero-touch potenzialmente trasferibile ora ha una dipendenza da Dynamo Core.

Non utilizzare il formato serializzato dei dati nel file .dyn, ma utilizzare l'interfaccia e l'attributo [Serializable].

ElementBinding, invece, è un elemento basato sulle API di traccia ed è implementato nell'integrazione di Dynamo *(DynamoRevit, Dynamo for Civil 3D e così via).*


#### API di Traccia
Alcune delle API di traccia di basso livello che vale la pena conoscere sono:

``` c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

È possibile vedere queste utilizzate nel seguente esempio:

Per interagire con i dati di traccia che Dynamo ha caricato da un file esistente o sta generando, è possibile esaminare:

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```
[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)


#### Esempio di traccia semplice di un nodo
----
Un esempio di nodo di Dynamo che utilizza direttamente la traccia è disponibile qui nel [repository DynamoSamples](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs).

Nel riepilogo della classe è spiegata l'essenza del concetto di traccia:

```
  /*
     * After a graph update, Dynamo typically disposes of all
     * objects created during the graph update. But what if there are 
     * objects which are expensive to re-create, or which have other
     * associations in a host application? You wouldn't want those those objects
     * re-created on every graph update. For example, you might 
     * have an external database whose records contain data which needs
     * to be re-applied to an object when it is created in Dynamo.
     * In this example, we use a wrapper class, TraceExampleWrapper, to create 
     * TraceExampleItem objects which are stored in a static dictionary 
     * (they could be stored in a database as well). On subsequent graph updates, 
     * the objects will be retrieved from the data store using a trace id stored 
     * in the trace cache.
     */
```

In questo esempio sono utilizzate direttamente le API di traccia in DynamoCore per memorizzare alcuni dati ogni volta che viene eseguito un nodo specifico. In questo caso, il dizionario funge da modello dell'applicazione host, come il database di modelli di Revit.

L'impostazione approssimativa è:

Una classe di utilità statica `TraceExampleWrapper` viene importata come nodo in Dynamo. Contiene un singolo metodo `ByString`, che crea `TraceExampleItem`. Si tratta di oggetti .NET normali che contengono una proprietà `description`.

Ogni `TraceExampleItem` viene serializzato in una traccia rappresentata come `TraceableId`, ovvero una classe contenente un `IntId` contrassegnato `[Serializeable]` in modo che possa essere serializzato con il formattatore `SOAP`. Per ulteriori informazioni sull'attributo serializzabile, vedere [qui](https://docs.microsoft.com/it-it/dotnet/api/system.serializableattribute?view=netframework-4.8).

È inoltre necessario implementare l'interfaccia `ISerializable` definita [qui](https://learn.microsoft.com/it-it/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8).


``` c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

Questa classe viene creata per ogni `TraceExampleItem` che si desidera salvare nella traccia, serializzata, con codifica base64 e salvata su disco quando il grafico viene salvato, in modo che i binding possano essere riassociati, anche in un secondo momento quando il grafico viene riaperto su un dizionario esistente di elementi. Questo metodo non funzionerà correttamente nel caso di questo esempio, poiché il dizionario non è realmente persistente come un documento di Revit.

Infine, l'ultima parte dell'equazione è `TraceableObjectManager`, che è simile a `ElementBinder` in `DynamoRevit`; gestisce la relazione tra gli oggetti presenti nel modello di documento dell'host e i dati memorizzati nella traccia di Dynamo.

Quando un utente esegue un grafico contenente il nodo `TraceExampleWrapper.ByString` per la prima volta, viene creato un nuovo `TraceableId` con un nuovo ID, `TraceExampleItem` viene memorizzato nel dizionario associato a tale nuovo ID e `TraceableID` viene memorizzato nella traccia.

Alla successiva esecuzione del grafico, esaminiamo la traccia, troviamo l'ID memorizzato, troviamo l'oggetto associato a tale ID e restituiamo tale oggetto. Invece di creare un oggetto completamente nuovo, modifichiamo quello esistente.

Il flusso di due esecuzioni consecutive del grafico che crea un singolo `TraceExampleItem` è simile al seguente:

![Prima chiamata](images/Trace-first-call.png)

![Seconda chiamata](images/Trace-second-call.png)

La stessa idea è illustrata nell'esempio successivo con un caso di utilizzo del nodo DynamoRevit più realistico.

#### Diagramma della traccia

![Passaggi della traccia](images/trace_diagram.png) ![Flusso della traccia](images/trace_alt_diagram.png)

#### NOTA
Nelle versioni recenti di Dynamo, l'utilizzo di TLS (archiviazione thread-local) è stato sostituito dall'uso di membri statici.

#### Esempio di implementazione de binding di elementi

-----
Vediamo rapidamente come appare un nodo che utilizza il binding di elementi quando viene implementato per DynamoRevit; questo è analogo al tipo di nodo utilizzato in precedenza negli esempi di creazione di muri specificati.


---


``` c#
    private void InitWall(Curve curve, Autodesk.Revit.DB.WallType wallType, Autodesk.Revit.DB.Level baseLevel, double height, double offset, bool flip, bool isStructural)
        {
            // This creates a new wall and deletes the old one
            TransactionManager.Instance.EnsureInTransaction(Document);

            //Phase 1 - Check to see if the object exists and should be rebound
            var wallElem =
                ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>(Document);

            bool successfullyUsedExistingWall = false;
            //There was a modelcurve, try and set sketch plane
            // if you can't, rebuild 
            if (wallElem != null && wallElem.Location is Autodesk.Revit.DB.LocationCurve)
            {
                var wallLocation = wallElem.Location as Autodesk.Revit.DB.LocationCurve;
                <SNIP>

                    if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;

                  <SNIP>
                
            }

            var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
            InternalSetWall(wall);

            TransactionManager.Instance.TransactionTaskDone();

            // delete the element stored in trace and add this new one
            ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
        }
```

Il codice precedente illustra un costruttore di esempio per un elemento muro. Questo costruttore verrebbe chiamato da un nodo in Dynamo come: `Wall.byParams`

Le fasi importanti dell'esecuzione del costruttore in relazione al binding di elementi sono:

1. Utilizzare `elementBinder` per verificare se sono presenti eventuali oggetti creati in precedenza che sono stati associati a questa CallSite in un'esecuzione passata. `ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. In tal caso, provare a modificare il muro invece di crearne uno nuovo.

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. Altrimenti creare un nuovo muro.
```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. Eliminare l'elemento precedente appena recuperato dalla traccia e aggiungere quello nuovo in modo da poter cercare questo elemento in futuro:
```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### Discussione

#### Efficienza
* Attualmente ogni oggetto traccia di serializzazione viene serializzato utilizzando la formattazione XML SOAP, che è piuttosto dettagliata e duplica molte informazioni. Quindi i dati vengono codificati in base64 due volte. Questo non è efficiente in termini di serializzazione o deserializzazione. Questo aspetto può essere migliorato in futuro, se il formato interno non si basa su tale codifica. Anche in questo caso, ripetiamo, non fare affidamento sul formato dei dati serializzati inattivi.

#### ElementBinding deve essere attivato per default?
* Esistono casi di utilizzo in cui non si desidera il binding di elementi. Che cosa succede se uno è un utente avanzato di Dynamo che sviluppa un programma che deve essere eseguito più volte per generare elementi di raggruppamento casuali? L'intento del programma è quello di creare elementi aggiuntivi ogni volta che il programma viene eseguito. Questo caso di utilizzo non è facilmente realizzabile senza soluzioni alternative che impediscano che il binding degli elementi venga attivato. È possibile disattivare elementBinding a livello di integrazione, ma probabilmente si tratta di una funzionalità di Dynamo di base. Non è chiaro quanto dovrebbe essere granulare questa funzionalità: a livello di nodo, di CallSite, di intera sessione di Dynamo, di area di lavoro e così via.

## Nodi Selection di Dynamo for Revit (cosa sono?) 

In generale, questi nodi consentono all'utente di descrivere in qualche modo un sottogruppo del documento di Revit attivo a cui desidera fare riferimento. Esistono vari modi in cui l'utente può fare riferimento ad un elemento di Revit (descritti di seguito) e l'output risultante del nodo può essere un wrapper di elementi di Revit (wrapper di DynamoRevit) o della geometria di Dynamo (convertita dalla geometria di Revit). La differenza tra questi tipi di output sarà utile da considerare nel contesto di altre integrazioni host.

A livello generale, **un buon modo per concettualizzare questi nodi è come una funzione che accetta un ID elemento e restituisce un puntatore a tale elemento o alla geometria che rappresenta tale elemento.** 

Sono presenti più nodi `�Selection�` in DynamoRevit. È possibile suddividerli in almeno due gruppi: 

![Nodi Selection di Revit](images/revitSelectionNodes.png)


 

1. Selezione dell'interfaccia utente: 

    I nodi di `DynamoRevit` di esempio di questa categoria sono `SelectModelElement`, `SelectElementFace`.

    Questi nodi consentono all'utente di passare al contesto dell'interfaccia utente di Revit e selezionare un elemento o un gruppo di elementi. Gli ID di tali elementi vengono acquisiti e alcune funzioni di conversione vengono eseguite; viene creato un wrapper o viene estratta e convertita la geometria dall'elemento. La conversione che viene eseguita dipende dal tipo di nodo scelto dall'utente. 

2. Query nel documento 

    I nodi di esempio in questa categoria sono `AllElementsOfClass`, `AllElementsOfCategory`.

    Questi nodi consentono all'utente di eseguire query su un sottogruppo di elementi nell'intero documento; in genere questi nodi restituiscono wrapper che puntano agli elementi di Revit sottostanti. Questi wrapper sono parte integrante dell'esperienza di DynamoRevit e forniscono funzionalità più avanzate, ad esempio il binding di elementi, e consentono agli integratori di Dynamo di selezionare quali API host sono esposte come nodi agli utenti.


### Workflow dell'utente di Dynamo for Revit: 

#### Casi di esempio 

1. 
    * L'utente seleziona un muro di Revit con `SelectModelElement`: un wrapper del muro di Dynamo viene restituito nel grafico (visibile nel simbolo circolare di anteprima del nodo). 

    * L'utente posiziona il nodo Element.Geometry e associa l'output `SelectModelElement` a questo nuovo nodo; la geometria del muro ripiegato viene estratta e convertita nella geometria di Dynamo utilizzando l'API di LibG. 

    * L'utente attiva la modalità di esecuzione Automatico per il grafico. 

    * L'utente modifica il muro originale in Revit. 

    * Il grafico viene rieseguito automaticamente quando il documento di Revit ha generato un evento che segnala che alcuni elementi sono stati aggiornati; il nodo di selezione controlla questo evento e vede che l'ID dell'elemento selezionato è stato modificato. 

### Workflow dell'utente di Dynamo for Civil 3D: 

I workflow in Dynamo for Civil 3D sono molto simili alla descrizione precedente per Revit. Di seguito sono riportati due gruppi tipici di nodi Selection in Dynamo for Civil 3D:  

![Nodi Selection di Civil 3D](images/civilSelectionNodes.png)



### Problemi: 

* A causa dell'aggiornamento delle modifiche del documento che i nodi Selection `DynamoRevit` implementano, i loop infiniti sono facili da costruire. Si immagini un nodo che controlla il documento per tutti gli elementi e quindi crea nuovi elementi in un punto a valle. Questo programma, una volta eseguito, attiverà un loop. `DynamoRevit` tenta di individuare questi casi in vari modi utilizzando gli ID transazione ed evita di modificare il documento quando gli input per i costruttori di elementi non sono cambiati.

    Questo aspetto deve essere preso in considerazione se la modalità di esecuzione Automatico del grafico viene avviata quando un elemento selezionato viene modificato nell'applicazione host. 

* I nodi Selection in `DynamoRevit` vengono implementati nel progetto `RevitUINodes.dll` che fa riferimento a WPF. Questo potrebbe non essere un problema  ma vale la pena tenerne conto a seconda della piattaforma di destinazione. 
 

### Diagrammi del flusso di dati

![Flusso di selezione](images/selectModelElement.png)

![Flusso di selezione2](images/selectElementFace.png)



### Implementazione tecnica: (fare riferimento ai diagrammi precedenti): 

I nodi Selection vengono implementati ereditando proprietà dai tipi `SelectionBase` generici: `SelectionBase<TSelection, TResult> ` e un gruppo minimo di membri: 

* Implementazione di un metodo `BuildOutputAST`; questo metodo deve restituire un albero sintattico astratto (AST), che verrà eseguito in un certo momento in futuro, quando il nodo deve essere eseguito. Nel caso dei nodi Selection, dovrebbe restituire elementi o geometria dagli ID elemento. https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280 

* L'implementazione di `BuildOutputAST` è una delle parti più difficili dell'implementazione dei nodi `NodeModel`/dell'interfaccia utente. È consigliabile inserire quanta più logica possibile in una funzione C# e incorporare semplicemente un nodo di chiamata di funzione AST in AST. Si noti che qui `node` è un nodo AST nell'albero sintattico astratto, non è un nodo nel grafico di Dynamo. 

![Flusso di selezione2](images/selectionAST.png)


* Serializzazione  

  * Poiché si tratta di tipi derivati da `NodeModel` espliciti (non ZeroTouch), richiedono anche l'implementazione di un [JsonConstructor] che verrà utilizzato durante la deserializzazione del nodo da un file .dyn. 

    I riferimenti agli elementi dell'host devono essere salvati nel file .dyn in modo che quando un utente apre un grafico contenente questo nodo, la selezione sia ancora impostata. I nodi NodeModel in Dynamo utilizzano json.net per la serializzazione. Eventuali proprietà pubbliche verranno serializzate automaticamente utilizzando Json.net. Utilizzare l'attributo [JsonIgnore] per serializzare solo ciò che è necessario. 

* I nodi di query nel documento sono un po' più semplici in quanto non richiedono di memorizzare un riferimento in alcun ID elemento. Vedere di seguito per le implementazioni della classe `ElementQueryBase` e della classe derivata. Quando vengono eseguiti, questi nodi effettuano una chiamata all'API di Revit ed effettuano query sugli elementi nel documento sottostante, quindi procedono con la conversione menzionata in precedenza nei wrapper di geometria o di elementi di Revit. 

### Riferimenti:

#### Classi di base di DynamoCore: 

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs )

* [Case study NodeModel - Interfaccia utente personalizzata](11\_developer\_primer/3\_developing\_for\_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Aggiornamento di pacchetti e librerie di Dynamo per Dynamo 2.x](11\_developer\_primer/3\_developing\_for\_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Aggiornamento di pacchetti e librerie di Dynamo per Dynamo 3.x](11\_developer\_primer/3\_developing\_for\_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)



#### DynamoRevit: 

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs )

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)


## Panoramica dei pacchetti incorporati di Dynamo

Il meccanismo dei pacchetti incorporati è un tentativo di raggruppare più contenuto dei nodi con Dynamo Core senza espandere struttura stessa del programma sfruttando la funzionalità di caricamento dei pacchetti di Dynamo implementata dalle estensioni `PackageLoader` e `PackageManager`.

In questo documento saranno utilizzati in modo intercambiabile i termini pacchetti incorporati e pacchetti incorporati di Dynamo per indicare la stessa cosa.

### Occorre inviare un pacchetto come pacchetto incorporato?
* Il pacchetto deve disporre di punti di ingresso binari firmati, altrimenti non verrà caricato.
* È necessario fare tutto il possibile per evitare di apportare in questi pacchetti modifiche che causano un'interruzione. Ciò significa che il contenuto del pacchetto deve prevedere test automatizzati.
* Schema di controllo semantico. È probabilmente una buona idea rilasciare il pacchetto utilizzando uno schema di controllo semantico delle versioni e comunicarlo agli utenti nella descrizione del pacchetto o nei documenti.
* Test automatizzati. Si tenga presente quanto riportato sopra. Se un pacchetto è incluso utilizzando il meccanismo del pacchetto incorporato, per un utente sembra essere parte del prodotto e deve essere testato come un prodotto.
* Alto livello di accuratezza: icone, documenti di nodi, contenuto localizzato.
* Non inviare pacchetti che l'utente o il team non è in grado di gestire.
* Non inviare pacchetti di terze parti in questo modo (vedere sopra).

In sostanza, è necessario avere il pieno controllo sul pacchetto, la possibilità di correggerlo, mantenerlo aggiornato e testarlo rispetto alle modifiche più recenti in Dynamo e nel prodotto in uso. È inoltre necessario disporre della capacità di firmarlo.


### Confronto tra pacchetti incorporati e pacchetti specifici dell'integrazione host

`Built-In Packages` è destinato ad essere una funzionalità di base, un gruppo di pacchetti a cui tutti gli utenti possono accedere, anche se non hanno accesso a Package Manager Attualmente il meccanismo sottostante per supportare questo funzionalità è un posizione di caricamento di default aggiuntiva per i pacchetti direttamente nella directory di Dynamo Core, rispetto a DynamoCore.dll.

Con alcuni vincoli, questa posizione sarà utilizzabile per i client e gli integratori di Autodesk Dynamo per distribuire i pacchetti specifici dell'integrazione. *Ad esempio, l'integrazione di Dynamo Formit richiede un pacchetto di Dynamo Formit personalizzato.*

Poiché il meccanismo di caricamento sottostante è lo stesso sia per i pacchetti specifici di Dynamo Core che dell'host, sarà necessario assicurarsi che i pacchetti distribuiti in questo modo non creino nell'utente confusione tra i pacchetti `Built-In Packages` di Dynamo Core e i pacchetti specifici dell'integrazione che sono disponibili solo in un singolo prodotto host. Per evitare confusione all'utente, è consigliabile introdurre pacchetti specifici dell'host nella discussione con i team di Dynamo.


### Localizzazione di pacchetti

Poiché i pacchetti inclusi in `Built-In Packages` saranno disponibili per più clienti e le garanzie che forniamo su di essi saranno più rigorose (vedere sopra), dovrebbero essere localizzati.

Per i pacchetti di Autodesk interni destinati all'inclusione di `Built-In Packages`, le attuali limitazioni di non poter pubblicare contenuti localizzati in Package Manager non sono un ostacolo, poiché i pacchetti non devono necessariamente essere pubblicati in Package Manager.

Utilizzando una soluzione alternativa, è possibile creare manualmente (e persino pubblicare) pacchetti con sottodirectory relative alla propria lingua nella cartella /bin di un pacchetto.

Innanzitutto, creare manualmente le sottodirectory specifiche della lingua necessarie nella cartella `/bin` dei pacchetti. 

Se per qualche motivo, è necessario pubblicare il pacchetto anche in Package Manager, è necessario prima pubblicare una versione del pacchetto in cui mancano queste sottodirectory relative alla propria lingua, quindi pubblicare una nuova versione del pacchetto utilizzando DynamoUI `publish package version`. Il caricamento della nuova versione in Dynamo non dovrebbe eliminare le cartelle e i file in `/bin`, aggiunti manualmente utilizzando Esplora risorse di Windows. Il processo di caricamento del pacchetto in Dynamo verrà aggiornato per soddisfare i requisiti per i file localizzati in futuro.

Queste sottodirectory relative alla propria lingua vengono caricate senza problemi dal runtime .NET se si trovano nella stessa directory dei file binari del nodo/dell'estensione.

Per ulteriori informazioni sugli assiemi di risorse e sui file .resx, vedere: [https://learn.microsoft.com/it-it/dotnet/core/extensions/create-resource-files](https://docs.microsoft.com/it-it/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

È probabile che i file `.resx` vengano creati e compilati con Visual Studio. Per un determinato assieme `xyz.dll`, le risorse risultanti verranno compilate in un nuovo assieme `xyz.resources.dll`, come descritto in precedenza, il posizione e il nome di questo assieme sono importanti.

Il file `xyz.resources.dll` generato dovrebbe trovarsi nella seguente posizione: `package\bin\culture\xyz.resources.dll`.

Per accedere alle stringhe localizzate nel pacchetto, è possibile utilizzare ResourceManager, ma ancora più semplicemente, si dovrebbe essere in grado di fare riferimento a `Properties.Resources.YourLocalizedResourceName` dall'interno del assieme per il quale è stato aggiunto un file `.resx`. Ad esempio, vedere: 

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457) per un esempio di messaggio di errore localizzato.

o [https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19) per un esempio di stringa di attributo NodeDescription specifica di Dynamo localizzata.

o [https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs) per un altro esempio.

### Layout della libreria di nodi

In genere, quando Dynamo carica nodi da un pacchetto, li posiziona nella sezione `Addons` della libreria di nodi. Per integrare meglio i nodi di pacchetti incorporati con altro contenuto incorporato, è stata aggiunta la possibilità per gli autori di pacchetti incorporati di fornire un file `layout specification` parziale per posizionare i nuovi nodi nella categoria di livello superiore corretta nella sezione della libreria `default`.

Ad esempio, il file .json delle specifiche di layout seguente, se si trova nel percorso `package/extra/layoutspecs.json` posizionerà i nodi specificati da `path` nella categoria `Revit` nella sezione `default`, che è la sezione principale dei nodi incorporati.

Si noti che i nodi importati da un pacchetto incorporato avranno il prefisso `bltinpkg://` quando vengono considerati per la corrispondenza con un percorso incluso nella specifica del layout.

```json
{
  "sections": [
    {
      "text": "default",
      "iconUrl": "",
      "elementType": "section",
      "showHeader": false,
      "include": [ ],
      "childElements": [
        {
          "text": "Revit",
          "iconUrl": "",
          "elementType": "category",
          "include": [],
          "childElements": [
            {
              "text": "some sub group name",
              "iconUrl": "",
              "elementType": "group",
              "include": [
                {
                  "path": "bltinpkg://namespace.namespace",
                  "inclusive": false
                }
              ],
              "childElements": []
            }
          ]
        }
      ]
    }
  ]
}
```

Le modifiche complesse al layout non sono ben testate o supportate. Lo scopo di questo particolare caricamento delle specifiche di layout è quello di spostare lo spazio dei nomi di un intero pacchetto in una determinata categoria host come `Revit` o `Formit`. 