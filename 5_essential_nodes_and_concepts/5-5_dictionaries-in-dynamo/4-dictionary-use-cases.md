# Cas d’utilisation de Revit

Avez-vous déjà voulu rechercher quelque chose dans Revit à partir d'un élément de données qu'il contient ?

Il y a des chances si vous avez procédé comme dans l’exemple suivant.

Dans l’image ci-dessous, vous allez rassembler toutes les pièces dans le modèle Revit, obtenir l’index de la pièce souhaitée (par numéro de pièce), puis sélectionner la pièce au niveau de l’index.

![](<../images/5-5/4/dictionary - collect room in revit model.jpg>)

> 1. Rassemblez toutes les pièces du modèle.
> 2. Numéro de pièce à rechercher.
> 3. Obtenez le numéro de chambre et trouvez l’index correspondant.
> 4. Obtenez la pièce au niveau de l’index.

## Exercice : Dictionnaire de pièces

### Partie I : Création d’un dictionnaire de pièces

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-5/4/roomDictionary.dyn" %}

Recréez maintenant cette idée à l'aide de dictionnaires. Vous devez d’abord rassembler toutes les pièces dans votre modèle Revit.

![](<../images/5-5/4/dictionary - exercise I - 01.jpg>)

> 1. Choisissez la catégorie Revit avec laquelle vous voulez travailler (dans le cas présent, vous travaillez avec des pièces).
> 2. Vous indiquez à Dynamo de collecter tous ces éléments.

Ensuite, vous devez décider des clés que vous allez utiliser pour rechercher ces données. (Pour plus d’informations sur les clés, consultez la section [Qu’est-ce qu’un dictionnaire ?](9-1\_what-is-a-dictionary.md)).

![](<../images/5-5/4/dictionary - exercise I - 02.jpg>)

> 1. Les données que vous utilisez sont les numéros de pièce.

Vous allez maintenant créer le dictionnaire avec les clés et les éléments donnés.

![](<../images/5-5/4/dictionary - exercise I - 03.jpg>)

> 1. Le nœud **Dictionary.ByKeysValues** crée un dictionnaire en fonction des entrées appropriées.
> 2. `Keys` doit être une chaîne, tandis que `values` peut être une variété de types d’objets.

Enfin, vous pouvez extraire une pièce du dictionnaire avec son numéro de pièce.

![](<../images/5-5/4/dictionary - exercise I - 04.jpg>)

> 1. `String` est la clé que vous utilisez pour rechercher un objet dans le dictionnaire.
> 2. Désormais, **Dictionary.ValueAtKey** obtiendra l'objet à partir du dictionnaire.

### Partie II : Recherche de valeurs

Cette logique de dictionnaire permet également de créer des dictionnaires contenant des objets groupés. Si vous voulez rechercher toutes les pièces à un niveau donné, vous pouvez modifier le graphique ci-dessus comme suit.

![](<../images/5-5/4/dictionary - exercise II - 01.jpg>)

> 1. Au lieu d'utiliser le numéro de pièce comme clé, vous pouvez maintenant utiliser une valeur de paramètre (dans le cas présent, vous allez utiliser le niveau).

![](<../images/5-5/4/dictionary - exercise II - 02.jpg>)

> 1. Maintenant, vous pouvez regrouper les pièces selon le niveau sur lequel elles résident.

![](<../images/5-5/4/dictionary - exercise II - 03.jpg>)

> 1. Avec les éléments regroupés par niveau, vous pouvez désormais utiliser les clés partagées (clés uniques) comme clés de votre dictionnaire, ainsi que les listes de pièces comme éléments.

![](<../images/5-5/4/dictionary - exercise II - 04.jpg>)

> 1. Enfin, en utilisant les niveaux du modèle Revit, vous pouvez rechercher les pièces qui résident sur ce niveau dans le dictionnaire. `Dictionary.ValueAtKey` prend le nom du niveau et renvoie les objets de pièce à ce niveau.

Les possibilités d'utilisation du dictionnaire sont infinies. La possibilité de lier vos données BIM dans Revit à l'élément lui-même offre une variété de cas d'utilisation.
