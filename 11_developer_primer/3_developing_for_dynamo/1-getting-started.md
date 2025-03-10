# Mise en route

Avant de se lancer dans le développement, il est important d’établir des bases solides pour un nouveau projet. Il existe plusieurs modèles de projets dans la communauté des développeurs Dynamo qui sont d’excellents points de départ, mais il est encore plus utile de savoir comment démarrer un projet à partir de zéro. La génération d’un projet à partir de zéro permet de mieux comprendre le processus de développement.

![Visual Studio](images/visual-studio.jpg)

### Créer un projet Visual Studio <a href="#creating-a-visual-studio-project" id="creating-a-visual-studio-project"></a>

Visual Studio est un environnement IDE puissant dans lequel vous pouvez créer un projet, ajouter des références, générer des `.dlls` et déboguer. Lors de la création d’un projet, Visual Studio crée également une solution, une structure pour l’organisation des projets. Plusieurs projets peuvent exister au sein d’une même solution et peuvent être créés ensemble. Pour créer un nœud ZeroTouch, nous devons démarrer un nouveau projet Visual Studio dans lequel nous écrirons une bibliothèque de classes en C# et générerons un `.dll`.

![Création d’un projet dans Visual Studio](images/vs-new-project-1.jpg)

![Configuration d’un nouveau projet dans Visual Studio](images/vs-new-project-2.jpg)

> La fenêtre Nouveau projet dans Visual Studio
>
> 1. Commencez par ouvrir Visual Studio et créez un projet : `File > New > Project`.
> 2. Choisissez le modèle de projet `Class Library`.
> 3. Donnez un nom au projet (nous avons nommé le projet MyCustomNode).
> 4. Définissez le chemin d’accès au fichier de votre projet. Dans cet exemple, nous le laisserons à l’emplacement par défaut.
> 5. Sélectionnez `Ok`.

Visual Studio crée et ouvre automatiquement un fichier C#. Donnez-lui un nom approprié, configurez l’espace de travail et remplacez le code par défaut par cette méthode de multiplication.

```
 namespace MyCustomNode
 {
     public class SampleFunctions
     {
         public static double MultiplyByTwo(double inputNumber)
         {
             return inputNumber * 2.0;
         }
     }
 }
```

![Utilisation de l’explorateur de solutions](images/vs-edit-class.jpg)

> 1. Ouvrez l’explorateur de solutions et les fenêtres de sortie à partir de `View`.
> 2. Renommez le fichier `Class1.cs` en `SampleFunctions.cs` dans l’explorateur de solutions sur la droite.
> 3. Ajoutez le code ci-dessus pour la fonction de multiplication. Nous verrons plus tard comment Dynamo lira vos classes C#.
> 4. L’explorateur de solutions : il vous permet d’accéder à tous les éléments de votre projet.
> 5. La fenêtre de sortie : nous en aurons besoin plus tard pour voir si la génération s’est bien déroulée.

L’étape suivante consiste à générer le projet, mais avant cela, nous devons vérifier quelques paramètres. Tout d’abord, assurez-vous que `Any CPU` ou `x64` est sélectionné comme plateforme cible et que la case `Prefer 32-bit` n’est pas cochée dans les propriétés du projet.

![Paramètres de génération de Visual Studio](images/vs-build-settings.jpg)

> 1. Ouvrez les propriétés du projet en sélectionnant `Project > "ProjectName" Properties`.
> 2. Sélectionnez la page `Build`.
> 3. Sélectionnez `Any CPU` ou `x64` dans le menu déroulant.
> 4. Assurez-vous que la case `Prefer 32-bit` n’est pas cochée.

Nous pouvons maintenant générer le projet pour créer un `.dll`. Pour ce faire, sélectionnez `Build Solution` dans le menu `Build` ou utilisez le raccourci `CTRL+SHIFT+B`.

![Génération d’une solution](images/vs-build.jpg)

> 1. Sélectionnez `Build > Build Solution`.
> 2. Vous pouvez déterminer si votre projet a été créé correctement en cochant la fenêtre Sortie.

