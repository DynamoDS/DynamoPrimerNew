# Données

Les données sont les éléments des programmes. Elles circulent par l'intermédiaire de fils, fournissant des entrées pour les nœuds où elles sont traitées dans une nouvelle forme de données de sortie. Vous trouverez ci-dessous des informations sur la définition des données, leur structure et leur utilisation dans Dynamo.

## En quoi consistent les données ?

Les données sont un ensemble de valeurs de variables qualitatives ou quantitatives. La forme de données la plus simple est celle des nombres tels que `0`, `3.14` ou `17`. Cependant, les données peuvent également être de différents types : une variable représentant des nombres changeants (`height`), des caractères (`myName`), une géométrie (`Circle`) ou une liste d’éléments de données (`1,2,3,5,8,13,...`).

Dans Dynamo, ajoutez/transmettez des données aux ports d’entrée des nœuds. Vous pouvez avoir des données sans actions, mais vous avez besoin de données pour traiter les actions que les nœuds représentent. Lorsque vous avez ajouté un nœud à l’espace de travail, si aucune entrée n’est fournie, le résultat est une fonction, et non le résultat de l’action elle-même.

![Données et actions](../images/5-3/1/data-whatisdata.jpg)

> 1. Données simples
> 2. Exécution réussie des données et des actions (nœud A)
> 3. Une action (nœud A) sans entrées de données renvoie une fonction générique

### Valeur nulle –·Absence de données

Attention aux valeurs nulles. Le type `'null'` représente l’absence de données. Bien qu'il s'agisse d'un concept abstrait, vous risquez de rencontrer ce phénomène lorsque vous travaillez avec la programmation visuelle. Si une action ne crée pas de résultat valide, le nœud renvoie une valeur nulle.

Il est crucial de tester les valeurs nulles et de les supprimer de la structure de données pour créer des programmes robustes.

| Icône                                                  | Nom/Syntaxe   | Entrées | Sorties |
| ----------------------------------------------------- | ------------- | ------ | ------- |
| ![](../images/5-3/1/data-objectIsNull.jpg) | Object.IsNull | obj    | bool    |

### Structures de données

Lorsque vous concevez des programmes visuels, vous pouvez générer très rapidement beaucoup de données, ce qui exige un moyen de gérer la hiérarchie. Il s'agit du rôle des structures de données, les schémas organisationnels dans lesquels sont stockées les données. Les spécificités des structures de données et leur utilisation varient d'un langage de programmation à un autre.

Dynamo permet de hiérarchiser les données via des listes. Vous allez explorer cela en détail dans les prochains chapitres, mais commençons simplement :

Une liste représente un ensemble d’éléments placés dans une structure de données :

* J’ai cinq doigts (_éléments_) sur la main (_liste_).
* Il y a dix maisons (_éléments_) dans ma rue (_liste_).

![Liste déroulante](../images/5-3/1/data-datastructures.jpg)

> 1. Un nœud **Number Sequence** définit une liste de nombres en utilisant les entrées _start_, _amount_ et _step_. Ces nœuds permettent de créer deux listes distinctes de dix nombres, l’une qui va de _100 à 109_ et l’autre qui va de _0 à 9_.
> 2. Le nœud **List.GetItemAtIndex** sélectionne un élément dans une liste à un index spécifique. Lorsque vous choisissez _0_, le premier élément de la liste est affiché (_100_ dans ce cas).
> 3. Si vous appliquez le même processus à la deuxième liste, vous obtenez la valeur _0_, le premier élément de la liste.
> 4. Vous allez maintenant fusionner les deux listes en une seule à l’aide du nœud **List.Create**. Le nœud crée une _liste de listes._ Cette action modifie la structure des données.
> 5. Lorsque vous utilisez de nouveau **List.GetItemAtIndex**, avec un index défini sur _0_, la première liste est répertoriée dans la liste de listes. C’est ce que signifie traiter une liste comme un élément, ce qui est légèrement différent dans d’autres langages de script. Les prochains chapitres vous permettront d’en savoir plus sur la manipulation des listes et la structure des données.

Voici le concept clé à retenir sur la hiérarchie des données dans Dynamo : **en ce qui concerne la structure des données, les listes sont considérées comme des éléments.** En d’autres termes, Dynamo fonctionne avec un processus descendant permettant de comprendre les structures de données. Que cela signifie-t-il ? Découvrez-le grâce à un exemple.

## Exercice : Utilisation de données pour créer une chaîne de cylindres

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-3/1/Building Blocks of Programs - Data.dyn" %}

Dans ce premier exemple, vous allez assembler un cylindre en forme de coque qui traverse la hiérarchie de la géométrie abordée dans cette section.

### Partie I : Configuration du graphique pour un cylindre avec des paramètres modifiables

1. Ajoutez **Point.ByCoordinates :** après l’ajout du nœud dans la zone de dessin, un point apparaît à l’origine de la grille d’aperçu Dynamo. Les valeurs par défaut des entrées _x,y_ et _z_ sont _0,0_, ce qui vous donne un point à cet emplacement.

![](../images/5-3/1/data-exercisestep1.jpg)

2\. **Plan.ByOriginNormal :** l’étape suivante dans la hiérarchie de la géométrie est un plan. Il existe plusieurs façons de construire un plan. Pour l’entrée, utilisez une origine et une normale. L’origine est le nœud de point créé à l’étape précédente.

**Vector.ZAxis :** il s’agit d’un vecteur unifié dans la direction Z. Notez qu’il n’y a pas d’entrées, seulement un vecteur de valeur [0,0,1]. Utilisez-le comme entrée _normal_ pour le nœud **Plane.ByOriginNormal**. Vous obtenez ainsi un plan rectangulaire dans l’aperçu Dynamo.

