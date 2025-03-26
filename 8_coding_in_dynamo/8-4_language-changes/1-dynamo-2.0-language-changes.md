# Modifiche al linguaggio

Nella presente sezione è fornita una panoramica degli aggiornamenti e delle modifiche apportate al linguaggio in Dynamo in ogni versione. Queste modifiche possono influire sulla funzionalità, sulle prestazioni e sull'utilizzo e questa guida aiuterà gli utenti a capire quando e perché adattarsi a questi aggiornamenti.

## Modifiche al linguaggio di Dynamo 2.0

1. La sintassi di list@level è stata cambiata passando da "@-1" a "@L1".
* È stata progettata una nuova sintassi per list@level, per utilizzare list@L1 invece di list@-1.
* La motivazione è quella di allineare la sintassi del codice con l'anteprima/interfaccia utente; i test degli utenti dimostrano che la nuova sintassi è più comprensibile.

2. Sono stati implementati i tipi Int e Double in TS per allinearli ai tipi di Dynamo.

3. Non sono consentite funzioni in overload in cui gli argomenti differiscono solo in base alla cardinalità.
* Per default, i grafici precedenti che utilizzano overload rimossi dovrebbero essere impostati sugli overload con classificazione più alta.
* La motivazione è quella di rimuovere l'ambiguità su quale funzione specifica viene eseguita.

4. È stato disattivato l'innalzamento di livello delle matrici con le guide per la replica.

5. Le variabili nei blocchi imperativi sono state rese locali rispetto all'ambito del blocco imperativo.
* I valori delle variabili definiti all'interno di blocchi di codice imperativi non verranno influenzati dalle modifiche apportate all'interno di blocchi imperativi che vi fanno riferimento.  

6. Le variabili sono state rese immutabili per disattivare l'aggiornamento associativo nei nodi di blocchi di codice.

7. Tutti i nodi dell'interfaccia utente vengono compilati come metodi statici.

8. Le dichiarazioni restituite sono supportate senza assegnazione.
* Il segno "=" non è necessario né nelle definizioni di funzione né nel codice imperativo.

9. È stata eseguita la migrazione dei nomi dei metodi precedenti nei nodi di blocchi di codice.
* Molti nodi sono stati rinominati per migliorare la leggibilità e il posizionamento nell'interfaccia utente del Browser libreria.

10. Viene utilizzato l'elenco come correzione del dizionario.

