# Nodi Dictionary

Dynamo 2.0 consente di visualizzare un'ampia varietà di nodi Dictionary da utilizzare. Ciò include i nodi di _Create, Action e Query_.

![](<../images/5-5/2/dictionary nodes - nodes.jpg>)

#### Crea

1. `Dictionary.ByKeysValues` creerà un dizionario con le chiavi e i valori forniti. _(il numero di voci corrisponderà all'input più breve dell'elenco)._

#### Azione

2\. `Dictionary.Components` produrrà i componenti del dizionario di input. _(è l'opposto del nodo Create)._

3\. `Dictionary.RemoveKeys` produrrà un nuovo oggetto dizionario con le chiavi di input rimosse.

4\. `Dictionary.SetValueAtKeys` creerà un nuovo dizionario basato sulle chiavi di input e sui valori per sostituire il valore corrente nelle chiavi corrispondenti.

5\. `Dictionary.ValueAtKey` restituirà il valore nella chiave di input.

#### Conteggio

6\. `Dictionary.Count` indicherà il numero di coppie chiave-valore presenti nel dizionario.

7\. `Dictionary.Keys` restituirà le chiavi attualmente memorizzate nel dizionario.

8\. `Dictionary.Values` restituirà i valori attualmente memorizzati nel dizionario.

La correlazione complessiva dei dati con i dizionari rappresenta un'alternativa significativa al vecchio metodo di utilizzo di indici ed elenchi.
