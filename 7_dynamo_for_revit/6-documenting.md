# Documentation

La modification des paramètres de la documentation suit les leçons apprises dans les sections précédentes. Dans cette section, vous allez découvrir les paramètres de modification qui n'affectent pas les propriétés géométriques d'un élément, mais qui permettent de préparer un fichier Revit pour la documentation.

### Écart

Dans l'exercice ci-dessous, vous allez utiliser un écart de base par rapport au nœud Plane pour créer une feuille Revit pour la documentation. Chaque panneau de la structure de toit définie de façon paramétrique possède une valeur d'écart différente. L'objectif est de définir l'intervalle de valeurs à l'aide de couleurs et en planifiant les points adaptatifs à transmettre à un consultant, un ingénieur ou un entrepreneur responsable de la façade.

![écart](./images/6/deviation.jpg)

> L'écart par rapport au nœud Plane permet de calculer la distance à laquelle l'ensemble de quatre points varie par rapport au plan ajusté au mieux entre eux. Il s'agit d'une méthode simple et rapide pour étudier la constructibilité.

## Exercice

### Partie I : Définition des panneaux d’Aperture Ratio en fonction de l’écart par rapport au nœud Plane

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="./datasets/6/Revit-Documenting.zip" %}

Commencez par utiliser le fichier Revit pour cette section (ou continuez à partir de la section précédente). Ce fichier contient un réseau de panneaux ETFE sur le toit. Faites référence à ces panneaux pour cet exercice.

![](<./images/6/documenting - exercise I - 01.jpg>)

> 1. Ajoutez un nœud _Family Types_ à la zone de dessin et choisissez _"ROOF-PANEL-4PT"_.
> 2. Connectez ce nœud à un nœud de sélection _All Elements of Family Type_ pour transférer tous les éléments de Revit dans Dynamo.

![](<./images/6/documenting - exercise I - 02.jpg>)

> 1. Recherchez l'emplacement des points adaptatifs pour chaque élément à l'aide du nœud _AdaptiveComponent.Locations_.
> 2. Créez un polygone à partir de ces quatre points avec le nœud _Polygon.ByPoints_. Vous avez maintenant une version abstraite du système à panneaux dans Dynamo sans avoir à importer la géométrie complète de l'élément Revit.
> 3. Calculez l'écart planaire grâce au nœud _Polygon.PlaneDeviation_.

Juste pour voir, comme dans l'exercice précédent, définissez le rapport d'ouverture de chaque panneau en fonction de son écart planaire.

![](<./images/6/documenting - exercise I - 03.jpg>)

> 1. Ajoutez un nœud _Element.SetParameterByName_ à la zone de dessin et connectez les composants adaptatifs à l'entrée _element_. Connectez un nœud _Code Block_ indiquant _« Aperture Ratio »_ à l’entrée _parameterName_.
> 2. Vous ne pouvez pas connecter directement les résultats de l'écart à l'entrée value, car vous devez remapper les valeurs avec l'intervalle de paramètres.

![](<./images/6/documenting - exercise I - 04.jpg>)

> 1. Utilisez _Math.RemapRange_ pour remapper les valeurs d’écart avec un domaine compris entre 0.15 et 0\_.\_45 en saisissant `0.15; 0.45;` dans le nœud _Code Bloc_.
> 2. Connectez ces résultats à l'entrée value de _Element.SetParameterByName_.

Dans Revit, vous pouvez _donner un sens_ au changement d'ouverture sur la surface.

![Exercice](./images/6/13.jpg)

En zoomant, il apparaît plus clairement que les panneaux fermés sont orientés vers les coins de la surface. Les coins ouverts sont orientés vers le haut. Les coins représentent les zones de plus grand écart, tandis que le renflement présente logiquement une courbure minimale.

![Exercice](./images/6/13a.jpg)

### Partie II : Couleur et documentation

La définition du rapport d'ouverture n'indique pas clairement l'écart des panneaux sur le toit, et vous modifiez également la géométrie de l'élément réel. Imaginez que vous souhaitiez simplement étudier l'écart du point de vue de la faisabilité de la fabrication. Il conviendrait de colorer les panneaux en fonction de l'intervalle d'écart de la documentation. Pour ce faire, utilisez la série d'étapes ci-dessous et suivez un procédé très similaire à celui susmentionné.

![](<./images/6/documenting - exercise II - 01.jpg>)

> 1. Supprimez _Element.SetParameterByName_ et ses nœuds d’entrée et ajoutez _Element.OverrideColorInView_.
> 2. Ajoutez un nœud _Color Range_ à la zone de dessin et connectez-le à l'entrée color de _Element.OverrideColorInView_. Vous devez toujours connecter les valeurs d'écart à l'intervalle de couleurs pour créer le dégradé.
> 3. Placez le curseur sur l'entrée _value_. Vous pouvez voir que les valeurs d'entrée doivent être comprises entre _0_ et _1_ pour mapper une couleur avec chaque valeur. Vous devez remapper les valeurs d'écart avec cet intervalle.

![](<./images/6/documenting - exercise II - 02.jpg>)

