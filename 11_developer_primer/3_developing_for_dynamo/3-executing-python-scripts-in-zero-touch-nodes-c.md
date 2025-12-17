# Executing Python Scripts in Zero-Touch Nodes (C#)

### Executing Python Scripts in Zero-Touch Nodes (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

If you are comfortable writing scripts in Python and want more functionality out of the standard Dynamo Python nodes, we can use Zero-Touch to create our own. Let's start with a simple example that allows us to pass a python script as a string to a Zero-Touch node where the script is executed and a result is returned. This case study will build on the walk-throughs and examples in the Getting Started section, please refer to those if you are completely new to creating Zero-Touch nodes.

![A Zero-Touch node that will execute a Python script string](images/python-case-study.png)

> A Zero-Touch node that will execute a Python script string

#### Python Engine <a href="#python-engine" id="python-engine"></a>

PythonNet3 is now the default engine with a smoother experience if you’re migrating from CPython. All new Python nodes created in Dynamo 4.0+ start with PythonNet3.

This node relies on an instance of the IronPython scripting engine. To do this, we need to reference a few additional assemblies. Follow the steps below to setup a basic template in Visual Studio:

* Create a new Visual Studio class project
* Add a reference to the `IronPython.dll` located in `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`
* Add a reference to the `Microsoft.Scripting.dll` located in `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`
* Include the `IronPython.Hosting` and `Microsoft.Scripting.Hosting` `using` statements in your class
* Add a private empty constructor to prevent an additional node from being added to the Dynamo library with our package
* Create a new method that takes a single string as an input parameter
* Within this method we will instantiate a new Python engine, and create an empty script scope. You can imagine this scope as the global variables within an instance of the Python interpreter
* Next, call `Execute` on the engine passing the input string and the scope as parameters
* Finally, retrieve and return the results of the script by calling `GetVariable` on the scope and passing the name of the variable from your Python script that contains the value you are trying to return. (See the example below for further details)

The following code provides an example for the step mentioned above. Building the solution will create a new `.dll` located in the bin folder of our project. This `.dll` can now be imported into Dynamo as part of a package or by navigating to`File < Import Library...`

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

The Python script is returning the variable `output`, which means we will need an `output` variable in the Python script. Use this sample script to test the node in Dynamo. If you have ever used the Python node in Dynamo the following should look familiar. For more information check out the [Python section of the primer](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Multiple Outputs <a href="#multiple-outputs" id="multiple-outputs"></a>

One limitation of the standard Python nodes is that they only have a single output port so if we wish to return multiple object we must construct a list and retrieve each object in. If we modify the example above to return a dictionary, we can add as many output ports as we want. Refer to the Returning Multiple Values section in Going Further With Zero-Touch for more specifics on dictionaries.

![This node is allowing us to return both the cuboid's volume and its centroid.](images/python-multi-case-study.png)

> This node is allowing us to return both the cuboid's volume and its centroid.

Let's modify the previous example with these steps:

* Add a reference to `DynamoServices.dll` from the NuGet package manager
* In addition to the previous assemblies include `System.Collections.Generic` and `Autodesk.DesignScript.Runtime`
* Modify the return type on our method to return a dictionary which will contain our outputs
* Each output must be individually retrieved from the scope (consider setting up a simple loop for larger sets of outputs)

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

We have also added an additional output variable (`output2`) to the sample python script. Keep in mind that these variables can use any legal Python naming conventions, output has been used strictly for the sake of clarity in this example.

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

#### PythonNet3 Known Limitaions and Workarounds<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

The following are few known limitations and workarounds while using PythonNet3

* .NET collections are not automatically converted to Python lists
    * You must explicitly convert ```.NET``` arrays or collections using ```list(...)``` before using ```len()```, indexing or iteration.
* Generic .NET methods may require explicit type parameters
    * Some methods (e.g. ```GroupBy```) will fail unless you manually specify the generic types instead of relying on automatic inference.
* Extension methods are not discoverable via ```dir()``` or auto complete
    * Extension methods may still work when called explicitly, but they do not appear in introspection or code completion.
* DataTable extension methods are not supported
    * Importing ```System.Data.DataTableExtensions``` fails; these helper methods cannot be used directly.
* Some Dynamo core methods behave differently in PythonNet3
    * Certain functions (e.g. list flattening) may not work as expected due to stricter collection handling.
* Python classes cannot be passed between nodes if they inherit from .NET types
    * Classes derived from .NET types or interfaces cannot be safely transferred between Python nodes.
* Python ```set()``` does not accept some .NET objects
    * Objects like ```InvalidElementId``` must be filtered out or handled using .NET collections instead.
* Frequent ```print()``` calls can cause memory growth
    * Avoid heavy use of ```print()``` in loops or long-running scripts.
* Dictionary interoperability between Dynamo and Python is limited
    * Dynamo dictionaries and Python dictionaries are not fully interchangeable and may require manual conversion.
* The method ```Marshal.GetActiveObject()``` to get the running COM instance of a specified object is no longer available
    * Use ```BindToMoniker``` if you know the path of the file in use.
    * Code a library in C# using the class structure ```Marshal.GetActiveObject()```

#### Migrating from CPython3 to PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo will automatically migrate CPython nodes to PythonNet 3. 
Here’s what happens:

> 1. A backup copy of your original file is created automatically.
> 2. All CPython nodes (including custom nodes that use CPython) are converted to PythonNet3. 
> 3. A toast notification lets you know how many nodes were migrated.
> 4. When saving, you’ll see a reminder that your Python nodes will now use PythonNet3. 
Again, don’t worry about backward computability: For those who work in multi-version shops (e.g., Revit or Civil 3D 2025/2026), install the PythonNet3 Engine package in Dynamo 3.3–3.6 to maintain compatibility. 

#### Migrating from IronPython2 to PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

If your graph uses an IronPython engine, there’s no auto-migration. 

If the matching IronPython package is installed, your graph runs normally. 
If it’s missing, you’ll see a dependency warning in the Workspace References extension asking you to download the package. 
You can continue using IronPython by reinstalling the package. But because IronPython hasn’t been updated in years and Dynamo hasn’t been actively supporting these engines in Dynamo for quite some time, we strongly recommend migrating to PythonNet3 to ensure your graphs keep working reliably going forward. While DynamoIronPython2.7 and DynamoIronPython3 will continue to remain available as packages on the Dynamo Package Manager they will no longer be maintained by the Dynamo team. 

In this case, the migration option available to you is node-by-node migration utilizing the Migration Assistant available within the Python Editor.  

More information on migration can be found in this [blog](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/)