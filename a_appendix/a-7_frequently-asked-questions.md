# Domande frequenti

## Come utilizzare le build di Dynamo

### Confronto tra build giornaliere e build stabili
Tradizionalmente il team Dynamo di Autodesk mantiene un ritmo veloce di iterazione rilasciando sia build giornaliere per ogni commit, sia build di release stabili dopo il ciclo di test e rilascio del sistema. Il nostro team vorrebbe riavviare le build giornaliere e stabili in modo che gli utenti possano controllare dove DynamoCore viene estratto sul loro disco locale e utilizzarlo in tutta tranquillità, senza ripercussioni su Dynamo per altri prodotti Autodesk. Esistono alcuni candidati naturali per questo scopo, tra cui file .nupkg, .zip o un programma di installazione dedicato in cui gli utenti possono scegliere il percorso di installazione o altre opzioni. 

Dato l'obiettivo di fornire agli utenti il codice più recente nel modo più semplice possibile, abbiamo deciso di fornire un file .zip contenente i file binari di DynamoCore e Dynamo Sandbox che può essere utilizzato senza Revit (con alcuni vincoli).

### Build di Dynamo come file .zip
#### Definizione e origine
La build di DynamoCoreRuntime come file .zip è un'istantanea dei file binari di DynamoCore che viene creata durante le build automatizzate. 

Dovrebbe essere possibile avviare DynamoSandbox.exe nella cartella estratta per utilizzare Dynamo con un'impostazione minima.


#### Componenti richiesti

| Versione di Dynamo  |Microsoft Visual C++  | DirectX  |   |   |   |   |
|---|---|---|---|---|---|---|
|  2.0-2.6 |  2015 Redistributable  | 10  |   |   |   |   |
| 2.7  | 2019 Redistributable  | 11/12 (incluso in Windows 10)  |   |   |   |   |
| >=2.8  | 2019 Redistributable  | 11/12 (incluso in Windows 10)  |   |   |   |   |
##### Microsoft DirectX, disponibile anche pubblicamente nel repository GitHub di Dynamo [qui](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)

##### 7zip utilizzato per decomprimere il pacchetto [qui](https://www.7-zip.org/download.html)


##### Microsoft Visual C++ 2015-2024 Redistributable (x64) ([collegamento](https://aka.ms/vs/17/release/vc_redist.x64.exe))

##### Componenti opzionali
Libreria della geometria (sarà disponibile solo con determinati strumenti di modellazione Autodesk, come Revit, Civil 3D, Advanced Steel e così via)

### Risoluzione dei problemi
Se la build è stata decompressa e non è stato possibile avviare DynamoSandbox.exe, assicurarsi di utilizzare [7zip](https://www.7-zip.org/download.html) per decomprimere la build. È inoltre possibile sbloccare manualmente l'archivio .zip *prima* di estrarlo, se si dispone delle autorizzazioni sul computer in uso.

![](images/a-7/dynamo-builds-1.png)


Se manca uno qualsiasi dei componenti richiesti, potrebbero verificarsi problemi durante l'utilizzo di Dynamo e alcune parti dell'interfaccia utente potrebbero non caricarsi.

Utilizzando il seguente screenshot come esempio, decomprimendo la nostra build su una macchina virtuale Windows 10 pulita senza GPU, nel computer mancano entrambi i componenti richiesti. Ciò è indicato nella console di Dynamo.

![](images/a-7/dynamo-builds-2.png)

##### Installazione di DirectX
Seguire le istruzioni di Microsoft per verificare se DirectX è già stato installato. In caso contrario, è possibile aprire DXSETUP.exe nel repository GitHub di Dynamo [qui](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX). Una volta visualizzata la finestra di dialogo riportata di seguito, fare clic su Next per installare DirectX nella posizione di default.

![](images/a-7/dynamo-builds-3.png)

##### Installazione di Microsoft Visual C++ 2015-2024 Redistributable (x64)
Scaricare la versione più recente [qui](https://aka.ms/vs/17/release/vc_redist.x64.exe). Quindi, dovrebbe essere possibile eseguire il programma di installazione denominato vc_redist.x64.exe nel posizione di download del browser. Una volta visualizzata la finestra di dialogo riportata di seguito, fare clic su Install per posizionare questo componente nella posizione di default.

![](images/a-7/dynamo-builds-4.png)

Dopo aver installato entrambi i componenti richiesti dal collegamento precedente, riavviare DynamoSandbox.exe. Si dovrebbe vedere il seguente risultato:

![](images/a-7/dynamo-builds-5.png)

##### Grafica 3D mancante 

Si potrebbero anche riscontrare problemi di grafica durante l'esecuzione della sandbox per la prima volta; è possibile seguire le domande frequenti sui problemi di grafica qui:

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

In generale, è probabile che sia necessario forzare la modalità GPU ad alte prestazioni per la scheda grafica quando si utilizza DynamoSandbox.exe.

_Esempio di pannello di controllo NVIDIA:_

![](images/a-7/dynamo-builds-6.png)

##### Installazione di WebView2 Runtime
Al momento, i moduli di Dynamo successivi utilizzano il componente WebView2: Browser della documentazione, Presentazioni guidate e Libreria, pertanto, per garantire che queste parti di Dynamo visualizzino correttamente il contenuto Web, è necessario installare il programma di installazione di WebView2 Evergreen Runtime (è necessario verificare se è già stato installato nel computer o se deve essere installato).

Questo è il collegamento per l'installazione di WebView2 Runtime: [https://developer.microsoft.com/it-it/microsoft-edge/webview2/?form=MA13LH#download-section](https://developer.microsoft.com/it-it/microsoft-edge/webview2/?form=MA13LH#download-section)

![](images/a-7/dynamo-builds-7.png)

I componenti che dovrebbero essere installati (solo uno) sono Evergreen Bootstrapper o Evergreen Standalone Installer, il primo scarica un programma di installazione da 1,50 MB e il secondo scarica un programma di installazione da 130 MB.

Dopo l'installazione di Runtime, i componenti successivi di Dynamo dovrebbero funzionare correttamente:

![](images/a-7/dynamo-builds-8.png)


##### Problemi relativi ai nodi Excel di Dynamo
Per la diagnostica, consultare questo [articolo](https://www.autodesk.com/it/support/technical/article/caas/sfdcarticles/sfdcarticles/ITA/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html).

### Posizione delle build di Dynamo
Release stabili

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

Build giornaliere e release stabili

[https://dynamobuilds.com/](https://dynamobuilds.com/)

