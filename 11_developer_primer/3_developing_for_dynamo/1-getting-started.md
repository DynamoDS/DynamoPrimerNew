# Per iniziare

Prima di dedicarci al tema dello sviluppo, è importante costruire una solida base per un nuovo progetto. Nella comunità di sviluppatori di Dynamo sono disponibili diversi modelli di progetto che rappresentano ottimi punti di partenza, ma è ancora più importante comprendere come iniziare un progetto da zero. Creare un progetto dalle fondamenta consente di comprendere meglio il processo di sviluppo.

![Visual Studio](images/visual-studio.jpg)

### Creazione di un progetto di Visual Studio <a href="#creating-a-visual-studio-project" id="creating-a-visual-studio-project"></a>

Visual Studio è un potente IDE in cui è possibile creare un progetto, aggiungere riferimenti, creare file `.dlls` ed eseguire il debug. Quando si crea un nuovo progetto, in Visual Studio viene creata anche una soluzione, una struttura per l'organizzazione dei progetti. Più progetti possono coesistere all'interno di un'unica soluzione ed essere costruiti insieme. Per creare un nodo ZeroTouch, è necessario avviare un nuovo progetto di Visual Studio in cui scrivere una libreria di classi C# e creare un file `.dll`.

![Creazione di un nuovo progetto in Visual Studio](images/vs-new-project-1.jpg)

![Configurazione di un nuovo progetto in Visual Studio](images/vs-new-project-2.jpg)

> La finestra di un nuovo progetto in Visual Studio
>
> 1. Iniziare aprendo Visual Studio e creando un nuovo progetto: `File > New > Project`.
> 2. Scegliere il modello di progetto `Class Library`.
> 3. Assegnare un nome al progetto (abbiamo denominato il progetto MyCustomNode).
> 4. Impostare il percorso del file per il progetto. In questo esempio, lo lasceremo nella posizione di default.
> 5. Selezionare `Ok`.

In Visual Studio si creerà e si aprirà automaticamente un file C#. Dobbiamo assegnargli un nome appropriato, impostare l'area di lavoro e sostituire il codice di default con questo metodo di moltiplicazione.

```
 namespace MyCustomNode
 {
     public class SampleFunctions
     {
         public static double MultiplyByTwo(double inputNumber)
         {
             return inputNumber * 2.0;
         }
     }
 }
```

![Utilizzo di Esplora soluzioni](images/vs-edit-class.jpg)

> 1. Aprire le finestre Esplora soluzioni e Output da `Visualizza`.
> 2. Rinominare il file `Class1.cs` con `SampleFunctions.cs` in Esplora soluzioni a destra.
> 3. Aggiungere il codice riportato sopra per la funzione di moltiplicazione. Descriveremo le specifiche di come Dynamo leggerà le classi C# in un secondo momento.
> 4. Esplora soluzioni: consente di accedere a tutto ciò che è presente nel progetto.
> 5. La finestra Output: ci servirà in seguito per vedere se la creazione è andata a buon fine.

Il passaggio successivo consiste nella creazione del progetto, ma prima di procedere con questa operazione dobbiamo verificare alcune impostazioni. Assicurarsi innanzitutto che l'opzione `Any CPU` o `x64` sia selezionata in Platform target e che l'opzione `Prefer 32-bit` sia deselezionata in Project Properties.

![Impostazioni di creazione di Visual Studio](images/vs-build-settings.jpg)

> 1. Aprire le proprietà del progetto selezionando `Progetto > "NomeProgetto" > Properties`.
> 2. Selezionare la pagina `Compilazione`.
> 3. Selezionare `Any CPU` o `x64` dal menu a discesa.
> 4. Assicurarsi che l'opzione `Preferisci 32 bit` sia deselezionata.

Ora possiamo realizzare il progetto per creare un file `.dll`. A tale scopo, selezionare `Compila soluzione` dal menu `Compilazione` o utilizzare il tasto di scelta rapida `CTRL+SHIFT+B`.

![Creazione di una soluzione](images/vs-build.jpg)

> 1. Selezionare `Compilazione > Compila soluzione`.
> 2. È possibile determinare se il progetto è stato creato correttamente controllando la finestra Output.

