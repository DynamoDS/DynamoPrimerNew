# Ausführen von Python-Skripts in Zero-Touch-Blöcken (C#)

### Ausführen von Python-Skripts in Zero-Touch-Blöcken (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Wenn wir mit dem Schreiben von Skripts in Python vertraut sind und mehr Funktionen aus den Dynamo-Python-Standardblöcken herausholen möchten, können wir mit Zero-Touch eigene Blöcke erstellen. Beginnen wir mit einem einfachen Beispiel, in dem ein Python-Skript als Zeichenfolge an einen Zero-Touch-Block übergeben werden kann, in dem das Skript ausgeführt und ein Ergebnis zurückgegeben wird. Diese Fallstudie baut auf den exemplarischen Vorgehensweisen und Beispielen im Abschnitt Erste Schritte auf. Wenn Sie mit der Erstellung von Zero-Touch-Blöcken noch nicht vertraut sind, finden Sie dort weitere Informationen.

![Ein Zero-Touch-Block, der eine Python-Skriptzeichenfolge ausführt](../../.gitbook/assets/python-case-study.png)

> Ein Zero-Touch-Block, der eine Python-Skriptzeichenfolge ausführt

#### Python-Modul <a href="#python-engine" id="python-engine"></a>

PythonNet3 ist jetzt die Vorgabe-Engine mit einer reibungsloseren Benutzererfahrung, wenn Sie von CPython migrieren. Alle neuen Python-Blöcke, die ab Dynamo 4.0 erstellt werden, beginnen mit PythonNet3.

Dieser Block ist von einer Instanz des IronPython-Skriptmoduls abhängig. Dazu müssen wir einige zusätzliche Assemblys referenzieren. Führen Sie die folgenden Schritte aus, um eine grundlegende Vorlage in Visual Studio einzurichten:

* Erstellen Sie ein neues Visual Studio-Klassenprojekt.
* Fügen Sie eine Referenz zur Datei `IronPython.dll` hinzu, die sich im Ordner `C:\Program Files (x86)\IronPython 2.7\IronPython.dll` befindet.
* Fügen Sie eine Referenz zur Datei `Microsoft.Scripting.dll` hinzu, die sich im Ordner `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll` befindet.
* Berücksichtigen Sie die `using`-Anweisungen `IronPython.Hosting` und `Microsoft.Scripting.Hosting` in Ihrer Klasse.
* Fügen Sie einen privaten, leeren Konstruktor hinzu, um zu verhindern, dass mit dem Paket ein zusätzlicher Block zur Dynamo-Bibliothek hinzugefügt wird.
* Erstellen Sie eine neue Methode, die eine einzelne Zeichenfolge als Eingabeparameter akzeptiert.
* Bei dieser Methode wird ein neues Python-Modul instanziiert und ein leerer Skriptbereich erstellt. Sie können sich diesen Bereich als globale Variablen innerhalb einer Instanz des Python-Interpreters vorstellen.
* Rufen Sie anschließend `Execute` im Modul auf, das die Eingabezeichenfolge und den Bereich als Parameter übergibt.
* Rufen Sie abschließend die Ergebnisse des Skripts ab, und geben Sie sie zurück, indem Sie `GetVariable` für den Bereich aufrufen und den Namen der Variablen aus dem Python-Skript übergeben, das den Wert enthält, den Sie zurückgeben möchten. (Weitere Informationen finden Sie im folgenden Beispiel.)

Der folgende Code stellt ein Beispiel für den oben genannten Schritt dar. Beim Erstellen der Projektmappe wird eine neue `.dll`-Datei im Ordner bin des Projekts erstellt. Diese `.dll`-Datei kann jetzt als Teil eines Pakets oder durch Navigieren zu `File < Import Library...` in Dynamo importiert werden.

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

