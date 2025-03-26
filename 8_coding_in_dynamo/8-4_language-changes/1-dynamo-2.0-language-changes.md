# Changements relatifs au langage 

La section Changements relatifs au langage fournit un aperçu des mises à jour et des modifications apportées au langage dans Dynamo pour chaque version. Ces modifications peuvent avoir un impact sur le fonctionnement, les performances et l’utilisation. Ce guide aide les utilisateurs à comprendre quand et pourquoi s’adapter à ces mises à jour.

## Changements relatifs au langage dans Dynamo 2.0

1. Changement de la syntaxe list@level de « @-1 » à « @L1 »
* Nouvelle syntaxe pour list@level, afin d’utiliser list@L1 au lieu de list@-1
* Raison : aligner la syntaxe du code sur l’aperçu/l’interface utilisateur ; les tests utilisateurs montrent que cette nouvelle syntaxe est plus compréhensible

2. Implémenter les types Int et Double dans TS pour s’aligner sur les types Dynamo

3. Interdiction des fonctions surchargées dans lesquelles les arguments ne diffèrent que par la cardinalité
* Les anciens graphes qui utilisent des surcharges qui ont été supprimées doivent utiliser par défaut les surcharges les mieux classées.
* Raison : éviter toute ambiguïté sur la fonction spécifique qui est exécutée

4. Interdiction de la conversion en tableaux avec les guides de réplication

5. Rendre les variables des blocs impératifs locales à la portée des blocs impératifs
* Les valeurs des variables définies à l’intérieur des blocs de code impératif ne seront pas affectées par les modifications à l’intérieur des blocs impératifs qui y font référence.  

6. Rendre les variables immuables pour désactiver la mise à jour associative dans les nœuds de bloc de code

7. Compiler tous les nœuds de l’interface utilisateur en méthodes statiques

8. Prendre en charge des instructions de retour sans affectation
* « = » n’est nécessaire ni dans les définitions de fonction ni dans le code impératif.

9. Migration des anciens noms de méthodes dans les CBN
* De nombreux nœuds ont été renommés pour améliorer la lisibilité et leur emplacement dans l’interface utilisateur de l’explorateur de bibliothèques

10. Lister comme un nettoyage de dictionnaire

