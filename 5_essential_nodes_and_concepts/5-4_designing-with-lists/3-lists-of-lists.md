# Elenchi di elenchi

### Elenchi di elenchi

Si aggiunge un altro livello alla gerarchia. Se si prende il mazzo di schede dell'esempio originale e si crea una scatola che contiene più mazzi, la scatola ora rappresenta un elenco di mazzi e ogni mazzo rappresenta un elenco di carte. Questo è un elenco di elenchi. Per analogia in questa sezione, l'immagine riportata di seguito contiene un elenco di rotoli di monete e ogni rotolo contiene un elenco di penny.

![Monete](../images/5-4/3/coins-521245\_640.jpg)

> Foto di [Dori](https://commons.wikimedia.org/wiki/File:Stack\_of\_coins\_0214.jpg).

### Query

Quali **query** è possibile eseguire dall'elenco di elenchi? Consente di accedere alle proprietà esistenti.

* Numero di tipi di moneta? 2\.
* Valori del tipo di moneta? $0.01 e $0.25.
* Materiale dei quarti? Rame al 75% e nichel al 25%.
* Materiale dei penny? Zinco al 97,5% e rame al 2,5%.

### Azione

Quali **azioni** è possibile eseguire nell'elenco di elenchi? In questo modo si modifica l'elenco di elenchi in base ad una determinata operazione.

* Selezionare una pila specifica di quarti o penny.
* Selezionare un quarto o un penny specifico.
* Ridisporre le pile di quarti e penny.
* Mischiare le pile insieme.

Anche in questo caso, Dynamo dispone di un nodo analogico per ciascuna delle operazioni riportate sopra. Poiché si sta lavorando con dati astratti e non con oggetti fisici, occorre una serie di regole per controllare il modo in cui ci spostiamo verso l'alto e verso il basso nella gerarchia dei dati.

Quando si utilizzano elenchi di elenchi, i dati sono stratificati e complessi, ma questo offre l'opportunità di eseguire alcune incredibili operazioni parametriche. Si esamineranno i principi fondamentali e si discuterà di alcune altre operazioni nelle lezioni riportate di seguito.

## Esercizio

### Gerarchia dall'alto verso il basso

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/Top-Down-Hierarchy.dyn" %}

Il concetto fondamentale da apprendere da questa sezione: **Dynamo tratta gli elenchi come oggetti di per sé**. Questa gerarchia dall'alto verso il basso viene sviluppata tenendo a mente la programmazione orientata agli oggetti. Anziché selezionare elementi secondari con un comando quale **List.GetItemAtIndex**, Dynamo selezionerà tale indice dell'elenco principale nella struttura di dati. E quell'elemento può essere un altro elenco. Verrà esaminato dettagliatamente con un'immagine di esempio:

![Dall'alto verso il basso](../images/5-4/3/listsoflists-topdownhierachy.jpg)

> 1. Con **Code Block**, sono stati definiti due intervalli: `0..2; 0..3;`
> 2. Questi intervalli sono connessi ad un nodo **Point.ByCoordinates** con il collegamento impostato su _Globale_. In questo modo viene creata una griglia di punti e viene inoltre restituito un elenco di elenchi come output.
> 3. Notare che il nodo **Watch** fornisce 3 elenchi con 4 elementi in ogni elenco.
> 4. Quando si utilizza **List.GetItemAtIndex**, con un indice 0, Dynamo seleziona il primo elenco e tutto il relativo contenuto. Altri programmi possono selezionare il primo elemento di ogni elenco nella struttura di dati, ma Dynamo utilizza una gerarchia dall'alto verso il basso durante la gestione dei dati.

### List.Flatten

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/Flatten.dyn" %}

Livella rimuove tutti i livelli di dati da una struttura di dati. Ciò è utile quando le gerarchie dei dati non sono necessarie per l'operazione desiderata, ma può essere rischioso perché rimuove le informazioni. L'esempio seguente mostra il risultato della riduzione di livelli di un elenco di dati.

![Esercizio](../images/5-4/3/listsoflists-flatten01.jpg)

> 1. Inserire una riga di codice per definire un intervallo in **Code Block**: `-250..-150..#4;`
> 2. Se si inserisce il _blocco di codice_ nell'input _x_ e _y_ di un nodo **Point.ByCoordinates**, è necessario impostare il collegamento su _Globale_ per ottenere una griglia di punti.
> 3. Il nodo **Watch** mostra che è presente un elenco di elenchi.
> 4. Un nodo **PolyCurve.ByPoints** farà riferimento a ciascun elenco e creerà la rispettiva PolyCurve. Nell'anteprima di Dynamo, sono presenti quattro PolyCurve che rappresentano ogni riga della griglia.

![Esercizio](../images/5-4/3/listsoflists-flatten02.jpg)

> 1. Inserendo un _riduzione di livelli_ prima del nodo PolyCurve, è stato creato un singolo elenco per tutti i punti. Il nodo **PolyCurve.ByPoints** fa riferimento ad un elenco per creare una curva. Poiché tutti i punti si trovano in un elenco, si otterrà una PolyCurve zig-zag che corre in tutto l'elenco di punti.

Sono inoltre disponibili opzioni per la riduzione di livelli isolati di dati. Utilizzando il nodo **List.Flatten**, è possibile definire un numero impostato di livelli di dati da ridurre nella parte superiore della gerarchia. Questo è uno strumento molto utile se sono presenti strutture di dati complesse che non sono necessariamente rilevanti per il workflow. Un'altra opzione consiste nell'utilizzare il nodo Flatten come funzione in **List.Map**. Ulteriori informazioni su **List.Map** sono disponibili di seguito.

### Suddivisione

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/Chop.dyn" %}

Quando si utilizza la modellazione parametrica, talvolta può essere utile modificare la strutture di dati in un elenco esistente. Anche per questo sono disponibili molti nodi, mentre la suddivisione è la versione più semplice. Con la suddivisione, è possibile suddividere un elenco in sottoelenchi con un determinato numero di elementi.

Il comando di suddivisione suddivide gli elenchi in base ad una determinata lunghezza dell'elenco. In alcuni modi, il comando di suddivisione è l'opposto della riduzione di livelli: anziché rimuovere la struttura di dati, ad esso vengono aggiunti nuovi livelli. Questo è uno strumento utile per operazioni geometriche come l'esempio seguente.

![Esercizio](../images/5-4/3/listsoflists-chop.jpg)

### List.Map

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/Map.dyn" %}

**List.Map/Combine** applica una funzione impostata ad un elenco di input, ma ad un livello inferiore nella gerarchia. Le combinazioni sono identiche a quelle delle mappe, tranne per il fatto che le combinazioni possono avere più input corrispondenti all'input di una funzione specificata.

_Nota Questo esercizio è stato creato con una versione precedente di Dynamo. In gran parte il funzionamento di_ **List.Map** _è stato risolto con l'aggiunta della funzionalità_ **List@Level**_. Per ulteriori informazioni, vedere_ [_List@Level_](6-3\_lists-of-lists.md#listlevel) _di seguito._

Come introduzione rapida, si esaminerà il nodo **List.Count** di una sezione precedente.

Il nodo **List.Count** conteggia tutti gli elementi di un elenco. Verrà utilizzato per illustrare il funzionamento di **List.Map**.

![](../images/5-4/3/listsoflists-map01.jpg)

> 1.  Inserire due righe di codice in **Code Block**: `-50..50..#Nx; -50..50..#Ny;`
>
>     Dopo aver digitato questo codice, il blocco di codice creerà due input per Nx e Ny.
> 2. Con due _Integer Slider_, definire i valori _Nx_ e _Ny_ collegandoli a **Code Block**.
> 3. Collegare ogni riga del blocco di codice ai rispettivi input _X_ e _Y_ di un nodo **Point.ByCoordinates**. Fare clic con il pulsante destro del mouse sul nodo, selezionare Collegamento e scegliere _Globale_. In questo modo viene creata una griglia di punti. Poiché è stato definito l'intervallo da -50 a 50, la griglia di Dynamo di default viene estesa.
> 4. Un nodo _**Watch**_ mostra i punti creati. Notare la struttura di dati. È stato creato un elenco di elenchi. Ogni elenco rappresenta una riga di punti della griglia.

![Esercizio](../images/5-4/3/listsoflists-map02(1).jpg)

> 1. Associare un nodo **List.Count** all'output del nodo Watch del passaggio precedente.
> 2. Collegare un nodo **Watch** all'output **List.Count**.

Notare che il nodo List.Count fornisce un valore pari a 5. Questo valore è uguale alla variabile "Nx" come definito nel blocco di codice. Perché?

* Innanzitutto, il nodo **Point.ByCoordinates** utilizza l'input "x" come input principale per la creazione di elenchi. Quando Nx è 5 e Ny è 3, viene visualizzato un elenco di 5 elenchi, ciascuno con 3 elementi.
* Poiché Dynamo tratta gli elenchi come oggetti di per sé, all'elenco principale della gerarchia viene applicato un nodo **List.Count**. Il risultato è un valore di 5 o il numero di elenchi nell'elenco principale.

![Esercizio](../images/5-4/3/listsoflists-map03.jpg)

> 1. Utilizzando un nodo **List.Map**, si scende di un livello nella gerarchia ed è possibile eseguire una _"function"_ a questo livello.
> 2. Notare che il nodo **List.Count** non contiene input. Viene utilizzato come funzione, pertanto il nodo **List.Count** verrà applicato a ogni singolo elenco, scendendo di un livello nella gerarchia. L'input vuoto di **List.Count** corrisponde all'input dell'elenco di **List.Map**.
> 3. I risultati di **List.Count** ora forniscono un elenco di 5 elementi, ciascuno con un valore pari a 3. Rappresenta la lunghezza di ogni sottoelenco.

### **List.Combine**

_Nota Questo esercizio è stato creato con una versione precedente di Dynamo. In gran parte, il funzionamento di List.Combine è stato risolto con l'aggiunta della funzionalità_ **List@Level**_. Per ulteriori informazioni, vedere_ [_List@Level_](6-3\_lists-of-lists.md#listlevel) _di seguito._

In questo esercizio, si utilizzerà **List.Combine** per dimostrare come tale nodo può essere utilizzato per applicare una funzione in elenchi separati di oggetti.

Iniziare impostando due elenchi di punti.

![Esercizio](../images/5-4/3/listsoflists-combined01.jpg)

> 1. Utilizzare il nodo **Sequence** per generare 10 valori, ciascuno con un incremento di 10 passi.
> 2. Collegare il risultato all'input x di un nodo **Point.ByCoordinates**. In questo modo verrà creato un elenco di punti in Dynamo.
> 3. Aggiungere un secondo nodo **Point.ByCoordinates** all'area di lavoro, utilizzare lo stesso output **Sequence** dell'input x, ma utilizzare un **Interger Slider** come input y e impostarne il valore su 31 (può essere qualsiasi valore, purché non si sovrapponga al primo gruppo di punti) in modo che i 2 gruppi di punti non si sovrappongano l'uno sull'altro.

Successivamente, verrà utilizzato **List.Combine** per applicare una funzione agli oggetti in 2 elenchi separati. In questo caso, si tratta di una funzione della linea di disegno semplice.

![Esercizio](../images/5-4/3/listsoflists-combined02.jpg)

> 1. Aggiungere **List.Combine** all'area di lavoro e collegare i 2 gruppi di punti come il relativo input list0 e list1.
> 2. Utilizzare un **Line.ByStartPointEndPoint** come funzione di input per **List.Combine**.

Una volta completato, i 2 gruppi di punti vengono compressi/associati tramite una funzione **Line.ByStartPointEndPoint** e restituiscono 10 righe in Dynamo.

{% hint style="info" %} Fare riferimento all'esercizio in Elenchi n-dimensionali per vedere un altro esempio di utilizzo di List.Combine. {% endhint %}

### List@Level

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/Listatlevel.dyn" %}

Preferita a **List.Map**, la funzionalità **List@Level** consente di selezionare direttamente il livello di elenco che si desidera utilizzare nella porta di input del nodo. Questa funzionalità può essere applicata a qualsiasi input in entrata di un nodo e consentirà di accedere ai livelli degli elenchi più rapidamente e più facilmente rispetto ad altri metodi. È sufficiente indicare al nodo il livello dell'elenco che si desidera utilizzare come input e lasciare che il nodo faccia il resto.

In questo esercizio, si utilizzerà la funzionalità **List@Level** per isolare un livello specifico di dati.

![List@Level](../images/5-4/3/listsoflists-listatlevel01.jpg)

Si inizierà con una semplice griglia di punti 3D.

> 1. Poiché la griglia è costruita con un intervallo per X, Y e Z, i dati sono strutturati in 3 ordini: un elenco X, un elenco Y e un elenco Z.
> 2. Questi ordini esistono in **livelli** diversi. I livelli sono indicati nella parte inferiore del simbolo circolare di anteprima. Le colonne dei livelli di elenco corrispondono ai dati dell'elenco riportati sopra per aiutare ad identificare il livello da utilizzare.
> 3. I livelli di elenco sono organizzati in ordine inverso, in modo che i dati del livello più basso siano sempre in "L1". In questo modo si garantisce che i grafici funzionino come previsto, anche se qualcosa è cambiato a monte.

![List@Level](../images/5-4/3/listsoflists-listatlevel02.jpg)

> 1. Per utilizzare la funzione **List@Level**, fare clic su '>'. All’interno di questo menu, verranno visualizzate due caselle di controllo.
> 2. **Usa livelli**: attiva la funzionalità **List@Level**. Dopo aver fatto clic su questa opzione, sarà possibile fare clic e selezionare i livelli di elenco di input che si desidera utilizzare per il nodo. Con questo menu, è possibile provare rapidamente diverse opzioni di livello facendo clic verso l'alto o verso il basso.
> 3. _Mantieni struttura elenco_: se questa opzione è attivata, sarà possibile mantenere la struttura dei livelli dell'input. Talvolta, è possibile organizzare i dati in sottoelenchi in modo specifico. Selezionando questa opzione, è possibile mantenere intatta l'organizzazione degli elenchi senza perdere alcuna informazione.

Grazie alla semplice griglia 3D, è possibile accedere alla struttura dell'elenco e visualizzarla attivando e disattivando i livelli di elenchi. Ogni combinazione di indice e livello di elenco restituirà un gruppo di punti diverso dell'insieme 3D originale.

![](../images/5-4/3/listsoflists-listatlevel03.jpg)

> 1. "@L2" in DesignScript consente di selezionare solo l'elenco al livello 2. L'elenco al livello 2 con l'indice 0 include solo il primo gruppo di punti Y, restituendo solo la griglia XZ.
> 2. Se si modifica il filtro del livello in "L1", sarà possibile vedere tutto nel primo livello di elenco. L'elenco al livello 1 con indice 0 include tutti i punti 3D in un elenco non strutturato.
> 3. Se si prova la stessa procedura per "L3", sarà possibile vedere solo i punti del terzo livello di elenco. L'elenco al livello 3 con l'indice 0 include solo il primo gruppo di punti Z, restituendo solo una griglia XY.
> 4. Se si prova la stessa procedura per "L4", sarà possibile vedere solo i punti del terzo livello di elenco. L'elenco al livello 4 con l'indice 0 include solo il primo gruppo di punti X, restituendo solo una griglia YZ.

Sebbene questo esempio particolare possa essere creato anche con **List.Map**, **List@Level** semplifica notevolmente l'interazione, semplificando così l'accesso ai dati dei nodi. Di seguito è riportato un confronto tra i metodi **List.Map** e **List@Level**:

![](../images/5-4/3/listsoflists-listatlevel04.jpg)

> 1. Sebbene entrambi i metodi consentano di accedere agli stessi punti, il metodo **List@Level** ci consente di passare facilmente da un livello di dati all'altro all'interno di un singolo nodo.
> 2. Per accedere ad una griglia di punti con **List.Map**, sarà necessario disporre di un nodo **List.GetItemAtIndex** insieme a **List.Map**. Per ogni livello di elenco inferiore, sarà necessario utilizzare un nodo **List.Map** aggiuntivo. A seconda della complessità degli elenchi, potrebbe essere necessario aggiungere una quantità significativa di nodi **List.Map** al grafico per accedere al livello di informazioni appropriato.
> 3. In questo esempio, un nodo **List.GetItemAtIndex** con un nodo **List.Map** restituisce lo stesso gruppo di punti con la stessa struttura dell'elenco di **List.GetItemAtIndex** con l'opzione "@L3" selezionata.

### Trasponi

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/Transpose.dyn" %}

Trasponi è una funzione fondamentale quando si lavora con elenchi di elenchi. Come nei programmi con fogli di calcolo, una trasposizione inverte le colonne e le righe di una struttura di dati. Verrà mostrata questa funzione con una matrice di base di seguito e nella seguente sezione sarà illustrato come utilizzare un trasposizione per creare relazioni geometriche.

![Trasponi](../images/5-4/3/transpose1.jpg)

Si eliminano i nodi **List.Count** dell'esercizio precedente e si passa ad una geometria per vedere la struttura di dati.

![](../images/5-4/3/listsoflists-transpose01.jpg)

> 1. Collegare un nodo **PolyCurve.ByPoints** all'output del nodo Watch da **Point.ByCoordinates**.
> 2. L'output mostra 5 PolyCurve e le curve vengono visualizzate nell'anteprima di Dynamo. Il nodo di Dynamo sta cercando un elenco di punti (o un elenco di elenchi di punti in questo caso) e sta creando una singola PolyCurve da essi. Essenzialmente, ogni elenco è stato convertito in una curva nella struttura di dati.

![](../images/5-4/3/listsoflists-transpose02.jpg)

> 1. Un nodo **List.Transpose** sposterà tutti gli elementi con tutti gli elenchi in un elenco di elenchi. Ciò risulta complicato, ma si tratta della stessa logica della trasposizione in Microsoft Excel: il passaggio tra colonne e righe in una struttura di dati.
> 2. Notare il risultato astratto: la trasposizione ha modificato la struttura dell'elenco da 5 elenchi con 3 elementi ciascuno a 3 elenchi con 5 elementi ciascuno.
> 3. Notare il risultato geometrico: utilizzando **PolyCurve.ByPoints**, si ottengono 3 PolyCurve nella direzione perpendicolare alle curve originali.

## Code Block per la creazione di elenchi

La sintassi abbreviata del blocco di codice utilizza "[]" per definire un elenco. Questo è un modo molto più rapido e più fluido per creare un elenco rispetto al nodo **List.Create**. **Code Block** viene descritto in modo più dettagliato in [Code Block e DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/). Fare riferimento all'immagine seguente per osservare come è possibile definire un elenco con più espressioni con un blocco di codice.

![](../images/5-4/3/listsoflists-codeblockforlistcreation01.jpg)

#### Query sul blocco codice

La sintassi abbreviata di **Code Block** utilizza "[]" come metodo rapido e semplice per selezionare gli elementi specifici desiderati da una struttura di dati complessa. I **Code Block** vengono descritti in modo più dettagliato nel [capitolo Code Block e DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/). Fare riferimento all'immagine seguente per osservare come è possibile eseguire query su un elenco con più tipi di dati con un blocco di codice.

![](../images/5-4/3/listsoflists-codeblockforlistcreation02.jpg)

## Esercizio - Esecuzione di query e inserimento di dati

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-4/3/ReplaceItems.dyn" %}

In questo esercizio viene utilizzata una parte della logica stabilita in un esercizio precedente per modificare una superficie. L'obiettivo è intuitivo, ma la navigazione nella struttura di dati risulterà più impegnativa. Si desidera articolare una superficie spostando un punto di controllo.

Iniziare con la stringa di nodi riportata sopra. Si sta creando una superficie di base che estende la griglia di default di Dynamo.

![](../images/5-4/3/listoflists-exercisecbinsert&query01.jpg)

> 1. Utilizzando **Code Block**, inserire queste due righe di codice e collegarle rispettivamente agli input _u_ e _v_ di **Surface.PointAtParameter**: `-50..50..#3;` `-50..50..#5;`.
> 2. Assicurarsi di impostare il collegamento di **Surface.PointAtParameter** su _Globale_.
> 3. Il nodo **Watch** mostra che è presente un elenco di tre elenchi, ciascuno con 5 elementi.

In questo passaggio, si desidera eseguire una query sul punto centrale nella griglia creata. Per eseguire questa operazione, selezionare il punto centrale nell'elenco centrale. Ha senso, giusto?

![](../images/5-4/3/listoflists-exercisecbinsert&query02.jpg)

> 1. Per confermare che si tratta del punto corretto, è anche possibile fare clic sugli elementi del nodo Watch per confermare che è quello corretto.
> 2. Utilizzando **Code Block**, si scriverà una riga di codice di base per l'esecuzione di una query su un elenco di elenchi:\
 `points[1][2];`.
> 3. Utilizzando **Geometry.Translate**, si sposterà il punto selezionato verso l'alto nella direzione _Z_ di _20_ unità.

![](../images/5-4/3/listoflists-exercisecbinsert&query03.jpg)

> 1. Selezionare anche la riga centrale dei punti con un nodo **List.GetItemAtIndex**. Nota Analogamente a un passaggio precedente, è anche possibile eseguire una query sull'elenco con **Code Block**, utilizzando una riga di `points[1];`.

Finora è stata eseguita correttamente la query sul punto centrale ed è stato spostato verso l'alto. Ora è necessario reinserire il punto spostato nella struttura di dati originale.

![](../images/5-4/3/listoflists-exercisecbinsert&query04.jpg)

> 1. Innanzitutto, si desidera sostituire l'elemento dell'elenco isolato in un passaggio precedente.
> 2. Utilizzando **List.ReplaceItemAtIndex**, si sostituirà l'elemento centrale utilizzando un indice di _"2"_, con l'elemento sostitutivo collegato al punto spostato (**Geometry.Translate**).
> 3. L'output mostra che è stato inserito il punto spostato nell'elemento centrale dell'elenco.

Dopo aver modificato l'elenco, è necessario reinserire l'elenco nella struttura di dati originale, ovvero l'elenco di elenchi.

![](../images/5-4/3/listoflists-exercisecbinsert&query05.jpg)

> 1. Seguendo la stessa logica, utilizzare **List.ReplaceItemAtIndex** per sostituire l'elenco centrale con l'elenco modificato.
> 2. Notare che i **Code Block**__ che definiscono l'indice per questi due nodi sono 1 e 2, che corrisponde alla query originale di **Code Block** (_points[1][2]_).
> 3. Selezionando l'elenco in corrispondenza dell'_index 1_, viene visualizzata la struttura di dati evidenziata nell'anteprima di Dynamo. Il punto spostato è stato unito correttamente nella struttura di dati originale.

Esistono molti modi per creare una superficie da questo gruppo di punti. In questo caso, si creerà una superficie eseguendo il loft delle curve insieme.

![](../images/5-4/3/listoflists-exercisecbinsert&query06.jpg)

> 1. Creare un nodo **NurbsCurve.ByPoints** e collegare la nuova struttura di dati per creare tre curve NURBS.

![](../images/5-4/3/listoflists-exercisecbinsert&query07.jpg)

> 1. Collegare un nodo **Surface.ByLoft** all'output di **NurbsCurve.ByPoints**. Ora è presente una superficie modificata. È possibile modificare il valore _Z_ originale della geometria. Eseguire la traslazione e osservare l’aggiornamento della geometria.
