# Unione di oggetti

Dynamo for Civil 3D contiene un meccanismo molto potente per "memorizzare" gli oggetti creati da ciascun nodo. Questo meccanismo è denominato **unione di oggetti** e consente di produrre un grafico di Dynamo con risultati coerenti ogni volta che viene eseguito nello stesso documento. Sebbene questo sia altamente auspicabile in molte situazioni, ce ne sono altre in cui si desidera avere un maggiore controllo sul funzionamento di Dynamo. Questa sezione aiuterà a capire come funziona il binding di oggetti e come è possibile sfruttarlo.

## Esempio

Considerare questo grafico che crea un cerchio nello spazio modello sul layer corrente.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Un grafico semplice per la creazione di un cerchio</p></figcaption></figure>

Notare cosa succede quando il raggio viene modificato.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Modifica dell'input del raggio in Dynamo</p></figcaption></figure>

Questa è l'unione di oggetti in azione. Il funzionamento di default di Dynamo consiste nel _modificare_ il raggio del cerchio, anziché creare un nuovo cerchio ogni volta che l'input del raggio viene modificato. Questo perché il nodo **Object.ByGeometry** "si ricorda" che ha creato questo cerchio _specifico_ ogni volta che il grafico viene eseguito. Inoltre, Dynamo memorizzerà queste informazioni in modo che la volta successiva che si apre il documento di Civil 3D e si esegue il grafico, avrà esattamente lo stesso funzionamento.

## Un altro esempio

Vediamo un esempio in cui si potrebbe voler cambiare il funzionamento di default di Dynamo per l'unione di oggetti. Supponiamo di voler creare un grafico che posiziona il testo al centro di un cerchio. Tuttavia, l'intento con questo grafico è quello di poterlo eseguire più volte e di inserire ogni volta un nuovo testo per qualsiasi cerchio selezionato. Ecco come potrebbe apparire quel grafico.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Un grafico semplice che posiziona il testo al centro di un cerchio selezionato</p></figcaption></figure>

Tuttavia, questo è ciò che accade quando viene selezionato un cerchio diverso.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Funzionamento di default di Dynamo quando si seleziona un nuovo cerchio</p></figcaption></figure>

Sembra che il testo venga eliminato e ricreato ad ogni esecuzione del grafico. In realtà, la posizione del testo viene _modificata_ a seconda del cerchio selezionato. Quindi è lo stesso testo, in un punto diverso. Per creare un nuovo testo ogni volta, è necessario modificare le impostazioni dell'unione di oggetti di Dynamo in modo che non vengano mantenuti i dati di unione (vedere [\#binding-settings](object-binding.md#binding-settings "mention") di seguito).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Impostazioni dell'unione di oggetti</p></figcaption></figure>

Dopo aver apportato questa modifica, otteniamo il funzionamento desiderato.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Funzionamento con l'unione di oggetti disattivata</p></figcaption></figure>

## Impostazioni di unione

Dynamo for Civil 3D consente di modificare il funzionamento di default dell'unione di oggetti tramite le impostazioni di **Archivio dati di unione** nel menu **Dynamo**.

{% hint style="info" %}\r\n Notare che le opzioni di Archivio dati di unione sono disponibili in **Civil 3D 2022.1** e versioni successive. \r\n{% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Tutte le opzioni sono attivate per default. Ecco un riepilogo delle funzionalità di ciascuna opzione.

### Opzione 1: Nessun dato di unione mantenuto

Quando questa opzione è attivata, Dynamo "dimentica" gli oggetti che ha creato l'ultima volta che il grafico è stato eseguito. Il grafico può essere eseguito in qualsiasi Carta in qualunque situazione e creerà nuovi oggetti ogni volta.

{% hint style="info" %}\r\n **Quando utilizzare**

Utilizzare questa opzione quando si desidera che Dynamo "dimentichi" tutto ciò che ha svolto nelle esecuzioni precedenti e crei nuovi oggetti ogni volta. \r\n{% endhint %}

### Opzione 2: Memorizza nel grafico per Dynamo

Questa opzione indica che i metadati dell'unione di oggetti verranno serializzati nel grafico (file .dyn) al momento del salvataggio. Se si chiude/riapre il grafico e lo si esegue nella **stessa Carta**, tutto dovrebbe funzionare come è stato lasciato. Se si esegue il grafico in una **Carta diversa**, i dati dell'unione verranno rimossi dal grafico e verranno creati nuovi oggetti. Ciò significa che se si apre la Carta originale ed si riesegue il grafico, verranno creati nuovi oggetti oltre a quelli precedenti.

{% hint style="info" %}\r\n **Quando utilizzare**

Utilizzare questa opzione quando si desidera che Dynamo "memorizzi" gli oggetti che ha creato l'ultima volta che è stato eseguito in una **Carta specifica**. \r\n{% endhint %}

{% hint style="warning" %}\r\n Questa opzione è particolarmente adatta per le situazioni in cui è possibile mantenere un rapporto 1:1 tra una **Carta specifica** e un grafico di Dynamo. Le opzioni 1 e 3 sono più adatte per i grafici progettati per l'esecuzione su più disegni. {% endpoint %}

### Opzione 3: Memorizza nella Carta per Dynamo

Questa opzione è simile all'opzione 2, tranne per il fatto che i dati dell'unione di oggetti vengono serializzati nella Carta anziché nel grafico (file .dyn). Se si chiude/riapre il grafico e lo si esegue nella **stessa Carta**, tutto dovrebbe funzionare come è stato lasciato. Se il grafico viene eseguito in una **Carta diversa**, i dati dell'unione sono comunque mantenuti nel disegno originale poiché vengono salvati nel disegno e non nel grafico.

{% hint style="info" %}\r\n **Quando utilizzare**

Utilizzare questa opzione quando si desidera utilizzare lo stesso grafico in **più Carte** e fare in modo che Dynamo "memorizzi"ciò che ha svolto in ciascuno. {% endpoint %}

### Opzione 4: Memorizza nella Carta per Lettore Dynamo

La prima cosa da notare con questa opzione è che non ha alcun effetto sul modo in cui il grafico interagisce con la Carta quando si esegue il grafico tramite l'interfaccia principale di Dynamo. Questa opzione è valida _solo_ quando il grafico viene eseguito utilizzando il Lettore Dynamo.

{% hint style="info" %}\r\n Se non si conosce il Lettore Dynamo, consultare la sezione [dynamo-player.md](../dynamo-player.md "mention"). \r\n{% endhint %}

Se si esegue il grafico utilizzando l'interfaccia principale di Dynamo e quindi si chiude ed esegue lo stesso grafico utilizzando il Lettore Dynamo, verranno creati nuovi oggetti sopra a quelli creati in precedenza. Tuttavia, una volta eseguito il grafico, il Lettore Dynamo serializzerà i dati dell'unione di oggetti nella Carta. Pertanto, se si esegue il grafico più volte tramite il Lettore Dynamo, gli oggetti verranno aggiornati anziché creati di nuovi. Se si esegue il grafico tramite il Lettore Dynamo in una **Carta diversa**, i dati dell'unione sono comunque mantenuti nella Carta originale poiché vengono salvati nella Carta e non nel grafico.

{% hint style="info" %}\r\n **Quando utilizzare**

Utilizzare questa opzione quando si desidera eseguire un grafico utilizzando il Lettore Dynamo in più Carte e fare in modo che "memorizzi" ciò che ha svolto in ciascuno. \r\n{% endhint %}
