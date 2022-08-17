# Documentazione

La modifica dei parametri per la documentazione segue le lezioni apprese nelle sezioni precedenti. In questa sezione verrà esaminata la modifica dei parametri che non hanno effetto sulle proprietà geometriche di un elemento, ma piuttosto si prepara un file di Revit per la documentazione.

### Deviazione

Nell'esercizio riportato di seguito, verrà utilizzata una deviazione di base dal nodo del piano per creare una tavola di Revit per la documentazione. Ogni pannello della struttura del tetto definita in modo parametrico ha un valore diverso per la deviazione e si desidera chiamare l'intervallo di valori utilizzando il colore e includendo nell'abaco i punti adattivi da consegnare ad un consulente per le facciate, un ingegnere o un appaltatore.

![deviazione](images/6/deviation.jpg)

> La deviazione dal nodo del piano calcolerà la distanza con cui l'insieme di quattro punti varia dal piano di adattamento tra di loro. Questo è un modo rapido e semplice per studiare l'edificabilità.

## Esercizio

### Parte I: Impostazione del rapporto di apertura dei pannelli in base alla deviazione dal nodo del piano

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="datasets/6/Revit-Documenting.zip" %}

Iniziare con il file di Revit per questa sezione (o continuare dalla sezione precedente). Questo file presenta una serie di pannelli ETFE sul tetto. Per questo esercizio, si farà riferimento a questi pannelli.

![](<images/6/documenting - exercise I - 01.jpg>)

> 1. Aggiungere un nodo _Family Types_ all'area di disegno e scegliere _ROOF-PANEL-4PT_.
> 2. Collegare il nodo ad un nodo di selezione _All Elements of Family Type_ per caricare tutti gli elementi da Revit in Dynamo.

![](<images/6/documenting - exercise I - 02.jpg>)

> 1. Eseguire una query sulla posizione dei punti adattivi per ogni elemento con il nodo _AdaptiveComponent.Locations_.
> 2. Creare un poligono da questi quattro punti con il nodo _Polygon.ByPoints_. Notare che ora è disponibile una versione astratta del sistema con pannelli in Dynamo senza dover importare la geometria completa dell'elemento di Revit.
> 3. Calcolare la deviazione planare con il nodo _Polygon.PlaneDeviation_.

Per divertimento, come nell'esercizio precedente, impostare il rapporto di apertura di ogni pannello in base alla sua deviazione planare.

![](<images/6/documenting - exercise I - 03.jpg>)

> 1. Aggiungere un nodo _Element.SetParameterByName_ all'area di disegno e collegare i componenti adattivi all'input _element_. Collegare un _Code Block_ che riporta _Aperture Ratio_ all'input _parameterName_.
> 2. Non è possibile collegare direttamente i risultati della deviazione all'input value perché è necessario riassociare i valori all'intervallo di parametri.

![](<images/6/documenting - exercise I - 04.jpg>)

> 1. Utilizzando _Math.RemapRange_, riassociare i valori di deviazione ad un dominio compreso tra 0,15 e 0_._45 immettendo `0.15; 0.45;` nel _Code Block_.
> 2. Collegare questi risultati all'input value per _Element.SetParameterByName_.

Tornando in Revit, è possibile _in un certo senso_ sfruttare la modifica dell'apertura sulla superficie.

![Esercizio](../.gitbook/assets/13.jpg)

Eseguendo lo zoom avanti, diventa più chiaro che i pannelli chiusi sono ponderati verso gli angoli della superficie. Gli angoli aperti sono verso la parte superiore. Gli angoli rappresentano aree di maggiore deviazione mentre la bombatura ha una curvatura minima, pertanto ciò è utile.

![Esercizio](../.gitbook/assets/13a.jpg)

### Parte II: Colore e documentazione

L'impostazione del rapporto di apertura non mostra chiaramente la deviazione dei pannelli sul tetto; inoltre si sta modificando la geometria dell'elemento effettivo. Si supponga di voler semplicemente studiare la deviazione dal punto di vista della fattibilità della fabbricazione. Sarebbe utile colorare i pannelli in base all'intervallo di deviazione per la documentazione. È possibile farlo con la serie di passaggi riportata di seguito e con un processo molto simile a quello descritto sopra.

![](<images/6/documenting - exercise II - 01.jpg>)

> 1. Rimuovere _Element.SetParameterByName_ e i relativi nodi di input e aggiungere _Element.OverrideColorInView_.
> 2. Aggiungere un nodo _Color Range_ all'area di disegno e collegarlo all'input color di _Element.OverrideColorInView_. Per creare la sfumatura, è ancora necessario collegare i valori della deviazione all'intervallo di colori.
> 3. Posizionando il cursore del mouse sull'input _value_, è possibile vedere che i valori per l'input devono essere compresi tra _0_ e _1_ per assegnare un colore a ciascun valore. È necessario riassociare i valori della deviazione a questo intervallo.

![](<images/6/documenting - exercise II - 02.jpg>)

