# Colore

Il colore è un ottimo tipo di dati per la creazione di effetti visivi accattivanti e per il rendering delle differenze nell'output del programma visivo in uso. Quando si utilizzano dati astratti e numeri variabili, a volte è difficile vedere cosa cambia e in che misura. Questa è un'ottima applicazione per i colori.

### Creazione di colori

I colori in Dynamo vengono creati utilizzando gli input ARGB. Corrispondono ai canali alfa, rosso, verde e blu. L'alfa rappresenta la _trasparenza_ del colore, mentre gli altri tre vengono utilizzati come colori primari per generare l'intero spettro dei colori.

| Icona                                     | Nome (Sintassi)                 | Input  | Output |
| ---------------------------------------- | ----------------------------- | ------- | ------- |
| ![](<../images/5-1/ColorbyARGB (1).jpg>) | Colore ARGB (**Color.ByARGB**) | A,R,G,B | colore   |

### Esecuzione di una query sui valori dei colori

I colori nella tabella riportata di seguito eseguono una query sulle proprietà utilizzate per definire il colore: alfa, rosso, verde e blu. Notare che il nodo Color.Components fornisce tutti e quattro come output diversi, il che lo rende il nodo preferito per l'esecuzione di una query sulle proprietà di un colore.

| Icona                                          | Nome (Sintassi)                     | Input | Output    |
| --------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](<../images/5-1/ColorAlpha(1)(1) (1).jpg>) | Alfa (**Color.Alpha**)           | colore  | A          |
| ![](<../images/5-1/ColorRed (1).jpg>)         | Rosso (**Color.Red**)               | colore  | R          |
| ![](<../images/5-1/ColorGreen(1)(1) (1).jpg>) | Verde (**Color.Green**)           | colore  | G          |
| ![](<../images/5-1/ColorBlue (1).jpg>)        | Blu (**Color.Blue**)             | colore  | B          |
| ![](<../images/5-1/ColorComponent (1).jpg>)   | Componenti (**Color.Components**) | colore  | A,R,G,B |

I colori nella tabella riportata di seguito corrispondono allo **spazio colore HSB**. La divisione del colore in tonalità, saturazione e luminosità è probabilmente più intuitiva per la modalità di interpretazione del colore. Quale colore dovrebbe essere? Quanto dovrebbe essere colorato? E quanto dovrebbe essere chiaro o scuro il colore? Si tratta rispettivamente della suddivisione di tonalità, saturazione e luminosità.

| Icona                                         | Nome (Sintassi)                     | Input | Output    |
| -------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](<../images/5-1/ColorHue (1).jpg>)        | Tonalità (**Color.Hue**)               | colore  | Tonalità        |
| ![](<../images/5-1/ColorSaturation (1).jpg>) | Saturazione (**Color.Saturation**) | colore  | Saturazione |
| ![](<../images/5-1/ColorBrightness (1).jpg>) | Luminosità (**Color.Brightness**) | colore  | Luminosità |

### Intervallo colori

