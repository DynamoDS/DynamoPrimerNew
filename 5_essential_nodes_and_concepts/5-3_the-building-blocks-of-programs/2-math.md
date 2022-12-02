# Math

Si la forme de données la plus simple est le nombre, la méthode la plus simple pour lier ces nombres est d'utiliser les mathématiques. Des opérateurs simples, tels que la division, aux fonctions trigonométriques et aux formules plus complexes, Math est un excellent moyen de commencer à explorer les relations numériques et les motifs.

### Opérateurs arithmétiques

Les opérateurs sont un ensemble de composants qui utilisent des fonctions algébriques avec deux valeurs numériques d'entrée, ce qui génère une valeur de sortie unique (addition, soustraction, multiplication, division, etc.). Ces commandes sont disponibles sous Opérateurs > Actions.

| Icône                                                | Nom (Syntaxe)     | Entrées                     | Sorties      |
| --------------------------------------------------- | ----------------- | -------------------------- | ------------ |
| ![](../images/5-3/2/addition.jpg)       | Ajouter (**+**)       | var[]…[], var[]…[] | var[]…[] |
| ![](../images/5-3/2/Subtraction.jpg)    | Soustraire (**-**)  | var[]…[], var[]…[] | var[]…[] |
| ![](../images/5-3/2/Multiplication.jpg) | Multiplier (**\***) | var[]…[], var[]…[] | var[]…[] |
| ![](../images/5-3/2/Division.jpg)       | Diviser (**/**)    | var[]…[], var[]…[] | var[]…[] |

## Exercice : Formule de la clothoïde dorée

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-3/2/Building Blocks of Programs - Math.dyn" %}

### Partie I : Formule paramétrique

Combinez les opérateurs et les variables pour créer une relation plus complexe à l’aide de **formules**. Utilisez les curseurs pour créer une formule qui peut être contrôlée à l’aide des paramètres d’entrée.

1. Créez une séquence de nombres qui représente le « t » dans l’équation paramétrique. Vous devez donc utiliser une liste suffisamment grande pour définir une clothoïde.

**Number Sequence :** définissez une séquence de nombres reposant sur les trois entrées _start, amount_ et _step_.

![](../images/5-3/2/math-partI-01.jpg)

2\. L’étape ci-dessus a permis de créer une liste de nombres pour définir le domaine paramétrique. Ensuite, créez un groupe de nœuds représentant l’équation de la clothoïde dorée.

La clothoïde dorée est définie comme l’équation suivante :

$$ x = r cos θ = a cos θ e^{bθ} $$

$$ y = r sin θ = a sin θe^{bθ} $$

L’image ci-dessous représente la spirale dorée sous forme de programmation visuelle. Lorsque vous parcourez le groupe de nœuds, essayez de faire le parallèle entre le programme visuel et l’équation écrite.

![](../images/5-3/2/math-partI-02.jpg)

> a. **Number Slider :** ajoutez deux curseurs de numérotation dans la zone de dessin. Ces curseurs représentent les variables _a_ et _b_ de l’équation paramétrique. Elles représentent une constante flexible, ou des paramètres que vous pouvez ajuster afin d’obtenir le résultat souhaité.
>
> b. **Multiplication (*)** : le nœud de multiplication est représenté par un astérisque. Vous utiliserez ce nœud à plusieurs reprises pour connecter des variables de multiplication.
>
> c. **Math.RadiansToDegrees :** les valeurs « _t_ » doivent être converties en degrés pour être évaluées dans les fonctions trigonométriques. N’oubliez pas que Dynamo utilise par défaut les degrés pour évaluer ces fonctions.
>
> d. **Math.Pow :** la fonction de « _t_ » et le numéro « _e_ » permettent de créer la séquence Fibonacci.
>
> e. **Math.Cos et Math.Sin :** ces deux fonctions trigonométriques différencient respectivement la coordonnée x et la coordonnée y de chaque point paramétrique.
>
> f. **Watch** : le résultat obtenu se compose de deux listes. Elles représentent les coordonnées _x_ et _y_ des points utilisés pour générer la clothoïde.

### Partie II : De la formule à la géométrie

Le bloc de nœuds de l’étape précédente fonctionne correctement, mais cela demande beaucoup de travail. Pour créer un workflow plus efficace, consultez la section [DesignScript](../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/2-design-script-syntax.md) pour définir une chaîne d’expressions Dynamo en un nœud. Dans cette prochaine série d’étapes, vous allez utiliser l’équation paramétrique pour dessiner la clothoïde de Fibonacci.

**Point.ByCoordinates :** connectez le nœud de multiplication supérieur à l’entrée « _x_ » et le nœud inférieur à l’entrée « _y_ ». Une clothoïde paramétrique de points apparaît à l’écran.

![](../images/5-3/2/math-partII-01.gif)

**Polycurve.ByPoints :** connectez **Point.ByCoordinates** de l’étape précédente à _points_. Vous pouvez laisser _connectLastToFirst_ sans entrée, car vous ne créez pas de courbe fermée. Cela permet de créer une spirale qui passe par chaque point défini à l’étape précédente.

![](../images/5-3/2/math-partII-02.jpg)

La clothoïde de Fibonacci est désormais terminée. Vous allez désormais effectuer deux exercices distincts, appelés Nautilus et Tournesol. Ce sont des abstractions de systèmes naturels, mais qui représentent bien les deux applications différentes de la clothoïde de Fibonacci.

### Partie III : De la clothoïde au Nautilus

**Circle.ByCenterPointRadius :** utilisez un nœud circulaire avec les mêmes entrées que celles de l’étape précédente. Étant donné que la valeur du rayon par défaut est de _1,0_, un réseau de cercles apparaît immédiatement. La façon dont les points divergent à partir du point d’origine est immédiatement lisible.

![](../images/5-3/2/math-partIII-01.jpg)

**Number Sequence :** réseau d’origine de « _t_ ». Si vous connectez ceci à la valeur du rayon de **Circle.ByCenterPointRadius**, les centres des cercles divergent davantage à partir de l’origine, mais le rayon des cercles augmente, créant ainsi un super graphique circulaire de Fibonacci.

Et c’est encore mieux en 3D !

![](../images/5-3/2/math-partIII-02.gif)

### Partie IV : Du Nautilus à la phyllotaxie

Maintenant que vous avez créé une coque Nautilus circulaire, passez aux grilles paramétriques. Vous allez utiliser une rotation de base sur la clothoïde Fibonacci pour créer une grille Fibonacci, et le résultat est modélisé après la [croissance des graines de tournesol](https://blogs.unimelb.edu.au/sciencecommunication/2018/09/02/this-flower-uses-maths-to-reproduce/).

Comme point de départ, commencez par la même étape qu’à l’exercice précédent : la création d’un réseau de points en forme de spirale avec le nœud **Point.ByCoordinates**.

\![](../images/5-3/2/math-part IV-01.jpg)

Ensuite, suivez ces courtes étapes pour générer une série de clothoïdes à différentes rotations.

![](../images/5-3/2/math-partIV-02.jpg)

> a. **Géométrie.Rotation :** il existe plusieurs options **Geometry.Rotate**. Assurez-vous d’avoir choisi le nœud avec les entrées _geometry_, _basePlane_ et _degrees_. Connectez **Point.ByCoordinates** à l’entrée geometry. Cliquez avec le bouton droit de la souris sur ce nœud et assurez-vous que la combinaison est définie sur « Produit vectoriel ».
>
> ![](../images/5-3/2/math-partIV-03crossproduct.jpg)
>
> b. **Plane.XY :** connexion à l’entrée _basePlane_. Vous allez effectuer une rotation autour de l’origine, servant également de base pour la clothoïde.
>
> c. **Intervalle de nombres :** pour la saisie des degrés, vous devez créer plusieurs rotations. Pour ce faire, il suffit d’utiliser un composant **Number Range**. Connectez-le à l’entrée _degrees_.
>
> d. **Number :** pour définir l’intervalle de nombres, ajoutez trois nœuds Number à la zone de dessin dans l’ordre vertical. De haut en bas, affectez respectivement les valeurs _0.0, 360.0_ et _120.0_. Elles pilotent la rotation de la clothoïde. Après avoir connecté les trois nœuds Number au nœud Range, observez les sorties du nœud **Number Range**.

Le résultat obtenu commence à ressembler à un tourbillon. Ajustez certains paramètres de **Number Range** et observez le changement des résultats.

Modifiez la taille du pas du nœud **Number Range** de _120.0_ à _36.0_. Cette action crée davantage de rotations et permet donc d’obtenir une grille plus dense.

![](../images/5-3/2/math-partIV-04.jpg)

Modifiez la taille du pas du nœud **Number Range** de _36.0_ à _3.6_. Vous obtenez une grille beaucoup plus dense, et la direction de la clothoïde n’est pas claire. Bravo, vous avez créé un tournesol.

![](../images/5-3/2/math-partIV-05.jpg)
