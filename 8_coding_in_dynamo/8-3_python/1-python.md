# Nodi Python

Perché è opportuno utilizzare la programmazione testuale nell'ambiente di programmazione visiva di Dynamo? La [programmazione visiva](../../a\_appendix/visual-programming-and-dynamo.md) offre molti vantaggi. Consente di creare programmi senza apprendere una sintassi speciale in un'interfaccia visiva intuitiva. Tuttavia, un programma visivo può diventare disordinato e a volte può risultare carente in termini di funzionalità. Ad esempio, Python offre metodi molto più facili da realizzare per la scrittura di istruzioni condizionali (if/then) e il loop. Python è un potente strumento in grado di ampliare le funzionalità di Dynamo e di consentire di sostituire molti nodi con alcune righe di codice concise.

**Programma visivo:**

![](<../images/8-3/1/python node - visual vs textual programming.jpg>)

**Programma testuale:**

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

solid = IN[0]
seed = IN[1]
xCount = IN[2]
yCount = IN[3]

solids = []

yDist = solid.BoundingBox.MaxPoint.Y-solid.BoundingBox.MinPoint.Y
xDist = solid.BoundingBox.MaxPoint.X-solid.BoundingBox.MinPoint.X

for i in xRange:
	for j in yRange:
		fromCoord = solid.ContextCoordinateSystem
		toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin,Vector.ByCoordinates(0,0,1),(90*(i+j%val)))
		vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
		toCoord = toCoord.Translate(vec)
		solids.append(solid.Transform(fromCoord,toCoord))

OUT = solids
```

### Nodo Python

Come i blocchi di codice, i nodi Python sono un'interfaccia di script all'interno di un ambiente di programmazione visiva. Il nodo Python è disponibile in Script>Editor>Script Python nella libreria.

![](<../images/8-3/1/python node - the python node 01.jpg>)

Facendo doppio clic sul nodo, viene aperto l'editor di script Python. È inoltre possibile fare clic con il pulsante destro del mouse sul nodo e scegliere _Modifica..._. Si noterà del testo boilerplate in alto, che è utile per fare riferimento alle librerie necessarie. Gli input vengono memorizzati nella matrice IN. I valori vengono restituiti a Dynamo assegnandoli alla variabile OUT.

![](<../images/8-3/1/python node - the python node 02.jpg>)

La libreria Autodesk.DesignScript.Geometry consente di utilizzare la notazione punto analogamente ai blocchi di codice. Per ulteriori informazioni sulla sintassi di Dynamo, fare riferimento a [7-2\_design-script-syntax.md](../../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md "mention") e a [DesignScript Guide](https://dynamobim.org/wp-content/links/DesignScriptGuide.pdf) (per scaricare questo documento PDF, fare clic con il pulsante destro del mouse sul collegamento e scegliere Salva link con nome...). Digitando un tipo di geometria, ad esempio Point., verrà visualizzato un elenco di metodi per la creazione e l'esecuzione di una query sui punti.

![](<../images/8-3/1/python node - the python node 03.jpg>)

> I metodi includono costruttori quali _ByCoordinates_, azioni quali _Add_ e query quali le coordinate _X_, _Y_ e _Z_.

## Esercizio: Nodo personalizzato con script Python per la creazione di motivi da un modulo solido

### Parte I: Impostazione dello script Python

> Scaricare il file di esempio facendo clic sul collegamento seguente.
>
> Un elenco completo di file di esempio è disponibile nell'Appendice.

{% file src="../datasets/8-2/1/Python_Custom-Node.dyn" %}

In questo esempio, si scriverà uno script di Python che crea modelli da un modulo solido e lo si trasformerà in un nodo personalizzato. Innanzitutto, creare il modulo solido utilizzando i nodi di Dynamo.

![](<../images/8-3/1/python node - exercise pt I-01.jpg>)

> 1. **Rectangle.ByWidthLength:** creare un rettangolo che sarà la base del solido.
> 2. **Surface.ByPatch:** collegare il rettangolo all'input "_closedCurve_" per creare la superficie inferiore.

![](<../images/8-3/1/python node - exercise pt I-02.jpg>)

> 1. **Geometry.Translate:** collegate il rettangolo all'input "_geometry_" per spostarlo verso l'alto, utilizzando un blocco di codice per specificare lo spessore di base del solido.
> 2. **Polygon.Points:** eseguire la query sul rettangolo traslato per estrarre i punti degli angoli.
> 3. **Geometry.Translate:** utilizzare un blocco di codice per creare un elenco di quattro valori corrispondenti ai quattro punti, traslando un angolo del solido verso l'alto.
> 4. **Polygon.ByPoints:** utilizzare i punti traslati per ricreare il poligono superiore.
> 5. **Surface.ByPatch:** collegare il poligono per creare la superficie superiore.

Ora che sono presenti le superfici superiore e inferiore, creati i lati del solido tramite loft tra i due profili.

![](<../images/8-3/1/python node - exercise pt I-03.jpg>)

> 1. **List.Create:** collegare il rettangolo inferiore e il poligono superiore agli input dell'indice.
> 2. **Surface.ByLoft:** eseguire il loft dei due profili per creare i lati del solido.
> 3. **List.Create:** collegare le superfici superiore, laterale e inferiore agli input dell'indice per creare un elenco di superfici.
> 4. **Solid.ByJoinedSurfaces:** unire le superfici per creare il modulo solido.

Ora che è stato realizzato il solido, rilasciare un nodo Script Python nell'area di lavoro.

![](<../images/8-3/1/python node - exercise pt I-04.jpg>)

> 1. Per aggiungere altri input al nodo, fare clic sull'icona + sul nodo. Gli input sono denominati IN\[0], IN\[1] e così via per indicare che rappresentano le voci di un elenco.

Iniziare definendo gli input e l'output. Fare doppio clic sul nodo per aprire l'editor di Python. Seguire il codice riportato qui sotto per modificare il codice nell'editor.

![](<../images/8-3/1/python node - exercise pt I-05.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []

# Place your code below this line


# Assign your output to the OUT variable.
OUT = solids
```

