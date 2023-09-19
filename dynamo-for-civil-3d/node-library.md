# Libreria di nodi

È stato detto in precedenza che i **nodi** sono i blocchi predefiniti principali di un grafico di Dynamo e sono organizzati in gruppi logici nella **libreria**. In Dynamo for Civil 3D, sono presenti due categorie (o **scaffali**) nella libreria che contengono nodi dedicati per l'utilizzo di oggetti di AutoCAD e Civil 3D, quali tracciati, profili, modellatori, riferimenti di blocco e così via. Il resto della libreria contiene nodi che sono di natura più generica e sono coerenti tra tutte le "versioni" di Dynamo (ad esempio, Dynamo for Revit, Dynamo Sandbox e così via).

{% hint style="info" %} Per ulteriori informazioni sull'organizzazione dei nodi nella libreria di Dynamo principale, consultare la sezione [2-library.md](../3\_user\_interface/2-library.md "mention"). {% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>La libreria di nodi in Dynamo for Civil 3D</p></figcaption></figure>

> 1. Nodi specifici per l'utilizzo di oggetti di AutoCAD e Civil 3D
> 2. Nodi generici
> 3. Nodi di **pacchetti** di terze parti che è possibile installare separatamente

{% hint style="warning" %} Utilizzando i nodi presenti negli scaffali di AutoCAD e Civil 3D, il grafico di Dynamo funzionerà solo in Dynamo for Civil 3D. Se un grafico di Dynamo for Civil 3D viene aperto altrove (ad esempio, in Dynamo for Revit), questi nodi verranno contrassegnati con un avviso e non verranno eseguiti. {% endhint %}

{% hint style="info" %} **Perché sono presenti due scaffali separati per AutoCAD e Civil 3D?**

Questa organizzazione distingue i nodi per gli oggetti nativi di AutoCAD (linee, polilinee, riferimenti di blocco e così via) dai nodi per gli oggetti di Civil 3D (tracciati, modellatori, superfici e così via). Dal punto di vista tecnico, AutoCAD e Civil 3D sono due cose separate: AutoCAD è l'applicazione di base e Civil 3D si basa su essa. {% endhint %}

## Gerarchia dei nodi

Per utilizzare i nodi di AutoCAD e Civil 3D, è importante avere una conoscenza approfondita della gerarchia degli oggetti all'interno di ogni scaffale. Vi ricordate la tassonomia in biologia? Regno, phylum, classe, ordine, famiglia, genere, specie? Gli oggetti di AutoCAD e Civil 3D vengono classificati in modo simile. Esaminiamo alcuni esempi da spiegare.

### Oggetti di Civil

Utilizziamo un tracciato come esempio.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Si supponga che l'obiettivo sia quello di modificare il nome del tracciato. Da qui, il nodo successivo che si desidera aggiungere è un nodo **CivilObject.SetName**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

All'inizio, questo potrebbe non sembrare molto intuitivo. Che cos'è **CivilObject** e perché la libreria non dispone di un nodo **Alignment.SetName**? La risposta è legata alla _riutilizzabilità_ e alla _semplicità._ Se ci si pensa, il processo di modifica del nome di un oggetto di Civil 3D è lo stesso, sia che si tratti di un tracciato, un modellatore, un profilo o un altro elemento. Pertanto, invece di avere nodi ripetitivi che essenzialmente fanno tutti la stessa cosa (ad esempio, **Alignment.SetName, Corridor.SetName, Profile.SetName** e così via), sarebbe opportuno racchiudere tale funzionalità in un singolo nodo. Questo è esattamente ciò che fa **CivilObject.SetName**.

Un altro modo per riflettere su questo aspetto è in termini di _relazioni_. Un tracciato e un modellatore sono tipi di **Oggetti Civil**, come una mela e una pera sono tipi di frutta. I nodi CivilObject si applicano a qualsiasi tipo di oggetto di Civil, proprio come se si volesse utilizzare un unico sbucciatore per sbucciare una mela e di una pera. La cucina diventerebbe piuttosto disordinata se si avesse uno sbucciatore separato per ogni tipo di frutta! In questo senso, la libreria dei nodi di Dynamo è simile alla cucina.

### Oggetti

Ora, facciamo un passo avanti. Supponiamo di voler modificare il layer del tracciato. Il nodo che si desidera utilizzare è il nodo **Object.SetLayer**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Perché non è presente un nodo denominato **CivilObject.SetLayer**? Gli stessi principi di riutilizzabilità e semplicità di cui abbiamo parlato prima si applicano qui. La proprietà _layer_ è comune a qualsiasi oggetto di AutoCAD che può essere disegnato o inserito, ad esempio linea, polilinea, pesto, riferimento di blocco e così via. Gli oggetti di Civil 3D, quali tracciati e modellatori, rientrano nella stessa categoria, pertanto qualsiasi nodo che si applica ad un **Object** può essere utilizzato anche con qualsiasi **CivilObject**.