----
Problèmes identifiés :
- Les conflits d’espaces de noms dans les blocs impératifs provoquent l’apparition imprévue de ports d’entrée. Pour plus d’informations, consultez le [problème Github](https://github.com/DynamoDS/Dynamo/issues/8796). Pour contourner ce problème, définissez la fonction en dehors du bloc impératif comme suit :
```
pnt = Autodesk.Point.ByCoordinates;
lne = Autodesk.Line.ByStartPointEndPoint;

[Imperative]
{
    x = 1;
    start = pnt(0,0,0);
    end = pnt(x,x,x);
    line = lne(start,end);
    return = line;
};
```

## Explication des changements relatifs au langage dans Dynamo 2.0

Un certain nombre d’améliorations ont été apportées au langage dans la version 2.0 de Dynamo. L’objectif principal était de simplifier le langage. L’accent a été mis sur l’amélioration de la compréhension et de la simplicité d’utilisation de DesignScript, afin de le rendre plus performant et plus flexible et ainsi d’améliorer la compréhension de l’utilisateur final.

Explication des changements effectués dans la version 2.0 :
* Simplification de la syntaxe List@Level
* Les méthodes surchargées avec des paramètres qui ne diffèrent que par le rang sont interdites 
* Compilation de tous les nœuds de l’interface utilisateur en tant que méthodes statiques
* Interdiction de la conversion en liste lorsqu’elle est utilisée avec des guides de réplication/liaison
* Les variables des blocs associatifs sont immuables pour empêcher la mise à jour associative
* Les variables des blocs Impératifs sont locales à la portée des blocs impératifs
* Séparation des listes et des dictionnaires

## 1\. Simplification de la syntaxe list@level 

Nouvelle syntaxe pour list@level, afin d’utiliser `list@L1` au lieu de `list@-1` ![](../images/8-4/1/lang2_1.png)


## 2\. Les fonctions surchargées avec des paramètres qui ne diffèrent que par le rang sont interdites
Les fonctions surchargées posent problème pour plusieurs raisons :
* Une fonction surchargée indiquée par un nœud d’interface utilisateur dans le graphe peut ne pas être la même que celle qui est utilisée au moment de l’exécution
* La résolution de la méthode est coûteuse et ne fonctionne pas bien pour les fonctions surchargées
* Il est difficile de comprendre le comportement de réplication pour les fonctions surchargées

Prenons l’exemple de `BoundingBox.ByGeometry`. Il y avait deux fonctions surchargées dans les anciennes versions de Dynamo, l’une prenait un argument à valeur unique et l’autre une liste de géométries comme argument :
```
BoundingBox BoundingBox.ByGeometry(geometry: Geometry) {...}
BoundingBox BoundingBox.ByGeometry(geometry: Geometry[]) {...}
```
Si l’utilisateur a déposé le premier nœud sur la zone de dessin et a connecté une liste de géométries, il s’attend à ce que la réplication se déclenche. Toutefois, cela ne se produira jamais, car au moment de l’exécution, la deuxième surcharge sera appelée à la place, comme indiqué ici : ![](../images/8-4/1/lang2_2.png)
 
C’est pour cette raison que dans la version 2.0, nous avons interdit les fonctions surchargées dont les paramètres ne diffèrent que par la cardinalité. Cela signifie que pour les fonctions surchargées qui ont le même nombre et le même type de paramètres, mais qui ont un ou plusieurs paramètres qui ne diffèrent que par le rang, la surcharge définie en premier l’emporte toujours, tandis que les autres sont rejetées par le compilateur. Le principal avantage est que cela simplifie la logique de résolution de la méthode en disposant d’un chemin rapide pour sélectionner les fonctions candidates.

Dans la bibliothèque de géométries de la version 2.0, la première surcharge de l’exemple `BoundingBox.ByGeometry` a été abandonnée et la seconde a été conservée. Donc si le nœud est destiné à être répliqué, c’est-à-dire utilisé dans le contexte du premier, il faut l’utiliser avec l’option de liaison la plus courte (ou la plus longue), ou dans un bloc de code avec des guides de réplication : 
```
BoundingBox.ByGeometry(geometry<1>);
```
Nous pouvons voir dans cet exemple que le nœud de rang supérieur peut être utilisé à la fois dans un appel répliqué et non répliqué et il est donc toujours prioritaire par rapport à une surcharge de rang inférieur. En règle générale, **nous recommandons aux créateurs de nœuds d’abandonner les surcharges de rang inférieur au profit de méthodes de rang supérieur**. Cela permet au compilateur DesignScript de toujours appeler la méthode de rang supérieur comme la première et la seule qu’il trouve.

### Exemples :
Dans l’exemple suivant, deux surcharges de fonction `foo` ont été définies. Dans la version 1.x, il est difficile de savoir quelle surcharge va être utilisée au moment de l’exécution. L’utilisateur peut s’attendre à ce que la deuxième surcharge `foo(a:int, b:int)` s’exécute, auquel cas la méthode devrait se répliquer trois fois en renvoyant trois fois la valeur `10`. En réalité, seule une valeur de `10` est renvoyée, car la première surcharge avec le paramètre list est appelée à la place.

### La deuxième surcharge est ignorée dans la version 2.0 :
Dans la version 2.0, c’est toujours la première méthode définie qui est choisie par rapport au reste. « Premier arrivé, premier servi ».

![](../images/8-4/1/lang2_3.png)

Dans tous les cas suivants, la première surcharge définie sera choisie. Notez que cela se base exclusivement sur l’ordre de définition des fonctions et non sur les rangs de paramètres, bien qu’il soit recommandé de donner la préférence aux méthodes avec des paramètres de rang plus élevé pour les nœuds définis par l’utilisateur et Zero Touch.
```
1)
foo(a: int[], b: int); ✓
foo(a: int, b: int); ✕
```
```
2) 
foo(x: int, y: int); ✓
foo(x: int[], y: int[]); ✕
```
## 3\. Compiler tous les nœuds de l’interface utilisateur en méthodes statiques
Dans Dynamo 1.x, les nœuds de l’interface utilisateur (sans blocs de code) étaient compilés en méthodes et en propriétés d’instance. Par exemple, le nœud `Point.X` a été compilé pour `pt.X` et `Curve.PointAtParameter` a été compilé pour `curve.PointAtParameter(param)`. Ce comportement posait deux problèmes :

__A. La fonction représentée par le nœud de l’interface utilisateur n’était pas toujours la même que celle utilisée au moment de l’exécution__

Un exemple classique est le nœud `Translate`. Il existe plusieurs nœuds `Translate` qui acceptent le même nombre et le même type d’arguments, tels que : `Geometry.Translate`, `Mesh.Translate` et `FamilyInstance.Translate`. En raison du fait que les nœuds ont été compilés en tant que méthodes d’instance, le passage d’une `FamilyInstance` à un nœud `Geometry.Translate` fonctionnerait toujours, car au moment de l’exécution, l’appel serait dirigé vers la méthode d’instance `Translate` sur une `FamilyInstance`. Cela induisait les utilisateurs en erreur, car le nœud ne faisait pas ce qu’il disait.

__B. Le deuxième problème était que les méthodes d’instance ne fonctionnaient pas avec des tableaux hétérogènes__

Au moment de l’exécution, le moteur d’exécution doit savoir quelle fonction doit être appelée. Si l’entrée est une liste, par exemple `list.Translate()`, comme parcourir chaque élément d’une liste et les méthodes de recherche sur son type est une tâche trop lourde, la logique de résolution de la méthode supposerait simplement que le type cible est le même que le type du premier élément, et essaierait de rechercher la méthode `Translate()` définie sur ce type. Par conséquent, si le premier type d’élément ne correspond pas au type cible de la méthode (ou même s’il était `null` ou la liste vide), la liste entière échoue même si d’autres types de la liste correspondent. 

Par exemple, si une entrée de liste avec les types de `[Arc, Line]` suivants est transmise à `Arc.CenterPoint`, le résultat contient un point central pour l’arc et une valeur `null` pour la ligne, comme prévu. Toutefois, si l’ordre est inversé, le résultat entier est null, car le premier élément avait échoué à la vérification de la résolution de la méthode :
### Dynamo 1.x teste uniquement le premier élément de la liste d’entrée pour la vérification de la résolution de la méthode
![](../images/8-4/1/lang2_4.png)
```
x = [arc, line];
y = x.CenterPoint; // y = [centerpoint, null] ✓
```
```
x = [line, arc];
y = x.CenterPoint; // y = null ✕
```
Dans la version 2.0, ces deux problèmes sont résolus, car les nœuds de l’interface utilisateur sont compilés en tant que propriétés statiques et méthodes statiques. 

Avec les méthodes statiques, la résolution de la méthode d’exécution est plus simple et tous les éléments de la liste d’entrée sont parcourus. Vous trouverez ci-dessous quelques exemples :

La sémantique `foo.Bar()` (méthode d’instance) doit vérifier le type de `foo` et s’il s’agit d’une liste, puis la faire correspondre avec les fonctions candidates. Il s’agit d’une lourde tâche. D’autre part, la sémantique `Foo.Bar(foo)` (méthode statique) n’a besoin de vérifier qu’une seule fonction avec le type de paramètre `foo` !

Voici ce qui se passe dans la version 2.0 :
* Un nœud de propriété d’interface utilisateur est compilé dans un getter statique : le moteur génère une version statique d’un getter pour chaque propriété. Par exemple, un nœud `Point.X` est compilé dans un getter statique`Point.get_X(pt)`. Notez que le getter statique peut également être appelé à l’aide de son alias `Point.X(pt)` dans un nœud de bloc de code.
* Un nœud de méthode d’interface utilisateur est compilé dans la version statique : le moteur génère une méthode statique correspondante pour le nœud. Par exemple, le nœud `Curve.PointAtParameter` est compilé en `Curve.PointAtParameter(curve: Curve, parameter:double)` au lieu de `curve.PointAtParameter(parameter)`. 

**Remarque :** nous n’avons pas supprimé la prise en charge des méthodes d’instance, de sorte que les méthodes d’instance existantes utilisées dans les CBN telles que `pt.X` et `curve.PointAtParameter(parameter)` dans les exemples ci-dessus fonctionnent toujours.

Cet exemple fonctionnait dans la version 1.x, car le graphe se compilait en `point.X;` et il trouvait la propriété `X` sur l’objet point. Il échoue désormais dans la version 2.0 car le code compilé `Vector.X(point)` n’accepte qu’un type `Vector` :

![](../images/8-4/1/lang2_5.png)

### Avantages :
**Cohérent/compréhensible :** les méthodes statiques lèvent toute ambiguïté sur la méthode qui sera utilisée au moment de l’exécution. La méthode correspond toujours au nœud d’interface utilisateur utilisé dans le graphe que l’utilisateur s’attend à voir appelé.

**Compatible :** amélioration de la corrélation entre le code et le programme visuel.

**Indicatif :** le passage d’entrées de liste hétérogènes aux nœuds entraîne désormais des valeurs non null pour les types qui sont acceptés par le nœud et des valeurs null pour les types qui n’implémentent pas le nœud. Les résultats sont plus prévisibles et donnent une meilleure indication des types autorisés pour le nœud.

### Mise en garde : ambiguïtés non résolues avec des méthodes surchargées

Étant donné que Dynamo prend en charge les surcharges de fonctions en général, il peut se tromper s’il existe une autre fonction surchargée avec le même nombre de paramètres. Par exemple, dans le graphe suivant, si nous connectons une valeur numérique à l’entrée `direction` de `Curve.Extrude` et un vecteur à l’entrée `distance` de `Curve.Extrude`. Les deux nœuds continuent de fonctionner, comme prévu. Dans ce cas, même si les nœuds sont compilés en méthodes statiques, le moteur ne peut toujours pas faire la différence au moment de l’exécution et choisit l’une ou l’autre en fonction du type d’entrée. ![](../images/8-4/1/lang2_6.png)
 
### Problèmes résolus :
Le passage à la sémantique des méthodes statiques a entraîné les effets collatéraux suivants qui méritent d’être mentionnés dans le cadre des changements relatifs au langage dans la version 2.0.

**1\. Perte de comportement polymorphe :**

Prenons un exemple de nœuds `TSpline` dans `ProtoGeometry` (notez que `TSplineTopology` hérite du type de base `Topology`) : le nœud `Topology.Edges` qui était auparavant compilé en méthode d’instance, `object.Edges`, est maintenant compilé en méthode statique, `Topology.Edges(object)`. L’appel précédent serait résolu de manière polymorphe à la méthode de classe dérivée `TsplineTopology.Edges` après l’appel d’une méthode sur le type d’objet à l’exécution. 

![](../images/8-4/1/lang2_7.png)

Alors que le nouveau comportement statique est forcé d’appeler la méthode de classe de base `Topology.Edges`. En conséquence, ce nœud a renvoyé la classe de base, des objets `Edge` au lieu des objets de classe dérivés de type `TSplineEdge`.
 
![](../images/8-4/1/lang2_8.png)

Il s’agissait d’une régression, car les nœuds `TSpline` en aval qui s’attendaient à `TSplineEdges` ont commencé à échouer. 

Le problème a été résolu par l’ajout d’une vérification d’exécution dans la logique d’appel de la méthode pour vérifier le type d’instance par rapport au type ou à un sous-type du premier paramètre de la méthode. Dans le cas d’une liste d’entrées, nous avons simplifié l’appel de la méthode pour vérifier simplement le type du premier élément. Ainsi, la solution finale était un compromis entre une recherche de méthode en partie statique et en partie dynamique.

**Nouveau comportement polymorphe en 2.0 :**

![](../images/8-4/1/lang2_9.png)

Dans ce cas, étant donné que le premier élément `a` est un `TSpline`, c’est la méthode dérivée `TSplineTopology.Edges` qui est invoquée au moment de l’exécution. Par conséquent, elle renvoie `null` pour `b` de type `Topology` de base. 

Dans le second cas, puisque `b` de type `Topology` général est le premier élément, la méthode `Topology.Edges` de base est appelée. Étant donné que `Topology.Edges` accepte également le type `TSplineTopology` dérivé, `a` en tant qu’entrée, elle renvoie `Edges` pour les deux entrées, `a` et `b`.

![](../images/8-4/1/lang2_10.png)
 
**2\. Régressions dues la production de listes externes redondantes**

Il existe une différence cruciale entre les méthodes d’instance et les méthodes statiques en ce qui concerne le comportement du guide de réplication. Avec les méthodes d’instance, les entrées à valeur unique avec des guides de réplication ne sont pas converties en listes alors qu’elles le sont pour les méthodes statiques.

Prenons l’exemple du nœud `Surface.PointAtParameter` avec inter-liaison, une entrée de surface unique et des tableaux de valeurs de paramètres `u` et `v`. La méthode d’instance est compilée comme suit :
```
surface<1>.PointAtParameter(u<1>, v<2>);
```
produit un réseau 2D de points.
 
La méthode statique est compilée comme suit :
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
produit une liste 3D de points avec une liste externe redondante.

Cet effet secondaire de la compilation des nœuds de l’interface utilisateur en méthodes statiques pourrait potentiellement provoquer des régressions dans ces cas d’utilisation. Ce problème a été résolu en désactivant la conversion des entrées de valeur unique en liste lorsqu’elles sont utilisées avec des guides de réplication/liaison (voir l’élément suivant).
 
**4\. Conversion de liste désactivée avec guides de réplication/liaison**

Dans la version 1.x, il y avait deux cas dans lesquels des valeurs uniques étaient converties en listes :

* Lorsque des entrées de rang inférieur étaient transmises dans des fonctions s’attendant à des entrées de rang supérieur
* Lorsque des entrées de rang inférieur étaient transmises dans des fonctions s’attendant au même rang, mais dans lesquelles les arguments d’entrée étaient enrichis avec des guides de réplication ou utilisaient la liaison

Dans la version 2.0, ce dernier cas n’est plus possible, car nous avons empêché la conversion de listes dans de tels scénarios.

Dans le graphe 1.x suivant, un niveau de guide de réplication pour chaque `y` et `z` a imposé une conversion en tableau de rang 1 pour chacun d’entre eux, c’est pourquoi le résultat avait un rang 3 (1 pour `x`, `y` et `z`). Au lieu de cela, un utilisateur s’attendrait à ce que le résultat soit de rang 1, car il n’est pas tout à fait évident que la présence de guides de réplication pour les entrées à valeur unique ajoutent des niveaux au résultat.
```
x = 1..5;
y = 0;
z = 0;
p = Point.ByCoordinates(x<1>, y<2>, z<3>); // cross-lacing
```

### Dynamo 1.x : liste 3D de points

![](../images/8-4/1/lang2_11.png)

Dans la version 2.0, la présence de guides de réplication pour chaque argument à valeur unique `y` et `z` ne provoque pas de conversion, ce qui crée une liste ayant la même dimension que la liste 1D d’entrée pour `x`. 

### Dynamo 2.0 : liste 1D de points

![](../images/8-4/1/lang2_12.png)

La régression mentionnée ci-dessus causée par la compilation de méthodes statiques avec la génération de listes externes redondantes a également été corrigée avec cette modification du langage.

Pour reprendre l’exemple ci-dessus, nous avons vu qu’un appel de méthode statique comme suit :
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>); 
```
a produit une liste 3D de points dans Dynamo 1.x. Cela s’est produit en raison de la conversion de la première surface d’argument à valeur unique en liste lorsqu’elle est utilisée avec un guide de réplication.
 
### Dynamo 1.x : conversion en liste des arguments avec guide de réplication

![](../images/8-4/1/lang2_13.png)

Dans la version 2.0, nous avons désactivé la conversion d’arguments à valeur unique en listes lorsqu’ils sont utilisés avec des guides de réplication ou des liaisons. Donc désormais, l’appel à :
```
Surface.PointAtParameter(surface<1>, u<2>, v<3>);
```
renvoie simplement une liste 2D, car la surface n’est pas convertie.

### Dynamo 2.0 : désactivation de la conversion en liste des arguments à valeur unique avec guide de réplication

![](../images/8-4/1/lang2_14.png)

Cette modification supprime l’ajout d’un niveau de liste redondant et résout également la régression causée par le passage à la compilation de méthodes statiques.

### Avantages :

**Lisible :** les résultats correspondent aux attentes des utilisateurs et sont plus faciles à comprendre

**Compatible :** les nœuds de l’interface utilisateur (avec option de liaison) et les CBN utilisant des guides de réplication donnent des résultats compatibles

**Cohérent :** 
* Les méthodes d’instance et les méthodes statiques sont cohérentes (cela résout les problèmes liés à la sémantique des méthodes statiques)
* Les nœuds avec des entrées et des arguments par défaut se comportent de manière cohérente (voir ci-dessous)

![](../images/8-4/1/lang2_15.png)

## 5\. Les variables sont immuables dans les nœuds de bloc de code afin d’empêcher la mise à jour associative 

DesignScript a toujours pris en charge deux paradigmes de programmation : la programmation associative et la programmation impérative. Le code associatif crée un graphe de dépendances à partir des instructions du programme où les variables dépendant les unes des autres. La mise à jour d’une variable peut déclencher la mise à jour de toutes les autres variables qui dépendent d’elle. Cela signifie que la séquence d’exécution des instructions dans un bloc associatif n’est pas basée sur leur ordre mais sur les relations de dépendance des variables.

Dans l’exemple suivant, la séquence d’exécution du code est : lignes 1 -> 2 -> 3 -> 2. Étant donné que `b` a une dépendance à `a`, lorsque `a` est mis à jour à la ligne 3, l’exécution revient à la ligne 2 pour mettre à jour `b` avec la nouvelle valeur de `a`. 
```
1. a = 1; 
2. b = a * 2;
3. a = 2;
```
En revanche, si le même code est exécuté dans un contexte impératif, les instructions sont exécutées dans un flux linéaire descendant. Les blocs de code impératifs sont donc adaptés à l’exécution séquentielle de constructions de code telles que les boucles et les conditions if-else.

### Ambiguïtés de la mise à jour associative :

**1\. Variables à dépendance cyclique :**

Dans certains cas, une dépendance cyclique entre les variables peut ne pas être aussi évidente que dans le cas suivant. Dans les cas où le compilateur ne peut pas détecter le cycle de manière statique, cela peut causer un cycle d’exécution indéfini.
```
a = 1;
b = a;
a = b;
```
**2\. Variables qui dépendent d’elles-mêmes :**

Si une variable dépend d’elle-même, sa valeur doit-elle s’accumuler ou revenir à sa valeur d’origine à chaque mise à jour ?
```
a = 1;
b = 1;
b = b + a + 2; // b = 4
a = 4;         // b = 10 or b = 7?
```
Dans cet exemple de géométrie, étant donné que le cube `b` dépend de lui-même ainsi que du cylindre `a`, le déplacement du curseur doit-il déplacer le trou le long du bloc ou doit-il créer un effet cumulatif de perforation de plusieurs trous le long de sa trajectoire à chaque mise à jour de la position du curseur ?

![](../images/8-4/1/lang2_16.gif)

**3\. Mise à jour des propriétés des variables :**

```
1: def foo(x: A) { x.prop = ...; return x; }
2: a = A.A();
3: p = a.prop;
4: a1 = foo(a);  // will p update?
```

**4\. Mise à jour des fonctions :**

```
1: def foo(v: double) { return v * 2; }// define “foo”
2: x = foo(5);                         // first definition of “foo” called
3: def foo(v: int) { return v * 3; }   // overload of “foo” defined, will x update?
```
Par expérience, nous avons constaté que la mise à jour associative ne s’avère pas utile dans les nœuds de bloc de code dans un contexte de graphe de flux de données basé sur des nœuds. Avant qu’un environnement de programmation visuelle ne soit disponible, la seule façon d’explorer les options était de modifier explicitement les valeurs de certaines variables dans le programme. Un programme textuel dispose de l’historique complet des mises à jour d’une variable, alors que dans un environnement de programmation visuelle, seule la dernière valeur d’une variable s’affiche. 

si cette fonction a été utilisée par des utilisateurs, c’est probablement sans le savoir, causant plus de tort que de bien. Nous avons donc décidé, dans la version 2.0, de masquer l’associativité dans l’utilisation des nœuds de blocs de code en rendant les variables immuables, tout en conservant la mise à jour associative en tant que fonctionnalité native du moteur DS uniquement. Ce changement s’inscrit également dans l’objectif de simplifier l’expérience de script pour les utilisateurs.

**La mise à jour associative est désactivée dans les CBN en empêchant la redéfinition des variables :** ![](../images/8-4/1/lang2_17.png)

**L’indexation de listes est toujours autorisée dans les blocs de code**

Une exception a été faite pour l’indexation de listes qui est toujours autorisée dans la version 2.0 avec l’affectation via l’opérateur d’index.

Dans l’exemple suivant, nous voyons que la liste `a` est initialisée, mais elle peut être remplacée ultérieurement par une affectation via l’opérateur d’index, et que toutes les variables dépendantes de `a` sont mises à jour de manière associative, comme le montre la valeur de `c`. De plus, l’aperçu du nœud affiche les valeurs mises à jour de `a` après la redéfinition d’un ou plusieurs de ses éléments.

![](../images/8-4/1/lang2_18.png)

## 6\. Les variables des blocs impératifs sont locales à la portée des blocs impératifs

Nous avons modifié les règles de portée impératives dans la version 2.0 afin de désactiver les scénarios de mise à jour complexes entre langages.

Dans Dynamo 1.x, la séquence d’exécution du script suivant est : lignes 1 -> 2 -> 4 -> 6 -> 4, où un changement est propagé de la portée de langage externe à la portée de langage interne. Étant donné que `y` est mis à jour dans le bloc associatif externe et que `x` dans le bloc impératif a une dépendance à `y`, le contrôle se déplace du programme associatif externe vers le langage impératif de la ligne 4. 
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3;
```

