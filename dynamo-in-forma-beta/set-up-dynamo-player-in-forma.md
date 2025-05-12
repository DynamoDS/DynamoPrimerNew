# Impostazione di Dynamo Player in Forma


Per utilizzare Dynamo con Forma, sono disponibili due opzioni: Dynamo as a Service (DaaS) basato sul cloud o Dynamo in versione desktop. Ciascuna ha i suoi vantaggi a seconda di ciò che si desidera fare, quindi prima di iniziare è bene valutare quale sia l'opzione più adatta alle proprie esigenze. Tuttavia, occorre tenere presente che è possibile passare da un'opzione all'altra in qualsiasi momento.

**Confronto tra Dynamo as a Service e Dynamo Desktop**

<table><thead><tr><th>Dynamo as a Service</th><th>Desktop</th><th data-hidden></th></tr></thead><tbody><tr><td>Installazione semplice</td><td>Processo di installazione in più fasi</td><td></td></tr><tr><td>Non è necessario installare Dynamo o tenerlo aperto</td><td>È necessario avere Dynamo aperto</td><td></td></tr><tr><td>Non riconosce un grafico già aperto in Dynamo</td><td>Aprire un grafico in Player aperto in Dynamo facendo clic con un solo clic</td><td></td></tr><tr><td>Non può utilizzare Python</td><td>Può utilizzare Python</td><td></td></tr><tr><td>Può utilizzare solo pacchetti approvati</td><td>Può utilizzare qualsiasi pacchetto</td><td></td></tr></tbody></table>

In questa pagina verrà spiegato come impostare entrambe le opzioni.

### Impostazione di Dynamo as a Service

Dynamo in Forma Beta è attualmente disponibile come versione beta aperta con accesso anticipato, il che significa che le funzionalità e l'interfaccia utente potrebbero cambiare frequentemente.

Innanzitutto, installiamo Dynamo Player in Forma.

1. Nel sito di Forma, accedere ad **Extensions** sulla barra laterale sinistra e fare clic su **Add extension**. Apre Autodesk App Store.
2. Cercare Dynamo e aggiungere Dynamo Player Beta. Leggere la dichiarazione di non responsabilità e fare clic su **Agree**.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Ora, Dynamo Player è disponibile nelle estensioni. Fare clic per aprirlo.
4. Ora è possibile utilizzare Dynamo Player.

### Impostazione di Dynamo Desktop

Per utilizzare Dynamo Desktop, è necessario disporre di Dynamo, come sandbox indipendente o connesso a Revit o Civil 3D. È inoltre necessario il pacchetto DynamoFormaBeta.

#### Revit

Seguire queste istruzioni per impostare Dynamo in Revit e il pacchetto DynamoFormaBeta.

1. Verificare che sia installato Revit 2024.1 o versione successiva.
2. Aprire Dynamo da Revit accedendo a Gestisci > Dynamo.
3. In Dynamo, installare il pacchetto DynamoFormaBeta. Accedere a Pacchetti > Package Manager, quindi cercare DynamoFormaBeta.
   1. Se si dispone di Revit 2024, installare il pacchetto DynamoFormaBeta per 2.x.
   2. Se si dispone di Revit 2025, installare il pacchetto DynamoFormaBeta.

#### Civil 3D

Seguire queste istruzioni per impostare Dynamo in Civil 3D e il pacchetto DynamoFormaBeta.

1. Assicurarsi di avere installato Civil 3D 2024.1 o versione successiva.
2. Aprire Dynamo da Civil 3D accedendo a Gestisci > Dynamo.
3. In Dynamo, installare il pacchetto DynamoFormaBeta. Accedere a Pacchetti > Package Manager, quindi cercare DynamoFormaBeta.
   1. Se si dispone di Civil 3D 2024, installare il pacchetto DynamoFormaBeta per 2.x.
   2. Se si dispone di Civil 3D 2025, installare il pacchetto DynamoFormaBeta.

#### Dynamo Sandbox

Seguire queste istruzioni per installare Dynamo Sandbox e il pacchetto DynamoFormaBeta.

1. Scaricare Dynamo 2.18.0 o versione successiva dalle [build di Dynamo](https://dynamobuilds.com/). Per un'esperienza ottimale, scegliere la versione più recente tra quelle più stabili, elencate in alto.
   1. Le versioni giornaliere sono versioni di sviluppo e possono includere funzionalità incomplete o in fase di sviluppo.
2. Estrarre Dynamo utilizzando [7zip](https://7-zip.org/) in una cartella di propria scelta.
3. Eseguire DynamoSandbox.exe dalla cartella di installazione Dynamo.
4. In Dynamo, installare il pacchetto DynamoFormaBeta. Accedere a Pacchetti > Package Manager, quindi cercare DynamoFormaBeta.
   1. Se si dispone di Dynamo 2.x, installare il pacchetto DynamoFormaBeta per 2.x.
   2. Se si dispone di Dynamo 3.x, installare il pacchetto DynamoFormaBeta.

Una volta installato Dynamo, è possibile utilizzarlo con Forma. Quando si esegue l'opzione desktop di Dynamo in Forma, è necessario che Dynamo sia aperto per poter utilizzare l'estensione Dynamo Player.

#### Accesso a Dynamo Desktop in Forma

Innanzitutto, installiamo Dynamo Player in Forma.

1. Nel sito di Forma, accedere ad **Extensions** sulla barra laterale sinistra e fare clic su **Add extension**. Apre Autodesk App Store.
2. Cercare Dynamo e aggiungere Dynamo Player Beta. Leggere la dichiarazione di non responsabilità e fare clic su **Agree**.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Ora, Dynamo Player è disponibile nelle estensioni. Fare clic per aprirlo.
4. Nella parte superiore, fare clic su Desktop per accedere a Dynamo Desktop.

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. Ora è possibile utilizzare Dynamo Player. Se in Dynamo è già aperto un grafico, fare clic su Open in **Connected graph** per visualizzarlo in Player.
