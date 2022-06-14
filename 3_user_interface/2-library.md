# Libreria

La Libreria contiene tutti i nodi caricati, compresi i dieci nodi delle categorie di default forniti con l'installazione, nonché eventuali pacchetti o nodi personalizzati caricati aggiuntivi. I nodi della Libreria sono organizzati in modo gerarchico all'interno di librerie, categorie e, se necessario, sottocategorie.

![](<images/3-2/library - library UI.jpg>)

* Nodi di base: vengono forniti con l'installazione di default.
* Nodi personalizzati: consentono di memorizzare le routine o il grafico speciale utilizzati più di frequente come nodi personalizzati. È inoltre possibile condividere i nodi personalizzati con la community.
* Nodi di Package Manager: raccolta di nodi personalizzati pubblicati.

Si esamineranno le categorie della [gerarchia dei nodi](3-3\_dynamo\_libraries.md#library-hierarchy-for-categories), si mostreranno come eseguire [ricerche rapide dalla libreria](3-3\_dynamo\_libraries.md#quick-search-in-library) e si apprenderanno delle informazioni su alcuni dei [nodi utilizzati più di frequente](3-3\_dynamo\_libraries.md#frequently-used-nodes) tra di essi.

### Gerarchia di librerie per le categorie

Sfogliare queste categorie è il modo più rapido per comprendere la gerarchia di ciò che è possibile aggiungere all'area di lavoro e il modo migliore per scoprire nuovi nodi non utilizzati in precedenza.

Sfogliare la Libreria facendo clic sui menu per espandere ogni categoria e la relativa sottocategoria.

{% hint style="info" %}
Geometry è un menu di grande utilità per iniziare ad esplorare poiché contiene la maggior quantità di nodi.
{% endhint %}

![](<images/3-2/library  - modified and resize library categories.jpg>)

> 1. Libreria
> 2. Categoria
> 3. Sottocategoria
> 4. Nodo

Questi consentono di suddividere ulteriormente i nodi nella stessa sottocategoria in base al fatto se i nodi **creano** dei dati, eseguono un'**Azione** o una **Query** sui dati.

* ![](<images/3-2/user interface - create.jpg>) **Crea**: consente di creare o costruire la geometria da zero. Ad esempio, un cerchio.
* ![](<images/3-2/user interface - action.jpg>) **Azione**: consente di eseguire un'azione su un oggetto. Ad esempio, la messa in scala di un cerchio.
* ![](<images/3-2/user interface - query.jpg>) **Query**: consente di ottenere una proprietà di un oggetto già esistente. Ad esempio, ottenere il raggio di un cerchio.

Posizionare il cursore del mouse su un nodo per visualizzare informazioni più dettagliate oltre al nome e all'icona. Questo offre un modo rapido per comprendere cosa fa il nodo, cosa richiederà per gli input e cosa verrà fornito come output.

![](<images/3-2/user interface - node description.jpg>)

> 1. Descrizione: descrizione con linguaggio normale del nodo
> 2. Icona: versione più grande dell'icona nel menu Libreria
> 3. Input: nome, tipo di dati e struttura di dati
> 4. Output: tipo di dati e struttura

### Ricerca rapida nella Libreria

Se si conosce con relativa specificità il nodo che si desidera aggiungere all'area di lavoro, digitare nel campo di **ricerca** per cercare tutti i nodi corrispondenti.

Scegliere facendo clic sul nodo che si desidera aggiungere o premere INVIO per aggiungere i nodi evidenziati al centro dell'area di lavoro.

![](<images/3-2/user interface - search.jpg>)

#### Ricerca per gerarchia

Oltre a utilizzare le parole chiave per cercare di trovare i nodi, è possibile digitare la gerarchia separata con un punto nel campo di ricerca o con i Code Block (che utilizzano il _linguaggio testuale di Dynamo_).

La gerarchia di ogni libreria si riflette nel nome dei nodi aggiunti all'area di lavoro.

Digitando parti differenti della posizione del nodo nella gerarchia della Libreria nel formato `library.category.nodeName`, vengono restituiti risultati diversi:

* `library.category.nodeName`

![](<images/3-2/library - search by hierarchy geometry point by coordinates (1).jpg>)

* `category.nodeName`

![](<images/3-2/library - search by hierarchy 2 point by coordinates.jpg>)

* `nodeName` o `keyword`

![](<images/3-2/library - search by hierarchy 3 by coordinates.jpg>)

In genere, il nome del nodo nell'area di lavoro verrà sottoposto a rendering nel formato `category.nodeName`, con alcune eccezioni significative, in particolare nelle categorie Input e View.

Tenere presente i nodi denominati in modo simile e osservare la differenza della categoria:

* I nodi della maggior parte delle librerie includeranno il formato della categoria.

![](<images/3-2/library - node category differences 1.jpg>)

* `Point.ByCoordinates` e `UV.ByCoordinates` hanno lo stesso nome ma provengono da categorie differenti.

![](<images/3-2/library - node category differences 2.jpg>)

* Eccezioni importanti includono funzioni integrate, Core.Input, Core.View e operatori.

![](<images/3-2/library - node category differences 3.jpg>)

### Nodi utilizzati di frequente

Con centinaia di nodi inclusi nell'installazione di base di Dynamo, quali sono essenziali per lo sviluppo dei programmi visivi? Ci si concentrerà su quelli che consentono di definire i parametri del programma (**Input**), vedere i risultati dell'azione di un nodo (**Watch**) e definire gli input o le funzionalità mediante una scorciatoia (**Code Block**).

#### Nodi di input

I nodi di input sono il mezzo principale per l'utente del programma visivo, sia che si tratti dell'utente corrente sia di qualcun altro, per interfacciarsi con i parametri chiave. Di seguito sono riportate alcune informazioni disponibili nella Libreria principale:

| Nodo |                                                | Nodo |                                                |
| -------------- | ---------------------------------------------- | -------------- | ---------------------------------------------- |
| Boolean | ![](<images/3-2/library - boolean.jpg>) | Number | ![](<images/3-2/library - number.jpg>) |
| String | ![](<images/3-2/library - string.jpg>) | Number Slider | ![](<images/3-2/library - number slider.jpg>) |
| Directory Path | ![](<images/3-2/library - directory path.jpg>) | Integer Slider | ![](<images/3-2/library - integer slider.jpg>) |
| File Path | ![](<images/3-2/library - file path.jpg>) |                |                                                |

#### Watch e Watch 3D

I nodi Watch sono essenziali per gestire i dati che fluiscono nel programma visivo. È possibile visualizzare il risultato di un nodo tramite l'**anteprima dei dati del nodo** posizionando il cursore del mouse sul nodo.

![](<images/3-2/library - node preview.jpg>)

Sarà utile per mantenere la visualizzazione in un nodo **Watch**.

![](<images/3-2/library - watch node.jpg>)

In alternativa, è possibile visualizzare i risultati della geometria tramite un nodo **Watch 3D**.

![](<images/3-2/library - watch3d node.gif>)

Entrambi sono disponibili nella categoria View della libreria Core.

{% hint style="info" %}
Suggerimento Talvolta l'anteprima 3D può distrarre l'utente quando il programma visivo contiene molti nodi. Per visualizzare l'anteprima della geometria, è consigliabile deselezionare l'opzione Mostra anteprima sfondo 3D nel menu Impostazioni e utilizzare un nodo Watch 3D.
{% endhint %}

#### Code Block

I nodi Code Block possono essere utilizzati per definire un blocco di codice con righe separate da punti e virgola. Può essere semplice come `X/Y`.

È inoltre possibile utilizzare i Code Block come scorciatoia per definire un input numerico o chiamare la funzionalità di un altro nodo. La sintassi per eseguire questa operazione segue la convenzione di denominazione del linguaggio testuale di Dynamo, [DesignScript](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md).

Di seguito è disponibile una semplice dimostrazione (con le istruzioni) per l'utilizzo di Code Block nello script.

![](<images/3-2/library - code block demo.gif>)

1. Fare doppio clic per creare un nodo Code Block.
2. `Circle.ByCenterPointRadius(x,y);`Tipo
3. Fare clic sull'area di lavoro per annullare la selezione e aggiungere automaticamente gli input `x` e `y`.
4. Creare un nodo Point.ByCoordinates e Number Slider, quindi collegarli agli input di Code Block.
5. Il risultato dell'esecuzione del programma visivo viene mostrato come cerchio nell'anteprima 3D.
