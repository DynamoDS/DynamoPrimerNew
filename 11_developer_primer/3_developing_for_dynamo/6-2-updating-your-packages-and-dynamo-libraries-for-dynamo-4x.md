# Aggiornamento di pacchetti e librerie di Dynamo per Dynamo 4.x

### Introduzione: <a href="#introduction" id="introduction"></a>

Questa sezione contiene informazioni sui problemi che potrebbero verificarsi durante la migrazione di grafici, pacchetti e librerie a Dynamo 4.x. Dynamo 4.0 introduce:
* Notevoli miglioramenti delle prestazioni
* Aggiornamenti di stabilità e correzione di bug
* Modernizzazione del codebase
* Rimozione delle API precedentemente contrassegnate come obsolete nella versione 1.x
* Un importante aggiornamento di runtime da .NET 8 a .NET 10
* PythonNet 3 è ora il motore Python di default per tutti i nuovi nodi Python

Il processo di migrazione a .NET 10 garantisce che Dynamo rimanga in linea con la roadmap tecnologica di Microsoft, ben prima della fine del supporto per .NET 8 nel novembre 2026.

Quando si avvia Dynamo 4.0, verrà richiesto di eseguire l'aggiornamento a .NET 10, se tale operazione non è già stata effettuata. Gli autori dei pacchetti devono aggiornare i progetti in modo che utilizzino .NET 10 per garantire la completa compatibilità.

Tutti i nuovi nodi Python creati in Dynamo 4.0+ iniziano con PythonNet3. Non occorre preoccuparsi della compatibilità con le versioni precedenti: per chi lavora con più versioni (ad esempio Revit o Civil 3D 2025/2026), è sufficiente installare il pacchetto PythonNet3 Engine in Dynamo 3.3-3.6 per mantenere la compatibilità. Ulteriori informazioni sono disponibili [qui](https://dynamobim.org/dynamo-core-4-0-release/).

L'API e i nodi contrassegnati come obsoleti nella versione 1.x sono stati rimossi in Dynamo 4.0. È possibile fare riferimento all'[elenco completo delle modifiche qui](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0).

### Compatibilità dei pacchetti <a href="#package-compatibility" id="package-compatibility"></a>

#### Utilizzo dei pacchetti di Dynamo 2.x e 3.x in Dynamo 4.x 
Poiché Dynamo 4.x viene ora eseguito nel runtime di .NET 10, non è garantito che i pacchetti creati per Dynamo 2.x (*utilizzando .NET 4.8*) e Dynamo 3.x (*utilizzando .NET 8*) funzionino in Dynamo 4.x. Quando si tenta di scaricare un pacchetto in Dynamo 4.x pubblicato da una versione di Dynamo precedente alla 4.0, viene visualizzato un avviso che indica che il pacchetto proviene da una versione meno recente di Dynamo.

**Ciò non significa che il pacchetto non funzionerà**. È semplicemente un avviso che segnala problemi di compatibilità. In generale, è consigliabile verificare se è stata creata una versione più recente specificamente per Dynamo 4.x.

È inoltre possibile che questo tipo di avviso venga visualizzato nei file di registro di Dynamo al caricamento del pacchetto. Se tutto funziona correttamente, è possibile ignorarlo.

#### Utilizzo dei pacchetti di Dynamo 4.x in Dynamo 2.x 

È molto improbabile che un pacchetto creato per Dynamo 4.x (*utilizzando .NET 10*) funzioni su Dynamo 2.x. L'avviso riportato di seguito verrà visualizzato anche quando si tenta di installare i pacchetti creati per Dynamo 4.x in Dynamo 2.x.

![Avviso di compatibilità dei pacchetti](images/6-2-packages-new-version-compatibility-warning.png)


#### Utilizzo dei pacchetti di Dynamo 4.x in Dynamo 3.x 

Il pacchetto creato per Dynamo 4.x (*utilizzando .NET 10*) potrebbe funzionare in Dynamo 3.x, purché tutte le API utilizzate nel pacchetto siano presenti in .NET 8. Tuttavia, non vi è alcuna garanzia che funzionerà. L'avviso riportato di seguito verrà visualizzato anche quando si tenta di installare i pacchetti creati per Dynamo 4.x in Dynamo 3.x.

![Avviso di compatibilità dei pacchetti](images/6-2-packages-compatibility-warning.png)

#### Pratiche ottimali per gli autori di pacchetti 
Le pratiche ottimali consistono nel configurare il progetto per supportare sia .NET 8 che .NET 10 modificando il file .csproj.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
Ciò garantisce:
* Supporto per le versioni di Dynamo ospitate da Revit ancora in .NET 8
* Compatibilità con la versione standalone di Dynamo 4.x in .NET 10