Se il progetto è stato creato correttamente, nella cartella `bin` del progetto sarà presente un file `.dll` denominato `MyCustomNode`. Per questo esempio abbiamo lasciato il percorso del file del progetto di default di Visual Studio: `c:\users\username\documents\visual studio 2015\Projects`. Diamo un'occhiata alla struttura dei file del progetto.

![Struttura dei file del progetto](images/folder-structure.jpg)

> 1. La cartella `bin` contiene il file `.dll` creato da Visual Studio.
> 2. Il file del progetto di Visual Studio.
> 3. Il file della classe.
> 4. Poiché la configurazione della nostra soluzione è stata impostata su `Debug`, il file `.dll` verrà creato in `bin\Debug`.

Ora possiamo aprire Dynamo e importare il file `.dll`. Con la funzione Add-ons, accedere alla posizione `bin` del progetto e selezionare il file `.dll` da aprire.

![Apertura del file .dll del progetto](images/dyn-import-dll.jpg)

> 1. Selezionare il pulsante Add-ons per importare un file `.dll`.
> 2. Individuare la posizione del progetto. Il nostro progetto si trova nel percorso dei file di default di Visual Studio: `C:\Users\username\Documents\Visual Studio 2015\Projects\MyCustomNode`.
> 3. Selezionare il file `MyCustomNode.dll` da importare.
> 4. Fare clic su `Apri` per caricare il file `.dll`.

Se nella libreria viene creata una categoria denominata `MyCustomNode`, il file .dll è stato importato correttamente. Tuttavia, Dynamo ha creato due nodi da quello che voleva essere un singolo nodo. Nella sezione successiva spiegheremo perché ciò accade e come Dynamo legge un file .dll.

![Nodi personalizzati](images/dyn-customnode.jpg)

> 1. MyCustomNode nella libreria di Dynamo. La categoria Libreria è determinata dal nome `.dll`.
> 2. SampleFunctions.MultiplyByTwo nell'area di disegno.

### Modalità di lettura di classi e metodi in Dynamo <a href="#how-dynamo-reads-classes-and-methods" id="how-dynamo-reads-classes-and-methods"></a>

Quando Dynamo carica un file .dll, espone tutti i metodi statici pubblici come nodi. I costruttori, i metodi e le proprietà verranno convertiti rispettivamente nei nodi Create, Action e Query. Nel nostro esempio di moltiplicazione, il metodo `MultiplyByTwo()` diventa un nodo Action in Dynamo. Ciò è dovuto al fatto che il nome del nodo è stato assegnato in base al metodo e alla classe corrispondenti.

![Nodo SampleFunction.MultiplyByTwo in un grafico](images/multiplybytwo.png)

> 1. L'input viene denominato `inputNumber` in base al nome del parametro del metodo.
> 2. Per default, l'output viene denominato `double` perché è il tipo di dati restituito.
> 3. Il nodo viene denominato `SampleFunctions.MultiplyByTwo`, perché si tratta dei nomi della classe e del metodo.

Nell'esempio precedente, è stato creato il nodo Create `SampleFunctions` aggiuntivo perché non è stato fornito esplicitamente un costruttore e pertanto ne è stato creato uno automaticamente. Per evitare questo problema, è possibile creare un costruttore privato vuoto nella nostra classe `SampleFunctions`.

