# Ciclo di vita e frequenza degli aggiornamenti del servizio Dynamo Cloud Compute



In questo documento sono descritte la frequenza degli aggiornamenti e la politica di supporto per Dynamo Cloud Compute. Quest'ultimo potrebbe essere indicato nel presente documento anche come "il servizio".

È illustrato come vengono gestite le versioni del motore, quando vengono effettuati gli aggiornamenti e cosa possono aspettarsi gli utenti quando eseguono grafici Dynamo nel cloud.

---

## Frequenza degli aggiornamenti

Per soddisfare le diverse esigenze degli utenti, Dynamo Cloud Compute offre **due diversi tipi di motore**. Ogni tipo ha uno scopo specifico e segue il proprio programma di aggiornamento:

### Motore stabile (produzione)

Il motore stabile è progettato per garantire affidabilità e coerenza negli ambienti di produzione. È basato sulla release stabile più recente di DynamoCore Runtime e viene aggiornato quando le release ufficiali di Dynamo sono accessibili agli utenti di Dynamo Desktop. Inizialmente seguiremo la frequenza degli aggiornamenti di DynamoRevit.

Questo tipo è destinato a carichi di lavoro di produzione in cui l'affidabilità e la prevedibilità sono fondamentali. Quando si utilizza il motore stabile, gli aggiornamenti seguiranno il calendario di rilascio pubblico di Dynamo, consentendo di prepararsi alle modifiche e di testare i grafici prima che queste incidano sui propri workflow.

### Motore di anteprima (anteprima/sandbox giornaliera)

Il motore di anteprima consente di accedere in anticipo agli sviluppi più recenti in Dynamo. Si basa sull'ultima versione di sviluppo di DynamoCore Runtime ed è aggiornato frequentemente man mano che vengono integrate nuove funzionalità e correzioni di bug.

Questo tipo è ideale per gli utenti che desiderano testare le prossime modifiche, provare le nuove funzionalità prima del loro rilascio ufficiale o verificare che i propri grafici continueranno a funzionare con le future versioni di Dynamo. Il motore di anteprima permette di anticipare le modifiche e fornire un feedback al team di Dynamo.

---


## Tempistica del supporto

Sapere per quanto tempo ogni versione del motore continuerà ad essere supportata aiuta a pianificare le finestre di manutenzione e gli aggiornamenti.

### Motore stabile

Il motore stabile riceve gli aggiornamenti quando Dynamo pubblica una nuova release stabile di Dynamo Core in Revit. Ogni versione stabile rimane disponibile e supportata fino a quando la versione stabile successiva non viene distribuita nel servizio.

Ad esempio, se il servizio esegue attualmente Dynamo 3.6 (stabile), continuerà ad eseguire tale versione fino a quando Dynamo 4.0 non sarà disponibile per tutti gli utenti (in genere quando verrà integrato in Revit). A questo punto, il servizio verrà aggiornato a Dynamo 4.0 (stabile).

Questo approccio garantisce che il servizio cloud rimanga in linea con l'esperienza che la maggior parte degli utenti vive negli ambienti desktop.

### Motore di anteprima

Il motore di anteprima viene aggiornato costantemente dalla versione più recente in fase di sviluppo di Dynamo. Con il progredire dello sviluppo di ciascuna versione, il motore di anteprima tiene traccia di tali modifiche.

Ad esempio, mentre Dynamo 4.1 è in fase di sviluppo attivo, il motore di anteprima potrebbe essere etichettato come "Dynamo Cloud Compute Service 4.1". Con il passaggio allo sviluppo della versione 4.2, il motore di anteprima inizierà a tenere traccia di tali modifiche e potrebbe essere etichettato come "Dynamo Cloud Compute Service 4.2".

Poiché il motore di anteprima viene aggiornato frequentemente, è possibile che si verifichino occasionalmente modifiche sostanziali o che vengano introdotte funzionalità sperimentali. È ideale per i test e la convalida piuttosto che per i workflow di produzione.

---

## Scelta del motore giusto

Quando si deve decidere quale tipo di motore utilizzare:

- **Scegliere il motore stabile** se serve un comportamento prevedibile e collaudato per i workflow di produzione o se si stanno distribuendo grafici agli utenti finali che si aspettano risultati coerenti.

- **Scegliere il motore di anteprima** se si desidera testare in anticipo le nuove funzionalità, verificare che i grafici funzionino con le versioni future o fornire un feedback sullo sviluppo di Dynamo.

Entrambi i motori eseguono lo stesso runtime di base di Dynamo; la differenza sta nel momento e nella frequenza con cui ricevono gli aggiornamenti. 

---

> **Nota sul servizio in versione beta**  
 Dynamo Cloud Compute è attualmente in versione beta. Le tempistiche del supporto e i criteri di aggiornamento descritti in questo documento riflettono le nostre attuali intenzioni mentre sperimentiamo e perfezioniamo il servizio. Non si tratta di garanzie e potrebbero subire modifiche in base al feedback degli utenti e all'esperienza operativa.