----
Problemi noti:
- I conflitti dello spazio dei nomi nei blocchi imperativi causano la visualizzazione di porte di input impreviste. Per ulteriori informazioni, vedere il [problema di Github](https://github.com/DynamoDS/Dynamo/issues/8796). Per ovviare a questo problema, definire la funzione all'esterno del blocco imperativo in questo modo:
```
pnt = Autodesk.Point.ByCoordinates;
lne = Autodesk.Line.ByStartPointEndPoint;

[Imperative]
{
    x = 1;
    start = pnt(0,0,0);
    end = pnt(x,x,x);
    line = lne(start,end);
    return = line;
};
```

## Spiegazione delle modifiche al linguaggio di Dynamo 2.0

Sono stati apportati numerosi miglioramenti al linguaggio per la release di Dynamo 2.0. Tra le motivazioni principali c'è la semplificazione del linguaggio. L'enfasi è stata posta sul rendere DesignScript più comprensibile e semplice da utilizzare a favore di una potenza e una flessibilità maggiori, con l'obiettivo di migliorare la comprensibilità per l'utente finale.

Di seguito è riportato l'elenco delle modifiche apportate alla versione 2.0:
* La sintassi di List@Level è stata semplificata.
* I metodi in overload con parametri che differiscono solo in base alla classificazione non sono validi. 
* Tutti i nodi dell'interfaccia utente vengono compilati come metodi statici.
* L'innalzamento di livello dell'elenco è disattivato quando viene utilizzato con le guide per la replica/il collegamento.
* Le variabili nei blocchi associativi sono immutabili per impedire l'aggiornamento associativo.
* Le variabili dei blocchi imperativi sono collocate nell'ambito imperativo.
* Esiste una separazione tra elenchi e dizionari.

## 1\. Sintassi di list@level semplificata 

È stata progettata una nuova sintassi per list@level, per utilizzare `list@L1` invece di `list@-1` ![](../images/8-4/1/lang2_1.png)


## 2\. Le funzioni in overload con parametri che differiscono solo in base alla classificazione non sono valide
Le funzioni in overload sono problematiche per diversi motivi:
* Una funzione in overload indicata da un nodo dell'interfaccia utente nel grafico potrebbe non corrispondere all'overload eseguito in fase di runtime.
* La risoluzione del metodo è costosa e non è adatta alle funzioni in overload.
* È difficile comprendere il comportamento della replica per le funzioni in overload.

Prendiamo `BoundingBox.ByGeometry` come esempio: c'erano due funzioni in overload nelle versioni precedenti di Dynamo, che come argomento richiedevano un valore singolo e un elenco di geometrie rispettivamente:
```
BoundingBox BoundingBox.ByGeometry(geometry: Geometry) {...}
BoundingBox BoundingBox.ByGeometry(geometry: Geometry[]) {...}
```
Se l'utente eliminava il primo nodo nell'area di disegno e collegava un elenco di geometrie, si aspettava che venisse avviata la replica, ma ciò non si verificava mai perché in fase di runtime veniva chiamato il secondo overload come mostrato qui: ![](../images/8-4/1/lang2_2.png)
 
Nella versione 2.0 non sono consentite funzioni in overload che differiscono solo nella cardinalità dei parametri per questo motivo. Ciò significa che per le funzioni in overload che hanno lo stesso numero e gli stessi tipi di parametri ma hanno uno o più parametri che differiscono solo per la classificazione, l'overload definito per primo ha sempre la precedenza, mentre gli altri vengono scartati dal compilatore. Il vantaggio principale di questa operazione è quello di semplificare la logica di risoluzione del metodo disponendo di un percorso rapido per selezionare le funzioni candidate.

Nella libreria della geometria per la versione 2.0, il primo overload nell'esempio `BoundingBox.ByGeometry` è stato eliminato e il secondo è stato mantenuto, quindi se il nodo è destinato alla replica, cioè utilizzato nel contesto del primo, dovrebbe essere utilizzato con l'opzione di collegamento più breve (o più lunga) o in un blocco di codice con le guide per la replica: 
```
BoundingBox.ByGeometry(geometry<1>);
```
In questo esempio possiamo vedere che il nodo con classificazione superiore può essere utilizzato sia in una chiamata replicata che in una non replicata e pertanto è sempre preferibile ad un overload con classificazione inferiore. Come regola generale, pertanto, **si consiglia sempre agli autori dei nodi di eliminare gli overload con classificazione inferiore a favore di metodi con classificazione superiore** in modo che il compilatore DesignScript chiami sempre il metodo con classificazione più alta come il primo e l'unico che trova.

### Esempi:
Nel seguente esempio sono stati definiti due overload della funzione `foo`. Nella versione 1.x, l'overload eseguito in fase di runtime è ambiguo. L'utente potrebbe aspettarsi l'esecuzione del secondo overload `foo(a:int, b:int)`, nel qual caso il metodo dovrebbe replicarsi tre volte restituendo un valore di `10` triplicato. In realtà quello che viene restituito è un valore singolo di `10`, poiché viene chiamato il primo overload con il parametro list.

### Il secondo overload viene omesso nella versione 2.0:
Nella versione 2.0, è sempre il primo metodo definito che viene scelto rispetto agli altri. Vale il principio "primo arrivato, primo servito".

![](../images/8-4/1/lang2_3.png)

Per ognuno dei seguenti casi, verrà utilizzato il primo overload definito. Si noti che si basa esclusivamente sull'ordine di definizione delle funzioni e non sulle classificazioni dei parametri, sebbene sia consigliabile dare la preferenza ai metodi con parametri con classificazione superiore per i nodi definiti dall'utente e zero-touch.
```
1)
foo(a: int[], b: int); ✓
foo(a: int, b: int); ✕
```
```
2) 
foo(x: int, y: int); ✓
foo(x: int[], y: int[]); ✕
```
## 3\. Tutti i nodi dell'interfaccia utente vengono compilati come metodi statici.
In Dynamo 1.x, i nodi dell'interfaccia utente (non i blocchi di codice) sono stati compilati rispettivamente come metodi di istanza e proprietà. Ad esempio, il nodo `Point.X` è stato compilato come `pt.X` e `Curve.PointAtParameter` è stato compilato come `curve.PointAtParameter(param)`. Questo comportamento presentava due problemi:

__A. La funzione rappresentata dal nodo dell'interfaccia utente non è sempre la stessa funzione eseguita in fase di runtime__

Un tipico esempio è il nodo `Translate`. Esistono più nodi `Translate` che richiedono lo stesso numero e gli stessi tipi di argomenti, ad esempio: `Geometry.Translate`, `Mesh.Translate` e `FamilyInstance.Translate`. A causa del fatto che i nodi sono stati compilati come metodi di istanza, il passaggio di `FamilyInstance` ad un nodo `Geometry.Translate` funzionerebbe sorprendentemente ancora in quanto in fase di runtime invierebbe la chiamata al metodo di istanza `Translate` in `FamilyInstance`. Ciò era ovviamente fuorviante per gli utenti, poiché il nodo non funzionava come dichiarato.

__B. Il secondo problema era che i metodi di istanza non funzionavano con matrici eterogenee__

In fase di runtime, il motore di esecuzione deve individuare la funzione a cui deve essere inviato. Se l'input è un elenco, ad esempio `list.Translate()`, poiché è costoso esaminare ogni elemento di un elenco e cercare metodi nel suo tipo, la logica di risoluzione del metodo presuppone semplicemente che il tipo di destinazione sia uguale al tipo del primo elemento e tenta di cercare il metodo `Translate()` definito in quel tipo. Di conseguenza, se il primo tipo di elemento non corrispondeva al tipo di destinazione del metodo (o anche se era `null` o un elenco vuoto), l'intero elenco aveva esito negativo, anche se erano presenti altri tipi corrispondenti. 

Ad esempio, se un input list con i seguenti tipi `[Arc, Line]` veniva passato in `Arc.CenterPoint`, il risultato conteneva un punto centrale per l'arco e un valore `null` per la linea come previsto. Tuttavia, se l'ordine era invertito, l'intero risultato era null poiché il primo elemento non aveva superato la verifica della risoluzione del metodo:
### Dynamo 1.x: verifica solo il primo elemento dell'input list per la verifica della risoluzione del metodo
![](../images/8-4/1/lang2_4.png)
```
x = [arc, line];
y = x.CenterPoint; // y = [centerpoint, null] ✓
```
```
x = [line, arc];
y = x.CenterPoint; // y = null ✕
```
Nella versione 2.0 entrambi questi problemi vengono risolti compilando i nodi dell'interfaccia utente come proprietà statiche e metodi statici. 

Con i metodi statici, la risoluzione del metodo di runtime è più semplice e tutti gli elementi nell'input list vengono iterati. Ad esempio:

La semantica di `foo.Bar()` (metodo di istanza) deve cercare il tipo di `foo` e anche controllare se si tratta di un elenco o meno e quindi abbinarlo alle funzioni candidate. Tutto questo è costoso. D'altra parte, la semantica di `Foo.Bar(foo)` (metodo statico) deve verificare solo una funzione con il tipo di parametro `foo`.

Ecco cosa succede nella versione 2.0:
* Un nodo di proprietà dell'interfaccia utente viene compilato come un getter statico: il motore genera una versione statica di un getter per ogni proprietà. Ad esempio, un nodo `Point.X` viene compilato come un getter statico `Point.get_X(pt)`. Si noti che il getter statico può anche essere chiamato utilizzando il relativo alias: `Point.X(pt)` in un nodo del blocco di codice.
* Un nodo del metodo dell'interfaccia utente viene compilato come versione statica: il motore genera un metodo statico corrispondente per il nodo. Ad esempio, il nodo `Curve.PointAtParameter` viene compilato come `Curve.PointAtParameter(curve: Curve, parameter:double)` anziché come `curve.PointAtParameter(parameter)`. 

**Nota** Con questa modifica, non è stato rimosso il supporto dei metodi di istanza, pertanto i metodi di istanza esistenti utilizzati nei nodi di blocchi di codice, come `pt.X` e `curve.PointAtParameter(parameter)` negli esempi precedenti, continueranno a funzionare.

Questo esempio funzionava in precedenza nella versione 1.x, poiché il grafico veniva compilato come `point.X;` e veniva trovata la proprietà `X` nell'oggetto punto. Ora ha esito negativo nella versione 2.0 poiché il codice compilato `Vector.X(point)` prevede solo un tipo `Vector`:

![](../images/8-4/1/lang2_5.png)

### Vantaggi:
**Metodi coerenti/comprensibili:** i metodi statici eliminano ogni ambiguità sul metodo da eseguire in fase di runtime. Il metodo corrisponde sempre al nodo dell'interfaccia utente utilizzato nel grafico che l'utente prevede di chiamare.

**Metodi compatibili:** esiste una migliore correlazione tra il codice e il programma visivo.

**Metodi istruttivi:** il passaggio di input list eterogenei ai nodi ora genera valori non null per i tipi accettati dal nodo e valori null per i tipi che non implementano il nodo. I risultati sono più prevedibili e forniscono una migliore indicazione dei tipi consentiti per il nodo.

### Avviso: ambiguità irrisolte con metodi in overload

Poiché Dynamo supporta in generale gli overload di funzioni, si potrebbe comunque generare confusione se è presente un'altra funzione in overload con lo stesso numero di parametri. Ad esempio, nel seguente grafico, se colleghiamo un valore numerico all'input `direction` di `Curve.Extrude` e un vettore all'input `distance` di `Curve.Extrude`, entrambi i nodi continuano a funzionare, il che è imprevisto. In questo caso, anche se i nodi vengono compilati come metodi statici, il motore non è ancora in grado di distinguere la differenza in fase di runtime e ne seleziona uno a seconda del tipo di input. ![](../images/8-4/1/lang2_6.png)
 
### Problemi risolti:
Il passaggio alla semantica del metodo statico ha introdotto i seguenti effetti collaterali, che vale la pena menzionare qui come modifiche correlate al linguaggio della versione 2.0.

**1\. Perdita di comportamento polimorfico:**

Consideriamo un esempio di nodi `TSpline` in `ProtoGeometry` (si noti che `TSplineTopology` eredita dal tipo di `Topology` base): il nodo `Topology.Edges`, che è stato precedentemente compilato come metodo di istanza, `object.Edges`, è ora compilato come metodo statico, `Topology.Edges(object)`. La chiamata precedente si risolveva in modo polimorfico nel metodo della classe derivata `TsplineTopology.Edges` dopo l'invio di un metodo sul tipo di oggetto in fase di runtime. 

![](../images/8-4/1/lang2_7.png)

Il nuovo comportamento statico, invece, veniva forzato a chiamare il metodo della classe di base, `Topology.Edges`. Di conseguenza, questo nodo restituiva gli oggetti `Edge` della classe di base anziché gli oggetti della classe derivata di tipo `TSplineEdge`.
 
![](../images/8-4/1/lang2_8.png)

Si trattava di una regressione perché i nodi `TSpline` a valle prevedevano che `TSplineEdges` iniziasse a non funzionare. 

Il problema è stato risolto aggiungendo una verifica di runtime nella logica di invio del metodo per cercare il tipo di istanza rispetto al tipo o ad un sottotipo del primo parametro del metodo. Nel caso di un input list, è stato semplificato l'invio del metodo per cercare semplicemente il tipo del primo elemento. Quindi la soluzione finale era un compromesso tra la ricerca di un metodo in parte statico e uno in parte dinamico.

**Nuovo comportamento polimorfico nella versione 2.0:**

![](../images/8-4/1/lang2_9.png)

In questo caso, poiché il primo elemento `a` è `TSpline`, si tratta del metodo derivato `TSplineTopology.Edges` che viene richiamato in fase di runtime. Di conseguenza, restituisce `null` per `Topology` base di tipo `b`. 

Nel secondo caso, poiché `Topology` generale di tipo `b` è il primo elemento, viene chiamato il metodo `Topology.Edges` di base. Poiché `Topology.Edges` accetta anche il tipo `TSplineTopology` derivato, `a` come input restituisce `Edges` per entrambi gli input, `a` e `b`.

![](../images/8-4/1/lang2_10.png)
 
**2\. Regressioni dovute alla produzione di elenchi esterni ridondanti**

Esiste una differenza principale tra i metodi di istanza e i metodi statici per quanto riguarda il comportamento delle guide per la replica. Con i metodi di istanza, gli input a valore singolo con le guide per la replica non vengono alzati di livello ad elenchi, mentre vengono alzati di livello per i metodi statici.

Si consideri l'esempio del nodo `Surface.PointAtParameter` con il collegamento incrociato e con un singolo input surface e matrici di valori di parametri `u` e `v`. Il metodo di istanza viene compilato in:
```
surface<1>.PointAtParameter(u<1>, v<2>);
```
ottenendo una matrice 2D di punti.
 
Il metodo statico viene compilato come:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
ottenendo un elenco 3D di punti con un elenco più esterno ridondante.

Questo effetto collaterale della compilazione dei nodi dell'interfaccia utente come metodi statici potrebbe causare regressioni nei casi d'uso esistenti. Questo problema è stato risolto disattivando l'innalzamento di livello di input a valore singolo ad elenco quando vengono utilizzati con le guide per la replica/il collegamento (vedere l'elemento successivo).
 