Si le projet a été créé correctement, un `.dll` nommé `MyCustomNode` figure dans le dossier `bin` du projet. Pour cet exemple, nous avons laissé le chemin d’accès au fichier du projet tel qu’il est défini par défaut par Visual Studio, à savoir `c:\users\username\documents\visual studio 2015\Projects`. Examinons la structure de fichiers du projet.

![Structure des fichiers du projet](images/folder-structure.jpg)

> 1. Le dossier `bin` contient le `.dll` créé à partir de Visual Studio.
> 2. Le fichier du projet Visual Studio.
> 3. Le fichier de classe.
> 4. La configuration de notre solution étant définie sur `Debug`, le `.dll` sera créé dans `bin\Debug`.

Nous pouvons maintenant ouvrir Dynamo et importer le `.dll`. Avec la fonction Ajouter, accédez à l’emplacement `bin` du projet et sélectionnez le `.dll` à ouvrir.

![Ouverture du fichier .dll du projet](images/dyn-import-dll.jpg)

> 1. Cliquez sur le bouton Ajouter pour importer un fichier `.dll`.
> 2. Accédez à l’emplacement du projet. Notre projet se trouve dans le chemin d’accès par défaut de Visual Studio : `C:\Users\username\Documents\Visual Studio 2015\Projects\MyCustomNode`.
> 3. Sélectionnez le `MyCustomNode.dll` à importer.
> 4. Cliquez sur `Open` pour charger le `.dll`.

Si une catégorie est créée dans la bibliothèque appelée `MyCustomNode`, le fichier .dll a été importé avec succès ! Cependant, Dynamo a créé deux nœuds alors que nous n’en voulions qu’un seul. Dans la section suivante, nous allons expliquer pourquoi cela se produit et comment Dynamo lit un fichier .dll.

![Nœuds personnalisés](images/dyn-customnode.jpg)

> 1. MyCustomNode dans la bibliothèque Dynamo. La catégorie Bibliothèque est déterminée par le nom `.dll`.
> 2. SampleFunctions.MultiplyByTwo dans la zone de dessin.

### Lecture des classes et des méthodes par Dynamo <a href="#how-dynamo-reads-classes-and-methods" id="how-dynamo-reads-classes-and-methods"></a>

Lorsque Dynamo charge un fichier .dll, il expose toutes les méthodes statiques publiques sous forme de nœuds. Les constructeurs, les méthodes et les propriétés seront transformés respectivement en nœuds de création, d’action et de requête. Dans notre exemple de multiplication, la méthode `MultiplyByTwo()` devient un nœud d’action dans Dynamo. Ceci est dû au fait que le nœud a été nommé en fonction de sa méthode et de sa classe.

![Noeud SampleFunction.MultiplyByTwo dans un graphique](images/multiplybytwo.png)

> 1. L’entrée est nommée `inputNumber` en fonction du nom du paramètre de la méthode.
> 2. La sortie est nommée `double` par défaut, car il s’agit du type de données renvoyé.
> 3. Le nœud est nommé `SampleFunctions.MultiplyByTwo` car il s’agit des noms de classe et de méthode.

Dans l’exemple ci-dessus, le nœud supplémentaire de création `SampleFunctions` a été créé, car nous n’avons pas fourni explicitement de constructeur et par conséquent un constructeur a été automatiquement créé. Nous pouvons éviter cela en créant un constructeur privé vide dans notre classe `SampleFunctions`.