Dans l’exemple suivant, la séquence d’exécution est : lignes 1 -> 2 -> 4 -> 2, où le changement se propage de la portée de langage interne vers la portée externe.
```
1: x = 1;
2: y = x * 2;
3: [Imperative] {
4:     x = 3;
5: }
```
Les scénarios ci-dessus font référence à la mise à jour entre langages, qui, tout comme la mise à jour associative, ne sont pas très utiles dans les nœuds de bloc de code. Pour désactiver les scénarios complexes de mise à jour entre langages, nous avons rendu les variables locales dans la portée impérative. 

Dans l’exemple suivant dans Dynamo 2.0 :
```
x = 1;
y = x * 2;
i = [Imperative] {
     x = 3;
     return x;
}
```
* `x` défini dans le bloc Impératif est désormais local à la portée impérative
* Les valeurs de `x` et `y` dans la portée externe restent respectivement `1` et `2`

Toute variable locale à l’intérieur d’un bloc impératif doit être renvoyée si sa valeur doit être accessible dans une portée externe.

Examinez l’exemple suivant°:
```
1: x = 1;
2: y = 2;
3: [Imperative] {
4:     x = 2 * y;
5: }
6: y = 3; // x = 1, y = 3
```
* `y` est copié localement dans la portée impérative  
* La valeur de `x` locale à la portée impérative est `4`
* La valeur de mise à jour de `y` dans la portée externe continue de provoquer la mise à jour de `x` en raison de la mise à jour entre langages, mais est désactivée dans les blocs de code dans la version 2.0 en raison de l’immuabilité des variables
* La valeur de `x` et `y` dans la portée associative externe reste `1` et `2` respectivement

