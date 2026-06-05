# Migrating from CPython3 (PythonNet2.5) to PythonNet3

This migration is simpler but involves fundamental changes in the management of .NET types.

**Of course, everything mentioned in the previous section must be taken into consideration.**

PythonNet3 aims to bring the “Python in Dynamo” experience closer to that offered by IronPython, while leveraging the CPython ecosystem (NumPy, Pandas, etc.). It is the industry standard for Python/.NET interoperability.

# Major Benefits and Improvements

| **Features**   | **PythonNet3(Python 3.7+/Python.NET v3)**     | **Comments**       |   
| -------------- | -------------------------------|----------------------- | 
| **CPython Ecosystem** | Excellent access to the entire modern PyPI ecosystem (NumPy, Pandas, etc.).|| 
| **.NET Collections Mechanism** | Automatic conversion is removed. The object is a “view” of the .NET collection, without copying data, which improves performance. However, they implement the standard Python collections interfaces of _collections.abc_.|| 
| **IEnumerable** | PyObject now implements IEnumerable in addition to IEnumerable<T>. A bug where all .NET class instances were treated as Iterable has been fixed.
(ex: _hasattr(pyObject, "\__iter___") )|| 
|**LINQ** | Ability to use LINQ method extensions on _IEnumerable<T>_.|Dynamo feature| 
| **Method overloading** |Support has been improved, including for methods with generic type parameters (<T>).|| 
| **C# Operators** |Python’s binary and unary arithmetic operators now call the corresponding C# operator methods.|| 
| **.NET Class Interfaces** |Simplified support for .NET Class Interfaces.|Dynamo feature| 
| **Out or ref parameters** | You can now overload .NET methods in Python that use _ref_ and _out_ parameters. To do this, you need to return the values ​​of these modified parameters as a tuple.|| 
| **C# Arithmetic Operators** | Python’s binary and unary arithmetic operators now call the corresponding C# operator methods.|| 
| **Heritage and Builders** | If you overload the method __init__ of a .NET type in Python, you must now explicitly call the base class constructor using _super().\__init___(...).|| 
| **Conversions of enumerations** | Implicit conversion between C# enums and Python integers is disabled. You must now use enum members (e.g.,_MonEnum.Option_ ). _Additionally, the .NET method **Enum.Value.ToString()** now returns the value name instead of its integer._|similar to IronPython|

# Details on some points
### Implementing .NET Class Interfaces
In CPython3 (PythonNet2.5), it is impossible to easily use .NET class interfaces. This is now fixed with Dynamo_PythonNet3.

Notes:

- A Python class that derives from a .NET class must have the attribute __namespace__
- The class _Custom_FamilyOption_ includes an example with _ref_ and _out_ parameters returning as a tuple

```
class Custom_SelectionElem(ISelectionFilter):
    __namespace__ = "SelectionNameSpace_tEfYX0DHE"
    #
    def __init__(self, bic):
        super().__init__() # necessary  if you override the __init__ method
        self.bic = bic
       
    def AllowElement(self, e):
        if e.Category.Id == ElementId(self.bic):
            return True
        else:
            return False
    def AllowReference(self, ref, point):
        return True

#        

class Custom_FamilyOption(IFamilyLoadOptions) :
    __namespace__ = "FamilyOptionNameSpace_tEfYX0DHE"

    def __init__(self):
        super().__init__() # necessary  if you override the __init__ method
       
    def OnFamilyFound(self, familyInUse, _overwriteParameterValues):
        overwriteParameterValues = True
        return (True, overwriteParameterValues)

    def OnSharedFamilyFound(self, sharedFamily, familyInUse, source, _overwriteParameterValues):
        overwriteParameterValues = True     
        return (True, overwriteParameterValues)
```

### No conversion of .NET Collections and Arrays

This is the major change from CPython3 (PythonNet 2.5), which performed automatic implicit conversion.

With PythonNet3:

