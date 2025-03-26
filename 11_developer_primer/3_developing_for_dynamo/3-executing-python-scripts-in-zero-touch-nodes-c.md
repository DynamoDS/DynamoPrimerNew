# Esecuzione di script Python in nodi zero-touch (C#)

### Esecuzione di script Python in nodi zero-touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Se si ha familiarità con la scrittura di script in Python e si desidera ottenere più funzionalità dai nodi Python di Dynamo standard, è possibile utilizzare nodi zero-touch per creare nodi personalizzati. Iniziamo con un semplice esempio che ci consente di trasferire uno script Python come stringa ad un nodo zero-touch dove viene eseguito lo script e viene restituito un risultato. Questo case study si baserà sulle simulazioni e sugli esempi della sezione Per iniziare, a cui fare riferimento se si è completamente inesperti nella creazione di nodi zero-touch.

![Nodo zero-touch che eseguirà una stringa di script Python](images/python-case-study.png)

> Nodo zero-touch che eseguirà una stringa di script Python

#### Motore Python <a href="#python-engine" id="python-engine"></a>

Questo nodo si basa su un'istanza del motore di scripting IronPython. A tale scopo, è necessario fare riferimento ad alcuni assiemi aggiuntivi. Per impostare un modello di base in Visual Studio, attenersi alla procedura descritta di seguito:

* Creare un nuovo progetto di classe di Visual Studio.
* Aggiungere un riferimento al file `IronPython.dll` situato in `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`.
* Aggiungere un riferimento al file `Microsoft.Scripting.dll` situato in `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`.
* Includere le istruzioni `IronPython.Hosting` e `Microsoft.Scripting.Hosting` `using` nella classe.
* Aggiungere un costruttore vuoto privato per impedire l'aggiunta di un altro nodo alla libreria di Dynamo con il pacchetto.
* Creare un nuovo metodo che utilizzi una singola stringa come parametro di input.
* All'interno di questo metodo verrà creata l'istanza di un nuovo motore Python e verrà creato un ambito di script vuoto. Si può immaginare questo ambito come variabili globali all'interno di un'istanza dell'interprete Python.
* Quindi, chiamare `Execute` nel motore che trasferisce la stringa di input e l'ambito come parametri.
* Infine, recuperare e restituire i risultati dello script chiamando `GetVariable` nell'ambito e trasferendo il nome della variabile dallo script Python che contiene il valore che si sta tentando di restituire. Per ulteriori dettagli, vedere l'esempio seguente.

Il seguente codice fornisce un esempio per il passaggio menzionato sopra. La creazione della soluzione creerà un nuovo file `.dll` situato nella cartella bin del progetto. Ora è possibile importare il file `.dll` in Dynamo come parte di un pacchetto o individuando `File < Import Library...`.

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Lo script Python restituisce la variabile `output`, il che significa che è necessaria una variabile `output` nello script Python. Utilizzare questo script di esempio per verificare il nodo in Dynamo. Se si è mai utilizzato il nodo Python in Dynamo, quanto segue dovrebbe risultare familiare. Per ulteriori informazioni, consultare la [sezione Python della Guida introduttiva](https://primer2.dynamobim.org/v/it/8_coding_in_dynamo/8-3_python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Più output <a href="#multiple-outputs" id="multiple-outputs"></a>

Una limitazione dei nodi Python standard è che hanno una sola porta di output, pertanto, se si desidera restituire più oggetti, è necessario costruire un elenco e recuperare ogni oggetto incluso. Se si modifica l'esempio precedente per restituire un dizionario, si possono aggiungere tutte le porte di output desiderate. Per ulteriori informazioni sui dizionari, fare riferimento alla sezione Restituzione di più valori in Ulteriori informazioni sul concetto di zero-touch.

![Questo nodo ci consente di restituire sia il volume del cuboide che il relativo baricentro.](images/python-multi-case-study.png)

> Questo nodo ci consente di restituire sia il volume del cuboide che il relativo baricentro.

Modifichiamo l'esempio precedente con questi passaggi:

* Aggiungere un riferimento a `DynamoServices.dll` da Gestione pacchetti NuGet.
* Oltre agli assiemi precedenti, includere `System.Collections.Generic` e `Autodesk.DesignScript.Runtime`.
* Modificare il tipo restituito nel metodo per restituire un dizionario che conterrà gli output.
* Ogni output deve essere recuperato singolarmente dall'ambito (si consideri la possibilità di impostare un ciclo semplice per gruppi più grandi di output).

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

È stata inoltre aggiunta un'altra variabile di output (`output2`) allo script Python di esempio. Tenere presente che queste variabili possono utilizzare qualsiasi convenzione di denominazione Python legale. L'output è stato utilizzato esclusivamente per maggiore chiarezza in questo esempio.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```