## 7\. Listes et dictionnaires

Dans Dynamo 1.x, les listes et les dictionnaires étaient représentés par un seul conteneur unifié, qui pouvait être indexé à la fois par un index entier et par une clé non intégrale. Le tableau suivant récapitule la séparation entre les listes et les dictionnaires dans la version 2.0 et les règles du nouveau type de données Dictionnaire :

|                               |    1.x                      |    2.0                                   |
| :---------------------------- | --------------------------- | ---------------------------------------- |
| **Initialisation de la liste**       | `a = {1, 2, 3};`            | `a = [1, 2, 3];`                         |
| **Liste vide**                | `a = {};`                   | `a = [];`                                |
| **Initialisation du dictionnaire** | **Peut être ajouté au même dictionnaire dynamiquement :** | **Peut uniquement créer de nouveaux dictionnaires :** |
|                           | `a = {};`                   | `a = {“foo” : 1, “bar” : 2};`            |
|                           | `a[“foo”] = 1;`             | `b = {“foo” : 1, “bar” : 2, “baz” : 3};` |
|                           | `a[“bar”] = 2;`             | `a = {};` // Crée un dictionnaire vide |
|                           | `a[“baz”] = 3;`             |                                          |
| **Indexation du dictionnaire**   | **Indexation de clés**            | **La syntaxe d’indexation reste la même**     |
|                           | `b = a[“bar”];`             | `b = a[“bar”];`                          |
| **Clés du dictionnaire**       | **N’importe quel type de clé était autorisé**  | **Seules les clés de chaîne sont autorisées**           |
|                           | `a = {};`                   | `a  = {“false” : 23, “point” : 12};`     |
|                           | `a[false] = 23;`            |                                          |
|                           | `a[point] = 12;`            |                                          |

