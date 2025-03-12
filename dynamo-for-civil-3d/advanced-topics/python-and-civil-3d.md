# Python e Civil 3D

Sebbene Dynamo sia estremamente potente come strumento di [programmazione visiva](../../a\_appendix/a-1\_visual-programming-and-dynamo.md), è anche possibile andare oltre i nodi e i fili e scrivere il codice in forma testuale. Esistono due modi per eseguire questa operazione:

1. Scrivere **DesignScript** utilizzando un blocco di codice
2. Scrivere **Python** utilizzando un nodo Python

In questa sezione verrà descritto come utilizzare Python nell'ambiente Civil 3D per sfruttare le API .NET di AutoCAD e Civil 3D.

{% hint style="info" %} Per ulteriori informazioni generali sull'utilizzo di Python in Dynamo, consultare la sezione [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention"). {% endhint %}

## Documentazione sull'API

Sia AutoCAD che Civil 3D dispongono di diverse API che consentono agli sviluppatori di estendere il prodotto principale con funzionalità personalizzate. Nel contesto di Dynamo, sono rilevanti le **API .NET gestite**. I collegamenti riportati di seguito sono essenziali per comprendere la struttura delle API e il loro funzionamento.

[AutoCAD .NET API Developer's Guide](https://help.autodesk.com/view/OARX/2024/ITA/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[Manuale di riferimento dell'API di AutoCAD .NET](https://help.autodesk.com/view/OARX/2024/ITA/?guid=OARX-ManagedRefGuide-What_s_New)

[Civil 3D .NET API Developer's Guide](https://help.autodesk.com/view/CIV3D/2024/ITA/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Civil 3D .NET API Reference Guide](https://help.autodesk.com/view/CIV3D/2024/ITA/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %} Nel corso di questa sezione, è possibile che vi siano alcuni concetti con cui non si ha familiarità, come i database, le transazioni, i metodi, le proprietà, ecc. Molti di questi concetti sono fondamentali per l'utilizzo delle API .NET e non sono specifici di Dynamo o Python. Non è compito di questa sezione della Guida introduttiva discutere in dettaglio queste voci, per cui si consiglia di consultare spesso i collegamenti riportati sopra per ulteriori informazioni. {% endhint %}

## Modello di codice

Quando si modifica per la prima volta un nuovo nodo Python, questo verrà precompilato con il codice modello per iniziare. Ecco una schematizzazione del modello con spiegazioni su ciascun blocco.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Modello Python di default in Civil 3D</p></figcaption></figure>

> 1. Importa i moduli `sys` e `clr`, entrambi necessari per il corretto funzionamento dell'interprete Python. In particolare, il modulo `clr` consente di trattare gli spazi dei nomi .NET essenzialmente come pacchetti Python.
> 2. Carica gli assiemi standard (ad esempio, DLL) per l'utilizzo delle API .NET gestite per AutoCAD e Civil 3D.
> 3. Aggiunge riferimenti agli spazi dei nomi standard di AutoCAD e Civil 3D. Sono equivalenti alle direttive `using` o `Imports` rispettivamente in C# o VB.NET.
> 4. Le porte di input del nodo sono accessibili utilizzando un elenco predefinito denominato `IN`. È possibile accedere ai dati in una porta specifica utilizzando il relativo numero di indice, ad esempio `dataInFirstPort = IN[0]`.
> 5. Ottiene il documento e l'editor attivi.
> 6. Blocca il documento e avvia una transazione di database.
> 7. Questo è il punto in cui si dovrebbe inserire la maggior parte della logica dello script.
> 8. Annullare i commenti di questa riga per eseguire il commit della transazione dopo il completamento del lavoro principale.
> 9. Se si desidera eseguire l'output di eventuali dati dal nodo, assegnarli alla variabile `OUT` alla fine dello script.

{% hint style="info" %} **Personalizzazione**\
 È possibile modificare il modello Python di default modificando il file `PythonTemplate.py` che si trova in `C:\ProgramData\Autodesk\C3D <versione>\Dynamo`. {% endhint %}

## Esempio

Esaminiamo un esempio per dimostrare alcuni dei concetti essenziali della scrittura di script Python in Dynamo for Civil 3D.

### Scopo

> :dart: Ottenere la geometria del contorno di tutti i drenaggi di un disegno.

### Set di dati

Di seguito sono riportati alcuni file di esempio a cui è possibile fare riferimento per questo esercizio.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Panoramica della soluzione

Ecco una panoramica della logica di questo grafico.

> 1. Rivedere la documentazione sull'API di Civil 3D
> 2. Selezionare tutti i drenaggi nel documento in base al nome del layer
> 3. Annullamento del wrapping degli oggetti di Dynamo per accedere ai membri interni dell'API di Civil 3D
> 4. Creare punti di Dynamo da punti di AutoCAD
> 5. Crea PolyCurve dai punti

Procediamo!

### Revisione della documentazione sull'API

Prima di iniziare a costruire il grafico e scrivere il codice, è consigliabile esaminare la documentazione sull'API di Civil 3D e avere un'idea di ciò che è disponibile nell'API. In questo caso, è presente una [proprietà nella classe Catchment](https://help.autodesk.com/view/CIV3D/2024/ITA/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c) che restituirà i punti di contorno del drenaggio. Tenere presente che questa proprietà restituisce un oggetto `Point3dCollection`, di cui Dynamo non sa che farsene. In altre parole, non sarà possibile creare una PolyCurve da un `Point3dCollection`, quindi alla fine sarà necessario convertire tutto in punti di Dynamo. Vedere più avanti per saperne di più.

### Recupero di tutti i drenaggi

Ora è possibile iniziare a costruire la logica del grafico. La prima cosa da fare è ottenere un elenco di tutti i drenaggi nel documento. Sono disponibili nodi per questo, pertanto non è necessario includerli nello script Python. L'utilizzo dei nodi offre una migliore visibilità per un altro utente che potrebbe leggere il grafico (anziché aggiungere codice eccessivo in uno script Python) e mantiene lo script Python concentrato su una cosa: restituire i punti di contorno dei drenaggi.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Recupero di tutti i drenaggi nel documento in base al layer</p></figcaption></figure>

Notare che l'output del nodo **All Objects on Layer** è un elenco di CivilObject. Questo perché Dynamo for Civil 3D non dispone attualmente di nodi per l'utilizzo dei drenaggi, che è il motivo completo per cui è necessario accedere all'API tramite Python.

### Annullamento del wrapping di oggetti

Prima di proseguire, occorre parlare brevemente di un concetto importante. Nella sezione [node-library.md](../node-library.md "mention"), è stata descritta la relazione tra Object e CivilObject. Per aggiungere un po' più di dettagli a questo argomento, un **Object di Dynamo** è un wrapper intorno ad un **Entity di AutoCAD**. Analogamente, un **CivilObject di Dynamo** è un wrapper intorno ad un **Entity di Civil 3D**. È possibile annullare il wrapping di un oggetto accedendo alle relative proprietà `InternalDBObject` o `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Tipo di Dynamo</th><th width="373">Wrapping</th></tr></thead><tbody><tr><td><strong>Object</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entity</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entity</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} In genere, è più sicuro ottenere l'ID oggetto utilizzando la proprietà `InternalObjectId` e quindi accedere all'oggetto con wrapping in una transazione. Questo perché la proprietà `InternalDBObject` restituisce un DBObject di AutoCAD non in stato scrivibile. {% endhint %}

### Script Python

Ecco l'intero script Python che esegue il lavoro di accesso agli oggetti Catchment interni, che ottengono i loro punti di contorno. Le righe evidenziate rappresentano quelle modificate/aggiunte dal codice modello di default.

{% hint style="info" %} Fare clic sul testo sottolineato nello script per una spiegazione di ogni riga. {% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Load the Python Standard and DesignScript Libraries
import sys
import clr

# Add Assemblies for AutoCAD and Civil3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Import references from AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Import references from Civil3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# The inputs to this node will be stored as a list in the IN variables.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("The input is null or empty.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Commit before end transaction
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Assign your output to the OUT variable.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} In genere, è consigliabile includere la maggior parte della logica di script all'interno di una transazione. In questo modo si garantisce l'accesso sicuro agli oggetti che lo script sta leggendo/scrivendo. In molti casi, omettere una transazione può causare un errore irreversibile. {% endhint %}

### Creazione di PolyCurve

In questa fase, lo script Python dovrebbe generare un elenco di punti di Dynamo che è possibile vedere nell'anteprima di sfondo. L'ultimo passaggio consiste nel creare semplicemente PolyCurve dai punti. Si noti che questa operazione potrebbe essere effettuata anche direttamente nello script Python, ma noi l'abbiamo volutamente inserita al di fuori dello script in un nodo, in modo che sia più visibile. Ecco come appare il grafico finale.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Il grafico finale</p></figcaption></figure>

### Risultato

Ed ecco la geometria finale di Dynamo.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Le PolyCurve di Dynamo risultanti per i contorni di drenaggio</p></figcaption></figure>

> :tada: Missione compiuta!

## Confronto tra IronPython e CPython

Solo una breve nota prima di concludere. A seconda della versione di Civil 3D in uso, il nodo Python potrebbe essere configurato in modo diverso. In **Civil 3D 2020 e 2021**, Dynamo ha utilizzato uno strumento denominato **IronPython** per spostare i dati tra gli oggetti .NET e gli script Python. In **Civil 3D 2022**, tuttavia, Dynamo è passato a utilizzare l'interprete Python nativo standard (noto anche come **CPython**) anziché Python 3. I vantaggi di questa transizione includono l'accesso alle librerie moderne più diffuse e alle nuove funzionalità della piattaforma, alla manutenzione essenziale e alle patch di sicurezza.

{% hint style="info" %} È possibile leggere ulteriori informazioni su questa transizione e su come aggiornare gli script precedenti nel [blog di Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Se si desidera continuare ad utilizzare IronPython, sarà sufficiente installare il pacchetto **DynamoIronPython2.7** utilizzando Dynamo Package Manager. {% endhint %}

[^1]: per default, la libreria della geometria di Dynamo non viene aggiunta all'ambiente Python. L'obiettivo con questo script è di generare un elenco di punti di Dynamo per i contorni di drenaggio, pertanto è necessario aggiungere questa linea per creare i punti in un secondo momento.

[^2]: questa riga ottiene la classe specifica necessaria dalla libreria della geometria di Dynamo. Notare che qui viene specificato `import Point as DynPoint` anziché `import *`, poiché quest'ultimo introdurrebbe interferenze di denominazione.

[^3]: qui la variabile di default `dataEnteringNode` viene rinominata in `objs` in modo che sia più descrittiva.

[^4]: qui è possibile specificare esattamente quale porta di input contiene i dati desiderati anziché il valore di default `IN`, che fa riferimento all'intero elenco di tutti gli input.

[^5]: crea un elenco vuoto a cui verranno aggiunti i dati di output in un secondo momento.

[^6]: serve per evitare un potenziale input vuoto o nullo nello script.

[^7]: anziché interrompere lo script, verrà visualizzato un messaggio utile per spiegare perché lo script non può continuare.

[^8]: il resto dello script presuppone che si disponga di un elenco di oggetti con cui lavorare (anziché di un singolo oggetto). Tuttavia, nel caso in cui sia presente un solo oggetto (ad esempio, un disegno con un singolo drenaggio), si desidera comunque che lo script possa essere eseguito.

[^9]: se l'input non è un elenco (ad esempio, un singolo oggetto), è sufficiente creare un nuovo elenco con l'oggetto come unica voce.

[^10]: consente di avviare un loop per ogni oggetto dell'elenco.

[^11]: annullare il wrapping dell'oggetto di Dynamo ottenendo il relativo ID oggetto.

[^12]: recuperare l'oggetto con wrapping dal database di AutoCAD. Notare che il parametro OpenMode è impostato su `ForRead`, perché non si prevede di apportare modifiche agli oggetti. Si sta semplificando la procedura per la creazione di query sui dati.

[^13]: è possibile che l'elenco di input degli oggetti contenga una combinazione di Catchment e altra voce non Catchment. È necessario verificare questa situazione e gestirla in modo appropriato (ad esempio, è sufficiente continuare questa iterazione del loop se la voce è effettivamente un drenaggio).

[^14]: se lo script arriva a questo punto, si sa che l'oggetto è effettivamente un drenaggio. Verrà aggiunta qui una nuova variabile solo per mantenere la denominazione ordinata e comprensibile.

[^15]: qui è possibile recuperare i punti di contorno del drenaggio utilizzando la proprietà appropriata che è stata cercata in precedenza nei documenti dell'API. Come accennato in precedenza, questo ci fornirà un oggetto `Point3dCollection`, che è essenzialmente un elenco di punti AutoCAD. Sarà necessario "convertire" tali punti in punti di Dynamo in modo che siano utili.

[^16]: creare un elenco vuoto per memorizzare i punti di contorno per questo drenaggio.

[^17]: avviare un loop per ogni oggetto `Point3d` in `Point3dCollection`.

[^18]: creare un punto di Dynamo utilizzando le coordinate del punto di AutoCAD.

[^19]: aggiungere il punto all'elenco.

[^20]: una volta completata la "conversione" di tutti i punti di AutoCAD in punti di Dynamo, si aggiunge l'elenco risultante all'output. In seguito, il loop verrà spostato sull'oggetto successivo nell'elenco di input.

[^21]: rimuovere i commenti da questa riga per eseguire il commit della transazione.

[^22]: e infine, ma non meno importante, viene generato l'elenco di punti di Dynamo.
