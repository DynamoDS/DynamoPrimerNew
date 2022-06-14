# Listes à n dimensions

Pour compliquer la tâche, vous allez ajouter encore plus de niveaux à la hiérarchie. La structure des données peut s'étendre au-delà d'une liste bidimensionnelle de listes. Étant donné que les listes sont des éléments dans Dynamo, vous pouvez créer des données avec autant de dimensions que possible.

L'analogie utilisée ici est celle des poupées russes. Chaque liste peut être considérée comme un conteneur contenant plusieurs éléments. Chaque liste possède ses propres propriétés et est considérée comme son propre objet.

![Poupées](../images/5-4/4/145493363\_fc9ff5164f\_o.jpg)

> L’ensemble de poupées russes (photo de [Zeta](https://www.flickr.com/photos/beppezizzi/145493363)) est une analogie pour les listes à n dimensions. Chaque couche représente une liste et chaque liste contient des éléments. Dans le cas de Dynamo, chaque conteneur peut contenir plusieurs conteneurs (représentant les éléments de chaque liste).

Les listes à n dimensions sont difficiles à expliquer visuellement, mais vous allez découvrir dans ce chapitre quelques exercices axés sur des listes de plus de deux dimensions.

### Mappage et combinaisons

Le mappage est sans doute la partie la plus complexe de la gestion des données dans Dynamo, et est particulièrement utile lorsque vous travaillez avec des hiérarchies de listes complexes. Grâce à la série d'exercices ci-dessous, vous allez découvrir quand utiliser le mappage et les combinaisons à mesure que les données deviennent multidimensionnelles.

Les nœuds **List.Map** et **List.Combine** sont présentés dans la section précédente. Dans le dernier exercice ci-dessous, vous allez utiliser ces nœuds sur une structure de données complexe.

## Exercice - Listes 2D - Basique

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-4/4/n-Dimensional-Lists.zip" %}

Cet exercice est le premier d'une série de trois exercices axés sur l'articulation de la géométrie importée. Chaque partie de cette série d'exercices va accroître la complexité de la structure des données.

![ Exercise](<../images/5-4/4/n-dimensional lists - 2d lists basic 01.jpg>)

> 1. Commencez par le fichier .sat dans le dossier des fichiers d'exercice. Vous pouvez sélectionner ce fichier à l'aide du nœud **File Path**.
> 2. Avec **Geometry.ImportFromSAT**, la géométrie est importée dans l'aperçu Dynamo en tant que deux surfaces.

Dans le cadre de cet exercice, utilisez l'une des surfaces pour faire simple.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 02.jpg>)

> 1. Sélectionnez l'index de 1 pour sélectionner la surface supérieure. Pour ce faire, utilisez le nœud **List.GetItemAtIndex**.
> 2. Désactivez l’aperçu de la géométrie à partir de l’aperçu de **Geometry.ImportFromSAT**.

L'étape suivante consiste à diviser la surface en une grille de points.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 03.jpg>)

> 1\. À l’aide d’un nœud **Code Block**, insérez les deux lignes de code suivantes : `0..1..#10;` `0..1..#5;`
>
> 2\. Avec le nœud **Surface.PointAtParameter**, connectez les deux valeurs de Code Block à u et _v_. Définissez la _combinaison_ de ce nœud sur _"Produit vectoriel"_.
>
> 3\. La sortie révèle la structure des données, également visible dans l'aperçu Dynamo.

Ensuite, utilisez les points de la dernière étape pour générer dix courbes le long de la surface.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 04.jpg>)

> 1. Pour voir comment la structure de données est organisée, connectez un noeud **NurbsCurve.ByPoints** à la sortie de **Surface.PointAtParameter**.
> 2. Vous pouvez désactiver l’aperçu du nœud **List.GetItemAtIndex** pour obtenir un résultat plus clair.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 05.jpg>)

> 1. Un noeud **List.Transpose** de base permet d'inverser les colonnes et les lignes d'une liste de listes.
> 2. Lorsque vous connectez la sortie de **List.Transpose** à **NurbsCurve.ByPoints**, vous obtenez cinq courbes placées horizontalement sur la surface.
> 3. Vous pouvez désactiver l’aperçu du nœud **NurbsCurve.ByPoints** à l’étape précédente pour obtenir le même résultat dans l’image.

## Exercice - Listes 2D - Avancé