```
namespace MyCustomNode
{
    public class SampleFunctions
    {
        //The empty private constructor.
        //This will be not imported into Dynamo.
        private SampleFunctions() { }

        //The public multiplication method. 
        //This will be imported into Dynamo.
        public static double MultiplyByTwo(double inputNumber)
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Metodo importato come nodo Create](images/private-constructor.jpg)

> 1. Dynamo ha importato il metodo come nodo Create.

### Aggiunta di riferimenti ai pacchetti NuGet di Dynamo <a href="#adding-dynamo-nuget-package-references" id="adding-dynamo-nuget-package-references"></a>

Il nodo di moltiplicazione è molto semplice e non sono necessari riferimenti a Dynamo. Se si desidera accedere ad una qualsiasi delle funzionalità di Dynamo per creare, ad esempio, la geometria, sarà necessario fare riferimento ai pacchetti NuGet di Dynamo.

* [ZeroTouchLibrary](https://www.nuget.org/packages/DynamoVisualProgramming.ZeroTouchLibrary/2.0.0-beta3026): pacchetto per la creazione di librerie di nodi zero touch per Dynamo che contiene le seguenti librerie: DynamoUnits.dll, ProtoGeometry.dll
* [WpfUILibrary](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026): pacchetto per la creazione di librerie di nodi per Dynamo con interfaccia utente personalizzata in WPF che contiene le seguenti librerie: DynamoCoreWpf.dll, CoreNodeModels.dll, CoreNodeModelWpf.dll
* [DynamoServices](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026): libreria di DynamoServices per Dynamo
* [Core](https://www.nuget.org/packages/DynamoVisualProgramming.Core/2.0.0-beta3026): infrastruttura di unit test e test di sistema per Dynamo che contiene le seguenti librerie: DSIronPython.dll, DynamoApplications.dll, DynamoCore.dll, DynamoInstallDetective.dll, DynamoShapeManager.dll, DynamoUtilities.dll, ProtoCore.dll, VMDataBridge.dll
* [Test](https://www.nuget.org/packages/DynamoVisualProgramming.Tests/2.0.0-beta3026): infrastruttura di unit test e test di sistema per Dynamo che contiene le seguenti librerie: DynamoCoreTests.dll, SystemTestServices.dll, TestServices.dll
* [DynamoCoreNodes](https://www.nuget.org/packages/DynamoVisualProgramming.DynamoCoreNodes/2.0.0-beta3026): pacchetto per la creazione di nodi di base per Dynamo che contiene le seguenti librerie: Analysis.dll, GeometryColor.dll, DSCoreNodes.dll

Per fare riferimento a questi pacchetti in un progetto di Visual Studio, scaricare il pacchetto da NuGet nei collegamenti precedenti e fare riferimento manualmente ai file .dll oppure utilizzare Gestione pacchetti NuGet in Visual Studio. Per prima cosa possiamo illustrare come installarli con NuGet in Visual Studio.

![Apertura di Gestione pacchetti NuGet](images/vs-nuget-package-manager2.jpg)

> 1. Aprire Gestione pacchetti NuGet selezionando `Strumenti > Gestione pacchetti NuGet > Gestione pacchetti NuGet per la soluzione`.

È Gestione pacchetti NuGet. Questa finestra mostra i pacchetti installati per il progetto e consente all'utente di cercarne altri. Se viene rilasciata una nuova versione del pacchetto DynamoServices, è possibile aggiornare i pacchetti da questa posizione o ripristinare una versione precedente.

![Gestione pacchetti NuGet](images/vs-nuget-package-manager.jpg)

> 1. Selezionare Sfoglia e cercare DynamoVisualProgramming per visualizzare i pacchetti di Dynamo.
> 2. I pacchetti di Dynamo. Selezionandone uno, verranno mostrate la versione corrente e la descrizione del loro contenuto.
> 3. Selezionare la versione del pacchetto desiderata e fare clic su Installa. In questo modo viene installato un pacchetto per il progetto specifico in cui si sta lavorando. Poiché si sta utilizzando la release stabile più recente di Dynamo, la versione 1.3, scegliere la versione del pacchetto corrispondente.

Per aggiungere manualmente un pacchetto scaricato dal browser, aprire Gestione riferimenti da Esplora soluzioni e cercare il pacchetto.

![Reference Manager](images/vs-manual-dynamo-package.jpg)

> 1. Fare clic con il pulsante destro del mouse su `Riferimenti` e selezionare `Aggiungi riferimento`.
> 2. Selezionare `Sfoglia` per accedere alla posizione del pacchetto.

Ora che Visual Studio è configurato correttamente e abbiamo aggiunto un file `.dll` a Dynamo, abbiamo una solida base per i concetti futuri. Questo è solo l'inizio, quindi è bene continuare a seguirci per ulteriori informazioni su come creare un nodo personalizzato.
