# Points

## Points dans Dynamo

### Qu’est-ce qu’un point ?

Un [point](5-3\_points.md#point-as-coordinates) est défini uniquement par une ou plusieurs valeurs, appelées coordonnées. Le nombre de valeurs de coordonnées requises pour définir le point dépend du système de coordonnées ou du contexte dans lequel il se trouve.

### Point 2D et 3D

Le type de point le plus courant dans Dynamo existe dans notre système de coordonnées général en trois dimensions et possède trois coordonnées [X,Y,Z] (Points 3D dans Dynamo).

![](../images/5-2/3/points-3dpointindynamo.jpg)

Un point 2D dans Dynamo possède deux coordonnées [X, Y].

![](../images/5-2/3/points-2dpointindynamo.jpg)

### Point sur des courbes et des surfaces

Les paramètres des courbes et des surfaces sont continus et s’étendent au-delà de l’arête de la géométrie donnée. Étant donné que les formes qui définissent l’espace des paramètres résident dans un système de coordonnées général en trois dimensions, vous pouvez toujours convertir une coordonnée paramétrique en coordonnée « universelle ». Le point [0.2, 0.5] sur la surface, par exemple, est le même que le point [1.8, 2.0, 4.1] en coordonnées universelles.

![](../images/5-2/3/points-xyzvscoordsysvsuv.jpg)

> 1. Point dans les coordonnées XYZ universelles supposées
> 2. Point relatif à un système de coordonnées donné (cylindrique)
> 3. Point comme coordonnée UV sur une surface

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-2/3/Geometry for Computational Design - Points.dyn" %}

## Approfondir...

Si la géométrie est la langue d'un modèle, les points en sont l'alphabet. Les points constituent la base sur laquelle est créée la géométrie. Il faut au moins deux points pour créer une courbe, au moins trois points pour créer un polygone ou une face maillée, etc. La définition de la position, de l’ordre et de la relation entre les points (essayez une fonction de sinus) nous permet de définir une géométrie d’ordre supérieur, comme des cercles ou des courbes.

![Point à courbe](../images/5-2/3/PointsAsBuildingBlocks-1.jpg)

> 1. Un cercle utilisant les fonctions `x=r*cos(t)` et `y=r*sin(t)`
> 2. Une courbe sinusoïdale utilisant les fonctions `x=(t)` et `y=r*sin(t)`

### Points en tant que coordonnées

Les points peuvent également exister dans un système de coordonnées bidimensionnel. La convention comporte différentes lettres de notation selon le type d’espace avec lequel vous travaillez. Il se peut que vous utilisiez [X,Y] sur un plan ou [U,V] pour une surface.

![Points en tant que coordonnées](../images/5-2/3/Coordinates.jpg)

> 1. Un point dans un système de coordonnées euclidien : [X,Y,Z]
> 2. Un point dans un système de coordonnées de paramètres de courbe : [t]
> 3. Un point dans un système de coordonnées de paramètres de surface : [U,V]