**4\. Innalzamento di livello ad elenco disattivato con guide per la replica/collegamento**

Nella versione 1.x c'erano due casi in cui valori singoli venivano innalzati al livello di elenco:

* Quando gli input con classificazione inferiore erano passati nelle funzioni che prevedevano input con classificazione superiore
* Quando gli input di livello inferiore erano passati nelle funzioni che prevedevano la stessa classificazione ma in cui gli argomenti di input erano caratterizzati dalle guide per la replica o utilizzavano il collegamento

Nella versione 2.0 quest'ultimo caso non è più supportato, pertanto l'innalzamento al livello di elenco in tali scenari non è possibile.

Nel seguente grafico della versione 1.x, un livello di guida per la replica per ognuno dei valori `y` e `z` forzava l'innalzamento di livello a matrice con classificazione 1 per ciascuno di essi, motivo per cui il risultato aveva una classificazione 3 (1 ciascuna per `x`, `y` e `z`). L'utente si aspettava invece che il risultato fosse di classificazione 1 poiché non era del tutto ovvio che la presenza di guide per la replica per gli input a valore singolo aggiungesse livelli al risultato.
```
x = 1..5;
y = 0;
z = 0;
p = Point.ByCoordinates(x<1>, y<2>, z<3>); // cross-lacing
```