> 1. À l’aide de _Math.RemapRange_, remappez les valeurs d’écart planaire sur un intervalle compris entre \* 0\* et _1_ (remarque : vous pouvez utiliser le nœud _« MapTo »_ pour définir également un domaine source).
> 2. Connectez les résultats à un nœud _Color Range_.
> 3. La sortie est un intervalle de couleurs, et non un intervalle de nombres.
> 4. Si vous avez défini le paramètre sur Manuel, cliquez sur _Exécuter_. À partir de ce point, vous devez être en mesure de choisir l'option Automatique.

Dans Revit, le dégradé est bien plus lisible, qui est représentatif de l'écart planaire basé sur l'intervalle de couleurs. Mais que faire si vous voulez personnaliser les couleurs ? Les valeurs d'écart minimal sont représentées en rouge, ce qui semble aller à contre-courant du résultat attendu. L'écart maximal doit être rouge, avec un écart minimal représenté par une couleur plus douce. Retournez dans Dynamo et corrigez le problème.

![](./images/6/09.jpg)

![](<./images/6/documenting - exercise II - 04.jpg>)

> 1. À l’aide d’un nœud _Code Block_, ajoutez deux nombres sur deux lignes différentes : `0;` et `255;`.
> 2. Pour créer une couleur rouge et une couleur bleue, connectez les valeurs appropriées à deux nœuds _Color.ByARGB_.
> 3. Créez une liste à partir de ces deux couleurs.
> 4. Connectez cette liste à l'entrée _colors_ de _Color Range_ et observez l'intervalle de couleurs personnalisé se mettre à jour.

Dans Revit, vous pouvez désormais mieux comprendre les zones d'écart maximal dans les coins. Rappelez-vous que ce nœud permet de remplacer une couleur dans une vue. Il serait donc utile si vous aviez une feuille particulière dans le jeu de dessins, axée sur un type particulier d'analyse.

![Exercise](<./images/6/07 (6).jpg>)

### Partie III : Planification

Dans Revit, sélectionnez un panneau ETFE. Quatre paramètres d'occurrence sont disponibles : XYZ1, XYZ2, XYZ3 et XYZ4. Ils sont tous vides après leur création. Il s'agit de paramètres basés sur du texte et des valeurs requises. Utilisez Dynamo pour écrire les emplacements de points adaptatifs sur chaque paramètre. Cela permet d'assurer l'interopérabilité si la géométrie doit être envoyée à un ingénieur consultant en façade.

![](<./images/6/documenting - exercise III - 01.jpg>)

Voici une feuille d'exemple qui contient une nomenclature volumineuse et vide. Les paramètres XYZ sont des paramètres partagés dans le fichier Revit, ce qui vous permet de les ajouter à la nomenclature.

![Exercise](<./images/6/03 (8).jpg>)

En zoomant, les paramètres XYZ restent à remplir. Les deux premiers paramètres sont pris en charge par Revit.

![Exercise](<./images/6/02 (9).jpg>)

L'écriture de ces valeurs requiert une opération de liste complexe. Le graphique lui-même est simple, mais les concepts sont très élaborés à partir du mappage de liste, comme décrit dans le chapitre de liste.

![](<./images/6/documenting - exercise III - 04.jpg>)

> 1. Sélectionnez tous les composants adaptatifs à l'aide de deux nœuds.
> 2. Extrayez l'emplacement de chaque point à l'aide de _AdaptiveComponent.Locations_.
> 3. Convertissez ces points en chaînes. N'oubliez pas que le paramètre est basé sur du texte. Vous devez donc saisir le type de données correct.
> 4. Créez une liste des quatre chaînes qui définissent les paramètres à modifier : _XYZ1, XYZ2, XYZ3_ et _XYZ4_.
> 5. Connectez cette liste à l'entrée _parameterName_ de _Element.SetParameterByName_.
> 6. Connectez _Element.SetParameterByName_ à l'entrée _combinator_ de _List.Combine._ Connectez les _composants adaptatifs_ à _list1_. Connectez l'élément _String_ de l'objet à _list2_.

Vous obtenez ici une correspondance de liste, car vous écrivez quatre valeurs pour chaque élément, ce qui crée une structure de données complexe. Le nœud _List.Combine_ définit une opération d'un pas vers le bas dans la hiérarchie de données. Par conséquent, les entrées element et value de _Element.SetParameterByName_ restent vides. _List.Combine_ connecte les sous-listes de ses entrées aux entrées vides de _Element.SetParameterByName_, en fonction de l’ordre dans lequel elles sont connectées.

Lorsque vous sélectionnez un panneau dans Revit, il y a désormais des valeurs de chaîne pour chaque paramètre. En réalité, vous créeriez un format plus simple pour écrire un point (X, Y, Z). Cela peut être effectué avec des opérations de chaîne dans Dynamo, mais ce thème n'est pas abordé ici pour ne pas sortir du cadre de ce chapitre.

![](<./images/6/04 (5).jpg>)

Vue de l'exemple de nomenclature avec des paramètres remplis.

![](<./images/6/01 (9).jpg>)

Chaque panneau ETFE possède désormais les coordonnées XYZ écrites pour chaque point adaptatif, représentant les coins de chaque panneau pour la fabrication.

![Exercise](<./images/6/00 (8).jpg>)
