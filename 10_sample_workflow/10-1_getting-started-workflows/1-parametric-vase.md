---
description: suggested exercise
---

# Vase paramétrique

La création d’un vase paramétrique est un excellent moyen d’apprendre à utiliser Dynamo.

Ce workflow vous apprendra à effectuer les actions suivantes :

* Utiliser les curseurs de numérotation pour contrôler les variables de votre conception.
* Créer et modifier des éléments géométriques à l’aide de nœuds.
* Visualiser les résultats de la conception en temps réel.

![](../images/10-1/1/vase1(3).gif)

## Définition des objectifs

Avant de se lancer dans Dynamo, concevons notre vase.

Imaginez que vous allez concevoir un vase en argile selon les pratiques de fabrication utilisées par les céramistes. Les céramistes utilisent normalement un tour de potier pour fabriquer des vases cylindriques. En appliquant une pression sur différentes hauteurs du vase, ils peuvent ainsi en modifier la forme et créer différents modèles.

Vous allez utiliser une méthodologie similaire pour définir notre vase. Vous allez créer 4 cercles à différentes hauteurs et de différents rayons, puis lisser ces cercles pour créer une surface.

![](../images/10-1/1/vase2.png)

## Mise en route

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d’exemple dans l’annexe.

{% file src="../datasets/10-1/1/DynamoSampleWorkflow-vase.dyn" %}

Vous avez besoin des nœuds qui représenteront la séquence d’actions exécutée par Dynamo. Étant donné que vous essayez de créer un cercle, commencez par rechercher un nœud qui réalise cette action. Utilisez le **champ de recherche** ou parcourez la **bibliothèque** pour rechercher le nœud **Circle.ByCenterPointRadius** et l’ajouter à l’espace de travail

![](../images/10-1/1/vase8.png)

> 1. Rechercher > « Circle… »
> 2. Sélectionner > « ByCenterPointRadius »
> 3. Le nœud s’affiche dans l’espace de travail

Examinez ce nœud de plus près. Sur le côté gauche se trouvent les entrées du nœud (_centerPoint_ et _radius_) et sur le côté droit se trouve la sortie du nœud (Circle). Notez que les sorties sont identifiées par une ligne bleu clair. Cela signifie que l’entrée a une valeur par défaut. Pour plus d’informations sur l’entrée, placez le curseur sur son nom. L’entrée _radius_ requiert une double entrée et a une valeur par défaut de 1.

![](../images/10-1/1/vase10.png)

Conservez la valeur par défaut de _centerPoint_, mais ajoutez un nœud **Number Slider** pour contrôler le rayon. Comme vous l’avez fait avec le nœud **Circle.ByCenterPointRadius**, utilisez la bibliothèque pour rechercher **Number Slider** et l’ajouter à votre graphique.

Ce nœud est légèrement différent du nœud précédent, car il contient un curseur. Vous pouvez utiliser l’interface pour modifier la valeur de sortie du curseur.

![](../images/10-1/1/vase13(1).gif)

Le curseur peut être configuré à l’aide du bouton déroulant situé à gauche du nœud. Limitez le curseur à une valeur maximale de 15.

![](../images/10-1/1/vase11.png)

Placez-le à gauche du nœud **Circle.ByCenterPointRadius** et connectez les deux nœuds en sélectionnant la sortie **Number Slider** et en la connectant à l’entrée Radius.

![](../images/10-1/1/vase12.png)

Remplacez également le nom de Number Slider par « Top Radius » en double-cliquant sur le nom du nœud.

![](../images/10-1/1/vase14.png)

## Etapes suivantes

Continuons à ajouter des nœuds et des connexions à notre logique pour définir notre vase.

### Création de cercles de rayons différents

Copiez ces nœuds 4 fois pour que ces cercles définissent la surface et modifiez les noms de Number Slider comme illustré ci-dessous.

![](../images/10-1/1/vase4(1)(1).png)

> 1. Les cercles sont créés à l’aide d’un point central et d’un rayon

### Déplacement de cercles sur la hauteur du vase

Il vous manque un paramètre clé pour votre vase, sa hauteur. Pour contrôler la hauteur du vase, vous allez créer un autre curseur de numérotation. Vous allez aussi ajouter un nœud **Code Block**. Les nœuds Code Block peuvent vous aider à ajouter des extraits de code personnalisés à votre workflow. Vous allez utiliser le nœud Code Block pour multiplier le curseur de hauteur par différents facteurs afin de pouvoir positionner nos cercles le long de la hauteur du vase.

![](../images/10-1/1/vase15(1).png)

Vous allez ensuite utiliser un nœud **Geometry.Translate** pour placer des cercles à la hauteur souhaitée. Comme vous voulez distribuer vos cercles à travers le vase, utilisez des nœuds Code Block pour multiplier le paramètre de hauteur par un facteur.

![](../images/10-1/1/vase5.png)

> 2\. Les cercles sont convertis (déplacés) par une variable dans l’axe Z.

### Création de la surface

Pour créer une surface à l’aide du nœud **Surface.ByLoft**, vous devez combiner tous les cercles convertis dans une liste. Utilisez **List.Create** pour combiner tous les cercles dans une liste unique, puis exportez cette liste vers le nœud **Surface.ByLoft** pour afficher les résultats.

Désactivez également l’aperçu dans les autres nœuds pour afficher uniquement l’affichage Surface.ByLoft.

![](../images/10-1/1/vase6(1)(1).png)

> 3\. Une surface est créée en lissant les cercles convertis.

## Résultats

Votre workflow est prêt ! Vous pouvez désormais utiliser les **Number Sliders** définis dans votre script pour créer différents modèles de vases.

![](../images/10-1/1/vase1(3).gif)

![](../images/10-1/1/vase7.png)