### Nouvelle syntaxe de liste `[]`
La syntaxe d’initialisation de liste a changé : les accolades `{}` ont été remplacées par des crochets `[]` dans la version 2.0. Tous les scripts de la version 1.x sont automatiquement migrés vers la nouvelle syntaxe lorsqu’ils sont ouverts dans la version 2.0. 

**Remarque sur les attributs d’argument par défaut sur les nœuds Zero Touch :**

Notez cependant que la migration automatique ne fonctionne pas pour l’ancienne syntaxe utilisée dans les attributs d’argument par défaut. Les créateurs de nœuds doivent mettre à jour manuellement leurs définitions de méthode Zero Touch pour utiliser la nouvelle syntaxe dans les attributs d’argument par défaut `DefaultArgumentAttribute` si nécessaire.

**Remarque sur l’indexation :**

Le nouveau comportement d’indexation a changé dans certains cas. L’indexation dans une liste/dictionnaire avec une liste arbitraire d’index/clés à l’aide de l’opérateur `[]` préserve désormais la structure de la liste d’entrée d’index/clés. Auparavant, il renvoyait toujours une liste 1D de valeurs :
```
Given:
a = {“foo” : 1, “bar” : 2};

1.x:
b = a[{“foo”, {“bar”}}];
returns {1, 2}

2.0:
b = a[[“foo”, [“bar”]]];
returns [1, [2]];
```

