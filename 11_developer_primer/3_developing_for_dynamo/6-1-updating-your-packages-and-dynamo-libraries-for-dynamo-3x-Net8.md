# Aggiornamento di pacchetti e librerie di Dynamo per Dynamo 3.x e NET8

### Introduzione: <a href="#introduction" id="introduction"></a>

Questa sezione contiene informazioni sui problemi che potrebbero verificarsi durante la migrazione di grafici, pacchetti e librerie a Dynamo 3.x.

Dynamo 3.0 è una release principale e alcune API sono state modificate o rimosse. La modifica più importante che probabilmente interesserà gli sviluppatori e gli utenti di Dynamo 3.x è il passaggio a .NET 8.

Dotnet/.NET è il runtime che attiva il linguaggio C# in cui viene scritto Dynamo. Abbiamo aggiornato l'ambiente passando ad una versione moderna di questo runtime insieme al resto dell'ecosistema Autodesk.

Per saperne di più, consultare il [nostro post del blog](https://dynamobim.org/dynamo-on-net-8/).
***

### Compatibilità dei pacchetti <a href="#package-compatibility" id="package-compatibility"></a>

#### Utilizzo dei pacchetti di Dynamo 2.x in Dynamo 3.x 
Poiché Dynamo 3.x viene ora eseguito nel runtime di .NET8, non è garantito che i pacchetti creati per Dynamo 2.x (*utilizzando .NET 4.8*) funzionino in Dynamo 3.x. Quando si tenta di scaricare un pacchetto in Dynamo 3.x pubblicato da una versione di Dynamo precedente alla 3.0, viene visualizzato un avviso che indica che il pacchetto proviene da una versione meno recente di Dynamo. 

**Ciò non significa che il pacchetto non funzionerà**. È semplicemente un avviso che segnala problemi di compatibilità. In generale, è consigliabile verificare se è stata creata una versione più recente specificamente per Dynamo 3.x.

È inoltre possibile che questo tipo di avviso venga visualizzato nei file di registro di Dynamo al caricamento del pacchetto. Se tutto funziona correttamente, è possibile ignorarlo.

#### Utilizzo dei pacchetti di Dynamo 3.x in Dynamo 2.x 

È molto improbabile che un pacchetto creato per Dynamo 3.x (*utilizzando .NET 8*) funzioni su Dynamo 2.x. Viene inoltre visualizzato un avviso quando si scaricano i pacchetti creati per le versioni più recenti di Dynamo mentre si utilizza una versione precedente.


