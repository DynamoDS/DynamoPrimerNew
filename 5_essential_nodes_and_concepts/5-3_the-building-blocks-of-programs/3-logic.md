# Logique

La **logique**, ou plus spécifiquement, la **logique conditionnelle**, vous permet de spécifier une action ou un jeu d'actions en fonction d'un test. Après avoir évalué le test, vous obtenez une valeur booléenne représentant `True` ou `False` que vous pouvez utiliser pour contrôler le flux du programme.

### Valeurs booléennes

Les variables numériques peuvent stocker un intervalle entier de nombres différents. Les variables booléennes ne peuvent stocker que deux valeurs appelées True ou False, Yes ou No, 1 ou 0. Les valeurs booléennes sont rarement utilisées pour effectuer des calculs en raison de leur intervalle limité.

### Instructions conditionnelles

L'instruction "If" est un concept clé de la programmation : "If" cet _élément_ a la valeur True, voici le _résultat_, sinon _autre chose_ se produit. L'action résultant de l'instruction est déterminée par une valeur booléenne. Il existe plusieurs méthodes pour définir une instruction "If" dans Dynamo :

| Icône | Nom (Syntaxe) | Entrées | Sorties |
| ----------------------------------------------- | ------------------------- | ----------------- | ------- |
| ![](<../images/5-3/3/If.jpg>) | If (**If**) | test, true, false | résultat |
| ![](../images/5-3/3/Formula.jpg) | Formula (**IF(x,y,z)**) | x, y, z | résultat |
| ![](<../images/5-3/3/Code Block.jpg>) | Code Block (**(x?y:z);**) | x? y, z | résultat |

Voici un bref exemple de chacun de ces trois nœuds en action à l’aide de l’instruction conditionnelle « If ».

Dans cette image, la _valeur booléenne_ est définie sur _True_, ce qui signifie que le résultat est une chaîne indiquant : _"voici le résultat si True"._ Les trois nœuds qui créent l'instruction _If_ fonctionnent de la même manière ici.

![](<../images/5-3/3/logic - conditional statements 01 false.jpg>)

Là encore, les nœuds fonctionnent de la même façon. Si la _valeur booléenne_ est définie sur _False_, le résultat est le nombre _Pi_, tel que défini dans l'instruction _If_ d'origine.

![ x](<../images/5-3/3/logic - conditional statements 02 true.jpg>)

## Exercice : Logique et géométrie

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-3/3/Building Blocks of Programs - Logic.dyn" %}

### Partie I : Filtrage d’une liste

1. Utilisez la logique pour séparer une liste de nombres en une liste de nombres pairs et une liste de nombres impairs.

![](<../images/5-3/3/logic - exercise part I-01.jpg>)

> a. **Number Range :** ajoutez un intervalle de nombres dans la zone de dessin.
>
> b. **Number :** ajoutez trois nœuds numériques dans la zone de dessin. La valeur de chaque nœud numérique doit être : _0.0_ pour _start_, _10.0_ pour _end_ et _1.0_ pour _step_.
>
> c. **Sortie :** le résultat est une liste de 11 chiffres compris entre 0 et 10.
>
> d. **Module (%) :** **Number Range** en _x_ et _2.0_ en _y_. Cela permet de calculer le reste pour chaque nombre de la liste divisé par 2. La sortie de cette liste vous donne une liste de valeurs alternant entre 0 et 1.
>
> e. **Test d’égalité (==) :** permet d’ajouter un test d’égalité à la zone de dessin. Connectez la sortie du _module_ à l'entrée _x_ et _0.0_ à l'entrée _y_.
>
> f. **Watch :** la sortie du test d’égalité est une liste de valeurs alternant entre true et false. Il s'agit des valeurs utilisées pour séparer les éléments de la liste. _0_ (ou _true_) représente des nombres pairs et _1_ (ou _false_) des nombres impairs.
>
> g. **List.FilterByBoolMask :** ce nœud filtre les valeurs dans deux listes différentes en fonction de la valeur booléenne d’entrée. Connectez le noeud _Number Range_ d'origine à l'entrée _list_ et la sortie du _test d'égalité_ à l'entrée _mask_. La sortie _in_ représente des valeurs True, tandis que la sortie _out_ représente des valeurs False.
>
> h. **Watch :** le résultat est une liste de nombres pairs et une liste de nombres impairs. Vous avez utilisé des opérateurs logiques pour séparer des listes en modèles.

### Partie II : De la logique à la géométrie

En partant de la logique établie dans le premier exercice, appliquez cette configuration dans une opération de modélisation.