Questo codice risulterà più utile mentre si prosegue nell'esercizio. Successivamente, occorre pensare alle informazioni necessarie per includere in una matrice il modulo solido. Per prima cosa, è necessario conoscere le quote del solido per determinare la distanza di traslazione. A causa di un bug del riquadro di delimitazione, è necessario utilizzare la geometria della curva dello spigolo per creare un riquadro di delimitazione.

![](../images/8-3/1/python07.png)

> Osservare il nodo Python in Dynamo. Si noti che viene utilizzata la stessa sintassi presente nei titoli dei nodi in Dynamo. Consultare il codice commentato qui sotto.

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

# Assign your output to the OUT variable.
OUT = solids
```

Poiché si eseguiranno sia la traslazione che la rotazione dei moduli solidi, utilizzare l'operazione Geometry.Transform. Guardando il nodo Geometry.Transform, si sa che per trasformare il solido sarà necessario un sistema di coordinate di origine e un sistema di coordinate di destinazione. L'origine è il sistema di coordinate contestuale del solido, mentre la destinazione sarà un sistema di coordinate diverso per ogni modulo incluso nella matrice. Ciò significa che si dovrà riprodurre a ciclo continuo i valori X e Y per trasformare il sistema di coordinate in modo diverso ogni volta.

![](<../images/8-3/1/python node - exercise pt I-06.jpg>)

```
# Load the Python Standard and DesignScript Libraries
import sys
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

# The inputs to this node will be stored as a list in the IN variables.
#The solid module to be arrayed
solid = IN[0]

#A Number that determines which rotation pattern to use
seed = IN[1]

#The number of solids to array in the X and Y axes
xCount = IN[2]
yCount = IN[3]

#Create an empty list for the arrayed solids
solids = []
#Create an empty list for the edge curves
crvs = []

# Place your code below this line
#Loop through edges an append corresponding curve geometry to the list
for edge in solid.Edges:
    crvs.append(edge.CurveGeometry)

#Get the bounding box of the curves
bbox = BoundingBox.ByGeometry(crvs)

#Get the x and y translation distance based on the bounding box
yDist = bbox.MaxPoint.Y-bbox.MinPoint.Y
xDist = bbox.MaxPoint.X-bbox.MinPoint.X

#Get the source coordinate system
fromCoord = solid.ContextCoordinateSystem

#Loop through x and y
for i in range(xCount):
    for j in range(yCount):
        #Rotate and translate the coordinate system
        toCoord = fromCoord.Rotate(solid.ContextCoordinateSystem.Origin, Vector.ByCoordinates(0,0,1), (90*(i+j%seed)))
        vec = Vector.ByCoordinates((xDist*i),(yDist*j),0)
        toCoord = toCoord.Translate(vec)
        #Transform the solid from the source coord syste, to the target coord system and append to the list
        solids.append(solid.Transform(fromCoord,toCoord))

# Assign your output to the OUT variable.
OUT = solids
```

Fare clic su Esegui, quindi salvare il codice. Collegare il nodo Python con lo script esistente come indicato di seguito.

![](<../images/8-3/1/python node - exercise pt I-07.jpg>)

> 1. Collegare l'output da **Solid.ByJoinedSurfaces** come primo input per il nodo Python e utilizzare un Code Block per definire gli altri input.
> 2. Creare un nodo **Topology.Edges** e utilizzare l'output del nodo Python come input.
> 3. Infine, creare un nodo **Edge.CurveGeometry** e utilizzare l'output di Topology.Edges come input.

Provare a modificare il valore iniziale (seme) per creare modelli diversi. È anche possibile modificare i parametri del modulo solido stesso per effetti differenti.

![](../images/8-3/1/python10.png)

### Parte II: Conversione del nodo dello script Python in nodo personalizzato

Dopo aver creato uno script di Python utile, salvarlo come nodo personalizzato. Selezionare il nodo dello script Python, fare clic con il pulsante destro del mouse su Area di lavoro e selezionare Crea nodo personalizzato.

![](<../images/8-3/1/python node - exercise pt II-01.jpg>)

Assegnare un nome, una descrizione e una categoria.

![](<../images/8-3/1/python node - exercise pt II-02.jpg>)

Verrà aperta una nuova area di lavoro in cui modificare il nodo personalizzato.

![](<../images/8-3/1/python node - exercise pt II-03.jpg>)

> 1. **Input:** modificare i nomi di input in modo che siano più descrittivi e aggiungere i tipi di dati e i valori di default.
> 2. **Output:** consente di modificare il nome di output.

Salvare il nodo come file .dyf e il nodo personalizzato dovrebbe riflettere le modifiche appena apportate.

![](<../images/8-3/1/python node - exercise pt II-04.jpg>)
