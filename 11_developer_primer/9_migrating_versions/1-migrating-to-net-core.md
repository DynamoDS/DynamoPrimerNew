# Migrating from Net Framework 4.x to Net Core
PythonNet3 is available from Revit 2025 as a package (called PythonNet3 Engine) and therefore runs under .NET 8+ (Core) instead of .NET Framework 4.x. This change has significant implications for interoperability and certain APIs.

Here are some common examples:

## Access to the Global Assembly Cache (GAC)

It is now impossible to directly use the “assemblies” present in the Windows GAC (like _Excel Interop_ or _Word Interop_).

**Solution:** Consider migrating to native Python solutions like _openpyxl_ or the _OpenXML SDK_. If using Excel Interop is absolutely necessary, you must create an instance and use **.NET Reflection** to interact with it.

**Example of using Reflection for Excel Interop (Excerpt):**
```python
import clr
import sys
import System
from System import Array
from System.Collections.Generic import List, Dictionary, IDictionary

clr.AddReference("System.Reflection")
from System.Reflection import BindingFlags

from System.Runtime.InteropServices import Marshal

clr.AddReference("System.Core")
clr.ImportExtensions(System.Linq)

xls_filePath = IN[0]
xls_SheetName = IN[1]
dict_values = {}

systemType = System.Type.GetTypeFromProgID("Excel.Application", True)
try:
    ex = System.Activator.CreateInstance(systemType)
except:
    methodCreate = next((m for m in clr.GetClrType(System.Activator)\
            .GetMethods() if "CreateInstance(System.Type)" in m.ToString()), None)
    ex = methodCreate.Invoke(None, (systemType, ))

ex.Visible = False
workbooks = ex.GetType().InvokeMember("Workbooks", BindingFlags.GetProperty ,None, ex, None)
workbook = workbooks.GetType().InvokeMember("Open", BindingFlags.InvokeMethod , None, workbooks, (xls_filePath, )) 
worksheets = workbook.GetType().InvokeMember("Worksheets", BindingFlags.GetProperty ,None, workbook, None)

ws = worksheets.GetType().InvokeMember("Item", BindingFlags.GetProperty ,None, worksheets, (xls_SheetName,))

```

# Changing Methods and Namespaces
Some Windows-specific or obsolete technologies have been removed or replaced by new implementations with new namespaces.

### Accessing COM objects
The method Marshal.GetActiveObject() to get the running COM instance of a specified object is no longer available.

**Solutions:**

 1. Use _BindToMoniker_ if you know the path of the file in use.
 2. Code a library in C# using the class structure _Marshal.GetActiveObject()_ 
**Example of using** _BindToMoniker_:

```python
import clr
import os
import time
import System

clr.AddReference("System.Reflection")
from System.Reflection import BindingFlags

clr.AddReference("AcMgd")
clr.AddReference("AcCoreMgd")
clr.AddReference("Autodesk.AutoCAD.Interop")

from System import *

from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.Interop import *
from Autodesk.AutoCAD.ApplicationServices import Application as acapp

changeViewCommand = "_VIEW "

adoc = Application.DocumentManager.MdiActiveDocument
currentFileName = adoc.Name
print(currentFileName)
com_doc = System.Runtime.InteropServices.Marshal.BindToMoniker(currentFileName)
args = System.Array[System.Object]([changeViewCommand])
com_doc.GetType().InvokeMember("SendCommand", BindingFlags.InvokeMethod, None, com_doc, args)

OUT = True
```