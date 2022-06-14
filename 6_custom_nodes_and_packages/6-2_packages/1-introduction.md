# Présentation des packages

En résumé, un package est un ensemble de nœuds personnalisés. Le gestionnaire de package Dynamo est un portail permettant à la communauté de télécharger tout package publié en ligne. Ces jeux d'outils sont développés par des tiers afin d'étendre les fonctionnalités clés de Dynamo, accessibles à tous, et prêts à être téléchargés en un clic.

![Site du gestionnaire de packages](../images/6-2/1/dpm.jpg)

Un projet Open Source comme Dynamo s'appuie sur ce type d'implication de la communauté. Avec ses développeurs tiers dédiés, Dynamo peut étendre sa portée aux workflows dans différents secteurs d’activité. Par conséquent, l'équipe de Dynamo a entrepris de rationaliser le développement et la publication des packages (sujets abordés plus en détail dans les sections suivantes).

### Installation d'un package

La méthode la plus simple pour installer un package consiste à utiliser la barre d'outils Packages de l'interface Dynamo. Passez à présent au vif du sujet et installez-en un. Dans cet exemple rapide, vous allez installer un package très utilisé pour créer des panneaux quadrilatéraux sur une grille.

Dans Dynamo, accédez à _Packages > Rechercher un package..._

![](<../images/6-2/1/package introduction - installing a package 01.jpg>)

Dans la barre de recherche, recherchez "quadrilatères à partir de la grille rectangulaire". Au bout de quelques instants, l'ensemble des packages correspondants à cette demande de recherche apparaissent. Sélectionnez le premier package avec le nom correspondant.

Cliquez sur Installer pour ajouter ce package à votre bibliothèque. Terminé !

![](<../images/6-2/1/package introduction - installing a package 02.jpg>)

Vous avez maintenant un autre groupe appelé "buildz" dans la bibliothèque Dynamo. Son nom fait référence au développeur du package. Le nœud personnalisé est placé dans ce groupe. Vous pouvez commencer à l'utiliser immédiatement.

![](<../images/6-2/1/package introduction - installing a package 03.jpg>)

Utilisez un nœud **Code Block**pour définir rapidement une grille rectangulaire, générer le résultat sur un nœud **Polygon.ByPoints**, puis sur un nœud **Surface.ByPatch** pour afficher la liste des panneaux rectangulaires que vous venez de créer.

![](<../images/6-2/1/package introduction - installing a package 04.jpg>)

### Installation du dossier de package – DynamoUnfold

L'exemple ci-dessus est axé sur un package avec un nœud personnalisé, mais vous utilisez le même processus pour télécharger des packages avec plusieurs nœuds personnalisés et des fichiers de données de prise en charge. Démontrez-le maintenant avec un package plus complet : Dynamo Unfold.

Comme dans l'exemple ci-dessus, commencez par sélectionner _Packages > Rechercher un package..._.

Cette fois, recherchez _"DynamoUnfold"_, en un mot, en gardant les majuscules. Lorsque les packages s’affichent, cliquez sur Installer pour ajouter Dynamo Unfold à votre bibliothèque Dynamo.

![](<../images/6-2/1/package introduction - installing package folder 01.jpg>)

La bibliothèque Dynamo contient un groupe _DynamoUnfold_ avec plusieurs catégories et nœuds personnalisés.

![](<../images/6-2/1/package introduction - installing package folder 02.jpg>)

Examinez maintenant la structure de fichiers du package. Tout d’abord, sélectionnez Dynamo > Préférences.

![](<../images/6-2/1/package introduction - installing package folder 03.jpg>)

Dans la fenêtre contextuelle Préférences, ouvrez Gestionnaire de package > en regard de DynamoUnfold, sélectionnez le menu des points verticaux ![](<../images/6-2/1/package introduction - vertical dots menu.jpg>) > Afficher le répertoire racine pour ouvrir le dossier racine de ce package.

![](<../images/6-2/1/package introduction - installing package folder 04.jpg>)

Cette action permet d'accéder au répertoire racine du package. Il contient trois dossiers et un fichier.

![](<../images/6-2/1/package introduction - installing package folder 05.jpg>)

> 1. Le dossier _bin_ contient les fichiers .dll. Ce package Dynamo a été développé à l'aide de la commande Zero Touch, de sorte que les nœuds personnalisés sont conservés dans ce dossier.
> 2. Le dossier _dyf_ contient les nœuds personnalisés. Ce package n'a pas été développé à l'aide de nœuds personnalisés Dynamo. Ce dossier est donc vide pour ce package.
> 3. Le dossier supplémentaire contient tous les fichiers supplémentaires, y compris les fichiers d'exemple.
> 4. Le fichier pkg est un fichier texte de base qui définit les paramètres du package. Pour l'instant, vous pouvez l'ignorer.

Lorsque vous ouvrez le dossier "extra", vous découvrez la présence de fichiers d'exemple téléchargés lors de l'installation. Les packages ne possèdent pas tous des fichiers d'exemple, mais lorsque ces derniers font partie d'un package, ils sont enregistrés ici.

Ouvrez "SphereUnfold".

![](../images/6-2/1/rd2.jpg)

Après avoir ouvert le fichier et cliqué sur "Exécuter" dans le solveur, vous obtenez une sphère dépliée. Les fichiers d'exemple comme ceux-ci sont utiles pour apprendre à utiliser un nouveau package Dynamo.

![](<../images/6-2/1/package introduction - installing package folder 07.jpg>)

### Gestionnaire de package Dynamo

Une autre façon de découvrir les packages Dynamo est d’explorer le [Gestionnaire de package Dynamo](http://dynamopackages.com) en ligne. C'est une bonne façon de rechercher des packages, car le référentiel trie les packages par ordre de nombre de téléchargements et de popularité. Il s'agit également d'un moyen facile de collecter des informations sur les dernières mises à jour des packages, car certains packages Dynamo sont soumis au contrôle des versions et aux dépendances des versions de Dynamo.

En cliquant sur _"Quadrilatères à partir de la grille rectangulaire_ dans le gestionnaire de package Dynamo, vous pouvez voir ses descriptions, ses versions, le développeur et les éventuelles dépendances.

![](../images/6-2/1/dpm2.jpg)

Vous pouvez également télécharger les fichiers de package à partir du gestionnaire de package Dynamo, mais le processus de Dynamo est plus simple.

### Où sont les fichiers de package stockés localement ?

Si vous téléchargez des fichiers à partir du gestionnaire de packages Dynamo ou si vous souhaitez voir où sont conservés tous vos fichiers de package, cliquez sur Dynamo > Gestionnaire de package > Chemins d’accès de nœud et de package, pour trouver votre répertoire de dossiers racine actuel.

![](<../images/6-2/1/package introduction - installing package folder 08.jpg>)

Par défaut, les packages sont installés dans un emplacement semblable à celui du dossier suivant : _C:/Utilisateurs/[nom d’utilisateur]/AppData/Itinérance/Dynamo/[Version Dynamo]_.

### Repousser les limites des modules

La communauté Dynamo est en constante évolution. Explorez le gestionnaire de package Dynamo de temps en temps : vous y découvrirez de nouvelles avancées intéressantes. Dans les sections suivantes, vous allez examiner les packages de manière plus approfondie, du point de vue de l'utilisateur final à la création de votre propre package Dynamo.