### Dynamo 1.x: elenco 3D di punti

![](../images/8-4/1/lang2_11.png)

Nella versione 2.0, la presenza di guide per la replica per ognuno degli argomenti a valore singolo `y` e `z` non causa un innalzamento di livello determinando un elenco con la stessa dimensione dell'elenco 1D di input per `x`. 

### Dynamo 2.0: elenco 1D di punti

![](../images/8-4/1/lang2_12.png)

La modifica del linguaggio ha risolto anche il problema della regressione di cui sopra, causata dalla compilazione come metodo statico e dalla generazione di elenchi esterni ridondanti.

Continuando con lo stesso esempio precedente, abbiamo visto che una chiamata al metodo statico come:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>); 
```
produceva un elenco 3D di punti in Dynamo 1.x. Ciò si verificava a causa dell'innalzamento di livello del primo argomento a valore singolo surface ad elenco quando veniva utilizzato con una guida per la replica.
 
### Dynamo 1.x: innalzamento di livello dell'argomento ad elenco con la guida per la replica

![](../images/8-4/1/lang2_13.png)

Nella versione 2.0 è stato disattivato l'innalzamento al livello di elenchi degli argomenti a valore singolo quando vengono utilizzati con le guide per la replica o il collegamento. Quindi ora la chiamata a:
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
restituisce semplicemente un elenco 2D poiché l'input surface non viene alzato di livello.

### Dynamo 2.0: disattivato l'innalzamento al livello di elenco dell'argomento a valore singolo, se utilizzato con una guida per la replica

![](../images/8-4/1/lang2_14.png)

Questa modifica ora rimuove l'aggiunta di un livello di elenco ridondante e risolve anche la regressione causata dal passaggio alla compilazione come metodo statico.

### Vantaggi:

**Risultati leggibili:** i risultati sono in linea con le aspettative dell'utente e sono più facili da comprendere.

**Risultati compatibili:** i nodi dell'interfaccia utente (con l'opzione di collegamento) e i nodi di blocchi di codice che utilizzano le guide per la replica offrono risultati compatibili.

**Risultati coerenti:** 
* I metodi di istanza e quelli statici sono coerenti (si risolvono i problemi relativi alla semantica del metodo statico).
* I nodi con input e con argomenti di default si comportano in modo coerente (vedere di seguito).

![](../images/8-4/1/lang2_15.png)

## 5\. Le variabili sono immutabili nei nodi di blocchi di codice per impedire l'aggiornamento associativo 

DesignScript supporta storicamente due paradigmi di programmazione: la programmazione associativa e la programmazione imperativa. Il codice associativo crea un grafico delle dipendenze dalle istruzioni del programma in cui le variabili dipendono l'una dall'altra. L'aggiornamento di una variabile può comportare l'aggiornamento di tutte le variabili che dipendono da essa. Ciò significa che la sequenza di esecuzione delle istruzioni in un blocco associativo non si basa sul loro ordine, ma sulle relazioni di dipendenza tra le variabili.

Nel seguente esempio, la sequenza di esecuzione del codice è costituita dalle righe 1 -> 2 -> 3 -> 2. Poiché `b` ha una dipendenza da `a`, quando `a` viene aggiornato nella riga 3, l'esecuzione passa di nuovo alla riga 2 per aggiornare `b` con il nuovo valore di `a`. 
```
1. a = 1; 
2. b = a * 2;
3. a = 2;
```
Al contrario, se lo stesso codice viene eseguito in un contesto imperativo, le istruzioni vengono eseguite in un flusso lineare, dall'alto verso il basso. I blocchi di codice imperativi sono quindi adatti per l'esecuzione sequenziale di costrutti di codice come i loop e le condizioni if-else.

### Ambiguità dell'aggiornamento associativo:

**1\. Variabili con dipendenza ciclica:**

In alcuni casi, una dipendenza ciclica tra le variabili potrebbe non essere così ovvia come nel seguente caso. In questi casi, in cui il compilatore non è in grado di rilevare il ciclo in modo statico, potrebbe portare a un ciclo di runtime indefinito.
```
a = 1;
b = a;
a = b;
```
**2\. Variabili che dipendono da se stesse:**

Se una variabile dipende da se stessa, il suo valore deve accumularsi o deve essere ripristinato il suo valore originale ad ogni aggiornamento?
```
a = 1;
b = 1;
b = b + a + 2; // b = 4
a = 4;         // b = 10 or b = 7?
```
In questo esempio di geometria, poiché il cubo `b` dipende da se stesso oltre che dal cilindro, `a`, lo spostamento del dispositivo di scorrimento dovrebbe far spostare il foro lungo il blocco o dovrebbe creare un effetto cumulativo di creazione di diversi fori lungo il suo percorso per ogni aggiornamento della posizione del dispositivo di scorrimento?

![](../images/8-4/1/lang2_16.gif)

**3\. Aggiornamento delle proprietà delle variabili:**

```
1: def foo(x: A) { x.prop = ...; return x; }
2: a = A.A();
3: p = a.prop;
4: a1 = foo(a);  // will p update?
```

**4\. Aggiornamento delle funzioni:**

```
1: def foo(v: double) { return v * 2; }// define “foo”
2: x = foo(5);                         // first definition of “foo” called
3: def foo(v: int) { return v * 3; }   // overload of “foo” defined, will x update?
```
L'esperienza ci insegna che l'aggiornamento associativo non risulta utile nei nodi di blocchi di codice in un contesto del grafico del flusso di dati basato sui nodi. Prima che fosse disponibile qualsiasi ambiente di programmazione visiva, l'unico modo per esplorare le opzioni era quello di modificare in modo esplicito i valori di alcune variabili nel programma. Un programma basato sul testo dispone della cronologia completa degli aggiornamenti di una variabile, mentre in un ambiente di programmazione visiva viene visualizzato solo l'ultimo valore di una variabile. 

Se qualche utente l'ha utilizzato, è molto probabile che l'abbia fatto inconsapevolmente, causando più danni che benefici. Pertanto, nella versione 2.0 si è deciso di nascondere l'associatività nell'utilizzo dei nodi di blocchi di codice rendendo le variabili immutabili, pur continuando a mantenere l'aggiornamento associativo solo come funzionalità nativa del motore DesignScript. Questa è un'altra modifica apportata con l'idea di semplificare l'esperienza di scripting per gli utenti.

**L'aggiornamento associativo è disattivato nei nodi di blocchi di codice impedendo la ridefinizione delle variabili:** ![](../images/8-4/1/lang2_17.png)

**L'indicizzazione dell'elenco è ancora consentita nei blocchi di codice**

È stata fatta un'eccezione per l'indicizzazione dell'elenco, che è ancora consentita nella versione 2.0 con l'assegnazione dell'operatore index.

In questo esempio l'elenco `a` viene inizializzato ma può essere successivamente sovrascritto con l'assegnazione di un operatore index ed eventuali variabili dipendenti da `a` vengono aggiornate in modo associativo, come si vede dal valore di `c`. Inoltre, l'anteprima del nodo visualizza i valori aggiornati di `a` dopo la ridefinizione di uno o più dei suoi elementi.

![](../images/8-4/1/lang2_18.png)

## 6\. Le variabili dei blocchi imperativi sono collocate nell'ambito del blocco imperativo

Le regole di definizione dell'ambito imperativo sono state modificate nella versione 2.0 per evitare complicati scenari di aggiornamento tra linguaggi differenti.

In Dynamo 1.x, la sequenza di esecuzione del seguente script prevedeva le righe 1 -> 2 -> 4 -> 6 -> 4, dove una modifica veniva propagata dagli ambiti di linguaggio esterni a quelli interni. Poiché il valore `y` viene aggiornato nel blocco associativo esterno e poiché `x` nel blocco imperativo ha una dipendenza da `y`, il controllo passa dal programma associativo esterno al linguaggio imperativo nella riga 4. 
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3;
```

