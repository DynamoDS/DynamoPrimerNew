# Functions

Le funzioni possono essere create in un blocco di codice e richiamate in un altro punto della definizione di Dynamo. In questo modo viene creato un altro livello di controllo in un file parametrico e può essere visualizzato come versione basata su testo di un nodo personalizzato. In questo caso, il blocco di codice "principale" è facilmente accessibile e può essere posizionato ovunque nel grafico. Non sono necessari i fili.

### Parent

La prima riga contiene la parola chiave "def", il nome della funzione e i nomi degli input tra parentesi. Le parentesi graffe definiscono il corpo della funzione. Restituiscono un valore con "return =". I blocchi di codice che definiscono una funzione non presentano porte di input o output, in quanto vengono chiamati da altri blocchi di codice.

![](../images/8-1/4/functionsparentdef.jpg)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

### Elementi derivati

Chiamano la funzione con un altro blocco di codice nello stesso file, assegnando lo stesso nome e lo stesso numero di argomenti. Funzionano come i nodi predefiniti nella libreria.

![](../images/8-1/4/functionschildrencalldef.jpg)

```
FunctionName(in1,in2);
```

## Esercizio: Sfera per Z

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/8-1/4/Functions_SphereByZ.dyn" %}

In questo esercizio verrà eseguita una definizione generica che creerà sfere da un elenco di input di punti. Il raggio di queste sfere viene gestito dalla proprietà Z di ogni punto.

Iniziare con un numero di dieci valori compresi tra 0 e 100. Collegarli ai nodi **Point.ByCoordinates** per creare una linea diagonale.

![](../images/8-1/4/functions-exercise-01.jpg)

Creare un **Code Block** e introdurre la definizione.

![](../images/8-1/4/functions-exercise-02.jpg)

> 1.  Utilizzare queste righe di codice:
>
>     ```
>     def sphereByZ(inputPt)
>     {
>
>     };
>     ```
>
> _inputPt_ è il nome fornito per rappresentare i punti che controlleranno la funzione. Per ora, la funzione non sta eseguendo alcuna operazione, ma si costruirà questa funzione nei prossimi passaggi.

![](../images/8-1/4/functions-exercise-03.jpg)

> 1. Alla funzione **Code Block**, si aggiungono un commento e una variabile _sphereRadius_ che esegue una query sulla posizione _Z_ di ogni punto. Tenere presente che _inputPt.Z_ non richiede parentesi come metodo. Si tratta di una _query_ sulle proprietà di un elemento esistente, pertanto non sono necessari input:
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

![](../images/8-1/4/functions-exercise-04.jpg)

> 1. Ora, richiamare la funzione che è stata creata in un altro **Code Block**. Se si fa doppio clic sull'area di disegno per creare un nuovo _Code Block_ e si digita _sphereB_, si noterà che Dynamo suggerisce la funzione _sphereByZ_ definita. La funzione è stata aggiunta alla libreria di IntelliSense. Ottimo.

![](../images/8-1/4/functions-exercise-05.jpg)

> 1.  Ora richiamare la funzione e creare una variabile denominata _Pt_ per collegare i punti creati nei passaggi precedenti:
>
>     ```
>     sphereByZ(Pt)
>     ```
> 2. Dall'output si noterà che tutti i valori sono nulli. Perché? Quando è stata definita la funzione, si stava calcolando la variabile _sphereRadius_, ma non è stato definito che cosa la funzione dovrebbe _restituire_ come _output_. È possibile risolvere il problema nel passaggio successivo.

![](../images/8-1/4/functions-exercise-06.jpg)

> 1. Un passaggio importante: è necessario definire l'output della funzione aggiungendo la riga `return = sphereRadius;` alla funzione _sphereByZ_.
> 2. Ora si può vedere che l'output di Code Block fornisce le coordinate Z di ciascun punto.

Procedere alla creazione di sfere effettive modificando la funzione _principale_.

![](../images/8-1/4/functions-exercise-07.jpg)

> 1. Per prima cosa si definisce una sfera con la riga di codice: `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`.
> 2. Successivamente, si modifica il valore restituito in modo che sia _sphere_ anziché _sphereRadius_: `return = sphere;`. In questo modo vengono restituite alcune sfere giganti nell'anteprima di Dynamo.

![](../images/8-1/4/functions-exercise-08.jpg)

> 1\. Per ridurre le dimensioni di queste sfere, aggiornare il valore sphereRadius aggiungendo un divisore: `sphereRadius = inputPt.Z/20;`. Ora è possibile vedere le sfere separate. Inizia ad avere un senso la relazione tra il raggio e il valore Z.

![](../images/8-1/4/functions-exercise-09.jpg)

> 1. Nel nodo **Point.ByCoordinates**, modificando il collegamento da Più breve a Globale, verrà creata una griglia di punti. La funzione _sphereByZ_ è ancora attiva, per cui tutti i punti creano sfere con i raggi basati sui valori Z.

![](../images/8-1/4/functions-exercise-10.jpg)

> 1. E solo per fare delle prove, è necessario collegare l'elenco originale di numeri all'input X per **Point.ByCoordinates**. Ora si ottiene un cubo di sfere.
> 2. Nota: se il calcolo al computer richiede molto tempo, provare a modificare _\#10_ impostandolo su un valore simile a _\#5_.

Tenere presente che la funzione _sphereByZ_ che è stata creata è una funzione generica, pertanto è possibile richiamare l'elica da una lezione precedente e applicare la funzione ad essa.

![](../images/8-1/4/functions-exercise-11.jpg)

Un passaggio finale: controllare il rapporto del raggio con un parametro definito dall'utente. A tale scopo, è necessario creare un nuovo input per la funzione e sostituire anche il divisore _20_ con un parametro.

![](../images/8-1/4/functions-exercise-12.jpg)

> 1.  Aggiornare la definizione di _sphereByZ_ a:
>
>     ```
>     def sphereByZ(inputPt,radiusRatio)
>     {
>     //get Z Value, use it to drive radius of sphere
>     sphereRadius=inputPt.Z/radiusRatio;
>     //Define Sphere Geometry
>     sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>     //Define output for function
>     return sphere;
>     };
>     ```
> 2. Aggiornare il **Code Block** degli elementi derivati aggiungendo una variabile ratio all'input: `sphereByZ(Pt,ratio);`. Collegare un dispositivo di scorrimento all'input **Code Block** appena creato e modificare la dimensione dei raggi in base al rapporto del raggio.
