# Logica

La **logica**, o più specificatamente, la **logica condizionale**, consente di specificare un'azione o un gruppo di azioni in base ad un test. Dopo aver valutato il test, sarà presente un valore booleano che rappresenta `True` o `False` che è possibile utilizzare per controllare il flusso del programma.

### Valori booleani

Le variabili numeriche possono memorizzare un intero intervallo di numeri diversi. Le variabili booleane possono memorizzare solo due valori indicati come True o False, Yes o No, 1 o 0. Per eseguire i calcoli, raramente vengono utilizzati valori booleani a causa del loro intervallo limitato.

### Istruzioni condizionali

L'istruzione "If" è un concetto chiave nella programmazione: "Se _ciò_ è vero, allora accade _questo_, altrimenti succede _qualcos'altro_. L'azione risultante dell'istruzione è determinata da un valore booleano. Esistono diversi modi per definire un'istruzione "If" in Dynamo:

| Icona | Nome (Sintassi) | Input | Output |
| ----------------------------------------------- | ------------------------- | ----------------- | ------- |
| ![](<../images/5-3/3/If.jpg>) | If (**If**) | test, true, false | risultato |
| ![](../images/5-3/3/Formula.jpg) | Formula (**IF(x,y,z)**) | x, y, z | risultato |
| ![](<../images/5-3/3/Code Block.jpg>) | Code Block (**(x?y:z);**) | x? y, z | risultato |

Si esaminerà un breve esempio per ciascuno di questi tre nodi in azione utilizzando l'istruzione "If" condizionale.

In questa immagine, _Boolean_ è impostato su _True_, ovvero il risultato è una stringa che riporta: _"This is the result if true";._ I tre nodi che creano l'istruzione _If_ funzionano in modo identico in questo contesto.

![](<../images/5-3/3/logic - conditional statements 01 false.jpg>)

Anche in questo caso, i nodi funzionano in modo identico. Se _Boolean_ viene modificato in _False_, il risultato è il numero _Pi_, come definito nell'istruzione _If_ originale.

![x](<../images/5-3/3/logic - conditional statements 02 true.jpg>)

## Esercizio: Logica e geometria

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-3/3/Building Blocks of Programs - Logic.dyn" %}

### Parte I: Filtraggio di un elenco

1. Si utilizzerà la logica per separare un elenco di numeri in un elenco di numeri pari e un elenco di numeri dispari.

![](<../images/5-3/3/logic - exercise part I-01.jpg>)

> a. **Range:** aggiungere un intervallo di numeri all'area di disegno.
>
> b. **Number:** aggiungere tre nodi di numeri all'area di disegno. Il valore per ogni nodo Number deve essere: _0.000_ per _start_, _10.000_ per _end_ e _1.000_ per _step_.
>
> c. **Output: **l'output è un elenco di 11 numeri da 0 a 10.
>
> d. **% (modulo):** collegare **Range** a _x_ e _2.0_ a _y_. In questo modo viene calcolato il resto per ogni numero dell'elenco diviso per 2. L'output di questo elenco fornisce un elenco di valori alternati tra 0 e 1.
>
> e. **= = (test di uguaglianza):** aggiungere un test di uguaglianza all'area di disegno. Collegare l'output del _modulo_ all'input _x_ e _0.000_ all'input _y_.
>
> f. **Watch:** l'output del test di uguaglianza è un elenco di valori alternati tra true e false. Questi sono i valori utilizzati per separare le voci dell'elenco. _0_ (o _true_) rappresenta i numeri pari e (_1_ o _false_) rappresenta i numeri dispari.
>
> g. **List.FilterByBoolMask:** questo nodo filtra i valori in due elenchi diversi in base al valore booleano di input. Collegare il nodo _Range_ originale all'input _list_ e l'output del _test di uguaglianza_ all'input _mask_. L'output _in_ rappresenta valori true, mentre l'output _out_ rappresenta valori false.
>
> h. **Watch:** il risultato è ora un elenco di numeri pari e un elenco di numeri dispari. Sono stati utilizzati operatori logici per separare gli elenchi in modelli.

### Parte II: Dalla logica alla geometria

Partendo dalla logica stabilita nel primo esercizio, si applicherà questa configurazione ad un'operazione di modellazione.

2\. Partire dall'esercizio precedente con gli stessi nodi. Le uniche eccezioni (oltre a modificare il formato sono):

![](<../images/5-3/3/logic - exercise part II-01.jpg>)

