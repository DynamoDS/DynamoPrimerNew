# Esecuzione di script Python in nodi zero-touch (C#)

### Esecuzione di script Python in nodi zero-touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Se si ha familiarità con la scrittura di script in Python e si desidera ottenere più funzionalità dai nodi Python di Dynamo standard, è possibile utilizzare nodi zero-touch per creare nodi personalizzati. Iniziamo con un semplice esempio che ci consente di trasferire uno script Python come stringa ad un nodo zero-touch dove viene eseguito lo script e viene restituito un risultato. Questo case study si baserà sulle simulazioni e sugli esempi della sezione Per iniziare, a cui fare riferimento se si è completamente inesperti nella creazione di nodi zero-touch.

![Nodo zero-touch che eseguirà una stringa di script Python](../../.gitbook/assets/python-case-study.png)

> Nodo zero-touch che eseguirà una stringa di script Python

#### Motore Python <a href="#python-engine" id="python-engine"></a>

PythonNet3 è ora il motore di default con un'esperienza più uniforme se si esegue la migrazione da CPython. Tutti i nuovi nodi Python creati in Dynamo 4.0+ iniziano con PythonNet3.

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

Lo script Python restituisce la variabile `output`, il che significa che è necessaria una variabile `output` nello script Python. Utilizzare questo script di esempio per verificare il nodo in Dynamo. Se si è mai utilizzato il nodo Python in Dynamo, quanto segue dovrebbe risultare familiare. Per ulteriori informazioni, consultare la [sezione Python della Guida introduttiva](https://primer2.dynamobim.org/it/8_coding_in_dynamo/8-3_python/1-python).

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

![Questo nodo ci consente di restituire sia il volume del cuboide che il relativo baricentro.](../../.gitbook/assets/python-multi-case-study.png)

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

#### Limitazioni note e soluzioni di PythonNet3<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

Di seguito sono riportate alcune limitazioni note e soluzioni alternative durante l'utilizzo di PythonNet3:

* Le raccolte .NET non vengono convertite automaticamente in elenchi Python.
    * È necessario convertire in modo esplicito matrici o raccolte ```.NET``` utilizzando ```list(...)``` prima di utilizzare ```len()```, indicizzazione o iterazione.
* I metodi .NET generici possono richiedere parametri di tipo esplicito.
    * Alcuni metodi (ad es. ```GroupBy```) avranno esito negativo a meno che non si specifichino manualmente i tipi generici invece di affidarsi all'inferenza automatica.
* I metodi di estensione non sono individuabili tramite ```dir()``` o il completamento automatico.
    * I metodi di estensione possono ancora funzionare quando vengono chiamati in modo esplicito, ma non vengono visualizzati nell'introspezione o nel completamento del codice.
* I metodi di estensione DataTable non sono supportati.
    * L'importazione di ```System.Data.DataTableExtensions``` non riesce; Questi metodi di helper non possono essere utilizzati direttamente.
* Alcuni metodi principali di Dynamo si comportano in modo diverso in PythonNet3.
    * Alcune funzioni (ad esempio, la riduzione della nidificazione degli elenchi) potrebbero non funzionare come previsto a causa di una gestione delle raccolte più rigorosa.
* Le classi Python non possono essere passate da un nodo all'altro se sono ereditate da tipi .NET.
    * Le classi derivate da tipi o interfacce .NET non possono essere trasferite in modo sicuro tra nodi Python.
* Python ```set()``` non accetta alcuni oggetti .NET.
    * Oggetti come ```InvalidElementId``` devono essere esclusi tramite filtri o gestiti utilizzando le raccolte .NET.
* Chiamate di ```print()``` frequenti possono causare l'aumento della memoria.
    * Evitare l'uso intensivo di ```print()``` in loop o script di lunga durata.
* L'interoperabilità del dizionario tra Dynamo e Python è limitata.
    * I dizionari di Dynamo e Python non sono completamente intercambiabili e potrebbero richiedere la conversione manuale.
* Il metodo ```Marshal.GetActiveObject()``` per ottenere l'istanza COM in esecuzione di un oggetto specificato non è più disponibile.
    * Utilizzare ```BindToMoniker``` se si conosce il percorso del file in uso.
    * Codificare una libreria in C# utilizzando la struttura delle classi ```Marshal.GetActiveObject()```.

#### Migrazione da CPython3 a PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo eseguirà automaticamente la migrazione dei nodi CPython a PythonNet 3. Ecco cosa succede:

> 1. Viene creata automaticamente una copia di backup del file originale.
> 2. Tutti i nodi CPython (inclusi i nodi personalizzati che utilizzano CPython) vengono convertiti in PythonNet3. 
> 3. Una notifica toast consente di sapere quanti nodi sono stati migrati.
> 4. Durante il salvataggio, verrà visualizzato un promemoria per ricordare che i nodi Python utilizzeranno PythonNet3. Ancora una volta, non occorre preoccuparsi della compatibilità con le versioni precedenti: per chi lavora con più versioni (ad esempio Revit o Civil 3D 2025/2026), è sufficiente installare il pacchetto PythonNet3 Engine in Dynamo 3.3-3.6 per mantenere la compatibilità. 

#### Migrazione da IronPython2 a PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Se il grafico utilizza un motore IronPython, non è prevista alcuna migrazione automatica. 

Se è installato il pacchetto IronPython corrispondente, il grafico viene eseguito normalmente. Se risulta mancante, verrà visualizzato un avviso di dipendenza nell'estensione Riferimenti area di lavoro in cui viene chiesto di scaricare il pacchetto. È possibile continuare ad utilizzare IronPython reinstallando il pacchetto. Tuttavia, poiché IronPython non viene aggiornato da anni e Dynamo non supporta attivamente questi motori in Dynamo da molto tempo, si consiglia di eseguire la migrazione a PythonNet3 per garantire che i grafici continuino a funzionare in modo affidabile in futuro. Sebben DynamoIronPython2.7 e DynamoIronPython3 continueranno ad essere disponibili come pacchetti in Dynamo Package Manager, non saranno più gestiti dal team di Dynamo. 

In questo caso, l'opzione di migrazione disponibile è la migrazione nodo per nodo che utilizza Migration Assistance presente nell'editor Python.  

Ulteriori informazioni sulla migrazione sono disponibili in questo [blog](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/).