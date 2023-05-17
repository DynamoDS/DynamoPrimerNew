# Interfaccia utente

### Panoramica sull'interfaccia utente

L'interfaccia utente per Dynamo è organizzata in cinque aree principali. Verrà illustrata brevemente la panoramica qui e verranno descritte ulteriormente l'area di lavoro e la Libreria nelle sezioni seguenti.

![](images/userinterface-ui.jpg)

> 1. Menu
> 2. Barra degli strumenti
> 3. Libreria
> 4. Area di lavoro
> 5. Barra di esecuzione

### Menu

![](../.gitbook/assets/userinterface-menu\(1\).jpg)

Di seguito sono riportati i menu per le funzionalità di base dell'applicazione Dynamo. Come la maggior parte del software Windows, i primi due menu sono relativi alla gestione dei file, alle operazioni di selezione e modifica del contenuto. I menu rimanenti sono più specifici di Dynamo.

#### Menu di Dynamo

Informazioni generali e impostazioni sono disponibili nel menu a discesa di **Dynamo**.

![](images/userinterface-dynamomenu.jpg)

> 1. Informazioni su: indica la versione di Dynamo installata nel computer in uso.
> 2. Contratto per raccogliere dati di usabilità: consente di aderire o meno alla condivisione dei dati utente per migliorare Dynamo.
> 3. Preferenze: include impostazioni quali la definizione della precisione decimale e della qualità di rendering della geometria dell'applicazione.
> 4. Chiudi Dynamo.

#### Guida

Se si è bloccati, consultare il menu **?**. È possibile accedere ad uno dei siti Web di riferimento di Dynamo tramite il browser Internet.

![](images/userinterface-helpmenu.jpg)

> 1. Introduzione: mostra una breve introduzione all'utilizzo di Dynamo.
> 2. Guide interattive -
> 3. Esempi: fornisce file di esempio di riferimento.
> 4. Dizionario di Dynamo: rappresenta una risorsa con documentazione su tutti i nodi.
> 5. Sito Web di Dynamo: consente di visualizzare il progetto di Dynamo su GitHub.
> 6. Wiki progetto di Dynamo: consente di visitare la pagina Wiki per informazioni sullo sviluppo mediante l'API di Dynamo, gli strumenti e le librerie di supporto.
> 7. Visualizza pagina iniziale: consente di tornare alla pagina iniziale di Dynamo quando ci si trova all'interno di un documento.
> 8. Segnala un bug: consente di aprire un problema in GitHub.

### Barra degli strumenti

La barra degli strumenti di Dynamo contiene una serie di pulsanti per l'accesso rapido ai file, nonché i comandi Annulla [CTRL+Z] e Ripeti [CTRL+Y]. All'estrema destra è presente un altro pulsante che consente di esportare un'istantanea dell'area di lavoro, che è estremamente utile per la documentazione e la condivisione.

* ![](images/userinterface-newfile.jpg) Nuovo: consente di creare un nuovo file .dyn.
* \![](<images/userinterface-open(1) (1).jpg>) Apri: consente di aprire un file .dyn (area di lavoro) o .dyf (nodo personalizzato) esistente.
* ![](images/userinterface-save.jpg) Salva/Salva con nome: consente di salvare il file .dyn o .dyf attivo.
* ![](images/userinterface-undo.jpg) Annulla: consente di annullare l'ultima azione.
* ![](images/userinterface-redo.jpg) Ripeti: consente di ripetere l'azione successiva.
* ![](images/userinterface-screenshot.jpg) Esporta area di lavoro come immagine: consente di esportare l'area di lavoro visibile come file PNG.

### Libreria

La Libreria di Dynamo è una raccolta di librerie funzionali, ciascuna contenente nodi raggruppati per categoria. È costituita da librerie di base che vengono aggiunte durante l'installazione di default di Dynamo. Nel corso dell'introduzione del suo l'utilizzo, verrà illustrato come estendere le funzionalità di base con nodi personalizzati e pacchetti aggiuntivi. La sezione [2-library.md](2-library.md "mention") fornirà una guida più dettagliata sull'utilizzo di questa funzionalità.

![](images/userinterface-library.jpg)

### Area di lavoro

L'area di lavoro è il luogo dove vengono creati i programmi visivi; è possibile modificare anche l'impostazione Anteprima per visualizzare le geometrie 3D da qui. Per ulteriori dettagli, fare riferimento a [1-workspace.md](1-workspace.md "mention").

![](images/userinterface-workspace.gif)

### Barra di esecuzione

Eseguire lo script di Dynamo da qui. Fare clic sull'icona dell'elenco a discesa sul pulsante di esecuzione per passare da una modalità all'altra.

![](images/userinterface-executionbar.gif)

* Automatico: consente di eseguire automaticamente lo script. Le modifiche vengono aggiornate in tempo reale.
* Manuale: lo script viene eseguito solo quando si fa clic sul pulsante Esegui. È utile per quando si apportano modifiche a script complessi e "pesanti".
* Periodico: questa opzione è disattivata per default. È disponibile solo quando viene utilizzato il nodo DateTime.Now. È possibile impostare l'esecuzione automatica del grafico ad un intervallo specificato.

![](images/userinterface-executionbarDateTimenode.jpg)
