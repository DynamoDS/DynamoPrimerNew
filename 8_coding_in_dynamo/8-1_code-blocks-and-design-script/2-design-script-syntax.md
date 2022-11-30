# Sintassi di DesignScript

Nei nomi dei nodi di Dynamo è possibile aver notato un tema comune: ogni nodo utilizza una sintassi _"."_ senza spazi. Questo perché il testo nella parte superiore di ogni nodo rappresenta la sintassi effettiva per lo scripting e _"."_ (o _notazione con punto_) separa un elemento dai metodi possibili che è possibile chiamare. In questo modo è possibile creare una facile conversione dallo scripting visivo nello scripting basato su testo.

![Nomi di nodi](../images/8-1/2/apple.jpg)

Come analogia generale per la notazione con punto, in che modo è possibile gestire una mela parametrica in Dynamo? Di seguito sono riportati alcuni metodi che si eseguiranno sulla mela prima di decidere di mangiarla (nota: questi non sono metodi di Dynamo effettivi):

| Testo leggibile dall'utente                 | Notazione con punto              | Output |
| ------------------------------ | ------------------------- | ------ |
| Di che colore è la mela?       | Apple.color               | rosso    |
| La mela è matura?             | Apple.isRipe              | true   |
| Quanto pesa la mela? | Apple.weight              | 170 gr  |
| Da dove è venuta la mela? | Apple.parent              | albero   |
| Cosa crea la mela?    | Apple.children            | semi  |
| Questa mela è cresciuta localmente?   | Apple.distanceFromOrchard | circa 96 km |

Non so quello che pensate, ma a giudicare dagli output nella tabella riportata sopra, sembra una mela gustosa. Il risultato è _Apple.eat()_.

### Notazione con punto in Code Block

Tenendo presente l'analogia con la mela, si esaminerà _Point.ByCoordinates_ e sarà mostrato com'è possibile creare un punto utilizzando Code Block.

La sintassi di _Code Block_ `Point.ByCoordinates(0,10);` fornisce lo stesso risultato di un nodo _Point.ByCoordinates_ in Dynamo, tranne per il fatto che è possibile creare un punto utilizzando un nodo. Ciò è più efficiente rispetto al collegamento di un nodo separato in _X_ e _Y_.

![](../images/8-1/2/codeblockdotnotation.jpg)

> 1. Utilizzando _Point.ByCoordinates_ in Code Block, si specificano gli input nello stesso ordine del nodo predefinito _(X,Y)_.

### Chiamata di nodi - Crea, Azioni, Query

È possibile chiamare qualsiasi nodo normale nella libreria tramite Code Block, purché il nodo non sia un _nodo "UI"_ speciale: quelli con una funzionalità speciale dell'interfaccia utente. Ad esempio, è possibile chiamare _Circle.ByCenterPointRadius_, ma non sarebbe molto utile chiamare un nodo _Watch 3D_.

I nodi regolari (nella maggior parte della libreria) in genere sono di tre tipi. Si scoprirà che la libreria è organizzata in base a queste categorie. I metodi, o nodi, di questi tre tipi vengono trattati in modo diverso quando vengono richiamati all'interno di Code Block.

![](../images/8-1/2/actioncreatequerycategory.jpg)

> 1. **Crea**: consente di creare (o costruire) un elemento.
> 2. **Azione**: consente di eseguire un'azione su un elemento.
> 3. **Query**: consente di ottenere una proprietà di un elemento già esistente.

#### Crea

La categoria Crea costruirà la geometria da zero. I valori vengono immessi in Code Block da sinistra a destra. Questi input sono nello stesso ordine degli input nel nodo dall'alto verso il basso.

Confrontando il nodo _Line.ByStartPointEndPoint_ e la sintassi corrispondente in Code Block, si ottengono gli stessi risultati.

![](../images/8-1/2/create.jpg)

#### Azione

Un'azione è un'operazione che si esegue su un oggetto di quel tipo. In Dynamo si utilizza la _notazione con punto_, comune a molti linguaggi di codifica, per applicare un'azione ad un elemento. Una volta ottenuto l'elemento, digitare un punto, quindi il nome dell'azione. L'input del metodo di tipo Azione viene inserito tra parentesi, come i metodi di tipo Crea, solo che non è necessario specificare il primo input visualizzato nel nodo corrispondente. È necessario specificare invece l'elemento su cui si sta eseguendo l'azione:

![](../images/8-1/2/DesignScript-action.jpg)

