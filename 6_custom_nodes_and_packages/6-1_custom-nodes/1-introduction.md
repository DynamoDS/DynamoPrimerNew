# Introduzione ai nodi personalizzati

I nodi personalizzati vengono costruiti nidificando altri nodi e nodi personalizzati all'interno di un "nodo personalizzato di Dynamo", che è possibile immaginare concettualmente come un contenitore. Quando viene eseguito questo nodo contenitore nel grafico, tutto ciò che si trova all'interno verrà eseguito per consentire di riutilizzare e condividere una combinazione utile di nodi.

### Adattamento alla modifica

Quando nel grafico sono presenti più copie di un nodo personalizzato, è possibile aggiornarle tutte modificando il nodo personalizzato di base. Ciò consente di aggiornare il grafico in modo semplice adattandolo a eventuali modifiche che potrebbero verificarsi nel workflow o nella progettazione.

### Condivisione del lavoro

Probabilmente la migliore funzionalità dei nodi personalizzati è la loro capacità di condivisione del lavoro. Se un utente avanzato crea un grafico di Dynamo complesso e lo consegna ad un progettista che non conosce Dynamo, può condensare il grafico agli elementi essenziali per l'interazione progettuale. Il nodo personalizzato può essere aperto per modificare il grafico interno, ma il "contenitore" può essere mantenuto semplice. Con questo processo, i nodi personalizzati consentono agli utenti di Dynamo di progettare un grafico ordinato e intuitivo.

![](<../images/6-1/1/custom node intro - work sharing 01.jpg>)

### Molti modi per creare un nodo

Esistono svariati modi per creare nodi personalizzati in Dynamo. Negli esempi riportati in questo capitolo, verranno creati nodi personalizzati direttamente dall'interfaccia utente di Dynamo. Se si è programmatori e si è interessati alla formattazione C# o zero-touch, è possibile fare riferimento a [questa pagina ](https://github.com/DynamoDS/Dynamo/wiki/How-To-Create-Your-Own-Nodes)nella Wiki di Dynamo per una revisione più approfondita.

### Ambiente di nodi personalizzati e creazione del primo nodo personalizzato

Si passerà all'ambiente dei nodi personalizzati e si creerà un nodo semplice per calcolare una percentuale. L'ambiente dei nodi personalizzati è diverso dall'ambiente dei grafici di Dynamo, ma l'interazione è fondamentalmente la stessa. Detto questo, procedere creando il primo nodo personalizzato.

Per creare un nodo personalizzato da zero, avviare Dynamo e selezionare Nodo personalizzato oppure digitare CTRL+MAIUSC+N nell'area di disegno.

![](<../images/6-1/1/custom node intro - custom node environment 01.jpg>)

Assegnare un nome, una descrizione e una categoria nella finestra di dialogo Proprietà nodo personalizzato.

![](<../images/6-1/1/custom node intro - custom node environment 02.jpg>)

> 1. **Nome:** Percentage
> 2. **Descrizione**: Calculate the percentage of one value in relation to another
> 3. **Categoria:** Math.Functions

Verrà aperta un'area di disegno con uno sfondo giallo, ad indicare che si sta lavorando all'interno di un nodo personalizzato. In questa area di disegno è possibile accedere a tutti i nodi di Dynamo principali, nonché ai nodi Input e Output, che etichettano il flusso di dati in entrata e in uscita nel nodo personalizzato. Sono disponibili in Input>Basic.

![](<../images/6-1/1/custom node intro - custom node environment 03.jpg>)

![](<../images/6-1/1/custom node intro - custom node environment 04.jpg>)

> 1. **Input:** i nodi di input creano porte di input nel nodo personalizzato. La sintassi per un nodo di input è _input\_name : datatype = default\_value(optional)._
> 2. **Output:** simili agli input, creeranno e assegneranno nomi alle porte di output nel nodo personalizzato. È possibile aggiungere un **commento personalizzato** alle porte di input e output per inserire un suggerimento nei tipi Input e Output. Questo argomento è descritto più dettagliatamente nella sezione [Creazione di nodi personalizzati](2-creating.md).

È possibile salvare questo nodo personalizzato come file .dyf (anziché come file .dyn standard) e verrà aggiunto automaticamente alla sessione corrente e alle sessioni future. Il nodo personalizzato è reperibile nella libreria dalla sezione Add-ons.

![](<../images/6-1/1/custom node intro - custom node environment 05.jpg>)

### Sezioni successive

Dopo aver creato il primo nodo personalizzato, nelle sezioni successive si approfondiranno la funzionalità dei nodi personalizzati e si spiegherà come pubblicare workflow generici. Nella seguente sezione verrà illustrato lo sviluppo di un nodo personalizzato che trasferisce la geometria da una superficie ad un'altra.
