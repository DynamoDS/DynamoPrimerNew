# Konfigurowanie własnego szablonu w języku Python

W dodatku Dynamo 2.0 można określić domyślny szablon `(.py extension)`, który ma być używany podczas otwierania okna języka Python po raz pierwszy. Od dawna proszono o dodanie tej funkcji, ponieważ przyspiesza ona korzystanie z języka Python w dodatku Dynamo. Dzięki możliwości użycia szablonu można przygotować domyślny import gotowy do użycia, gdy chcemy utworzyć skrypt niestandardowy w języku Python.

Położenie tego szablonu to `APPDATA` dla instalacji dodatku Dynamo.

Zwykle jest to ścieżka `( %appdata%\Dynamo\Dynamo Core\{version}\ )`.

![](../images/8-3/3/pythontemplates-appdatafolderlocation.jpg)

### Konfigurowanie szablonu

Aby korzystać z tej funkcji, należy dodać następujący wiersz w pliku `DynamoSettings.xml`. _(Edytuj w Notatniku)_

![](../images/8-3/3/pythontemplates-dynamosettingsxmlfile.png)

Część `<PythonTemplateFilePath />` można po prostu zastąpić następującą treścią:

```
<PythonTemplateFilePath>
<string>C:\Users\CURRENTUSER\AppData\Roaming\Dynamo\Dynamo Core\2.0\PythonTemplate.py</string>
</PythonTemplateFilePath>
```

{% hint style="warning" %} 
_Uwaga: zastąp element CURRENTUSER nazwą użytkownika_ 
{% endhint %}

Następnie musimy utworzyć szablon z funkcjami, które mają być wbudowane. W tym przypadku osadźmy powiązane z programem Revit importy i niektóre inne typowe elementy podczas pracy z programem Revit.

Można rozpocząć od pustego dokumentu Notatnika i wkleić do niego następujący kod:

``` py
import clr

clr.AddReference('RevitAPI')
from Autodesk.Revit.DB import *
from Autodesk.Revit.DB.Structure import *

clr.AddReference('RevitAPIUI')
from Autodesk.Revit.UI import *

clr.AddReference('System')
from System.Collections.Generic import List

clr.AddReference('RevitNodes')
import Revit
clr.ImportExtensions(Revit.GeometryConversion)
clr.ImportExtensions(Revit.Elements)

clr.AddReference('RevitServices')
import RevitServices
from RevitServices.Persistence import DocumentManager
from RevitServices.Transactions import TransactionManager

doc = DocumentManager.Instance.CurrentDBDocument
uidoc=DocumentManager.Instance.CurrentUIApplication.ActiveUIDocument

#Preparing input from dynamo to revit
element = UnwrapElement(IN[0])

#Do some action in a Transaction
TransactionManager.Instance.EnsureInTransaction(doc)

TransactionManager.Instance.TransactionTaskDone()

OUT = element
```

Po zakończeniu zapisz ten plik jako `PythonTemplate.py` w lokalizacji `APPDATA`.

### Późniejsze zachowanie skryptu w języku Python

_P_o zdefiniowaniu szablonu w języku Python dodatek Dynamo będzie go szukać po każdym umieszczeniu węzła w języku Python. Jeśli go nie znajdzie, okno będzie wyglądać jak domyślne okno języka Python.

![](../images/8-3/3/pythontemplates-beforesetuptemplate.jpg)

Jeśli szablon w języku Python zostanie znaleziony (np. nasz szablon dotyczący programu Revit), zostaną wyświetlone wszystkie domyślne wbudowane elementy.

![](../images/8-3/3/pythontemplates-aftersetuptemplate.jpg)

Dodatkowe informacje dotyczące tego wspaniałego dodatku (którego autorem jest Radu Gidei) można znaleźć tutaj. https://github.com/DynamoDS/Dynamo/pull/8122