> 1. Il nodo **Point.Add** è un nodo di tipo Azione, pertanto la sintassi funziona in modo leggermente diverso.
> 2. Gli input sono (1) _Point_ e (2) _Vector_ da aggiungere ad esso. In **Code Block**, è stato denominato il punto (l'elemento) _"pt"_. Per aggiungere un vettore denominato *"vec" *a _"pt"_, è necessario scrivere _pt.Add(vec)_ o: elemento, punto, azione. L'azione Add presenta solo un input o tutti gli input del nodo **Point.Add** meno il primo. Il primo input per il nodo **Point.Add** è il punto stesso.

#### Query

I metodi di tipo Query consentono di ottenere una proprietà di un oggetto. Poiché l'oggetto stesso è l'input, non è necessario specificare alcun input. Non sono richieste parentesi.

![](../images/8-1/2/query.jpg)

### Informazioni sul collegamento

Il collegamento con i nodi è piuttosto diverso dal collegamento con il blocco di codice. Con i nodi, l'utente fa clic con il pulsante destro del mouse sul nodo e seleziona l'opzione Collegamento da eseguire. Con il blocco di codice, l'utente dispone di un maggiore controllo sulla modalità di strutturazione dei dati. Il metodo abbreviato del blocco di codice utilizza _guide di replica_ per impostare il modo in cui occorre associare diversi elenchi unidimensionali. I numeri tra parentesi angolari "<>" definiscono la gerarchia dell'elenco nidificato risultante: <1>,<2>,<3> e così via.

![](../images/8-1/2/DesignScript-lacing.jpg)

> 1. In questo esempio, si utilizza un metodo abbreviato per definire due intervalli (ulteriori informazioni sul metodo abbreviato saranno fornite nella sezione seguente di questo capitolo). In breve, `0..1;` è equivalente a `{0,1}` e `-3..-7` è equivalente a `{-3,-4,-5,-6,-7}`. Il risultato offre un elenco di 2 valori X e 5 valori Y. Se non si utilizzano le guide di replica con questi elenchi non corrispondenti, si ottiene un elenco di due punti, che rappresenta la lunghezza dell'elenco più breve. Utilizzando le guide di replica, è possibile trovare tutte le combinazioni possibili di 2 e 5 coordinate (o un collegamento Globale).
> 2. Utilizzando la sintassi **Point.ByCoordinates**`(x_vals<1>,y_vals<2>);` si ottengono _due_ elenchi con _cinque_ voci in ogni elenco.
> 3. Utilizzando la sintassi **Point.ByCoordinates**`(x_vals<2>,y_vals<1>);` si ottengono _cinque_ elenchi con _due_ voci in ogni elenco.

Con questa notazione, è possibile anche specificare quale elenco sarà dominante: 2 elenchi di 5 cose o 5 elenchi di 2 cose. Nell'esempio, la modifica dell'ordine delle guide di replica restituisce come risultato un elenco di righe di punti o un elenco di colonne di punti in una griglia.

### Nodo da aggiungere al codice

Sebbene i metodi del blocco di codice riportati sopra possano richiedere del tempo per acquisire familiarità, in Dynamo è disponibile una funzionalità denominata Nodo da aggiungere al codice che facilita il processo. Per utilizzare questa funzionalità, selezionare una serie di nodi nel grafico di Dynamo, fare clic con il pulsante destro del mouse sull'area di disegno e selezionare Nodo da aggiungere al codice. In Dynamo vengono compressi questi nodi in un blocco di codice, con tutti gli input e gli output. Questo non è solo un ottimo strumento per apprendere il blocco di codice, ma consente anche di utilizzare un grafico di Dynamo più efficiente e parametrico. Per concludere l'esercizio riportato di seguito, utilizzare Nodo da aggiungere al codice.

![](../images/8-1/2/DesignScript-nodetocode.jpg)

## Esercizio: Attrattore di superficie

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/8-1/2/Dynamo-Syntax_Attractor-Surface.dyn" %}

Per mostrare l'efficacia del blocco di codice, verrà convertita una definizione di campo attrattore esistente nel formato blocco di codice. L'utilizzo di una definizione esistente dimostra come il blocco di codice si correla allo scripting visivo ed è utile per apprendere la sintassi di DesignScript.

Iniziare ricreando la definizione nell'immagine riportata sopra (o aprendo il file di esempio).

![](../images/8-1/2/DesignScript-exercise-01.jpg)

> 1. Notare che il collegamento in **Point.ByCoordinates** è stato impostato su _Globale_.
> 2. Ogni punto di una griglia viene spostato verso l'alto nella direzione Z in base alla sua distanza dal punto di riferimento.
> 3. Viene ricreata e ispessita una superficie, creando una bombatura nella geometria rispetto alla distanza dal punto di riferimento.

![](../images/8-1/2/DesignScript-exercise-02.jpg)

> 1. Partendo dall'inizio, definire innanzitutto il punto di riferimento: **Point.ByCoordinates**`(x,y,0);`. Viene utilizzata la stessa sintassi di **Point.ByCoordinates** specificata nella parte superiore del nodo del punto di riferimento.
> 2. Le variabili _x_ e _y_ vengono inserite in **Code Block** in modo che sia possibile aggiornarle dinamicamente con i dispositivi di scorrimento.
> 3. Aggiungere alcuni _dispositivi di scorrimento_ agli input di **Code Block** che vanno da -50 a 50. In questo modo, è possibile estendersi in tutta la griglia di default di Dynamo.

![](../images/8-1/2/DesignScript-exercise-03.jpg)

> 1. Nella seconda riga di **Code Block**, definire una sintassi abbreviata per sostituire il nodo della sequenza numerica: `coordsXY = (-50..50..#11);`. Se ne discuterà più in dettaglio nella prossima sezione. Per il momento, notare che questo metodo abbreviato è equivalente al nodo **Number Sequence** nello script visivo.

![](../images/8-1/2/DesignScript-exercise-04.jpg)

> 1. A questo punto, si desidera creare una griglia di punti dalla sequenza _coordsXY_. A tale scopo, si desidera utilizzare la sintassi di **Point.ByCoordinates**, ma è anche necessario avviare un collegamento _Globale_ dell'elenco nello stesso modo di quello utilizzato nello script visivo. A tale scopo, digitare la riga: `gridPts = Point.ByCoordinates(coordsXY<1>,coordsXY<2>,0);`. Le parentesi angolari indicano il riferimento Globale.
> 2. Notare nel nodo **Watch3D** che è presente una griglia di punti nella griglia di Dynamo.

![](../images/8-1/2/DesignScript-exercise-05.jpg)

> 1. Ora per la parte complessa: si desidera spostare la griglia di punti verso l'alto in base alla loro distanza dal punto di riferimento. Innanzitutto, denominare questo nuovo gruppo di punti _transPts_. E poiché la traslazione è un'azione su un elemento esistente, anziché utilizzare `Geometry.Translate...`, utilizzare `gridPts.Translate`.
> 2. Leggendo dal nodo effettivo nell'area di disegno, si noterà che sono presenti tre input. La geometria da traslare è già stata dichiarata perché si sta eseguendo l'azione su quell'elemento (con _gridPts.Translate_). I due input rimanenti verranno inseriti tra le parentesi della funzione: direction e _distance_.
> 3. direction è abbastanza semplice. Per spostarla verticalmente, utilizzare `Vector.ZAxis()`.
> 4. distance tra il punto di riferimento e ogni punto della griglia deve ancora essere calcolata, pertanto è necessario eseguire questa operazione come azione per il punto di riferimento nello stesso modo: `refPt.DistanceTo(gridPts)`.
> 5. La riga di codice finale fornisce i punti traslati: `transPts=gridPts.Translate(Vector.ZAxis(),refPt.DistanceTo(gridPts));`.

![](../images/8-1/2/DesignScript-exercise-06.jpg)

> 1. Ora si ha una griglia di punti con la struttura di dati appropriata per creare una superficie NURBS. Costruire la superficie utilizzando `srf = NurbsSurface.ByControlPoints(transPts);`.

![](../images/8-1/2/DesignScript-exercise-07.jpg)

> 1. E infine, per aggiungere profondità alla superficie, costruire un solido utilizzando `solid = srf.Thicken(5);`. In questo caso, la superficie è stata ispessita di 5 unità nel codice, ma la si potrebbe sempre dichiarare come variabile (ad esempio, chiamandola thickness) e poi controllare quel valore con un dispositivo di scorrimento.

#### Semplificazione del grafico con Nodo da aggiungere al codice

La funzionalità Nodo da aggiungere al codice consente di automatizzare l'intero esercizio appena completato facendo clic su un pulsante. Questa funzione non solo è efficiente per la creazione di definizioni personalizzate e blocchi di codice riutilizzabili, ma è anche uno strumento molto utile per apprendere come eseguire lo script in Dynamo:

![](../images/8-1/2/DesignScript-exercise-08.jpg)

> 1. Iniziare con lo script visivo esistente del passaggio 1 dell'esercizio. Selezionare tutti i nodi, fare clic con il pulsante destro del mouse sull'area di disegno e selezionare _Nodo da aggiungere al codice_. È semplicissimo.

Dynamo dispone di una versione automatizzata basata su testo del grafico visivo, del collegamento e di tutto il resto. Verificare tutto questo negli script visivi e sfruttare la potenza del blocco di codice.

![](../images/8-1/2/DesignScript-exercise-09.jpg)
