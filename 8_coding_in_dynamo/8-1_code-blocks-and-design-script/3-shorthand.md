# Raccourci

### Raccourci

Il existe quelques méthodes de base pour raccourcir le bloc de code qui simplifient _énormément_ la gestion des données. Vous allez découvrir les concepts de base ci-dessous et comprendre comment ce raccourci peut être utilisé à la fois pour créer et interroger des données.

| **Type de données** | **Dynamo standard** | **Bloc de code équivalent** |
| ---------------------- | -------------------------------------------------------- | ------------------------------------------------------------- |
| Nombres | ![](<../images/8-1/3/01 node - numbers.jpg>) | ![](<../images/8-1/3/01 codeblock - numbers.jpg>) |
| Chaînes | ![](<../images/8-1/3/02 node - string.jpg>) | ![](<../images/8-1/3/02 codeblock- string.jpg>) |
| Séquences | ![](<../images/8-1/3/03 node- sequence.jpg>) | ![](<../images/8-1/3/03 codeblock- sequence.jpg>) |
| Intervalles | ![](<../images/8-1/3/04 node- range.jpg>) | ![](<../images/8-1/3/04 codeblock - range.jpg>) |
| Obtenir l'élément au niveau de l'index | ![](<../images/8-1/3/05 node - list get item.jpg>) | ![](<../images/8-1/3/05 codeblock - list get item.jpg>) |
| Création d'une liste | ![](<../images/8-1/3/06 node - list create.jpg>) | ![](<../images/8-1/3/06 codeblock - list create.jpg>) |
| Concaténer des chaînes | ![](<../images/8-1/3/07 node - string concat.jpg>) | ![](<../images/8-1/3/07 codeblock - string concat.jpg>) |
| Instructions conditionnelles | ![](<../images/8-1/3/08 node - conditional.jpg>) | ![](<../images/8-1/3/08 codeblock - conditional.jpg>) |

### Syntaxe supplémentaire

|                                     |                           |                                                                                          |
| ----------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------- |
| **Nœud(s)** | **Bloc de code équivalent** | **Remarque** |
| Tout opérateur (+, &&, >=, Not, etc.) | +, &&, >=, !, etc. | Notez que "Not" devient "!" mais que le nœud est appelé "Not" pour faire la distinction avec "Factorial" |
| Valeur booléenne True | true; | Minuscules |
| Valeur booléenne False | false; | Minuscules |

### Intervalles et séquences

La méthode de définition des intervalles et des séquences peut être réduite au raccourci de base. Utilisez l'image ci-dessous comme guide de la syntaxe ".." pour définir une liste de données numériques avec un bloc de code. Après avoir obtenu l’arrêt de cette notation, la création de données numériques est un processus vraiment efficace :

![](<../images/8-1/3/shorthand - ranges and sequences.jpg>)

> 1. Dans cet exemple, un intervalle de nombres est remplacé par la syntaxe **Code Block** de base définissant `beginning..end..step-size;` . Voici le résultat représenté numériquement : `0..10..1;`
> 2. Notez que la syntaxe `0..10..1;` est équivalente à `0..10;`. Une taille de pas de 1 est la valeur par défaut de la notation abrégée. Par conséquent, `0..10;` donne une séquence de 0 à 10 avec une taille de pas de 1.
> 3. L’exemple du nœud _Sequence_ est similaire, sauf que vous devez utiliser « # » pour indiquer que vous voulez 15 valeurs dans la liste, plutôt qu’une liste qui atteint 15. Dans ce cas, vous définissez : `beginning..#ofSteps..step-size:`. La syntaxe réelle de la séquence est `0..#15..2`.
> 4. Placez _"#"_ de l'étape précédente dans la partie _"taille de pas"_ de la syntaxe. À présent, vous avez un _intervalle de nombres_ qui s’étend du _« début »_ à la _« fin »_ et la notation _« taille de pas »_ distribue uniformément un certain nombre de valeurs entre les deux valeurs : `beginning..end..#ofSteps`.

### Intervalles avancés