> a. Utilizzare un nodo **Sequence** con questi valori di input.
>
> b. È stato scollegato l'input list da **List.FilterByBoolMask**. Questi nodi verranno messi da parte per ora, ma saranno presto utili più avanti nell'esercizio.

3\. Iniziare creando un gruppo separato del grafico come mostrato nell'immagine precedente. Questo gruppo di nodi rappresenta un'equazione parametrica per definire una curva di linea. Ecco alcune note:

![](<../images/5-3/3/logic - exercise part II-02.jpg>)

> a. Il primo **Number Slider** rappresenta la frequenza dell'onda, deve avere un minimo di 1, un massimo di 4 e un passo di 0.01.
>
> b. Il secondo **Number Slider** rappresenta l'ampiezza dell'onda, deve avere un minimo di 0, un massimo di 1 e un passo di 0.01.
>
> c. **PolyCurve.ByPoints:** se il diagramma dei nodi riportato sopra viene copiato, il risultato è una curva seno nella finestra di anteprima di Dynamo.

Il metodo indicato qui per gli input è questo: utilizzare i nodi Number per ottenere proprietà più statiche e i dispositivi di scorrimento numerici su quelle più flessibili. Si desidera mantenere l'intervallo di numeri originale definito all'inizio di questo passaggio. Tuttavia, la curva seno creata qui dovrebbe avere una certa flessibilità. È possibile spostare questi dispositivi di scorrimento per controllare l'aggiornamento della frequenza e dell'ampiezza della curva.

![](<../images/5-3/3/logic - exercise part II-03.gif>)

4\. Si passerà alla definizione, quindi si osserverà il risultato finale in modo da poter fare riferimento a quello che si sta ottenendo. I primi due passaggi vengono eseguiti separatamente. Ora si desidera collegarli. Si utilizzerà la curva seno di base per determinare la posizione dei componenti della cerniera e si utilizzerà la logica true/false per alternare tra caselle di piccole dimensioni e caselle più grandi.

![](<../images/5-3/3/logic - exercise part II-04.jpg>)

> a. **Math.RemapRange:** utilizzando la sequenza numerica creata nel passaggio 02, creare una nuova serie di numeri riassociando l'intervallo. I numeri originali del passaggio 01 sono compresi tra 0 e 100. Questi numeri variano da 0 a 1, rispettivamente in base agli input _newMin_ e _newMax_.

5\. Creare un nodo **Curve.PointAtParameter**, quindi collegare l'output **Math.RemapRange** del passaggio 04 come il relativo input _param_.

![](<../images/5-3/3/logic - exercise part II-05.jpg>)

Questo passaggio crea punti lungo la curva. I numeri sono stati riassociati da 0 a 1 perché l'input di _param_ sta cercando valori in questo intervallo. Un valore di _0_ rappresenta il punto iniziale, mentre un valore di _1_ rappresenta i punti finali. Tutti i numeri compresi vengono valutati nell'intervallo _\[0,1]_.

6\. Collegare l'output da **Curve.PointAtParameter** a **List.FilterByBoolMask** per separare l'elenco di indici pari e dispari.

![](<../images/5-3/3/logic - exercise part II-06.jpg>)

> a. **List.FilterByBoolMask:** collegare **Curve.PointAtParameter** del passaggio precedente all'input _list_.
>
> b. **Watch:** un nodo di controllo per _in_ e un nodo di controllo per _out_ indicano che sono presenti due elenchi che rappresentano indici pari e indici dispari. Questi punti sono ordinati nello stesso modo sulla curva, che è possibile mostrare nel passaggio successivo.

7\. Quindi, si utilizzerà il risultato dell'output di **List.FilterByBoolMask** nel passaggio 05 per generare geometrie con dimensioni in base ai relativi indici.

**Cuboid.ByLengths:** ricreare i collegamenti visti nell'immagine riportata sopra per ottenere una cerniera lungo la curva seno. Un cuboide qui è solo un parallelepipedo e si sta definendo la sua dimensione in base al punto della curva al centro del parallelepipedo stesso. La logica della divisione pari/dispari dovrebbe ora essere chiara nel modello.

![](<../images/5-3/3/logic - exercise part II-07.jpg>)

> a. Elenco di cuboidi in corrispondenza di indici pari.
>
> b. Elenco di cuboidi in corrispondenza di indici dispari

Voila! È stato appena programmato un processo per definire le quote della geometria in base all'operazione logica illustrata in questo esercizio.
