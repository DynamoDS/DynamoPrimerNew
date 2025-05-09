# Differenze tra Dynamo compute service e Desktop Dynamo

In questa pagina vengono evidenziate le differenze di cui è necessario tenere conto durante la scrittura di programmi di Dynamo da eseguire nel contesto cloud di Dynamo compute service.

## Che cos'è DaaS?

DaaS, Dynamo as a Service, Dynamo compute service, ecc. si riferiscono tutti alla stessa cosa: il runtime di base di Dynamo in esecuzione in un contesto cloud. Ciò significa che il grafico non è in esecuzione sul computer in uso. Attualmente è possibile accedere a DaaS solo tramite l'estensione Dynamo Player per Forma, in cui gli utenti possono caricare e gestire i file `.dyn` creati nell'ambiente desktop, eseguire i file `.dyn` condivisi dai colleghi tramite l'estensione o utilizzare le routine `.dyn` precaricate fornite da Autodesk come esempi.

Poiché i grafici vengono eseguiti in questo contesto cloud e non sul computer in uso, DaaS attualmente non può utilizzare direttamente i contesti host di Dynamo tradizionali (Revit, Civil 3D e così via). Se si desidera utilizzare i tipi di tali programmi nel grafico, sarà necessario serializzarli (salvarli) nel grafico utilizzando il nodo `Data.Remember` o altre tecniche di serializzazione nel grafico. Si tratta di workflow simili a quelli da utilizzare durante la scrittura di grafici per Progettazione generativa in Revit.

## Quale versione di Dynamo sta eseguendo il codice?

La versione è la 3.x e viene aggiornata di frequente in base al ramo principale open source di Dynamo.

## Quali pacchetti/nodi sono disponibili in questa versione di Dynamo?

* Per la maggior parte dei nodi principali, vedere la sezione successiva per alcune limitazioni specifiche.
* `DynamoFormaBeta` per l'interazione con l'API di Forma.
* `VASA` per la voxelizzazione/l'analisi efficiente.
* `MeshToolKit` per la manipolazione della mesh. Mesh Toolkit è disponibile anche al momento dell'acquisto, a partire da Dynamo 3.4.
* `RefineryToolkit` per algoritmi utili che consentono test di interferenza, distanza dalla vista, il percorso più breve, isovista e così via.

## A cosa occorre prestare attenzione quando si scrivono i grafici per DaaS?

* I nodi Python non funzioneranno. Questi _attualmente_ non verranno eseguiti.
* Non è possibile utilizzare pacchetti personalizzati.
* Il layer interfaccia utente/vista dei nodi dell'interfaccia utente non verrà eseguito. Non prevediamo che questo sia un problema con le funzionalità principali, ma è bene tenerlo presente se si verifica un bug relativo ad un nodo con l'interfaccia utente personalizzata.
* Le funzionalità solo per Windows non funzioneranno. Ad esempio, se si tenta di utilizzare il Registro di sistema di Windows o WPF, l'operazione non riuscirà.
* Le estensioni della vista non verranno caricate.
* I nodi del file system non saranno molto utili. Eventuali file a cui si fa riferimento sul computer locale non esisteranno durante l'esecuzione in DaaS.
* I nodi di interoperabilità di Excel/DSOffice non funzioneranno. I nodi di Open XML dovrebbero funzionare.
* Le richieste di rete in generale non funzioneranno, anche se è possibile effettuare chiamate all'API di Forma.

## Come si fa a ricordare tutto questo? E se cambiasse?

* In futuro, intendiamo fornire strumenti all'interno di Dynamo per il desktop che renderanno più semplice garantire che il grafico venga eseguito allo stesso modo in entrambi i contesti.

## Quanto costa?

* Durante questa versione Beta, il tempo di calcolo non viene attualmente addebitato.

## Cosa occorre fare per iniziare?

Per iniziare, consultare il [post del blog](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), la [serie YouTube](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) o gli esempi nell'estensione Forma. Questi guideranno nei seguenti passaggi:

* Accesso ad Autodesk Forma.
* Installazione di DynamoFormaBeta per Dynamo sul desktop e dell'estensione Dynamo in Forma.
* Scrittura del primo grafico.

## Protezione

* Tenere presente che i grafici condivisi sono memorizzati in Forma.
* Il tempo massimo di esecuzione del grafico è attualmente inferiore a 30 minuti. Questo valore può cambiare.
* Le richieste di esecuzione sono a velocità limitata, quindi potrebbero verificarsi errori se si effettuano molte richieste di calcolo in un periodo di tempo troppo breve.