La création d'intervalles avancés vous permet de travailler avec une liste de listes en toute simplicité. Dans les exemples ci-dessous, découvrez comment isoler une variable de la notation d'intervalle principale et créer une autre série de cette liste.

![](<../images/8-1/3/shorthand - advance range 01.jpg>)

> 1\. En créant des intervalles imbriqués, comparez la notation avec "#" et la notation sans. La même logique s'applique aux intervalles de base, à la différence qu'elle devient un peu plus complexe.
>
> 2\. Vous pouvez définir un sous-intervalle à n'importe quel endroit de l'intervalle principal, et avoir aussi deux sous-intervalles.
>
> 3\. En contrôlant la valeur de "fin" dans un intervalle, vous pouvez créer davantage d'intervalles de longueurs différentes.

À des fins d’exercice de logique, comparez les deux raccourcis ci-dessus et analysez comment les _sous-intervalles_ et la notation _#_ déterminent la sortie résultante.

![](<../images/8-1/3/shorthand - advance range 02.jpg>)

### Création de listes et obtention d’éléments à partir d’une liste

Outre la création de listes avec un raccourci, vous pouvez également créer des listes à la volée. Ces listes peuvent contenir une large gamme de types d'éléments et peuvent également être interrogées (rappelez-vous que les listes sont des objets en eux-mêmes). Pour résumer, un nœud Code Block vous permet de créer des listes et d’interroger des éléments d’une liste avec des crochets (accolades) :

![](<../images/8-1/3/shorthand - list & get from list 01.jpg>)

> 1\. Créez rapidement des listes avec des chaînes et interrogez-les à l'aide de l'index d'éléments.
>
> 2\. Créez des listes avec des variables et interrogez-les à l'aide de la notation du raccourci d'intervalle.

La gestion de listes imbriquées est un processus similaire. Veillez à l'ordre de la liste et n'oubliez pas d'utiliser plusieurs jeux de crochets :

![](<../images/8-1/3/shorthand - list & get from list 02.jpg>)

> 1\. Définissez une liste de listes.
>
> 2\. Interrogez une liste avec une notation de crochet simple.
>
> 3\. Interrogez un élément avec une notation entre crochets.

## Exercice : Surface sinusoïdale

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/8-1/3/Obsolete-Nodes_Sine-Surface.dyn" %}

Dans cet exercice, vous allez perfectionner vos nouvelles compétences en concevant une super surface en coquille d'oeuf définie par des intervalles et des formules. Au cours de cet exercice, vous découvrirez comment utiliser le bloc de code et les nœuds Dynamo existants en tandem : le bloc de code est utilisé pour le gros volume de données, tandis que les nœuds Dynamo sont visuellement disposés pour la lisibilité de la définition.

Commencez par créer une surface en connectant les nœuds ci-dessus. Au lieu d’utiliser un nœud Number pour définir la largeur et la longueur, cliquez deux fois sur la zone de dessin et tapez `100;` dans un nœud Code Block.

![](<../images/8-1/3/shorthand - exercise 01.jpg>)

![](<../images/8-1/3/shorthand - exercise 02.jpg>)

> 1. Définissez un intervalle compris entre 0 et 1 et 50 divisions en tapant `0..1..#50` dans un nœud **Code Block**.
> 2. Connectez l’intervalle à **Surface.PointAtParameter**, qui prend les valeurs u et v entre 0 et 1 sur la surface. Pensez à définir la liaison sur Produit cartésien en cliquant avec le bouton droit de la souris sur le nœud **Surface.PointAtParameter**.

Dans cette étape, vous allez utiliser votre première fonction pour déplacer la grille de points vers le haut sur l'axe Z. Cette grille pilotera une surface générée reposant sur la fonction sous-jacente. Ajoutez de nouveaux nœuds comme illustrés dans l’image ci-dessous.

![](<../images/8-1/3/shorthand - exercise 03.jpg>)

