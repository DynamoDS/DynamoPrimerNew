# Ejecución de secuencias de comandos de Python en nodos Zero-Touch (C#)

### Ejecución de secuencias de comandos de Python en nodos Zero-Touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Si sabe cómo escribir secuencias de comandos en Python y desea obtener más funcionalidad de los nodos estándar de Python de Dynamo, podemos utilizar Zero-Touch para crear nuestros propios nodos. Comencemos con un ejemplo sencillo que nos permite pasar una secuencia de comandos de Python como una cadena a un nodo Zero-Touch donde se ejecuta la secuencia de comandos y se devuelve un resultado. Este caso real se basará en los recorridos y los ejemplos de la sección Introducción. Consulte estos ejemplos si es la primera vez que crea nodos Zero-Touch.

![Un nodo Zero-Touch que ejecutará una cadena de secuencia de comandos de Python.](images/python-case-study.png)

> Un nodo Zero-Touch que ejecutará una cadena de secuencia de comandos de Python.

#### Motor de Python <a href="#python-engine" id="python-engine"></a>

Este nodo se basa en una instancia del motor de secuencias de comandos de IronPython. Para ello, debemos hacer referencia a algunos montajes adicionales. Siga los pasos que se indican a continuación para configurar una plantilla básica en Visual Studio:

* Cree un nuevo proyecto de clase de Visual Studio.
* Añada una referencia al archivo `IronPython.dll`, que se encuentra en `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`.
* Añada una referencia al archivo `Microsoft.Scripting.dll`, que se encuentra en `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`.
* Incluya las instrucciones `IronPython.Hosting` y `Microsoft.Scripting.Hosting` `using` en la clase.
* Añada un constructor privado vacío para evitar que se agregue un nodo adicional a la biblioteca de Dynamo con nuestro paquete.
* Cree un nuevo método que utilice una única cadena como parámetro de entrada.
* En este método, se creará una instancia de un nuevo motor de Python y un ámbito de secuencias de comandos vacío. Puede considerar este ámbito como variables globales de una instancia del intérprete de Python.
* A continuación, llame a `Execute` en el motor que transfiere la cadena de entrada y el ámbito como parámetros
* Por último, recupere y devuelva los resultados de la secuencia de comandos. Para ello, llame a `GetVariable` en el ámbito y transfiera el nombre de la variable desde la secuencia de comandos de Python que contiene el valor que intenta devolver. (Consulte el siguiente ejemplo para obtener más información).

El siguiente código proporciona un ejemplo del paso mencionado anteriormente. La compilación de la solución creará un nuevo archivo `.dll` ubicado en la carpeta bin del proyecto. Este archivo `.dll` ahora se puede importar en Dynamo como parte de un paquete o accediendo a `File < Import Library...`.

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

La secuencia de comandos de Python devuelve la variable `output`, lo que significa que necesitaremos una variable `output` en la secuencia de comandos de Python. Utilice esta secuencia de comandos de ejemplo para probar el nodo en Dynamo. Si alguna vez ha utilizado el nodo de Python en Dynamo, debería resultarle familiar lo siguiente. Para obtener más información, consulte la [sección sobre Python de Dynamo Primer](https://primer2.dynamobim.org/v/es/8_coding_in_dynamo/8-3_python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Varias salidas <a href="#multiple-outputs" id="multiple-outputs"></a>

Una limitación de los nodos estándar de Python es que solo tienen un único puerto de salida, por lo que si deseamos devolver varios objetos, debemos crear una lista y recuperar cada objeto. Si modificamos el ejemplo anterior para devolver un diccionario, podemos añadir tantos puertos de salida como deseemos. Consulte la sección Devolución de varios valores de Conceptos avanzados de Zero-Touch para obtener información detallada sobre los diccionarios.

![Este nodo nos permite devolver tanto el volumen del ortoedro como su centroide.](images/python-multi-case-study.png)

> Este nodo nos permite devolver tanto el volumen del ortoedro como su centroide.

Vamos a modificar el ejemplo anterior con los siguientes pasos:

* Añada una referencia a `DynamoServices.dll` desde el administrador de paquetes NuGet.
* Además de los montajes anteriores, se incluyen `System.Collections.Generic` y `Autodesk.DesignScript.Runtime`.
* Modifique el tipo de valor devuelto en el método para que se devuelva un diccionario que contendrá las salidas.
* Cada salida se debe recuperar individualmente desde el ámbito (considere la posibilidad de configurar un bucle sencillo para conjuntos de salidas de mayor tamaño).

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

También hemos añadido una variable de salida adicional (`output2`) a la secuencia de comandos de Python de ejemplo. Tenga en cuenta que estas variables pueden utilizar cualquier convención de nomenclatura legal de Python; la salida se ha utilizado estrictamente para brindar mayor claridad a este ejemplo.

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
