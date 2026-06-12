# Dynamo Cloud Compute



Dynamo Cloud Compute offre tutta la potenza del runtime di programmazione visiva di Dynamo nel cloud. Anziché eseguire i grafici sul computer locale, il servizio di calcolo li esegue in un ambiente cloud sicuro e restituisce i risultati.

## Cos'è Dynamo?

Dynamo è un linguaggio di programmazione visiva e un ambiente di creazione che consente di generare programmi collegando tra loro dei nodi in un grafico. Il runtime di Dynamo esegue questi grafici, consentendo di automatizzare attività complesse, generare la geometria e integrarsi con altro software.

## Funzionamento del servizio di calcolo

Quando si utilizza Dynamo tramite un client basato sul cloud (ad esempio il Lettore Dynamo in Forma), i file dei grafici `.dyn` vengono inviati al servizio di calcolo per l'esecuzione. Il servizio:

1. Riceve il grafico ed eventuali parametri di input.
2. Esegue il grafico in un ambiente cloud isolato.
3. Restituisce i risultati all'applicazione client.

Questo approccio basato sul cloud consente di eseguire i grafici Dynamo senza dover installare Dynamo in locale e di sfruttare la potenza di calcolo del cloud per operazioni complesse.

## Perché utilizzare Dynamo Cloud Compute?

Dynamo Cloud Compute consente di realizzare scenari in cui si desidera:

**Eseguire grafici senza installazione desktop**: eseguire grafici Dynamo direttamente dalle applicazioni Web senza richiedere agli utenti di installare Dynamo Desktop sui propri computer.

**Collaborare e condividere**: condividere i grafici con i membri dei team che possono eseguirli tramite interfacce Web come Forma, facilitando così la distribuzione dei workflow automatizzati all'interno dell'organizzazione.

**Sfruttare il cloud computing**: sfruttare l'infrastruttura cloud per operazioni con un'elevata potenza di calcolo e che potrebbero richiedere più tempo se eseguite su computer locali.

**Standardizzare l'ambiente di esecuzione**: garantire un comportamento coerente tra utenti e computer diversi eseguendo grafici in un ambiente cloud controllato.

**Connettersi a Forma**: interagire con l'API di Forma tramite Dynamo. [Per ulteriori dettagli, vedere questo post del blog.](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/)

## Caratteristiche principali

**Esecuzione nel cloud**: i grafici vengono eseguiti nel cloud, non nel computer locale. Ciò significa:
- Nessuna necessità di installare Dynamo Desktop per eseguire i grafici
- Accesso alle risorse di cloud computing
- Un ambiente di esecuzione coerente tra utenti diversi.

**Sicurezza**: il servizio esegue i grafici di ogni utente in ambienti isolati per garantire la sicurezza e la privacy dei dati. I grafici e i dati sono conservati separatamente da quelli degli altri utenti.

**Elaborazione asincrona**: l'esecuzione dei grafici avviene in modo asincrono, ossia i client inviano un processo e possono verificarne lo stato fino al suo completamento. Ciò consente di eseguire calcoli di lunga durata senza bloccare il workflow.

## Disponibilità corrente

Dynamo Cloud Compute è attualmente disponibile tramite:
- **Lettore Dynamo in Forma in versione beta aperta**: caricare, condividere ed eseguire grafici Dynamo direttamente nell'interfaccia Web di Autodesk Forma

## Ulteriori informazioni

- [Differenze tra Dynamo Cloud Compute e Desktop Dynamo](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md): differenze importanti da tenere presenti quando si scrivono grafici per l'esecuzione sul cloud
- [Ciclo di vita del motore](engine-lifecycle.md): informazioni sulle versioni di motore supportate e sul relativo ciclo di vita

-----


> **Nota sul servizio in versione beta**  
 Dynamo Cloud Compute è attualmente in versione beta. Le tempistiche del supporto e i criteri di aggiornamento descritti in questo documento riflettono le nostre attuali intenzioni mentre sperimentiamo e perfezioniamo il servizio. Non si tratta di garanzie e potrebbero subire modifiche in base al feedback degli utenti e all'esperienza operativa.