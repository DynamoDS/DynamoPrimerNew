# Fonctions

Les fonctions peuvent être créées dans un bloc de code et rappelées ailleurs dans une définition Dynamo. Cela permet de créer un autre calque de contrôle dans un fichier paramétrique et de l'afficher en tant que version de texte d'un nœud personnalisé. Dans ce cas, le bloc de code "parent" est facilement accessible et peut être situé n'importe où sur le graphique. Aucun fil n'est nécessaire.

### Parent

La première ligne comporte le mot clé "def", puis le nom de la fonction, puis les noms des entrées entre parenthèses. Les contreventements définissent le corps de la fonction et renvoient une valeur avec "return =". Les noeuds Code Block qui définissent une fonction ne disposent pas de ports d'entrée ou de sortie, car ils sont appelés à partir d'autres noeuds Code Block.

![](<../images/8-1/4/functions parent def.jpg>)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

### Enfants

Appelez la fonction avec un autre noeud Code Block dans le même fichier en donnant le nom et le même nombre d'arguments. Cela fonctionne comme les nœuds prêts à l'emploi de votre bibliothèque.

![](<../images/8-1/4/functions children call def.jpg>)

```
FunctionName(in1,in2);
```

## Exercice : Sphère par Z

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/8-1/4/Functions_SphereByZ.dyn" %}

Dans cet exercice, vous allez créer une définition générique permettant d'obtenir des sphères à partir d'une liste de points d'entrée. Le rayon de ces sphères est défini par la propriété Z de chaque point.

Commencez par une plage de dix valeurs allant de 0 à 100. Connectez-les à un nœud **Point.ByCoordinates** afin de créer une ligne diagonale.

![](<../images/8-1/4/functions - exercise - 01.jpg>)

Créez un nœud **Code Block** et introduisez votre définition.

![](<../images/8-1/4/functions - exercise - 02.jpg>)

> 1. Utilisez les lignes de code suivantes :
>
>    ```
>    def sphereByZ(inputPt)
>    {
>    
>    };
>    ```
>
> Le terme _inputPt_ est le nom donné pour représenter les points qui contrôlent la fonction. Pour l'instant, la fonction ne fait rien, mais vous allez la construire dans les étapes à venir.

![](<../images/8-1/4/functions - exercise - 03.jpg>)

> 1. L’ajout de la fonction **Code Block** permet de placer un commentaire et une variable _sphereRadius_ qui interroge la position _Z_ de chaque point. N'oubliez pas que _inputPt.Z_ n'a pas besoin de parenthèses comme méthode. Il s'agit d'une _requête_ des propriétés d'un élément existant, donc aucune entrée n'est nécessaire :
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

![](<../images/8-1/4/functions - exercise - 04.jpg>)

> 1. Rappelez-vous la fonction créée dans un autre nœud **Code Block**. Si vous double-cliquez sur la zone de dessin pour créer un _Code Block_ et que vous tapez _sphereB_, Dynamo suggère la fonction _sphereByZ_ que vous avez définie. Votre fonction a été ajoutée à la bibliothèque Intellisense. Bien.

![](<../images/8-1/4/functions - exercise - 05.jpg>)

> 1. Vous allez maintenant appeler la fonction et créer une variable appelée _Pt_ afin de connecter les points créés dans les étapes précédentes :
>
>    ```
>    sphereByZ(Pt)
>    ```
> 2. La sortie ne contient que des valeurs nulles. Pourquoi ? Lorsque vous avez défini la fonction, vous avez calculé la variable _sphereRadius_, mais vous n'avez pas défini ce que la fonction doit _renvoyer_ en tant que _sortie_. Vous pourrez résoudre ce problème à l'étape suivante.

![](<../images/8-1/4/functions - exercise - 06.jpg>)

> 1. Une étape importante consiste à définir la sortie de la fonction en ajoutant la ligne `return = sphereRadius;` à la fonction _sphereByZ_.
> 2. La sortie du nœud Code Block indique désormais les coordonnées Z de chaque point.

Modifiez la fonction _Parent_ pour créer des sphères réelles.

![](<../images/8-1/4/functions - exercise - 07.jpg>)

> 1. Commencez par définir une sphère avec la ligne de code suivante : `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`
> 2. Ensuite, modifiez la valeur renvoyée pour la définir comme _sphere_ au lieu de _sphereRadius_ : `return = sphere;` Vous obtenez ainsi des sphères géantes dans l’aperçu de Dynamo.

![](<../images/8-1/4/functions - exercise - 08.jpg>)

> 1\. Pour modifier la taille de ces sphères, mettez à jour la valeur sphereRadius en ajoutant un séparateur : `sphereRadius = inputPt.Z/20;`Les sphères distinctes sont maintenant visibles et la relation entre le rayon et la valeur Z devient plus claire.

![](<../images/8-1/4/functions - exercise - 09.jpg>)

> 1. Sur le nœud **Point.ByCoordinates**, remplacez la liaison Liste la plus courte par Produit cartésien afin de créer une grille de points. La fonction _sphereByZ_ reste pleinement effective, de sorte que tous les points créent des sphères avec des rayons fondés sur des valeurs Z.

![](<../images/8-1/4/functions - exercise - 10.jpg>)

> 1. Pour tâter le terrain, connectez simplement la liste de nombres d'origine à l'entrée X de **Point.ByCoordinates**. Vous obtenez ainsi un cube de sphères.
> 2. Remarque : si le calcul sur votre ordinateur prend beaucoup de temps, essayez de remplacer _nº 10_ par un autre élément, _nº 5_ par exemple.

N'oubliez pas que la fonction _sphereByZ_ créée est générique. Vous pouvez donc rappeler l'hélice d'une leçon précédente et lui appliquer la fonction.

![](<../images/8-1/4/functions - exercise - 11.jpg>)

Dernière étape : déterminez le rapport des rayons avec un paramètre défini par l'utilisateur. Pour ce faire, vous devez créer une entrée pour la fonction et remplacer également le séparateur _20_ par un paramètre.

![](<../images/8-1/4/functions - exercise - 12.jpg>)

> 1. Remplacez la définition de _sphereByZ_ par :
>
>    ```
>    def sphereByZ(inputPt,radiusRatio)
>    {
>    //get Z Value, use it to drive radius of sphere
>    sphereRadius=inputPt.Z/radiusRatio;
>    //Define Sphere Geometry
>    sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>    //Define output for function
>    return sphere;
>    };
>    ```
> 2. Mettez à jour les nœuds**Code Block** enfant en ajoutant une variable ratio à l’entrée : `sphereByZ(Pt,ratio);` Connectez un curseur à la nouvelle entrée du nœud **Code Block** et modifiez la taille des rayons en fonction du rapport des rayons.
