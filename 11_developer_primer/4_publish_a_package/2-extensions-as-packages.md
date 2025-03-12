# Estensioni come pacchetti

### Estensioni come pacchetti <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### Panoramica <a href="#overview" id="overview"></a>

È possibile eseguire l'installazione client delle estensioni di Dynamo nel gestore di pacchetti come le normali librerie di nodi di Dynamo. Quando un pacchetto installato contiene un'estensione della vista, l'estensione viene caricata in fase di esecuzione al caricamento di Dynamo. È possibile controllare la console di Dynamo per verificare che l'estensione sia stata caricata correttamente.

### Struttura di un pacchetto <a href="#package-structure" id="package-structure"></a>

La struttura di un pacchetto di estensione è la stessa di un normale pacchetto contenente...

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

Si supponga di aver già creato l'estensione; si avrà (almeno) un assieme NET e un file manifesto. L'assieme deve contenere una classe che implementa `IViewExtension` o `IExtension`. Il file XML manifesto indica a Dynamo la classe di cui creare un'istanza per avviare l'estensione. Affinché il gestore di pacchetti possa individuare correttamente l'estensione, il file manifesto deve corrispondere in modo preciso alla posizione e alla denominazione dell'assieme.

Posizionare eventuali file di assiemi nella cartella `bin` e il file manifesto nella cartella `extra`. In questa cartella è possibile inserire anche eventuali risorse aggiuntive.

Esempio di file XML manifesto:

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### Caricamento <a href="#uploading" id="uploading"></a>

Una volta creata una cartella contenente le sottodirectory descritte in precedenza, è possibile eseguire il push (caricamento) nel gestore di pacchetti. Una cosa di cui tenere conto è che attualmente non è possibile pubblicare pacchetti da Dynamo Sandbox. Ciò significa che è necessario utilizzare Dynamo for Revit. Una volta all'interno di Dynamo for Revit, individuare Pacchetti => Pubblica nuovo pacchetto. In questo modo, l'utente dovrà accedere all'Autodesk Account a cui desidera associare il pacchetto.

A questo punto dovrebbe comparire la normale finestra di pubblicazione del pacchetto, dove verranno compilati tutti i campi obbligatori relativi al pacchetto/all'estensione. Esiste un passaggio aggiuntivo **molto importante** che richiede di assicurarsi che nessuno dei file di assiemi sia contrassegnato come libreria di nodi. A tale scopo, fare clic con il pulsante destro del mouse sui file importati (la cartella del pacchetto creata in precedenza). Verrà visualizzato un menu contestuale che consente di selezionare (o deselezionare) questa opzione. Tutti gli assiemi dell'estensione dovrebbero essere deselezionati.

![Pubblicazione di un pacchetto](images/ViewExtension_Search.png)

Prima di pubblicare in linea, si dovrebbe sempre pubblicare localmente per assicurarsi che tutto funzioni come previsto. Una volta verificato questo aspetto, si è pronti per l'attivazione selezionando Pubblica in linea.

### Pull <a href="#pulling" id="pulling"></a>

Per verificare che il pacchetto sia stato caricato correttamente, si dovrebbe essere in grado di cercarlo in base alla denominazione e alle parole chiave specificate nel passaggio di pubblicazione. Infine, è importante notare che le stesse estensioni richiederanno un riavvio di Dynamo prima di funzionare. In genere, queste estensioni richiedono parametri specificati all'avvio di Dynamo.

![Ricerca di pacchetti](images/ViewExtension_Search.jpg)