> 1. Au lieu d’utiliser un nœud Formula, utilisez un nœud **Code Block** avec la ligne : `(0..Math.Sin(x*360)..#50)*5;`. Pour décomposer rapidement cet intervalle, définissez un intervalle contenant une formule. Cette formule est la fonction Sinus. La fonction Sinus reçoit les entrées de degrés dans Dynamo. Ainsi, pour obtenir une onde sinusoïdale complète, multipliez les valeurs x (valeur d'entrée de l'intervalle de 0 à 1) par 360. Ensuite, utilisez le même nombre de divisions que les points de grille de contrôle pour chaque ligne. Définissez donc 50 sous-divisions avec #50. Enfin, le multiplicateur de 5 augmente simplement l'amplitude de la translation de sorte que l'effet soit visible dans l'aperçu Dynamo.

![](<../images/8-1/3/shorthand - exercise 04.jpg>)

> 1. Même si le nœud **Code Block** précédent fonctionnait correctement, il n’était pas entièrement paramétrique. Étant donné que vous voulez piloter ses paramètres dynamiquement, vous allez remplacer la ligne de l’étape précédente par `(0..Math.Sin(x*360*cycles)..#List.Count(x))*amp;`. Cela vous donne la possibilité de définir ces valeurs en fonction des entrées.

La modification des curseurs (de 0 à 10) permet d'obtenir des résultats intéressants.

![](<../images/8-1/3/shorthand - exercise 05.gif>)

![](<../images/8-1/3/shorthand - exercise 06.jpg>)

> 1. Lorsque vous effectuez une transposition sur l’intervalle de nombres, vous inversez la direction de l’onde du rideau : `transposeList = List.Transpose(sineList);`.

![](<../images/8-1/3/shorthand - exercise 07.jpg>)

> 1. Lorsque vous ajoutez sineList et tranposeLit, vous obtenez une surface en coquille d’œuf déformée : `eggShellList = sineList+transposeList;`.

Modifiez les valeurs de curseurs spécifiées ci-dessous pour retrouver un algorithme « paisible ».

![](<../images/8-1/3/shorthand - exercise 08.jpg>)

Enfin, recherchez des parties isolées des données avec le nœud Code Block. Pour régénérer la surface avec un intervalle de points spécifique, ajoutez le bloc de code ci-dessus entre le nœud **Geometry.Translate** et le nœud **NurbsSurface.ByPoints**. Cette ligne contient la ligne de texte : `sineStrips[0..15..1];`. Cela permet de sélectionner les 16 premières lignes de points (sur 50). Recréez la surface. Vous pouvez voir que vous avez généré une partie isolée de la grille de points.

![](<../images/8-1/3/shorthand - exercise 09.jpg>)

![](<../images/8-1/3/shorthand - exercise 10.jpg>)

> 1. Dans la dernière étape, pour rendre ce nœud **Code Block** encore plus paramétrique, pilotez la requête en utilisant un curseur compris entre 0 et 1. Pour ce faire, utilisez la ligne de code suivante : `sineStrips[0..((List.Count(sineStrips)-1)*u)];`. Cela peut sembler déroutant, mais la ligne de code vous donne un moyen rapide de mettre à l'échelle la longueur de la liste en un multiplicateur entre 0 et 1.

Une valeur de `0.53` sur le curseur permet de créer une surface juste au-delà du milieu de la grille.

![](<../images/8-1/3/shorthand - exercise 11.jpg>)

Comme prévu, un curseur de `1` crée une surface à partir de la grille complète de points.

![](<../images/8-1/3/shorthand - exercise 12.jpg>)

En examinant le graphique visuel, vous pouvez mettre en surbrillance les nœuds Code Block et voir chacune de leurs fonctions.

![](<../images/8-1/3/shorthand - exercise 13.jpg>)

> 1\. Le premier nœud **Code Block** remplace le nœud **Number**.
>
> 2\. Le deuxième nœud **Code Block** remplace le nœud **Number Range**.
>
> 3\. Le troisième nœud **Code Block** remplace le nœud **Formula** (ainsi que **List.Transpose**, **List.Count** et **Number Range**).
>
> 4\. Le quatrième nœud **Code Block** interroge une liste de listes, remplaçant ainsi le nœud **List.GetItemAtIndex**.