> 1. Automatic conversion is removed.
> 2. .NET collections and arrays now implement the standard Python collections interfaces (collections.abc).
> 3. The .NET object behaves like a “view”, which is efficient because there is no data copying.
> 4. You can use .NET methods like LINQ directly on these objects.
> 5. To use methods specific to Python lists (like append()) or bracket indexing, an explicit conversion via list() is required.

**Consequences on the indexing of IEnumerable\<T>:**

Direct indexing _[index]_ is impossible on types _IEnumerable_ returned by certain methods.

Here is an example with PythonNet3 where we cannot use indexing because the method _face.ToProtoType()_ returns an IEnumerable and not a IList\<T>.

**Old (or incorrect) code:**
```python
element = UnwrapElement(IN[0])

ref = HostObjectUtils.GetSideFaces(element, ShellLayerType.Exterior)[0] # GetSideFaces return an Ilist so we can use indexer
face = element.GetGeometryObjectFromReference(ref)
ds_surface = face.ToProtoType()[0] # ToProtoType() on Revit face return an IEnumerable so we can't use indexer
```

**New code (Using LINQ or converting):**
```python
clr.AddReference("System.Core")
clr.ImportExtensions(System.Linq)

element = UnwrapElement(IN[0])

ref = HostObjectUtils.GetSideFaces(element, ShellLayerType.Exterior)[0] # GetSideFaces return an Ilist so we can use indexer
face = element.GetGeometryObjectFromReference(ref)
ds_surface = face.ToProtoType().First() # ToProtoType() on Revit face return an IEnumerable so we can use LINQ
# OR convert to python list
ds_surface = list(face.ToProtoType())[0] # ToProtoType() on Revit face return an IEnumerable so we need convert to python list to user indexer
OUT = ds_surface
```

### LINQ

Advice:

- When passing a lambda function as a function parameter, it must be explicitly converted, for example to _System.Func[\<input_type>, \<output_type>]_.
- Some extension libraries still don’t work, like _DataTableExtensions_ the .NET.

**Example of using LINQ extension methods**
```python
import sys
import clr
import System
from System.Collections.Generic import List, IList

clr.AddReference("System.Reflection")
from System.Reflection import Assembly

clr.AddReference('RevitAPI')
import Autodesk
from Autodesk.Revit.DB import *
import Autodesk.Revit.DB as DB


clr.AddReference('RevitServices')
import RevitServices
from RevitServices.Persistence import DocumentManager
doc = DocumentManager.Instance.CurrentDBDocument

clr.AddReference("System.Core")
clr.ImportExtensions(System.Linq)
#

resultB = FilteredElementCollector(doc).OfClass(FamilySymbol).WhereElementIsElementType()\
            .Where(System.Func[DB.Element, System.Boolean](lambda e_type : "E1" in e_type.Name))\
            .ToList()
           
resultC = FilteredElementCollector(doc).OfClass(FamilySymbol).WhereElementIsElementType()\
            .FirstOrDefault(System.Func[DB.Element, System.Boolean](lambda e_type : "E1" in e_type.Name))
           
resultD = FilteredElementCollector(doc).OfClass(FamilySymbol).WhereElementIsElementType()\
            .FirstOrDefault(System.Func[DB.Element, System.Boolean](lambda e_type : "E12" in e_type.Name))
           
resultGroup = FilteredElementCollector(doc).OfClass(FamilySymbol).WhereElementIsElementType()\
            .GroupBy[DB.Element, System.String](System.Func[DB.Element, System.String](lambda e_type :  e_type.Name))\
            .Select[System.Object, System.Object](System.Func[System.Object, System.Object](lambda x : x.ToList()))
           
           
assemblies = System.AppDomain.CurrentDomain.GetAssemblies()
assemblies_name = assemblies\
    .OrderBy[Assembly, System.String](System.Func[Assembly, System.String](lambda x : x.GetName().Name))\
    .Select[Assembly, System.String](System.Func[Assembly, System.Int32, System.String](lambda asm, index : f"{index + 1} : {asm.GetName().Name}"))
           
print(assemblies_name.GetType())
OUT = resultB.ToList(), resultC, resultD, resultGroup.ToList(), assemblies_name
```