### Syntaxe d’initialisation du dictionnaire :

Les `{}` (syntaxe des accolades) pour l’initialisation du dictionnaire ne peuvent être utilisées que dans le 
```
dict = {<key> : <value>, …}; 
```
format de paire clé-valeur où seule une chaîne est autorisée pour `<key>` et où plusieurs paires clé-valeur sont séparées par des virgules.

![](../images/8-4/1/lang2_19.png)

La méthode Zero Touch `Dictionary.ByKeysValues` constitue un moyen plus polyvalent d’initialiser un dictionnaire en transmettant une liste de clés et de valeurs et en bénéficiant de tous les avantages de l’utilisation de méthodes Zero Touch comme les guides de réplication, etc.

![](../images/8-4/1/lang2_20.png)

### Pourquoi n’avons-nous pas utilisé d’expressions arbitraires pour la syntaxe d’initialisation du dictionnaire ?

Nous avons envisagé d’utiliser des expressions arbitraires pour les clés dans la syntaxe d’initialisation clé-valeur du dictionnaire, mais cela pouvait entraîner des résultats vagues, en particulier lorsqu’une syntaxe comme `{keys : vals}` (`keys` et `vals` représentant des listes) entrait en conflit avec d’autres fonctionnalités du langage de DesignScript comme la réplication et produisait des résultats différents de ceux du nœud d’initialisation Zero Touch. 