Passons aux choses sérieuses. Imaginez que vous souhaitiez effectuer une opération sur les courbes créées lors de l'exercice précédent. Vous devrez peut-être lier ces courbes à une autre surface et effectuer un lissage entre elles. Pour ce faire, il convient d'accorder une plus grande attention à la structure des données, même si la logique sous-jacente est la même.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 01.jpg>)

> 1. Commencez par une étape de l'exercice précédent : isolez la surface supérieure de la géométrie importée grâce au nœud **List.GetItemAtIndex**.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 02.jpg>)

> 1. À l'aide du noeud **Surface.Offset**, décalez la surface par une valeur de _10_.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 03.jpg>)

> 1. De la même façon que dans l’exercice précédent, définissez un nœud _Code Block_ avec les deux lignes de code suivantes : `0..1..#10;` `0..1..#5;`
> 2. Connectez ces sorties à deux nœuds **Surface.PointAtParameter** dont la _combinaison_ est définie sur _"Produit vectoriel"_. L'un de ces nœuds est connecté à la surface d'origine, tandis que l'autre est connecté à la surface décalée.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 04.jpg>)

> 1. Désactivez l’aperçu de ces surfaces.
> 2. Comme dans l'exercice précédent, connectez les sorties à deux nœuds **NurbsCurve.ByPoints**. Le résultat affiche les courbes correspondant à deux surfaces.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 05.jpg>)

> 1. Le noeud **List.Create** vous permet de combiner les deux jeux de courbes en une liste de listes.
> 2. La sortie affiche deux listes contenant chacune dix éléments, représentant chaque ensemble de connexions de courbes NURBS.
> 3. Grâce au noeud **Surface.ByLoft**, vous pouvez visualiser cette structure de données. Le nœud lisse toutes les courbes de chaque sous-liste.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 06.jpg>)

> 1. Désactivez l’aperçu du nœud **Surface.ByLoft** à l’étape précédente.
> 2. Si vous utilisez le noeud **List.Transpose**, n'oubliez pas qu'il permet de retourner toutes les colonnes et les lignes. Ce nœud convertit deux listes de dix courbes en dix listes de deux courbes. Chaque courbe NURBS est désormais liée à la courbe voisine sur l'autre surface.
> 3. Le noeud **Surface.ByLoft** vous permet d'obtenir une structure nervurée.

Vous allez ensuite découvrir un autre processus pour atteindre ce résultat.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 07.jpg>)

> 1. Avant de commencer, désactivez l’aperçu de **Surface.ByLoft** à l’étape précédente pour éviter toute confusion.
> 2. Le noeud **List.Combine** constitue une alternative au noeud **List.Transpose**. Il exécute un _"combinateur"_ sur chaque sous-liste.
> 3. Dans ce cas, vous utilisez **List.Create** en tant que _"combinateur"_ qui va créer une liste de chaque élément dans les sous-listes.
> 4. Le nœud **Surface.ByLoft** vous permet d'obtenir les mêmes surfaces que lors de l'étape précédente. L'option Transposer est plus facile à utiliser dans ce cas, mais lorsque la structure de données devient encore plus complexe, le noeud **List.Combine** s'avère plus fiable.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 08.jpg>)

> 1. Si vous souhaitez inverser l’orientation des courbes dans la structure nervurée, utilisez un nœud **List.Transpose** avant de les connecter à **NurbsCurve.ByPoints**. Cette action permet d'inverser les colonnes et les lignes et d'obtenir 5 nervures horizontales.

## Exercice - Listes 3D

Vous allez désormais aller encore un peu plus loin. Dans cet exercice, vous allez travailler avec les deux surfaces importées et créer une hiérarchie de données complexe. L'objectif est néanmoins d'effectuer la même opération avec la même logique sous-jacente.

Commencez par le fichier importé de l'exercice précédent.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 01.jpg>)

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 02.jpg>)

> 1. Comme dans l'exercice précédent, utilisez le nœud **Surface.Offset** pour effectuer un décalage d'une valeur de _10_.
> 2. Dans la sortie, vous pouvez voir que le nœud de décalage a créé deux surfaces.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 03.jpg>)

> 1. De la même façon que dans l’exercice précédent, définissez un nœud **Code Block** avec les deux lignes de code suivantes : `0..1..#20;` `0..1..#20;`
> 2. Connectez ces sorties à deux nœuds **Surface.PointAtParameter** dont la combinaison est définie sur _"Produit vectoriel"_. L'un de ces nœuds est connecté aux surfaces d'origine, tandis que l'autre est connecté aux surfaces décalées.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 04.jpg>)

