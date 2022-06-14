# Configurar seu próprio modelo Python

Com o Dynamo 2.0, podemos especificar um modelo padrão `(.py extension)` para usar ao abrir a janela do Python pela primeira vez. Essa foi uma solicitação muito desejada, pois isso acelera o uso do Python no Dynamo. Ter a capacidade de usar um modelo nos permite ter as importações padrão prontas para serem usadas quando quisermos desenvolver um script Python personalizado.

A localização desse modelo está em `APPDATA` da instalação do Dynamo.

Normalmente, isso é da seguinte maneira `( %appdata%\Dynamo\Dynamo Core\{version}\ )`.

![](<../images/8-3/3/python templates - appdata folder location.jpg>)

### Configurar o modelo

Para usar essa funcionalidade, é necessário adicionar a seguinte linha em nosso arquivo `DynamoSettings.xml`. _(Editar no bloco de notas)_

![](<../images/8-3/3/python templates -dynamo settings xml file.png>)

Onde vemos `<PythonTemplateFilePath />`, basta substituir por:

```
<PythonTemplateFilePath>
<string>C:\Users\CURRENTUSER\AppData\Roaming\Dynamo\Dynamo Core\2.0\PythonTemplate.py</string>
</PythonTemplateFilePath>
```

{% hint style="warning" %}
_Observação: Substitua CURRENTUSER por seu nome de usuário_
{% endhint %}

Em seguida, precisamos criar um modelo com a funcionalidade que desejamos usar incorporada. Em nosso caso, vamos incorporar as importações relacionadas ao Revit e alguns dos outros itens típicos ao trabalhar com o Revit.

É possível iniciar um documento do bloco de notas em branco e colar o seguinte código:

```
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

Uma vez feito isso, salve o arquivo como `PythonTemplate.py` na localização `APPDATA`.

### Comportamento posterior do script Python

Após o modelo do Python ser definido, o Dynamo procurará esses dados sempre que um nó do Python for inserido. Se não forem encontrados, será semelhante à janela do Python padrão.

![](<../images/8-3/3/python templates - before setup template.jpg>)

Se o modelo do Python for encontrado (como nosso modelo do Revit, por exemplo), será possível ver todos os itens padrão incorporados.

![](<../images/8-3/3/python templates - after setup template.jpg>)

Pode obter mais informações sobre esta grande adição (desenvolvida por Radu Gidei) aqui. https://github.com/DynamoDS/Dynamo/pull/8122