Par exemple, il existe d’autres cas comme cette instruction où il est difficile de définir le comportement attendu :
```
dict = {["foo", "bar"] : "baz" };
```
Ajouter la syntaxe du guide de réplication, etc., et pas seulement les identifiants, irait à l’encontre de l’idée de simplicité du langage. 

Nous _pourrions_ étendre les clés de dictionnaire pour prendre en charge des expressions arbitraires à l’avenir, mais nous devrons également nous assurer que l’interaction avec d’autres fonctionnalités du langage est cohérente et intelligible, au risque d’augmenter la complexité plutôt que de rendre le système moins puissant mais simple à comprendre. Étant donné qu’il y a toujours une autre façon de résoudre ce problème en utilisant la méthode `Dictionary.ByKeysValues(keyList, valueList)`, ce qui n’est pas si difficile.

### Interaction avec les nœuds Zero Touch :

__1\. Le nœud Zero Touch renvoyant un dictionnaire .NET est renvoyé en tant que dictionnaire Dynamo__

**Examinez la méthode C# Zero Touch suivante qui renvoie un IDictionary :** ![](../images/8-4/1/lang2_21.png)

**La valeur de retour du nœud ZT correspondante est convertie en dictionnaire Dynamo :** ![](../images/8-4/1/lang2_22.png)