```
namespace MyCustomNode
{
    public class SampleFunctions
    {
        //The empty private constructor.
        //This will be not imported into Dynamo.
        private SampleFunctions() { }

        //The public multiplication method. 
        //This will be imported into Dynamo.
        public static double MultiplyByTwo(double inputNumber)
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Méthode importée en tant que nœud de création](images/private-constructor.jpg)

> 1. Dynamo a importé notre méthode en tant que nœud de création

### Ajouter des références de package Dynamo NuGet <a href="#adding-dynamo-nuget-package-references" id="adding-dynamo-nuget-package-references"></a>

Le nœud de multiplication est très simple et aucune référence à Dynamo n’est nécessaire. Si nous voulons accéder à l’une des fonctionnalités de Dynamo pour créer une géométrie par exemple, nous devrons faire référence aux packages NuGet de Dynamo.

* [ZeroTouchLibrary](https://www.nuget.org/packages/DynamoVisualProgramming.ZeroTouchLibrary/2.0.0-beta3026) : package permettant de générer des bibliothèques de nœuds Zero-Touch pour Dynamo, qui contient les bibliothèques suivantes : DynamoUnits.dll, ProtoGeometry.dll.
* [WpfUILibrary](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) : package permettant de générer des bibliothèques de nœuds pour Dynamo avec une interface utilisateur personnalisée dans WPF, qui contient les bibliothèques suivantes : DynamoCoreWpf.dll, CoreNodeModels.dll, CoreNodeModelWpf.dll.
* [DynamoServices](https://www.nuget.org/packages/DynamoVisualProgramming.WpfUILibrary/2.0.0-beta3026) : bibliothèque DynamoServices pour Dynamo.
* [Core](https://www.nuget.org/packages/DynamoVisualProgramming.Core/2.0.0-beta3026) : infrastructure de test unitaire et système pour Dynamo qui contient les bibliothèques suivantes : DSIronPython.dll, DynamoApplications.dll, DynamoCore.dll, DynamoInstallDetective.dll, DynamoShapeManager.dll, DynamoUtilities.dll, ProtoCore.dll, VMDataBridge.dll.
* [Tests](https://www.nuget.org/packages/DynamoVisualProgramming.Tests/2.0.0-beta3026) : infrastructure de test unitaire et système pour Dynamo qui contient les bibliothèques suivantes : DynamoCoreTests.dll, SystemTestServices.dll, TestServices.dll.
* [DynamoCoreNodes](https://www.nuget.org/packages/DynamoVisualProgramming.DynamoCoreNodes/2.0.0-beta3026) : package pour la génération de nœuds de noyau pour Dynamo qui contient les bibliothèques suivantes : Analysis.dll, GeometryColor.dll, DSCoreNodes.dll.

Pour référencer ces packages dans un projet Visual Studio, téléchargez le package à partir de NuGet à l’aide des liens ci-dessus et référencez manuellement les fichiers .dll ou utilisez le gestionnaire de package NuGet dans Visual Studio. Nous pouvons d’abord voir comment les installer avec NuGet dans Visual Studio.

![Ouverture du gestionnaire de package NuGet](images/vs-nuget-package-manager2.jpg)

> 1. Ouvrez le gestionnaire de package NuGet en sélectionnant `Tools > NuGet Package Manager > Manage NuGet Packages for Solution...`.

Il s’agit du gestionnaire de package NuGet. Cette fenêtre indique les packages qui ont été installés pour le projet et permet à l’utilisateur d’en rechercher d’autres. Si une nouvelle version du package DynamoServices est publiée, les packages peuvent être mis à jour à partir d’ici ou rétablis à une version antérieure.

![Gestionnaire de package NuGet](images/vs-nuget-package-manager.jpg)

> 1. Sélectionnez Parcourir et recherchez DynamoVisualProgramming pour faire apparaître les packages Dynamo.
> 2. Les packages Dynamo. En sélectionnant l’un d’entre eux, vous verrez sa version actuelle et une description de son contenu.
> 3. Sélectionnez la version du package dont vous avez besoin et cliquez sur installer. Cela permet d’installer un package pour le projet spécifique sur lequel vous travaillez. Puisque nous utilisons la dernière version stable de Dynamo (version 1.3), choisissez la version du package correspondante.

Pour ajouter manuellement un package téléchargé à partir du navigateur, ouvrez le gestionnaire des références à partir de l’explorateur de solutions et recherchez le package.

![Gestionnaire des références](images/vs-manual-dynamo-package.jpg)

> 1. Cliquez avec le bouton droit de la souris sur `References` et sélectionnez `Add Reference`.
> 2. Sélectionnez `Browse` pour accéder à l’emplacement du package.

Maintenant que Visual Studio est configuré correctement et que nous avons réussi à ajouter un `.dll` à Dynamo, nous disposons d’une base solide pour les concepts à venir. Ce n’est que le début, alors poursuivez votre lecture pour en savoir plus sur la création d’un nœud personnalisé.