> 1. Utilizzando _Math.RemapRange_, riassociare i valori della deviazione planare ad un intervallo compreso tra* 0* e _1_. Nota È anche possibile utilizzare il nodo _MapTo_ per definire un dominio di origine.
> 2. Collegare i risultati ad un nodo _Color Range_.
> 3. Notare che l'output è un intervallo di colori anziché un intervallo di numeri.
> 4. Se l'opzione è impostata su Manuale, fare clic su _Esegui_. Dovrebbe essere possibile procedere facilmente da questo momento in poi avendo impostato l'opzione su Automatico.

In Revit, si nota una sfumatura molto più leggibile che è rappresentativa della deviazione planare basata sull'intervallo di colori. Ma se si vogliono personalizzare i colori? Notare che i valori della deviazione minima sono rappresentati in rosso, che sembra essere l'opposto di ciò che ci si aspetterebbe. Si desidera visualizzare la deviazione massima in rosso, con la deviazione minima rappresentata da un colore più tenue. Tornare in Dynamo e modificare questo aspetto.

![](../.gitbook/assets/09.jpg)

![](<images/6/documenting - exercise II - 04.jpg>)

> 1. Utilizzando un _Code Block_, aggiungere due numeri su due righe diverse: `0;` e `255;`.
> 2. Creare un colore rosso e blu collegando i valori appropriati a due nodi _Color.ByARGB_.
> 3. Creare un elenco da questi due colori.
> 4. Collegare questo elenco all'input _colors_ di _Color Range_ e osservare l'aggiornamento dell'intervallo di colori personalizzato.

Tornando in Revit, è ora possibile distinguere meglio le aree di deviazione massima negli angoli. Ricordarsi che questo nodo serve per sostituire un colore in una vista, così può essere davvero utile disporre di una tavola specifica nel gruppo di disegni che si concentra su un particolare tipo di analisi.

![Exercise](<../.gitbook/assets/07 (6).jpg>)

### Parte III: Creazione di abachi

Selezionando un pannello ETFE in Revit, si noterà che sono presenti quattro parametri di istanza, XYZ1, XYZ2, XYZ3 e XYZ4. Questi sono tutti vuoti dopo la loro creazione. Si tratta di parametri basati sul testo che richiedono valori. Verrà utilizzato Dynamo per scrivere le posizioni dei punti adattivi su ciascun parametro. Ciò consente l'interoperabilità se la geometria deve essere inviata ad un ingegnere o un consulente per le facciate.

![](<images/6/documenting - exercise III - 01.jpg>)

In una tavola di esempio, è presente un abaco di grandi dimensioni vuoto. I parametri XYZ sono parametri condivisi nel file di Revit, il che consente di aggiungerli all'abaco.

![Exercise](<../.gitbook/assets/03 (8).jpg>)

Se si esegue lo zoom avanti, i parametri XYZ devono ancora essere immessi. I primi due parametri vengono gestiti da Revit.

![Exercise](<../.gitbook/assets/02 (9).jpg>)

Per scrivere questi valori, verrà eseguita un'operazione sugli elenchi complessa. Il grafico stesso è semplice, ma i concetti si basano in modo significativo sul mappaggio degli elenchi, come discusso nel capitolo sugli elenchi.

![](<images/6/documenting - exercise III - 04.jpg>)

> 1. Selezionare tutti i componenti adattivi con due nodi.
> 2. Estrarre la posizione di ciascun punto con _AdaptiveComponent.Locations_.
> 3. Convertire questi punti in stringhe. Ricordarsi che il parametro è basato sul testo, quindi occorre immettere il tipo di dati corretto.
> 4. Creare un elenco delle quattro stringhe che definiscono i parametri da modificare: _XYZ1, XYZ2, XYZ3_ e _XYZ4_.
> 5. Collegare questo elenco all'input _parameterName_ di _Element.SetParameterByName_.
> 6. Collegare _Element.SetParameterByName_ all'input _combinator_ di _List.Combine._ Collegare i _componenti adattivi_ a _list1_. Collegare _String from Object_ a _list2_.

Qui si sta eseguendo il mappaggio degli elenchi, poiché si stanno scrivendo quattro valori per ogni elemento, il che crea una struttura dei dati complessa. Il nodo _List.Combine_ definisce un'operazione di un livello più basso nella gerarchia dei dati. Per questo motivo, gli input element e value di _Element.SetParameterByName_ restano vuoti. _List.Combine_ collega i sottoelenchi dei relativi input agli input vuoti di _Element.SetParameterByName_, in base all'ordine in cui sono collegati.

Selezionando un pannello in Revit, notare che sono presenti valori di stringa per ogni parametro. Realisticamente, è necessario creare un formato più semplice per scrivere un punto (X, Y, Z). Questa operazione può essere eseguita con operazioni di stringa in Dynamo, ma in questo caso viene ignorata per rimanere all'interno dell'ambito di questo capitolo.

![](<../.gitbook/assets/04 (5).jpg>)

Vista dell'abaco di esempio con i parametri compilati.

![](<../.gitbook/assets/01 (9).jpg>)

Ogni pannello ETFE ora dispone delle coordinate XYZ scritte per ogni punto adattivo, che rappresentano gli angoli di ogni pannello per la fabbricazione.

![Exercise](<../.gitbook/assets/00 (8).jpg>)