__2\. Les nœuds à retours multiples sont prévisualisés en tant que dictionnaires__

**Nœud Zero Touch renvoyant IDictionary avec l’attribut Multi-Return renvoie un dictionnaire Dynamo :** ![](../images/8-4/1/lang2_23.png)

![](../images/8-4/1/lang2_24.png)

__3\. Le dictionnaire Dynamo peut être transmis en tant qu’entrée dans le nœud Zero Touch acceptant le dictionnaire .NET__

**Méthode ZT avec un paramètre IDictionary :** ![](../images/8-4/1/lang2_25.png)

**Le nœud ZT accepte le dictionnaire Dynamo en tant qu’entrée :** ![](../images/8-4/1/lang2_26.png)

### Aperçu du dictionnaire dans les nœuds à retours multiples

Les dictionnaires sont des paires clé-valeur non ordonnées. Il n’est donc pas garanti que les aperçus de paires clé-valeur des nœuds renvoyant des dictionnaires soient triés dans l’ordre des valeurs de retour des nœuds. 

Nous avons cependant fait une exception pour les nœuds à retours multiples dont le `MultiReturnAttribute` est défini. Dans l’exemple suivant, `DateTime.Components` est un nœud « à retours multiples » et l’aperçu du nœud montre que ses paires clé-valeur sont dans le même ordre que celui des ports de sortie sur le nœud, qui correspond à l’ordre dans lequel les sorties sont spécifiées en fonction du`MultiReturnAttribute` de la définition du nœud.

Notez également que les aperçus des blocs de code ne sont pas ordonnés, contrairement au nœud d’interface utilisateur, car les informations du port de sortie (sous la forme d’un attribut MultiReturn) n’existent pas pour le nœud du bloc de code : ![](../images/8-4/1/lang2_27.png)