> 1. Comme dans l'exercice précédent, connectez les sorties à deux nœuds **NurbsCurve.ByPoints**.
> 2. Observez la sortie de **NurbsCurve.ByPoints** : il s’agit d’une liste de deux listes, ce qui est plus complexe que l’exercice précédent. Étant donné que les données sont classées par la surface sous-jacente, un autre niveau a été ajouté à la structure des données.
> 3. Les choses deviennent plus complexes dans le nœud **Surface.PointAtParameter**. Dans ce cas, vous avez une liste de listes de listes.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 05.jpg>)

> 1. Avant de continuer, désactivez l’aperçu des surfaces existantes.
> 2. À l’aide du nœud **List.Create**, fusionnez les courbes NURBS en une structure de données, créant ainsi une liste de listes de listes.
> 3. Lorsque vous connectez un nœud **Surface.ByLoft**, vous obtenez une version des surfaces d'origine, car elles restent toutes dans leur propre liste telle qu'elle a été créée à partir de la structure de données d'origine.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 06.jpg>)

> 1. Dans l'exercice précédent, le fichier **List.Transpose** a été utilisé pour créer une structure nervurée. Ce ne sera pas possible ici. Vous devez utiliser une transposition sur une liste bidimensionnelle. Étant donné que vous avez une liste tridimensionnelle, une opération de "basculement des colonnes et des lignes" ne fonctionnera pas aussi facilement. N'oubliez pas que les listes sont des objets. Par conséquent, le noeud **List.Transpose** inverse vos listes sans les sous-listes, mais n'inverse pas les courbes NURBS d'une liste plus bas dans la hiérarchie.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 07.jpg>)

> 1. Le noeud **List.Combine** fonctionne mieux ici. Lorsque vous obtenez des structures de données plus complexes, utilisez les nœuds **List.Map** et **List.Combine**.
> 2. L'utilisation du noeud **List.Create** comme _"combinateur"_ vous permet de créer une structure de données plus appropriée.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 08.jpg>)

> 1. La structure de données doit toujours être transposée à une étape inférieure de la hiérarchie. Pour ce faire, utilisez **List.Map**. Ce noeud fonctionne comme **List.Combine**, sauf qu'il utilise une liste d'entrées, au lieu de deux listes ou plus.
> 2. La fonction appliquée à **List.Map** est **List.Transpose**, permettant d'inverser les colonnes et les lignes des sous-listes dans la liste principale.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 09.jpg>)

> 1. Enfin, vous pouvez lisser les courbes NURBS avec une hiérarchie de données correcte. Vous obtenez ainsi une structure nervurée.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 10.jpg>)

> 1. Ajoutez de la profondeur à la géométrie à l’aide d’un nœud **Surface.Thicken** avec les paramètres d’entrée, comme indiqué.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 11.jpg>)

> 1. Il peut être utile d’ajouter un support de surface à deux de ces structures. Ajoutez un autre nœud **Surface.ByLoft** et utilisez la première sortie de **NurbsCurve.ByPoints** d’une étape précédente comme entrée.
> 2. Comme l’aperçu est de plus en plus encombré, désactivez l’aperçu de ces nœuds en cliquant avec le bouton droit sur chacun d’eux et désélectionnez l’option « Aperçu » pour mieux voir le résultat.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 12.jpg>)

> 1. L'articulation se termine lorsque vous avez épaissi les surfaces sélectionnées.

Ce n'est pas la chaise à bascule la plus confortable au monde, mais elle contient de nombreuses données.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 13.jpg>)

Pour la dernière étape, inversez la direction des éléments striés. Dans l'exercice précédent, vous avez utilisé l'option Transposer. La procédure à suivre est similaire ici.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 14.jpg>)

> 1. Étant donné qu’un niveau supplémentaire a été ajouté à la hiérarchie, vous devez utiliser **List.Map** avec une fonction **List.Tranpose** pour modifier la direction des courbes NURBS.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 15.jpg>)

> 1. Augmentez le nombre de girons de façon à pouvoir modifier le nœud **Code Block** comme suit : `0..1..#20;` `0..1..#30;`

La première version de la chaise à bascule était lisse. Le second modèle propose une chaise à bascule sportive originale.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 16.jpg>)
