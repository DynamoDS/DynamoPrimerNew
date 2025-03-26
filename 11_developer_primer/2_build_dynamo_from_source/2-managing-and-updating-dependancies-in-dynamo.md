# Come aggiornare o aggiungere una nuova dipendenza a Dynamo

### Quando si applica questa wiki
Quando si lavora su una nuova funzionalità o semplicemente si aggiorna una dipendenza esistente, prima di importare una nuova dipendenza nel repository di Dynamo si dovrebbe valutare quanto segue.

### Considerazioni
1. Qual è la licenza della dipendenza nuova o aggiornata - solo alcune licenze open source vengono approvate senza parlare con l'ufficio legale Autodesk.
    * Dopo aver risolto la licenza, assicurarsi che la dipendenza e la versione siano registrate nella wiki interna.
    * Se la licenza è `LGPL`, `GPL` o `Apache`, il file di licenza deve essere copiato nella sottocartella _"Open Source Licenses"_ della build di Dynamo.
    * Se la licenza è `LGPL`, il codice sorgente completo per tutti i componenti di terze parti, insieme alle copie di testo delle licenze open source appropriate, deve essere caricato su [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. In caso di aggiornamento, il tipo di licenza è cambiato rispetto alla versione precedente?
3. La dipendenza è multipiattaforma? 
    * Ha componenti nativi (come `CEFSharp` o `ImageMagick`)? Ciò renderà più difficile l'installazione client multipiattaforma.
    * Ha riferimenti solo a Windows? In tal caso, non dovrebbe essere come dipendenza di DynamoCore o di altre parti multipiattaforma di Dynamo (il layer del modello).
4. La dipendenza è correttamente inclusa nella cartella bin durante la compilazione con tutte le dipendenze necessarie?
    * In caso di aggiornamento, alcuni file vengono rimossi come conseguenza di tale operazione? Questa versione di Dynamo è destinata ad una versione patch di prodotti host? In tal caso, sarà necessario mantenere i vecchi file binari fino all'anno del lancio globale per supportare i programmi di installazione delle patch. Vedere [qui](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. La dipendenza o la relativa struttura delle dipendenze è in conflitto con altre dipendenze esistenti in Dynamo?
6. ?? La dipendenza o la relativa struttura delle dipendenze è in conflitto con le dipendenze esistenti nei prodotti che integrano Dynamo nel processo (Revit, Civil e così via)? **Questo è importante, poiché questi problemi possono essere scoperti solo in fase di integrazione, a meno che il lavoro non si intervenga in anticipo.**