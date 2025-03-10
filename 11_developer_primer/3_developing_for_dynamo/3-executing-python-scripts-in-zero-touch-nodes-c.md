# Exécuter des scripts Python dans des nœuds Zero-Touch (C#)

### Exécuter des scripts Python dans des nœuds Zero-Touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Si vous êtes à l’aise avec l’écriture de scripts en Python et que vous voulez plus de fonctionnalités à partir des nœuds Python standard de Dynamo, nous pouvons utiliser Zero-Touch pour créer les nôtres. Commençons par un exemple simple qui nous permet de faire passer un script python sous la forme d’une chaîne à un nœud Zero-Touch où le script est exécuté et un résultat est renvoyé. Cette étude de cas s’appuie sur les guides et les exemples de la section Mise en route. Reportez-vous à ces derniers si vous êtes novice en matière de création de nœuds Zero-Touch.

![Un nœud Zero-Touch qui exécute une chaîne de script Python](images/python-case-study.png)

> Un nœud Zero-Touch qui exécute une chaîne de script Python

#### Moteur Python <a href="#python-engine" id="python-engine"></a>

Ce nœud s’appuie sur une instance du moteur de script IronPython. Pour faire cela, nous devons référencer quelques assemblages supplémentaires. Suivez les étapes ci-dessous pour configurer un modèle de base dans Visual Studio :

* créez un projet de classe Visual Studio ;
* ajoutez une référence au `IronPython.dll` situé dans `C:\Program Files (x86)\IronPython 2.7\IronPython.dll` ;
* ajoutez une référence au `Microsoft.Scripting.dll` situé dans `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll` ;
* incluez les instructions `IronPython.Hosting` et `Microsoft.Scripting.Hosting` `using` dans votre classe ;
* ajoutez un constructeur privé vide pour empêcher l’ajout d’un nœud supplémentaire à la bibliothèque Dynamo avec votre package ;
* créez une méthode qui prend une seule chaîne comme paramètre d’entrée ;
* dans cette méthode, nous allons instancier un nouveau moteur Python et créer une portée de script vide. Vous pouvez imaginer cette portée comme les variables globales dans une instance de l’interpréteur Python ;
* appelez ensuite `Execute` sur le moteur en passant la chaîne d’entrée et la portée comme paramètres ;
* enfin, récupérez et renvoyez les résultats du script en appelant `GetVariable` sur la portée et en transmettant le nom de la variable de votre script Python qui contient la valeur que vous essayez de renvoyer (Voir l’exemple ci-dessous pour plus de détails).

Le code suivant fournit un exemple pour l’étape mentionnée ci-dessus. La génération de la solution entraîne la création d’un nouveau `.dll` situé dans le dossier bin de notre projet. Ce `.dll` peut désormais être importé dans Dynamo en tant que composant d’un package ou en accédant à `File < Import Library...`

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

Le script Python renvoie la variable `output`, ce qui signifie que vous aurez besoin d’une variable `output` dans le script Python. Utilisez cet exemple de script pour tester le nœud dans Dynamo. Si vous avez déjà utilisé le nœud Python dans Dynamo, ce qui suit devrait vous sembler familier. Pour plus d’informations, consultez la [section Python du guide](http://dynamoprimer.com/en/09\_Custom-Nodes/9-4\_Python.html).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Sorties multiples <a href="#multiple-outputs" id="multiple-outputs"></a>

L’une des limites des nœuds Python standard est qu’ils n’ont qu’un seul port de sortie. Si nous souhaitons renvoyer plusieurs objets, nous devons construire une liste et récupérer chaque objet à l’intérieur. Si nous modifions l’exemple ci-dessus pour qu’il renvoie un dictionnaire, nous pouvons ajouter autant de ports de sortie que nous le souhaitons. Reportez-vous à la section Renvoyer plusieurs valeurs dans Aller plus loin avec Zero-Touch pour plus de détails sur les dictionnaires.

![Ce nœud permet de renvoyer le volume du cuboïde et son centre de gravité.](images/python-multi-case-study.png)

> Ce nœud permet de renvoyer le volume du cuboïde et son centre de gravité.

Modifions l’exemple précédent en procédant comme suit :

* ajoutez une référence `DynamoServices.dll` du gestionnaire de package NuGet ;
* en plus des assemblages précédents, incluez `System.Collections.Generic` et `Autodesk.DesignScript.Runtime` ;
* modifiez le type de retour de notre méthode pour qu’elle renvoie un dictionnaire qui contiendra nos résultats ;
* chaque sortie doit être extraite individuellement de la portée (il est conseillé de configurer une boucle simple pour des ensembles de sorties plus importants).

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

Nous avons également ajouté une variable de sortie supplémentaire (`output2`) à l’exemple de script Python. N’oubliez pas que ces variables peuvent utiliser n’importe quelle convention de dénomination légale de Python. La sortie a été utilisée uniquement pour des raisons de clarté dans cet exemple.

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