Das Python-Skript gibt die Variable `output` zurück, d. h., wir benötigen eine `output`-Variable im Python-Skript. Verwenden Sie dieses Beispielskript, um den Block in Dynamo zu testen. Wenn Sie den Python-Block schon einmal in Dynamo verwendet haben, sollte Ihnen die folgende Darstellung bekannt sein. Weitere Informationen finden Sie im [Primer-Abschnitt zu Python](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Mehrere Ausgaben <a href="#multiple-outputs" id="multiple-outputs"></a>

Eine Einschränkung der Python-Standardblöcke besteht darin, dass sie nur einen einzigen Ausgabeanschluss haben. Wenn wir also mehrere Objekte zurückgeben möchten, müssen wir eine Liste erstellen und jedes Objekt abrufen. Wenn wir das obige Beispiel ändern, um ein Wörterbuch zurückzugeben, können wir beliebig viele Ausgabeanschlüsse hinzufügen. Weitere Informationen zu Wörterbüchern finden Sie im Abschnitt Zurückgeben mehrerer Werte unter Weitere Schritte mit Zero-Touch.

![Mit diesem Block können wir sowohl das Volumen des Quaders als auch seinen Schwerpunkt zurückgeben.](../../.gitbook/assets/python-multi-case-study.png)

> Mit diesem Block können wir sowohl das Volumen des Quaders als auch seinen Schwerpunkt zurückgeben.

Wir ändern das vorherige Beispiel mit den folgenden Schritten:

* Fügen Sie im NuGet-Paket-Manager eine Referenz auf `DynamoServices.dll` hinzu.
* Schließen Sie zusätzlich zu den vorherigen Assemblys `System.Collections.Generic` und `Autodesk.DesignScript.Runtime` mit ein.
* Ändern Sie den Rückgabetyp für die Methode, um ein Wörterbuch mit unseren Ausgaben zurückzugeben.
* Jede Ausgabe muss einzeln aus dem Bereich abgerufen werden. (Richten Sie ggf. eine einfache Schleife für größere Ausgabesätze ein.)

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

Außerdem wurde dem Beispiel-Python-Skript eine zusätzliche Ausgabevariable (`output2`) hinzugefügt. Beachten Sie, dass diese Variablen alle gültigen Python-Namenskonventionen verwenden können. Der Name Output wurde in diesem Beispiel einzig aus Gründen der Übersichtlichkeit verwendet.

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

#### Bekannte Einschränkungen und Umgehungslösungen in PythonNet3<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

Im Folgenden sind einige bekannte Einschränkungen und Umgehungslösungen bei der Verwendung von PythonNet3 aufgeführt.

* .NET-Sammlungen werden nicht automatisch in Python-Listen konvertiert.
    * Sie müssen ```.NET```-Arrays oder -Sammlungen explizit mit ```list(...)``` konvertieren, bevor Sie ```len()```, Indizierung oder Iteration verwenden können.
* Allgemeine .NET-Methoden erfordern möglicherweise explizite Typparameter.
    * Einige Methoden (z. B. ```GroupBy```) schlagen fehl, wenn Sie die allgemeinen Typen nicht manuell angeben und sich auf die automatische Ableitung verlassen.
* Erweiterungsmethoden können nicht über ```dir()``` oder die automatische Vervollständigung gefunden werden.
    * Erweiterungsmethoden funktionieren möglicherweise weiterhin, wenn sie explizit aufgerufen werden, sie werden jedoch nicht in der Introspektion oder Codevervollständigung angezeigt.
* DataTable-Erweiterungsmethoden werden nicht unterstützt.
    * Das Importieren von ```System.Data.DataTableExtensions``` schlägt fehl. Diese Hilfsmethoden können nicht direkt verwendet werden.
* Einige Dynamo Core-Methoden verhalten sich in PythonNet3 anders.
    * Bestimmte Funktionen (z. B. Listenvereinfachung) funktionieren aufgrund der strengeren Sammlungsverarbeitung möglicherweise nicht wie erwartet.
* Python-Klassen können nicht zwischen Blöcken übergeben werden, wenn sie Informationen von .NET-Typen übernehmen.
    * Klassen, die von .NET-Typen oder -Schnittstellen abgeleitet werden, können nicht sicher zwischen Python-Blöcken übertragen werden.
* Die Python-Option ```set()``` akzeptiert einige .NET-Objekte nicht.
    * Objekte wie ```InvalidElementId``` müssen stattdessen herausgefiltert oder mithilfe von .NET-Sammlungen verarbeitet werden.
* Häufige ```print()```-Anrufe können zu großen Speichermengen führen.
    * Vermeiden Sie die übermäßige Verwendung von ```print()``` in Schleifen oder Skripten mit langer Laufzeit.
* Die Interoperabilität von Wörterbüchern zwischen Dynamo und Python ist begrenzt.
    * Dynamo-Wörterbücher und Python-Wörterbücher sind nicht vollständig austauschbar und müssen möglicherweise manuell konvertiert werden.
* Die ```Marshal.GetActiveObject()```-Methode zum Abrufen der ausgeführten COM-Instanz eines angegebenen Objekts ist nicht mehr verfügbar.
    * Verwenden Sie ```BindToMoniker```, wenn Sie den Pfad der verwendeten Datei kennen.
    * Codieren Sie eine Bibliothek in C# mithilfe der Klassenstruktur ```Marshal.GetActiveObject()```.

#### Migrieren von CPython3 zu PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo migriert CPython-Blöcke automatisch zu PythonNet3. Dabei geschieht Folgendes:

> 1. Es wird automatisch eine Sicherungskopie Ihrer ursprünglichen Datei erstellt.
> 2. Alle CPython-Blöcke (einschließlich benutzerdefinierter Blöcke, die CPython verwenden) werden in PythonNet3 konvertiert. 
> 3. Eine Popup-Benachrichtigung informiert Sie darüber, wie viele Blöcke migriert wurden.
> 4. Beim Speichern wird eine Erinnerung darüber angezeigt, dass Ihre Python-Blöcke jetzt PythonNet3 verwenden. Informationen zur Abwärtskompatibilität: Wenn Sie in Umgebungen mit mehreren Versionen arbeiten (z. B. Revit oder Civil 3D 2025/2026), installieren Sie das PythonNet3 Engine-Paket in Dynamo 3.3–3.6, um die Kompatibilität zu gewährleisten. 

#### Migrieren von IronPython2 zu PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Wenn Ihr Diagramm eine IronPython-Engine verwendet, wird keine automatische Migration durchgeführt. 

Wenn das entsprechende IronPython-Paket installiert ist, wird das Diagramm normal ausgeführt. Wenn es fehlt, wird in der Erweiterung der Arbeitsbereichsreferenzen eine Abhängigkeitswarnung angezeigt, in der Sie aufgefordert werden, das Paket herunterzuladen. Sie können IronPython weiterhin verwenden, indem Sie das Paket neu installieren. Da IronPython jedoch seit Jahren nicht mehr aktualisiert wurde und Dynamo diese Engines seit geraumer Zeit nicht mehr aktiv in Dynamo unterstützt, empfehlen wir dringend die Migration zu PythonNet3, um sicherzustellen, dass Ihre Diagramme auch in Zukunft zuverlässig funktionieren. DynamoIronPython2.7 und DynamoIronPython3 bleiben zwar weiterhin als Pakete im Dynamo Package Manager verfügbar, werden jedoch nicht mehr vom Dynamo-Team betreut. 

In diesem Fall besteht die Migrationsoption darin, die Blöcke einzeln mithilfe des Migrationsassistenten, der im Python-Editor verfügbar ist, zu migrieren.  

Weitere Informationen zur Migration finden Sie in diesem [Blog](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/).