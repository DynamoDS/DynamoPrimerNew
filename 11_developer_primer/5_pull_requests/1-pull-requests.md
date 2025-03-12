# Richieste pull

Dynamo dipende dalla creatività e dall'impegno della sua comunità e il team di Dynamo incoraggia i collaboratori ad esplorare le possibilità, a testare le idee e a coinvolgere la comunità per ottenere un feedback. Sebbene l'innovazione sia fortemente incoraggiata, le modifiche verranno unite solo se rendono Dynamo più facile da utilizzare e soddisfano le linee guida definite in questo documento. Le modifiche con vantaggi definiti in modo restrittivo non verranno unite.

#### Richieste pull previste <a href="#pull-request-expectations" id="pull-request-expectations"></a>

Il team di Dynamo prevede che le richieste pull seguano alcune linee guida:

* Rispettare gli [standard di codifica](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) e gli [standard di denominazione dei nodi](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards).
* Includere unit test durante l'aggiunta di nuove funzionalità.
* Quando si corregge un bug, aggiungere uno unit test che evidenzi come il funzionamento corrente sia compromesso.
* Mantenere la discussione concentrata su un solo problema. Creare un nuovo caso se si presenta un argomento nuovo o correlato.

E alcune linee guida su cosa non fare:

* Sorprenderci con grandi richieste pull. Invece, segnalare un problema e avviare una discussione in modo da concordare una direzione prima di investire una grande quantità di tempo.
* Eseguire il commit del codice che non si è scritto. Se si trova del codice che si ritiene appropriato aggiungere a Dynamo, segnalare un problema e avviare una discussione prima di procedere.
* Inviare richieste pull che modifichino le intestazioni o i file correlati alle licenze. Se si ritiene che ci sia un problema con tali richieste, segnalarlo e saremo lieti di discuterne.
* Aggiungere elementi all'API senza prima segnalare un problema e discuterne con noi.

#### Compilazione del modello di richiesta pull <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

Quando si invia una richiesta pull, utilizzare il [modello di richiesta pull di default](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Prima di inviare la richiesta pull, assicurarsi che lo scopo sia chiaramente descritto e che tutte le dichiarazioni possano essere ritenute veritiere:

* Il codebase è in uno stato migliore dopo questa richiesta pull.
* È documentato in base agli [standard](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards).
* Il livello di test che questa richiesta pull include è appropriato.
* Le stringhe destinate all'utente, se presenti, vengono estratte nei file `*.resx`.
* Tutti test vengono superati utilizzando l'integrazione continua self-service.
* Istantanea delle modifiche apportate all'interfaccia utente, se presenti.
* Le modifiche all'API seguono il [controllo delle versioni semantico](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) e sono registrate nel documento [Modifiche all'API](https://github.com/DynamoDS/Dynamo/wiki/API-Changes).

Il team di Dynamo assegnerà alla richiesta pull un revisore appropriato.

#### Processo di revisione delle richieste pull <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Dopo aver inviato una richiesta pull, potrebbe essere necessario rimanere coinvolti nel processo di revisione. Si tenga conto dei seguenti criteri di revisione:

* Il team di Dynamo si riunisce una volta al mese per esaminare le richieste pull dalla meno recente a quella più recente.
* Se una richiesta pull rivista richiede modifiche da parte del proprietario, quest'ultimo ha 30 giorni per rispondere. Se la richiesta pull non ha rilevato alcuna attività entro la sessione successiva, verrà chiusa dal team o, a seconda della sua utilità, sarà gestita da un membro del team.
* Le richieste pull devono utilizzare il modello di default di Dynamo.
* Le richieste pull che non dispongono dei modelli di Dynamo compilati completamente con tutte le dichiarazioni soddisfatte non verranno esaminate.

#### Selezione dei commit di DynamoRevit <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Poiché sul mercato sono disponibili più versioni di Revit, potrebbe essere necessario selezionare le modifiche in specifici rami delle release di DynamoRevit, in modo che versioni diverse di Revit possano usufruire delle nuove funzionalità. Durante il processo di revisione, i collaboratori saranno responsabili della selezione dei loro commit rivisti agli altri rami di DynamoRevit specificati dal team di Dynamo.
