# Miniature dei grafici per la pagina iniziale di Dynamo

Nella pagina iniziale di Dynamo vengono visualizzate le miniature dei grafici su un'ampia scheda. La scheda ha un formato 16:9. La pagina iniziale centra l'immagine e ritaglia la parte superiore e quella inferiore.

Una miniatura quadrata può apparire completa in Progettazione generativa, che mostra l'intero quadrato. Nella pagina iniziale la stessa immagine potrebbe non visualizzare più i titoli o i loghi.

Questo vale per qualsiasi grafico (`.dyn`), non solo per quelli contenuti in un pacchetto. Impostare l'immagine in Proprietà grafico. L'immagine viene memorizzata nel file del grafico.

{% hint style="info" %}
Mantenere i titoli, i loghi e l'immagine principale nell'**area di sicurezza** centrale. Utilizzare solo la parte superiore e quella inferiore di una miniatura quadrata come sfondo.
{% endhint %}

## Modelli di miniatura

Scaricare un modello e utilizzarlo come base per la propria progettazione. La fascia centrale più scura è quella che viene visualizzata nella pagina iniziale. Le fasce più chiare in alto e in basso vengono ritagliate nella pagina iniziale. Progettazione generativa mostra il quadrato completo.

> Scaricare il modello SVG per modificarlo con un'app di grafica vettoriale o il modello PNG per inserire gli elementi grafici in un editor di immagini.

{% file src="../.gitbook/assets/Template_Thumbnail.svg" %}

{% file src="../.gitbook/assets/DynamoThumbnailTemplate.png" %}

<figure><img src="../.gitbook/assets/DynamoThumbnailTemplate.png" alt="Modello di miniatura quadrata. La fascia centrale più scura è visibile nella pagina iniziale di Dynamo. Le fasce superiore e inferiore più chiare vengono ritagliate nella pagina iniziale e rimangono visibili in Progettazione generativa."><figcaption><p>La fascia centrale più scura è visibile nella pagina iniziale. Le fasce superiore e inferiore più chiare vengono ritagliate.</p></figcaption></figure>

## Composizione della miniatura

1. Aprire il modello SVG o PNG in un editor di immagini.
2. Posizionare gli elementi grafici sul quadrato rosa presente nel modello.
3. Mantenere il titolo, il logo e l'immagine principale all'interno della fascia centrale più scura.
4. Utilizzare le fasce superiore e inferiore più chiare solo come colore di sfondo o per aggiungere ulteriori elementi grafici.
5. Nascondere o eliminare la guida rosa, quindi esportare un'immagine PNG **quadrata**.

Utilizzare un'immagine **quadrata** (1:1). La pagina iniziale mostra sempre la fascia centrale più scura come ritaglio in formato 16:9. Le dimensioni in pixel non modificano il ritaglio. Se si parte da un file vuoto anziché dal modello, crearne uno quadrato. Il modello della miniatura è di 840 × 840 pixel.

## Aggiunta della miniatura ad un grafico

La pagina iniziale legge la miniatura dal file del grafico (`.dyn`), non dal modulo di pubblicazione di Package Manager.

1. Aprire il grafico in Dynamo.
2. Passare a **File > Mostra proprietà grafico > Generale**.
3. Aggiungere il file PNG esportato nel campo dell'immagine.
4. Salvare il grafico.

Se il grafico viene fornito in un pacchetto, includere il file `.dyn` al momento della pubblicazione. Vedere [Pubblicazione di un pacchetto](../6_custom_nodes_and_packages/6-2_packages/4-publishing.md).

## Verifica del risultato nella pagina iniziale

1. Aprire la pagina iniziale di Dynamo.
2. In Recenti, individuare il grafico appena salvato.
3. Verificare che il titolo e il logo siano completamente visibili sulla scheda.

Se il testo risulta troncato, spostarlo verso il centro, esportare nuovamente l'immagine e aggiornare Proprietà grafico. Non affidarsi esclusivamente a Progettazione generativa. Quest'ultima mostra il quadrato per intero.