L'intervallo di colori è simile al nodo **Remap Range** dell'esercizio [\#part-ii-from-logic-to-geometry](3-logic.md#part-ii-from-logic-to-geometry "mention"): riassocia un elenco di numeri ad un altro dominio. Tuttavia, anziché il mappaggio ad un dominio di _numeri_, si associa ad una _sfumatura di colore_ in base a numeri di input compresi tra 0 e 1.

L'attuale nodo funziona bene, ma può essere un po' complicato fare in modo che tutto funzioni la prima volta. Il modo migliore per acquisire familiarità con la sfumatura di colore consiste nel verificarla in modo interattivo. Ecco un rapido esercizio per esaminare come impostare una sfumatura con colori di output corrispondenti ai numeri.

![](../images/5-3/5/color-colorrange.jpg)

> 1. Definire tre colori: utilizzando un nodo **Code Block**, definire _rosso, verde_ e _blu_ collegando le combinazioni appropriate di _0_ e _255_.
> 2. **Creare l'elenco:** unire i tre colori in un elenco.
> 3. Definire gli indici: creare un elenco per definire le posizioni dei grip di ciascun colore (comprese tra 0 e 1). Notare il valore di 0.75 per il verde. In questo modo il colore verde viene posizionato per 3/4 nella sfumatura orizzontale sul dispositivo di scorrimento Color Range.
> 4. **Code Block:** immettere i valori (compresi tra 0 e 1) da convertire in colori.

### Anteprima colori

Il nodo **Display.ByGeometry** offre la possibilità di colorare la geometria nella finestra di Dynamo. Ciò è utile per separare diversi tipi di geometria, dimostrare un concetto parametrico o definire una legenda di analisi per la simulazione. Gli input sono semplici: geometry e color. Per creare una sfumatura come l'immagine riportata sopra, l'input color è collegato al nodo **Color** **Range**.

![](../images/5-3/5/color-colorpreview.jpg)

### Colore sulle superfici

Il nodo **Display.BySurfaceColors** consente di associare dati su una superficie con il colore. Questa funzionalità introduce alcune straordinarie possibilità di visualizzazione dei dati ottenuti tramite l'analisi discreta, come quella solare, energetica e di prossimità. L'applicazione di colore ad una superficie in Dynamo è simile all'applicazione di una composizione ad un materiale in altri ambienti CAD. Si dimostrerà come utilizzare questo strumento nel breve esercizio riportato di seguito.

![](../images/5-3/5/12\(1\).jpg)

## Esercizio

### Elica di base con colori

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-3/5/Building Blocks of Programs - Color.dyn" %}

Questo esercizio si concentra sul controllo parametrico del colore in parallelo con la geometria. La geometria è un'elica di base, che è possibile definire di seguito utilizzando **Code Block**. Si tratta di un metodo rapido e semplice per creare una funzione parametrica; poiché l'attenzione è rivolta al colore (anziché alla geometria), si utilizza il blocco di codice per creare in modo efficiente l'elica senza sovraccaricare l'area di disegno. Il blocco di codice verrà utilizzato più frequentemente man mano che nella guida introduttiva si passa ad un argomento più avanzato.

![](../images/5-3/5/color-basichelixwithcolors01.jpg)

> 1. **Code Block:** definire i due blocchi di codice con le formule indicate sopra. Si tratta di un metodo parametrico rapido per la creazione di una spirale.
> 2. **Point.ByCoordinates:** collegare i tre output del blocco di codice alle coordinate per il nodo.

Ora è presente una serie di punti che creano un'elica. Il passaggio successivo consiste nel creare una curva attraverso i punti in modo da poter visualizzare l'elica.

![](../images/5-3/5/color-basichelixwithcolors02.jpg)

> 1. **PolyCurve.ByPoints:** collegare l'output **Point.ByCoordinates** all'input _points_ per il nodo. Si ottiene una curva elicoidale.
> 2. **Curve.PointAtParameter:** collegare l'output **PolyCurve.ByPoints** all'input _curve_. Lo scopo di questo passaggio è creare un punto attrattore parametrico che scorre lungo la curva. Poiché la curva sta valutando un punto in corrispondenza di un parametro, sarà necessario immettere un valore _param_ compreso tra 0 e 1.
> 3. **Number Slider:** dopo l'aggiunta all'area di disegno, modificare il valore _min_ in _0.0_, il valore _max_ in _1.0_ e il valore _step_ in _0.01_. Collegare l'output del dispositivo di scorrimento all'input _param_ per **Curve.PointAtParameter**. Ora è visibile un punto per la lunghezza dell'elica, rappresentato da una percentuale del dispositivo di scorrimento (0 in corrispondenza del punto iniziale, 1 in corrispondenza del punto finale).

Una volta creato il punto di riferimento, verrà confrontata la distanza dal punto di riferimento ai punti originali che definiscono l'elica. Questo valore della distanza controllerà la geometria e il colore.

![](../images/5-3/5/color-basichelixwithcolors03.jpg)

> 1. **Geometry.DistanceTo:** collegare l'output **Curve.PointAtParameter** all'_input_. Collegare **Point.ByCoordinates** all'input geometry.
> 2. **Watch:** l'output risultante mostra un elenco di distanze da ogni punto elicoidale al punto di riferimento lungo la curva.

Il passaggio successivo consiste nel controllare i parametri con l'elenco di distanze dai punti elicoidali al punto di riferimento. È possibile utilizzare questi valori della distanza per definire i raggi di una serie di sfere lungo la curva. Per mantenere le sfere ad una dimensione adatta, è necessario _riassociare_ i valori per la distanza.

![](../images/5-3/5/color-basichelixwithcolors04.jpg)

> 1. **Math.RemapRange:** collegare l'output **Geometry.DistanceTo** all'input numbers.
> 2. **Code Block:** collegare un blocco di codice con un valore di _0.01_ all'input _newMin_ e un blocco di codice con un valore di _1_ all'input di _newMax_.
> 3. **Watch:** collegare l'output **Math.RemapRange** ad un nodo e l'output **Geometry.DistanceTo** ad un altro. Confrontare i risultati.

In questo passaggio è stato riassociato l'elenco di distanze in modo che siano un intervallo più piccolo. È possibile modificare i valori _newMin_ e _newMax_ in base alle esigenze specifiche. I valori verranno riassociati e avranno lo stesso _rapporto di distribuzione_ nel dominio.

![](../images/5-3/5/color-basichelixwithcolors05.jpg)

> 1. **Sphere.ByCenterPointRadius:** collegare l'output **Math.RemapRange** all'input _radius_ e l'output **Point.ByCoordinates** originale all'input _centerPoint_.

Modificare il valore del dispositivo di scorrimento dei numeri e osservare la dimensione delle sfere aggiornate. Si ottiene una maschera di inserimento parametrica.

![](../images/5-3/5/color-basichelixwithcolors06.gif)

La dimensione delle sfere dimostra la serie parametrica definita da un punto di riferimento lungo la curva. Per controllare il colore, utilizzare lo stesso concetto per il raggio della sfera.

![](../images/5-3/5/color-basichelixwithcolors07.jpg)

> 1. **Color Range:** aggiungere all'area di disegno. Quando si posiziona il cursore sull'input _value_, si noterà che i numeri richiesti sono compresi tra 0 e 1. È necessario riassociare i numeri dell'output **Geometry.DistanceTo** in modo che siano compatibili con questo dominio.
> 2. **Sphere.ByCenterPointRadius:** per il momento, disattivare l'anteprima in questo nodo (_fare clic con il pulsante destro del mouse > Anteprima_).

![](../images/5-3/5/color-basichelixwithcolors08.jpg)

> 1. **Math.RemapRange:** questo processo dovrebbe essere noto. Collegare l'output **Geometry.DistanceTo** all'input numbers.
> 2. **Code Block:** analogamente ad un passaggio precedente, creare un valore di _0_ per l'input _newMin_ e un valore di _1_ per l'input _newMax_. In questo caso, è possibile definire due output da un blocco di codice.
> 3. **Color Range:** collegare l'output **Math.RemapRange** all'input _value_.

![](../images/5-3/5/color-basichelixwithcolors09.jpg)

> 1. **Color.ByARGB:** questa è la procedura che consente di creare due colori. Sebbene questo processo possa sembrare complesso, è identico ai colori RGB di un altro software. Per eseguirlo, occorre semplicemente utilizzare la programmazione visiva.
> 2. **Code Block:** creare due valori di _0_ e _255_. Collegare i due output ai due input **Color.ByARGB** come mostra l'immagine riportata sopra (o creare i due colori preferiti).
> 3. **Color Range:** l'input _colors_ richiede un elenco di colori. È necessario creare questo elenco dai due colori creati nel passaggio precedente.
> 4. **List.Create:** unire i due colori in un elenco. Collegare l'output all'input _colors_ per **Color Range**.

![](../images/5-3/5/color-basichelixwithcolors10.jpg)

> 1. **Display.ByGeometryColor:** collegare **Sphere.ByCenterPointRadius** all'input _geometry_ e _Color Range_ all'input _color_. Si ottiene una sfumatura uniforme nel dominio della curva.

Se si modifica il valore di **Number Slider** di prima nella definizione, i colori e le dimensioni vengono aggiornati. In questo caso, i colori e la dimensione del raggio sono direttamente correlati: ora si dispone di un collegamento visivo tra due parametri.

![](../images/5-3/5/color-basichelixwithcolors11.gif)

### Esercizio con il colore sulle superfici

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/5-3/5/BuildingBlocks of Programs - ColorOnSurface.zip" %}

Innanzitutto, è necessario creare (o fare riferimento ad) una superficie da utilizzare come input per il nodo **Display.BySurfaceColors**. In questo esempio si esegue il loft tra una curva seno e una curva coseno.

![](../images/5-3/5/color-coloronsurface01.jpg)

> 1. Questo gruppo di nodi sta creando punti lungo l'asse Z, quindi li disloca in base alle funzioni seno e coseno. Gli elenchi a due punti vengono quindi utilizzati per generare curve NURBS.
> 2. **Surface.ByLoft**: generare una superficie interpolata tra l'elenco di curve NURBS.

![](../images/5-3/5/color-coloronsurface02.jpg)

> 1. **File Path**: selezionare un file di immagine da campionare per i dati di pixel a valle.
> 2. Utilizzare **File.FromPath** per convertire il percorso del file in un file, quindi passare a **Image.ReadFromFile** per produrre un'immagine per il campionamento.
> 3. **Image.Pixels**: immettere un'immagine e fornire un valore di esempio da utilizzare lungo le quote X e Y dell'immagine.
> 4. **Dispositivo di scorrimento**: fornire valori di esempio per **Image.Pixels**.
> 5. **Display.BySurfaceColors**: associare la serie di valori del colore sulla superficie rispettivamente lungo X e Y.

Anteprima ravvicinata della superficie di output con campioni dalla risoluzione di 400 x 300

![](../images/5-3/5/color-coloronsurface03.jpg)
