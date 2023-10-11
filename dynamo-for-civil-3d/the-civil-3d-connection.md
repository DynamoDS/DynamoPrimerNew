# Connessione a Civil 3D

<figure><img src="../.gitbook/assets/DynamoSwissKnife-WhiteBackground_edit (2).jpg" alt="" width="563"><figcaption></figcaption></figure>

Dynamo for Civil 3D offre il paradigma della _programmazione visiva_ agli ingegneri e ai progettisti che lavorano su progetti di infrastrutture civili. Si può pensare a Dynamo come a una sorta di strumento digitale multiplo per gli utenti di Civil 3D; qualunque sia il compito da svolgere, c'è lo strumento giusto per farlo. La sua interfaccia intuitiva consente di creare routine potenti e personalizzabili senza dover scrivere una singola riga di codice. Non è necessario _essere_ programmatori per utilizzare Dynamo, ma è necessario essere in grado di _pensare_ con la logica di un programmatore. Insieme agli altri capitoli del Guida introduttiva, questo capitolo aiuterà a sviluppare le competenze logiche in modo da poter affrontare qualsiasi attività con una mentalità di progettazione computazionale.

## Cronologia

Dynamo è stato introdotto per la prima volta in Civil 3D 2020 e da allora ha continuato a evolversi. Inizialmente installato separatamente tramite un aggiornamento software, ora è incluso in tutte le versioni di Civil 3D. A seconda della versione di Civil 3D in uso, si può notare che l'interfaccia di Dynamo è leggermente diversa dagli esempi visualizzati in questo capitolo. Ciò è dovuto al fatto che l'interfaccia è stata notevolmente rinnovata in Civil 3D 2023.

<figure><img src="../.gitbook/assets/c3d-ui-old.png" alt=""><figcaption><p>Interfaccia di Dynamo, Civil 3D 2020-2022</p></figcaption></figure>

<figure><img src="../.gitbook/assets/c3d-ui-new.png" alt=""><figcaption><p>Interfaccia di Dynamo, Civil 3D 2023-Presente</p></figcaption></figure>

È consigliabile consultare il [blog di Dynamo](https://dynamobim.org/blog/) per le informazioni più aggiornate relative allo sviluppo di Dynamo. Nella tabella seguente sono riassunte le tappe principali dello sviluppo di Dynamo for Civil 3D. 

<table data-full-width="false"><thead><tr><th width="180">Versione di Civil 3D</th><th width="161">Versione di Dynamo</th><th>Note</th></tr></thead><tbody><tr><td>2024.1</td><td>2.18</td><td></td></tr><tr><td>2024</td><td>2.17</td><td>Aggiornamento dell'interfaccia utente del Lettore Dynamo</td></tr><tr><td>2023.2</td><td>2.15</td><td></td></tr><tr><td>2023</td><td>2.13</td><td>Aggiornamento dell'interfaccia utente di Dynamo</td></tr><tr><td>2022.1</td><td>2.12</td><td><ul><li>Aggiunte le impostazioni Archivio dati di unione per gli oggetti</li><li>Nuovi nodi per il controllo dell'unione di oggetti</li></ul></td></tr><tr><td>2022</td><td>2.10</td><td><ul><li>Inclusione nell'installazione principale di Civil 3D</li><li>Transizione da IronPython a Python.NET</li></ul></td></tr><tr><td>2021</td><td>2.5</td><td></td></tr><tr><td>2020.2</td><td>2.4</td><td></td></tr><tr><td>2020 Update 2</td><td>2.4</td><td>Nuovi nodi aggiunti</td></tr><tr><td>2020.1</td><td>2.2</td><td></td></tr><tr><td>2020</td><td>2.1</td><td>Rilascio iniziale</td></tr></tbody></table>
