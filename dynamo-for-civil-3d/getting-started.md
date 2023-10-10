# Per iniziare

Ora che si sa un po' di più sul quadro generale, è possibile iniziare subito a creare il primo grafico di Dynamo in Civil 3D.

{% hint style="info" %}\r\n Questo è un semplice esempio che serve a dimostrare le funzionalità di base di Dynamo. Si consiglia di eseguire i passaggi descritti in un nuovo documento vuoto di Civil 3D. \r\n{% endhint %}

## Apertura di Dynamo

La prima cosa da fare è aprire un documento vuoto in Civil 3D. Una volta arrivati a questo punto, accedere alla scheda **Gestione** sulla barra multifunzione di Civil 3D e cercare il gruppo **Programmazione visiva**.

![](<../.gitbook/assets/image (7).png>)

Fare clic sul pulsante **Dynamo**, che consente di avviare Dynamo in una finestra separata.

{% hint style="info" %}\r\n **Qual è la differenza tra Dynamo e il Lettore Dynamo?**

Dynamo è quello che si utilizza per creare ed eseguire grafici. Il Lettore Dynamo è un modo semplice per eseguire i grafici senza doverli aprire in Dynamo.

Accedere alla sezione [dynamo-player.md](dynamo-player.md "mention") quando si è pronti per provarlo. \r\n{% endhint %}

## Avvio di un nuovo grafico

Una volta aperto Dynamo, verrà visualizzata la schermata iniziale. Fare clic su **Nuovo** per aprire un'area di lavoro vuota.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Schermata iniziale di Dynamo</p></figcaption></figure>

{% hint style="info" %}\r\n **E gli esempi?**

Dynamo for Civil 3D include alcuni grafici predefiniti che possono aiutare a dare vita ad altre idee su come utilizzare Dynamo. È consigliabile esaminare tali elementi, nonché [sample-workflows](sample-workflows/ "mention") qui nella Guida introduttiva. \r\n{% endhint %}

## Aggiunta di nodi

Ora dovrebbe essere visualizzata un'area di lavoro vuota. Vediamo Dynamo in azione! Ecco il nostro obiettivo:

>  :dart: **Creare un grafico di Dynamo che inserirà il testo nello spazio modello.**

Molto semplice, giusto? Prima di iniziare, però, è necessario illustrare alcuni aspetti fondamentali.

I blocchi predefiniti principali di un grafico di Dynamo sono denominati **nodi**. Un nodo è come un piccolo computer: si inseriscono dei dati, la macchina li elabora e produce un risultato. Dynamo for Civil 3D dispone di una **libreria** di nodi che è possibile collegare insieme a dei **fili** per formare un **grafico** il quale consente di eseguire operazioni più grandi e migliori di quelle che un singolo nodo può fare da solo.

{% hint style="info" %}\r\n **E se Dynamo non fosse mai stato utilizzato prima?**

Alcune di queste nozioni potrebbero essere nuove, ma non importa perché le sezioni seguenti saranno d'aiuto.

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") \r\n{% endhint %}

Costruiamo il nostro grafico. Ecco un elenco di tutti i nodi che ci serviranno.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

È possibile trovare questi nodi digitandone i nomi sulla barra di ricerca nella libreria oppure facendo clic con il pulsante destro del mouse in un punto qualsiasi dell'area di disegno ed eseguendo ricerche in tale area.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>I nodi possono essere posizionati dalla libreria oppure facendo clic con il pulsante destro del mouse nell'area di disegno</p></figcaption></figure>

{% hint style="info" %}\r\n **Come è possibile sapere quali nodi utilizzare e dove trovarli?**

I nodi della libreria sono raggruppati in categorie logiche in base alle loro funzioni. Per una presentazione più approfondita, consultare la sezione [node-library.md](node-library.md "mention"). \r\n{% endhint %}

Ecco come dovrebbe apparire il grafico finale.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>Il grafico completato</p></figcaption></figure>

Riepiloghiamo quello che abbiamo fatto qui:

> 1. Abbiamo scelto il documento in cui lavorare. In questo caso (e in molti casi), si desidera lavorare nel documento attivo in Civil 3D.
> 2. È stato definito il blocco di destinazione in cui creare l'oggetto text (in questo caso lo spazio modello).
> 3. È stato utilizzato un nodo _String_ per specificare il layer su cui posizionare il testo.
> 4. È stato creato un punto utilizzando il nodo _Point.ByCoordinates_ per definire la posizione in cui inserire il testo.
> 5. Sono state definite le coordinate X e Y del punto di inserimento del testo utilizzando due nodi _Number Slider_.
> 6. È stato utilizzato un altro nodo _String_ per definire il contenuto dell'oggetto text.
> 7. Infine, è stato creato l'oggetto text.

Vediamo i risultati del nostro nuovo grafico!

## Visualizzazione del risultato

In Civil 3D, verificare che la scheda **Modello** sia selezionata. Dovrebbe essere visualizzato il nuovo oggetto text creato da Dynamo.

{% hint style="info" %}\r\n Se non è possibile visualizzare il testo, potrebbe essere necessario eseguire il comando ZOOM -> ESTENSIONI per eseguire lo zoom sul punto giusto. \r\n{% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

Forte! Ora facciamo degli aggiornamenti al testo.

Tornando al grafico di Dynamo, procedere e modificare alcuni dei valori di input, ad esempio la stringa di testo, le coordinate del punto di inserimento e così via. Dovrebbe essere possibile vedere l'aggiornamento automatico del testo in Civil 3D. Notare inoltre che se si scollega una delle porte di input, il testo viene rimosso. Se si ricollega tutto, il testo viene creato di nuovo. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>Il grafico completato in azione</p></figcaption></figure>

</div>

{% hint style="info" %}\r\n **Perché Dynamo non inserisce un nuovo oggetto text ogni volta che il grafico viene eseguito?**

Per default, Dynamo "memorizza" gli oggetti che crea. Se si modificano i valori di input del nodo, gli oggetti in Civil 3D vengono aggiornati anziché crearne di nuovi. Per ulteriori informazioni su questo funzionamento, consultare la sezione [object-binding.md](advanced-topics/object-binding.md "mention"). \r\n{% endhint %}

> :tada: Missione compiuta!

## Passaggi successivi

Questo esempio è solo un assaggio di ciò che si può fare con Dynamo for Civil 3D. Continuare a leggere per saperne di più.
