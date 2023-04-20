---
description: suggested exercise
---

# Vaso parametrico

La creazione di un vaso parametrico è un ottimo modo per iniziare a imparare a utilizzare Dynamo.

Questo workflow illustra come:

* Utilizzare i dispositivi di scorrimento numerici per controllare le variabili nel progetto.
* Creare e modificare elementi geometrici mediante nodi.
* Visualizzare i risultati di progettazione in tempo reale.

![](../../1\_introduction/images/1-2/vase1.gif)

## Definizione degli obiettivi

Prima di passare a Dynamo, procedere progettando concettualmente un vaso.

Si supponga di voler progettare un vaso di argilla che tenga conto delle pratiche di produzione utilizzate dai ceramisti. I ceramisti normalmente utilizzano un tornio per fabbricare vasi cilindrici. Quindi, applicando pressione su varie altezze del vaso, possono modificare la forma del vaso e creare progetti diversi.

Verrà utilizzata una metodologia simile per definire il nostro vaso. Si creeranno 4 cerchi ad altezze e raggi diversi e quindi si creerà una superficie mediante il loft di questi cerchi.

![](../images/10-1/1/vase2.png)

## Introduzione

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/10-1/1/DynamoSampleWorkflow-vase.dyn" %}

Sono necessari i nodi che rappresenteranno la sequenza di azioni che verranno eseguite da Dynamo. Poiché si sta cercando di creare un cerchio, iniziare individuando un nodo che rispetti questo requisito. Utilizzare il **campo di ricerca** o sfogliare la **Libreria** per trovare il nodo **Circle.ByCenterPointRadius** e aggiungerlo all'area di lavoro.

![](../images/10-1/1/vase8.png)

> 1. Cercare circle.
> 2. Selezionare ByCenterPointRadius.
> 3. Il nodo verrà visualizzato nell'area di lavoro.

Si esaminerà più da vicino questo nodo. Sul lato sinistro sono presenti gli input del nodo (_centerPoint_ e _radius_) e sul lato destro è presente l'output del nodo (Circle). Notare che gli output hanno una linea celeste. Ciò significa che l'input ha un valore di default. Per ottenere ulteriori informazioni sull'input, posizionare il cursore sul relativo nome. L'input _radius_ richiede un doppio input e ha un valore di default di 1.

![](../images/10-1/1/vase10.png)

Lasciare il valore di default di _centerPoint_ ma aggiungere un **Number Slider** per controllare il raggio. Come è stato fatto con il nodo **Circle.ByCenterPointRadius**, utilizzare la libreria per cercare **Number Slider** e aggiungerlo al grafico.

Questo nodo è un po' diverso dal nodo precedente poiché contiene un dispositivo di scorrimento. È possibile utilizzare l'interfaccia per modificare il valore di output del dispositivo di scorrimento.

![](../images/10-1/1/vase13\(1\).gif)

Il dispositivo di scorrimento può essere configurato utilizzando il pulsante a discesa a sinistra del nodo. Limitare il dispositivo di scorrimento ad un valore massimo di 15.

![](../images/10-1/1/vase11.png)

Posizionarlo a sinistra del nodo **Circle.ByCenterPointRadius** e collegare entrambi i nodi selezionando l'output **Number Slider** e collegandolo all'input radius.

![](../images/10-1/1/vase12.png)

Cambiare anche il nome Number Slider in Top Radius facendo doppio clic sul nome del nodo.

![](../images/10-1/1/vase14.png)

## Passaggi successivi

Continuare ad aggiungere alcuni nodi e collegamenti alla logica per definire il nostro vaso.

### Creazione di cerchi con raggi diversi

Copiare questi nodi 4 volte in modo che i cerchi definiscano la nostra superficie, quindi modificare i nomi di Number Slider come mostrato qui sotto.

\![](<../images/10-1/1/vase4 (1).png>)

> 1. I cerchi vengono creati da un punto centrale e da un raggio.

### Spostamento di cerchi per l'altezza del vaso

Un parametro chiave del nostro vaso risulta mancante, ossia la sua altezza. Per controllare l'altezza del vaso, creare un altro dispositivo di scorrimento numerico. È inoltre possibile aggiungere un nodo **Code Block**. I Code Block possono essere utili come aggiunta di frammenti di codice personalizzati al nostro workflow. Si utilizzerà il Code Block per moltiplicare il dispositivo di scorrimento dell'altezza in base a fattori diversi, in modo da poter posizionare i nostri cerchi lungo l'altezza del vaso.

![](../images/10-1/1/vase15\(1\).png)

Quindi, utilizzare un nodo **Geometry.Translate** per posizionare i cerchi all'altezza desiderata. Poiché si desidera distribuire i cerchi nel vaso, utilizzare i Code Block per moltiplicare il parametro dell'altezza in base ad un fattore.

![](../images/10-1/1/vase5.png)

> 2\. I cerchi vengono traslati (spostati) da una variabile nell'asse Z.

### Creazione della superficie

Per creare una superficie utilizzando il nodo **Surface.ByLoft**, si devono combinare in un elenco tutti i cerchi traslati. Utilizzare **List.Create** per combinare tutti i nostri cerchi in un unico elenco, quindi infine eseguire l'output di questo elenco nel nodo **Surface.ByLoft** per visualizzare i risultati.

Disattivare anche l'anteprima in altri nodi per visualizzare solo Surface.ByLoft.

\![](<../images/10-1/1/vase6 (1).png>)

> 3\. Una superficie viene creata tramite il loft dei cerchi traslati.

## Risultati

Il nostro workflow è pronto. Ora è possibile utilizzare i **Number Slider** definiti nello script per creare diversi progetti di vasi.

![](../../1\_introduction/images/1-2/vase1.gif)

![](../images/10-1/1/vase7.png)
