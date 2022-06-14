# Nœuds de dictionnaire

Dynamo 2.0 présente une variété de nœuds de dictionnaire à notre disposition. Cela inclut les nœuds _create, action et query_.

![](<../images/5-5/2/dictionary nodes - nodes.jpg>)

#### Create

1.`Dictionary.ByKeysValues` crée un dictionnaire avec les valeurs et les clés fournies. _(Le nombre d'entrées correspond à la liste d'entrées la plus courte possible)_

#### Action

2\. `Dictionary.Components` produit les composants du dictionnaire d’entrée. _(Il s'agit de l'inverse du nœud create.)_

3\. `Dictionary.RemoveKeys` crée un objet de dictionnaire dont les clés d’entrée ont été supprimées.

4\. `Dictionary.SetValueAtKeys` crée un dictionnaire basé sur les clés d’entrée et les valeurs pour remplacer la valeur en cours au niveau des clés correspondantes.

5\. `Dictionary.ValueAtKey` renvoie la valeur à la clé d’entrée.

#### Nombre

6\. `Dictionary.Count` vous indique le nombre de paires clés-valeurs dans le dictionnaire.

7\. `Dictionary.Keys` renvoie les clés actuellement stockées dans le dictionnaire.

8\. `Dictionary.Values` renvoie les valeurs actuellement stockées dans le dictionnaire.

Le fait de pouvoir lier les données de manière globale avec les dictionnaires constitue une excellente solution de remplacement, par rapport à la méthode de travail classique avec les index et les listes.
