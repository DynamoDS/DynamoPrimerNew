# Étude de cas Zero-Touch : nœud grille

Avec un projet Visual Studio opérationnel, nous allons voir comment générer un nœud personnalisé qui crée une grille rectangulaire de cellules. Bien que nous puissions créer ce nœud avec plusieurs nœuds standard, il s’agit d’un outil utile qui peut être facilement contenu dans un nœud Zero-Touch. Contrairement aux lignes de grille, les cellules peuvent être mises à l’échelle autour de leurs points centraux, interrogées pour leurs sommets d’angle ou créées dans des faces.

Cet exemple aborde quelques-unes des caractéristiques et des concepts à prendre en compte lors de la création d’un nœud Zero-Touch. Après avoir générer le nœud personnalisé et l’avoir ajouté à Dynamo, assurez-vous de consulter la page Aller plus loin avec Zero-Touch pour obtenir plus de détails sur les valeurs d’entrée par défaut, le renvoi de valeurs multiples, la documentation, les objets, l’utilisation des types de géométrie Dynamo et les migrations.

![Graphique de grille rectangulaire](images/cover-image.jpg)

### Noeud de grille rectangulaire personnalisé <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

Pour commencer à générer le nœud de grille, créez un nouveau projet de bibliothèque de classes Visual Studio. Reportez-vous à la page Mise en route pour obtenir un guide détaillé de la configuration d’un projet.

![Création d’un projet dans Visual Studio](images/vs-new-project-1.jpg)

![Configuration d’un nouveau projet dans Visual Studio](images/vs-new-project-2.jpg)

> 1. Choisissez `Class Library` comme type de projet
> 2. Nommez le projet `CustomNodes`

Comme nous allons créer une géométrie, nous devons référencer le package NuGet approprié. Installez le package ZeroTouchLibrary à partir du gestionnaire de package NuGet. Ce package est nécessaire pour l’instruction `using Autodesk.DesignScript.Geometry;`.

![Package ZeroTouchLibrary](images/vs-nugetpackage.jpg)

> 1. Recherchez le package ZeroTouchLibrary.
> 2. Nous utiliserons ce nœud dans la version actuelle de Dynamo Studio, qui est la 1.3. Sélectionnez la version du package qui correspond à cette version.
> 3. Notez que nous avons également renommé le fichier de classe en `Grids.cs`.

Ensuite, nous devons établir un espace de noms et une classe dans lesquels la méthode RectangularGrid sera intégrée. Le nœud sera nommé dans Dynamo en fonction des noms de la méthode et de la classe. Nous n’avons pas besoin de copier ceci dans Visual Studio pour le moment.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
        //The method for creating a rectangular grid will live in here
        }
    }
}
```

> `Autodesk.DesignScript.Geometry;` fait référence au fichier ProtoGeometry.dll du package et ZeroTouchLibrary `System.Collections.Generic` est nécessaire pour créer des listes

Nous pouvons maintenant ajouter la méthode pour dessiner les rectangles. Le fichier de classe doit ressembler à ceci et peut être copié dans Visual Studio.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                }
            }
            return pList;
        }
    }
}
```

Si le projet ressemble à ceci, essayez de générer le `.dll`.

![Génération d’un DLL](images/vs-grids.jpg)

> 1. Choisissez Générer > Générer une solution

Vérifiez si le dossier `bin` du projet contient un `.dll`. Si la génération est réussie, nous pouvons ajouter le `.dll` à Dynamo.

![Nœuds personnalisés dans Dynamo](images/RectangularGrid-Dynamo.jpg)

> 1. Le nœud RectangularGrids personnalisé dans la bibliothèque Dynamo
> 2. Le nœud personnalisé sur la zone de dessin
> 3. Le bouton Ajouter permet d’ajouter le `.dll` à Dynamo

### Modifications de nœuds personnalisé <a href="#custom-node-modifications" id="custom-node-modifications"></a>

Dans l’exemple ci-dessus, nous avons créé un nœud assez simple qui définit essentiellement la méthode `RectangularGrids`. Cependant, nous pouvons vouloir créer des infobulles pour les ports d’entrée ou donner au nœud un résumé comme les nœuds Dynamo standard. L’ajout de ces fonctionnalités aux nœuds personnalisés les rend plus faciles à utiliser, en particulier si un utilisateur souhaite les rechercher dans la bibliothèque.

![Info-bulle d’entrée](images/nodemodification.png)

> 1. Une valeur d’entrée par défaut
> 2. Une Info-bulle pour l’entrée xCount

Le nœud RectangularGrid a besoin de certaines de ces caractéristiques de base. Dans le code ci-dessous, nous avons ajouté des descriptions des ports d’entrée et de sortie, un résumé et des valeurs d’entrée par défaut.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }
            return pList;
        }
    }
}
```

* Donnez aux entrées des valeurs par défaut en attribuant des valeurs aux paramètres de la méthode : `RectangularGrid(int xCount = 10, int yCount = 10)`.
* Créez des info-bulles d’entrée et de sortie, des mots-clés de recherche et un résumé avec la documentation XML précédée de `///`.

Pour ajouter des info-bulles, vous devez disposer d’un fichier XML dans le répertoire du projet. Un `.xml` peut être généré automatiquement par Visual Studio en activant l’option.

![Activation de la documentation XML](images/vs-xml.jpg)

> 1. Activez ici le fichier de documentation XML et indiquez un chemin d’accès au fichier. Cela génère un fichier XML.

Et voilà ! Nous avons créé un nouveau nœud doté de plusieurs fonctions standard. Le chapitre suivant, Bases de Zero-Touch, décrit plus en détail le développement des nœuds Zero-Touch et les problèmes à prendre en compte.
