# Pubblicazione di un pacchetto

### Pubblicazione di un pacchetto <a href="#publish-a-package" id="publish-a-package"></a>

I pacchetti rappresentano un modo comodo per memorizzare e condividere i nodi con la comunità di Dynamo. Un pacchetto può contenere tutti gli elementi, dai nodi personalizzati creati nell'area di lavoro di Dynamo ai nodi derivati NodeModel. I pacchetti vengono pubblicati e installati utilizzando Package Manager. Oltre a questa pagina, la [Guida introduttiva](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) include una guida generale sui pacchetti.

#### Che cos'è Package Manager? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Dynamo Package Manager è un Registro di sistema del software (simile a npm) accessibile da Dynamo o in un browser Web. Package Manager include l'installazione, la pubblicazione, l'aggiornamento e la visualizzazione di pacchetti. Come npm, mantiene diverse versioni dei pacchetti. Inoltre, consente di gestire le dipendenze del progetto.

Nel browser, cercare i pacchetti e visualizzare le statistiche: [https://dynamopackages.com/](https://dynamopackages.com).

* In Dynamo, Package Manager include i pacchetti di installazione, pubblicazione e aggiornamento.

![Ricerca di pacchetti](images/dynamopackagemanager.jpg)

> 1. Cercare i pacchetti in linea: `Pacchetti > Ricerca pacchetti`.
> 2. Visualizzare/Modificare i pacchetti installati: `Pacchetti > Gestisci pacchetti`.
> 3. Pubblicare un nuovo pacchetto: `Pacchetti > Pubblica nuovo pacchetto`.

#### Pubblicazione di un pacchetto <a href="#publishing-a-package" id="publishing-a-package"></a>

I pacchetti vengono pubblicati da Package Manager all'interno di Dynamo. Il processo consigliato consiste nel pubblicare localmente, verificare il pacchetto e quindi pubblicarlo in linea per condividerlo con la comunità. Utilizzando il case study NodeModel, verranno eseguiti i passaggi necessari per pubblicare il nodo RectangularGrid come pacchetto localmente e quindi in linea.

Avviare Dynamo e selezionare `Pacchetti > Pubblica nuovo pacchetto... ` per aprire la finestra `Pubblica un pacchetto`.

![Pubblicazione di un pacchetto](images/dyn-publish-package-add-files.jpg)

> 1. Selezionare `Aggiungi file` per cercare i file da aggiungere al pacchetto.
> 2. Selezionare i due file `.dll` dal case study NodeModel.
> 3. Selezionare `Ok`.

Con i file aggiunti al contenuto del pacchetto, assegnare al pacchetto un nome, una descrizione e una versione. La pubblicazione di un pacchetto utilizzando Dynamo crea automaticamente un file `pkg.json`.

![Impostazioni del pacchetto](images/dyn-publish-package.jpg)

> Un pacchetto pronto per essere pubblicato.
>
> 1. Fornire le informazioni richieste per nome, descrizione e versione.
> 2. Pubblicare facendo clic su Pubblica localmente e selezionare la cartella del pacchetto di Dynamo: `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages` per rendere disponibile il nodo in Core. Pubblicare sempre localmente il pacchetto fino a quando non è pronto per la condivisione.

Dopo la pubblicazione di un pacchetto, i nodi saranno disponibili nella libreria di Dynamo nella categoria `CustomNodeModel`.

![Pacchetto nella libreria di Dynamo](images/dyn-publish-package-library.jpg)

> 1. Il pacchetto appena creato nella libreria di Dynamo

Quando il pacchetto è pronto per la pubblicazione in linea, aprire Package Manager e scegliere `Pubblica`, quindi `Pubblica in linea`.

![Pubblicazione di un pacchetto in Package Manager](images/dyn-publish-package-directory.jpg)

> 1. Per vedere come Dynamo ha formattato il pacchetto, fare clic sui tre punti verticali a destra di CustomNodeModel e scegliere Mostra directory principale.
> 2. Selezionare `Pubblica`, quindi `Pubblica in linea` nella finestra Pubblica un pacchetto di Dynamo.
> 3. Per eliminare un pacchetto, selezionare `Elimina`.

#### Come aggiornare un pacchetto <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

L'aggiornamento di un pacchetto è un processo simile alla pubblicazione. Aprire Package Manager, selezionare `Pubblica versione...` nel pacchetto che deve essere aggiornato e immettere una versione successiva.

![Pubblicazione della versione di un pacchetto](images/dyn-publish-package-version.jpg)

> 1. Selezionare `Pubblica versione...` per aggiornare un pacchetto esistente con nuovi file nella directory principale, quindi scegliere se deve essere pubblicato localmente o in linea.

#### Client Web di Package Manager <a href="#package-manager-web-client" id="package-manager-web-client"></a>

Il client Web di Package Manager viene utilizzato esclusivamente per la ricerca e la visualizzazione dei dati relativi ai pacchetti, come ad esempio le versioni e le statistiche di download.

Il client Web di Package Manager è accessibile tramite il seguente collegamento: [https://dynamopackages.com/](https://dynamopackages.com).

![Client Web di Package Manager](images/packagemanager-browser.jpg)