2\. Partez de l'exercice précédent avec les mêmes nœuds. Les seules exceptions (outre la modification du format) sont les suivantes :

![](<../images/5-3/3/logic - exercise part II-01.jpg>)

> a. Utilisez un nœud **Sequence** avec ces valeurs d’entrée.
>
> b. L’entrée de liste in de **List.FilterByBoolMask** est déconnectée. Pour l'instant, mettez ces nœuds de côté. Vous les utiliserez plus tard dans l'exercice.

3\. Commencez par créer un groupe distinct de graphiques, comme illustré dans l’image ci-dessus. Ce groupe de nœuds représente une équation paramétrique permettant de définir une courbe de ligne. Remarques :

![](<../images/5-3/3/logic - exercise part II-02.jpg>)

> a. Le premier nœud **Number Slider** représente la fréquence de l’onde. Il doit avoir une valeur minimale de 1, une valeur maximale de 4 et un pas de 0,01.
>
> b. Le second nœud **Number Slider** représente l’amplitude de l’onde. Il doit avoir une valeur minimale de 0, une valeur maximale de 1 et un pas de 0,01.
>
> c. **PolyCurve.ByPoints :** si vous copiez le diagramme de nœud ci-dessus, vous obtenez une courbe sinusoïdale dans la fenêtre d’aperçu Dynamo.

Méthode utilisée ici pour les entrées : utilisez des nœuds Number pour obtenir davantage de propriétés statiques et des curseurs de numérotation sur les valeurs plus flexibles. Conservez l'intervalle de nombres d'origine défini au début de cette étape. Toutefois, la courbe sinusoïdale créée ici doit garder une certaine flexibilité. Vous pouvez déplacer ces curseurs pour observer la fréquence et l'amplitude de la courbe se mettre à jour.

![](<../images/5-3/3/logic - exercise part II-03.gif>)

4\. Vous allez désormais passer à la définition. Examinez le résultat final pour pouvoir référencer ce que vous obtenez. Les deux premières étapes sont effectuées séparément, mais vous devez maintenant les connecter. Utilisez la courbe sinusoïdale de base pour déterminer l'emplacement des composants de zipper, et utilisez la logique True/False pour alterner entre les petites boîtes et les grandes boîtes.

![](<../images/5-3/3/logic - exercise part II-04.jpg>)

> a. **Math.RemapRange :** à l’aide de la séquence de nombres créée à l’étape 2, créez une nouvelle série de nombres en remappant l’intervalle. Les nombres d'origine de l'étape 1 sont compris entre 0 et 100. Ces nombres sont compris entre 0 et 1, respectivement par les entrées _newMin_ et _newMax_.

5\. Créez un nœud **Curve.PointAtParameter**, puis connectez la sortie **Math.RemapRange** de l’étape 4 en tant qu’entrée _param_.

![](<../images/5-3/3/logic - exercise part II-05.jpg>)

Cette étape permet de créer des points le long de la courbe. Remappez les nombres entre 0 et 1, car l'entrée _param_ recherche les valeurs dans cet intervalle. Une valeur de _0_ représente le point de départ, une valeur de _1_ représente les points de fin. Tous les nombres compris entre ces valeurs sont évalués dans l’intervalle _[0,1]_.

6\. Connectez la sortie de **Curve.PointAtParameter** à **List.FilterByBoolMask** pour séparer la liste des index impairs et pairs.

![](<../images/5-3/3/logic - exercise part II-06.jpg>)

> a. **List.FilterByBoolMask :** connectez **Curve.PointAtParameter** de l’étape précédente à l’entrée _list_.
>
> b. **Watch :** le fait d’avoir un nœud Watch pour _in_ et un nœud Watch pour _out_ indique que deux listes représentent des index pairs et des index impairs. Ces points sont ordonnés de la même façon sur la courbe, illustrée à l'étape suivante.

7\. Vous allez maintenant utiliser le résultat de la sortie de **List.FilterByBoolMask** à l’étape 5 pour générer des géométries avec des tailles en fonction de leurs index.

**Cuboid.ByLengths :** recréez les connexions illustrées à l'image ci-dessus pour obtenir un zipper le long de la courbe sinusoïdale. Ici, un cuboïde ne représente qu'une boîte, et vous définissez sa taille en fonction du point de courbe au centre de la boîte. La logique de la division paire/impaire doit maintenant être claire dans le modèle.

![](<../images/5-3/3/logic - exercise part II-07.jpg>)

> a. Liste de cuboïdes à des index pairs.
>
> b. Liste de cuboïdes à des index impairs.

Voilà ! Vous venez de programmer un processus de définition de cotes de géométrie en fonction de l’opération logique présentée dans cet exercice.