La sequenza di esecuzione nell'esempio successivo prevede le righe 1 -> 2 -> 4 -> 2, dove la modifica si propaga dall'ambito di linguaggio interno a quello esterno.
```
1: x = 1;
2: y = x * 2;
3: [Imperative] {
4:     x = 3;
5: }
```
Gli scenari precedenti si riferiscono all'aggiornamento tra i vari linguaggi, che proprio come l'aggiornamento associativo non è molto utile nei nodi di blocchi di codice. Per evitare complicati scenari di aggiornamento tra linguaggio differenti, le variabili sono state rese locali rispetto all'ambito imperativo. 

Nel seguente esempio in Dynamo 2.0:
```
x = 1;
y = x * 2;
i = [Imperative] {
     x = 3;
     return x;
}
```
* Il valore di `x` definito nel blocco imperativo è ora locale rispetto all'ambito Imperativo.
* I valori di `x` e `y` nell'ambito esterno rimangono rispettivamente `1` e `2`.

Qualsiasi variabile locale all'interno di un blocco imperativo deve essere restituita se si desidera accedere al relativo valore in un ambito esterno.

Si consideri il seguente esempio:
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3; // x = 1, y = 3
```
* Il valore di `y` viene copiato localmente all'interno dell'ambito imperativo.  
* Il valore di `x` collocato nell'ambito imperativo è `4`.
* L'aggiornamento del valore di `y` nell'ambito esterno continua a determinare l'aggiornamento di `x` a causa dell'aggiornamento tra i vari linguaggi, ma è disattivato nei blocchi di codice nella versione 2.0 a causa dell'immutabilità delle variabili.
* I valori di `x` e `y` nell'ambito associativo esterno rimangono rispettivamente `1` e `2`.

## 7\. Elenchi e dizionari

In Dynamo 1.x, elenchi e dizionari erano rappresentati da un singolo contenitore unificato, che poteva essere indicizzato sia mediante un indice di numeri interi che da una chiave non integrale. Nella seguente tabella sono riepilogate la separazione tra elenchi e dizionari nella versione 2.0 e le regole del nuovo tipo di dati del dizionario:

|                               |    1.x                      |    2.0                                   |
| :---------------------------- | --------------------------- | ---------------------------------------- |
| **Inizializzazione dell'elenco**       | `a = {1, 2, 3};`            | `a = [1, 2, 3];`                         |
| **Elenco vuoto**                | `a = {};`                   | `a = [];`                                |
| **Inizializzazione del dizionario** | **Poteva essere aggiunto allo stesso dizionario in modo dinamico:** | **È possibile creare solo nuovi dizionari:** |
|                           | `a = {};`                   | `a = {“foo” : 1, “bar” : 2};`            |
|                           | `a[“foo”] = 1;`             | `b = {“foo” : 1, “bar” : 2, “baz” : 3};` |
|                           | `a[“bar”] = 2;`             | `a = {};` // Crea un dizionario vuoto |
|                           | `a[“baz”] = 3;`             |                                          |
| **Indicizzazione del dizionario**   | **Indicizzazione delle chiavi**            | **La sintassi di indicizzazione rimane invariata**     |
|                           | `b = a[“bar”];`             | `b = a[“bar”];`                          |
| **Chiavi del dizionario**       | **Qualsiasi tipo di chiave era valido**  | **Solo le chiavi stringa sono valide**           |
|                           | `a = {};`                   | `a  = {“false” : 23, “point” : 12};`     |
|                           | `a[false] = 23;`            |                                          |
|                           | `a[point] = 12;`            |                                          |

### Nuova sintassi dell'elenco con `[]`
La sintassi di inizializzazione dell'elenco è stata modificata passando dalle parentesi graffe `{}` alle parentesi quadre `[]` nella versione 2.0. Viene eseguita automaticamente la migrazione di tutti gli script della versione 1.x alla nuova sintassi quando vengono aperti nella versione 2.0. 

**Nota sugli attributi degli argomenti di default nei nodi zero-touch:**

Si noti, tuttavia, che la migrazione automatica non funzionerà con la vecchia sintassi utilizzata negli attributi degli argomenti di default. Se necessario, gli autori dei nodi devono aggiornare manualmente le definizioni del metodo zero-touch per utilizzare la nuova sintassi negli attributi degli argomenti di default `DefaultArgumentAttribute`.

**Nota sull'indicizzazione:**

Il comportamento della nuova indicizzazione è cambiato in alcuni casi. L'indicizzazione in un elenco/dizionario con un elenco arbitrario di indici/chiavi utilizzando l'operatore `[]` ora mantiene la struttura dell'elenco di input di indici/chiavi. In precedenza restituiva sempre un elenco 1D di valori:
```
Given:
a = {“foo” : 1, “bar” : 2};

