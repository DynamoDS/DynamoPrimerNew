# Cosa non può fare Autodesk Assistant

### Aggiornamento dei pacchetti

Autodesk Assistant non è ancora in grado di scaricare e installare i pacchetti in Dynamo.

![](../../.gitbook/assets/Aa_Update.jpg)

Come si può vedere nell'immagine precedente, Autodesk Assistant non è in grado di modificare, installare o disinstallare i pacchetti esterni associati a Dynamo. È in grado di fornire istruzioni per consentire all'utente di installarli da solo.


### Lettura e scrittura dirette dei file

Autodesk Assistant funziona con i file di grafici di Dynamo: può aprire un grafico, inserirne uno nell'area di disegno, eseguirne uno e salvare il lavoro. Non è in grado di leggere o scrivere autonomamente altri file presenti sul disco rigido.

Nell'esempio seguente, è presente una geometria creata nell'area di disegno. Ad Autodesk Assistant viene richiesto di selezionare i punti che compongono la geometria ed esportarli in formato JSON. Sebbene Autodesk Assistant non sia in grado di generare direttamente questo file JSON, suggerisce che potrebbe essere creato uno script di Dynamo per eseguire questa operazione.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

Quel suggerimento è lo schema da tenere a mente. Tutto ciò che si trova al di fuori del grafico è raggiungibile tramite i nodi, non tramite Assistant stesso.

### Modifica del modello di Revit senza un grafico

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant for Dynamo non dispone di una connessione diretta al modello di Revit. Se si richiede di modificare il modello come si farebbe con Assistant in Revit, verrà indicato che non è possibile.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

È comunque possibile modificare il modello di Revit. Lo fa nel modo in cui lo farebbe l'utente: posiziona i nodi di Revit, li collega tra loro e il modello cambia quando il grafico viene eseguito.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Da questo modo di lavorare derivano due cose:

- **Non cambia nulla fino a quando il grafico non viene eseguito.** È possibile prima leggere il grafico e regolare i valori. Il modello viene modificato solo durante l'esecuzione del grafico, sia da parte dell'utente che da parte di Assistant nell'ambito della richiesta.
- **La modifica è ripetibile.** Il grafico rimane nell'area di disegno. Salvandolo, è possibile eseguire nuovamente la stessa attività su un altro modello oppure condividerlo con il proprio team.

Se si desidera una modifica singola e diretta senza ricorrere a grafici, utilizzare Autodesk Assistant in Revit.