![](../images/5-3/1/data-exercisestep2.jpg)

3\. **Circle.ByPlaneRadius :** en avançant dans la hiérarchie, vous créez une courbe à partir du plan de l’étape précédente. Après avoir connecté le nœud, un cercle apparaît à l’origine. Le rayon par défaut sur le nœud est de _1_.

![](../images/5-3/1/data-exercisestep3.jpg)

4\. **Curve.Extrude :** permet de faire apparaître cet élément en lui donnant une certaine profondeur et en allant dans la troisième dimension. Ce nœud crée une surface à partir d’une courbe par extrusion. La distance par défaut sur le nœud est _1_ et un cylindre doit apparaître dans la fenêtre.

![](../images/5-3/1/data-exercisestep4.jpg)

5\. **Surface.Thicken :** ce nœud vous donne un solide fermé grâce au décalage de la surface d’une distance donnée et à la fermeture de la forme. La valeur d’épaisseur par défaut est de _1_ et un cylindre en forme de coque s’affiche dans la fenêtre coformément à ces valeurs.

![](../images/5-3/1/data-exercisestep5.jpg)

6\. **Number Slider :** au lieu d’utiliser les valeurs par défaut pour toutes ces entrées, ajoutez un contrôle paramétrique au modèle.

**Domain Edit :** après avoir ajouté le curseur de numérotation à la zone de dessin, cliquez sur le curseur situé en haut à gauche pour afficher les options du domaine.

**Min/Max/Step :** remplacez les valeurs _Min_, _Max_ et _Step_ par _0_, _2_ et _0,01_ respectivement. Ces valeurs servent à contrôler la taille de la géométrie globale.

![](../images/5-3/1/data-exercisestep6.gif)

7\. **Number Slider :** dans toutes les entrées par défaut, copiez et collez ce curseur de numérotation (sélectionnez le curseur, appuyez plusieurs fois sur Ctrl+C, puis sur Ctrl+V), jusqu’à ce que toutes les entrées avec des valeurs par défaut disposent d’un curseur à la place. Certaines valeurs du curseur doivent être supérieures à zéro pour que la définition fonctionne (p.ex., vous devez disposer d’une profondeur d’extrusion pour disposer d’une surface à épaissir).

![](../images/5-3/1/data-exercisestep7a.gif)

![](../images/5-3/1/data-exercisestep7b.gif)

8\. Vous avez maintenant créé un cylindre paramétrique en forme de coque avec ces curseurs. Ajustez certains de ces paramètres et observez la géométrie se mettre à jour dynamiquement dans la fenêtre Dynamo.

![](../images/5-3/1/data-exercisestep8a.gif)

**Number Slider :** pour aller plus loin, ajoutez de nombreux curseurs dans la zone de dessin et nettoyez l’interface de l’outil que vous venez de créer. Cliquez avec le bouton droit de la souris sur un curseur, choisissez « Renommer… » et remplacez chaque curseur par le nom approprié pour son paramètre (Épaisseur, Rayon, Hauteur, etc.).

![](../images/5-3/1/data-exercisestep8bstep.jpg)

### Partie II : Alimentation d’un réseau de cylindres à partir de la partie I

9\. À ce stade, vous avez créé un élément cylindrique épaississant fantastique. Il s'agit d'un seul objet actuellement. Découvrez désormais comment créer un réseau de cylindres liés dynamiquement. Pour ce faire, vous allez créer une liste de cylindres, plutôt que de travailler avec un seul élément.

**Ajout (+) :** votre objectif est d’ajouter une ligne de cylindres à côté du cylindre créé. Si vous souhaitez ajouter un cylindre adjacent au cylindre en cours, vous devez prendre en compte à la fois le rayon du cylindre et l’épaisseur de sa coque. Pour obtenir ce nombre, ajoutez les deux valeurs des curseurs.

![](../images/5-3/1/data-exercisestep9.jpg)

10\. Étant donné que cette étape est plus importante, allez-y étape par étape : l’objectif final consiste à créer une liste de nombres qui définissent les emplacements de chaque cylindre dans une ligne.

![](../images/5-3/1/data-exercisestep10.jpg)

> a. **Multiplication :** tout d’abord, multipliez la valeur de l’étape précédente par 2. La valeur de l’étape précédente représente un rayon et il convient de déplacer le cylindre sur le diamètre complet.
>
> b. **Number Sequence :** créez un réseau de nombres à l’aide de ce nœud. La première entrée est le nœud de _multiplication_ de l’étape précédente vers la valeur _step_. Pour définir la valeur _start_ sur _0,0_, utilisez un nœud _Number_.
>
> c. **Integer Slider :** connectez un curseur d’entier à la valeur _amount_. Cela permet de définir le nombre de cylindres créés.
>
> d. **Sortie :** cette liste affiche la distance déplacée pour chaque cylindre du réseau et est paramétriquement définie par les curseurs d’origine.

11\. Cette étape est relativement simple : connectez la séquence définie à l’étape précédente à l’entrée _x_ du nœud **Point.ByCoordinates** d’origine. Cette action permet de remplacer le curseur _pointX_ que vous pouvez supprimer. Un réseau de cylindres apparaît maintenant dans la fenêtre (assurez-vous que la valeur du curseur d’entier est supérieure à 0).

![](../images/5-3/1/data-exercisestep11.gif)

12\. La chaîne de cylindres est toujours liée dynamiquement à tous les curseurs. Ajustez chaque curseur et observez la définition se mettre à jour.

![](../images/5-3/1/data-exercisestep12.gif)
