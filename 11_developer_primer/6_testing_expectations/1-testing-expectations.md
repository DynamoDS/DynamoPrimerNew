# Aspettative di test

In questa pagina viene descritto ciò che si cerca di ottenere con i test sul nuovo codice aggiunto a Dynamo.

Quindi, si desidera aggiungere un nuovo nodo. Bene! È arrivato il momento di aggiungere alcuni test. Ci sono due motivi per farlo.

1. Serve a scoprire dove non funziona.
2. Quando un altro utente modifica qualcosa che interrompe il nodo, questo dovrebbe interrompere i test. In questo modo la persona che ha interrotto i test deve andare a correggerlo. Se non interrompe i test, il problema è in gran parte dell'utente che deve occuparsi dei modelli che si danneggiano.

I test in Dynamo sono disponibili in due tipi principali: unit test e test di sistema.

## Unit test

Gli unit test dovrebbero testare il meno possibile. Se è stato creato un nodo che calcola le radici quadrate tramite un nodo zero-touch C#, il test deve testare solo tale file DLL ed effettuare chiamate C# direttamente a tale codice. Gli unit test non dovrebbero nemmeno includere Dynamo.

Dovrebbero includere:

* Test positivi (verificano se il codice viene eseguito correttamente)
* Test negativi (verificano che il codice non si blocchi quando viene immesso input non valido)
* Test di regressione (in caso di bug del codice, servono per assicurarsi che non si ripresenti)

Dovrebbero essere brevi, veloci e affidabili. La maggior parte dei test dovrebbe essere unit test.

## Test di sistema

I test di sistema funzionano su più componenti e ne verificano l'integrazione. Devono essere utilizzati e creati con attenzione. 

Perché? Sono costosi da eseguire. Dobbiamo mantenere la suite di test in esecuzione con la minor latenza possibile.

Quando si scrive un test di sistema, la prima cosa da fare è guardare [questo](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) diagramma e capire quali sono i sottosistemi da coprire.

L'ideale sarebbe una serie progressiva di test che coprano insiemi crescenti di blocchi interagenti, in modo che quando i test iniziano a fallire sia molto rapido scoprire dove si trova il problema.

Esempi di operazioni che richiedono test di sistema:

* Un nuovo tipo di nodo di Revit che memorizza più elementi nella traccia anziché un singolo elemento
* Un nuovo nodo Watch che visualizza i dati in modo diverso.

Esempio di elementi che non richiedono test di sistema:

* Un nuovo nodo Math
* Una libreria di elaborazione delle stringhe.

I test di sistema dovrebbero:

* Asserire il comportamento corretto
* Asserire l'assenza di comportamenti patologici, ad es. nessuna eccezione.