1.x:
b = a[{“foo”, {“bar”}}];
returns {1, 2}

2.0:
b = a[[“foo”, [“bar”]]];
returns [1, [2]];
```

### Sintassi di inizializzazione del dizionario:

Il segno `{}` (la sintassi con parentesi graffe) per l'inizializzazione del dizionario può essere utilizzata solo nel formato di coppia chiave-valore 
```
dict = {<key> : <value>, …}; 
```
dove è consentita solo una stringa per `<key>` e più coppie chiave-valore sono separate da virgole.

![](../images/8-4/1/lang2_19.png)

Il metodo zero-touch `Dictionary.ByKeysValues` può essere utilizzato come un modo più versatile per inizializzare un dizionario passando rispettivamente un elenco di chiavi e valori e sfruttando tutti i vantaggi dei metodi zero-touch come le guide per la replica e così via.

![](../images/8-4/1/lang2_20.png)

### Perché non sono state utilizzate espressioni arbitrarie per la sintassi di inizializzazione del dizionario?

Abbiamo sperimentato l'idea di utilizzare espressioni arbitrarie per le chiavi nella sintassi di inizializzazione delle coppie chiave-valore del dizionario e abbiamo scoperto che poteva portare a risultati confusi, soprattutto quando una sintassi come `{keys : vals}` (dove entrambi gli elementi `keys`, `vals` rappresentano elenchi) interferiva con altre caratteristiche del linguaggio di DesignScript come la replica e produceva risultati diversi dal nodo di inizializzazione zero-touch. 

Ad esempio, potrebbero esserci altri casi come questa istruzione in cui sarebbe difficile definire il comportamento previsto:
```
dict = {["foo", "bar"] : "baz" };
```
Aggiungere la sintassi delle guide per la replica o altro, invece di limitarsi ai soli identificatori, sarebbe andare contro l'idea di semplicità del linguaggio. 

In futuro _potremmo_ estendere le chiavi del dizionario per supportare espressioni arbitrarie, ma dovremmo anche garantire che l'interazione con altre caratteristiche del linguaggio rimanga coerente e comprensibile. Il rischio di rendere il sistema più complesso è quello di sacrificare un po' della sua potenza e facilità d'uso. In ogni caso, c'è sempre un modo alternativo per affrontare questo problema utilizzando il metodo `Dictionary.ByKeysValues(keyList, valueList)`, che non è poi così difficile.

### Interazione con nodi zero-touch:

__1\. Il nodo zero-touch che restituisce un dizionario .NET viene restituito come dizionario di Dynamo__

**Si consideri il seguente metodo zero-touch C# che restituisce un IDictionary:** ![](../images/8-4/1/lang2_21.png)

**Viene eseguito il marshalling del valore restituito del nodo ZT corrispondente come dizionario di Dynamo:** ![](../images/8-4/1/lang2_22.png)

__2\. I nodi a restituzione multipla vengono visualizzati in anteprima come dizionari__

**Il nodo zero-touch che restituisce IDictionary con attributo a restituzione multipla restituisce un dizionario di Dynamo:** ![](../images/8-4/1/lang2_23.png)

![](../images/8-4/1/lang2_24.png)

__3\. Il dizionario di Dynamo può essere passato come input nel nodo zero-touch che accetta il dizionario .NET__

**Metodo ZT con un parametro IDictionary:** ![](../images/8-4/1/lang2_25.png)

**Il nodo ZT accetta il dizionario di Dynamo come input:** ![](../images/8-4/1/lang2_26.png)

### Anteprima del dizionario nei nodi a restituzione multipla

I dizionari sono coppie chiave-valore non ordinate. Coerentemente con questa idea, non è quindi garantito che le anteprime delle coppie chiave-valore dei nodi che restituiscono dizionari siano disposte secondo l'ordine dei valori restituiti dei nodi. 

È stata tuttavia fatta un'eccezione per i nodi a restituzione multipla con valori `MultiReturnAttribute` definiti. Nell seguente esempio, il nodo `DateTime.Components` è un nodo a restituzione multipla e l'anteprima del nodo riflette le relative coppie chiave-valore in modo che siano nello stesso ordine di quello delle porte di output nel nodo, che è anche l'ordine in cui gli output vengono specificati in base ai valori `MultiReturnAttribute` nella definizione del nodo.

Si noti inoltre che le anteprime per i blocchi di codice non vengono ordinate diversamente dal nodo dell'interfaccia utente, poiché le informazioni sulla porta di output (sotto forma di un attributo a restituzione multipla) non esistono per il nodo del blocco di codice: ![](../images/8-4/1/lang2_27.png)
