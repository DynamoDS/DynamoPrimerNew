# Création d'un nœud personnalisé

Dynamo propose plusieurs méthodes de création de nœuds personnalisés. Vous pouvez créer des nœuds personnalisés à partir de zéro, à partir d'un graphique existant ou de façon explicite en C#. Dans cette section, vous allez découvrir comment créer un nœud personnalisé dans l'interface utilisateur de Dynamo à partir d'un graphique existant. Cette méthode est idéale pour nettoyer l'espace de travail, ainsi que pour regrouper une séquence de nœuds à réutiliser ailleurs.

## Exercice : Nœuds personnalisés pour le mappage UV

### Partie I : Commencer par un graphique

Dans l'image ci-dessous, un point d'une surface est mappé sur une autre surface à l'aide des coordonnées UV. Ce concept va vous permettre de créer une surface de panneaux qui référence des courbes dans le plan XY. Dans le cadre de la construction de panneaux, vous allez créer ici des panneaux quadrilatéraux. En utilisant la même logique, vous pouvez créer un grand nombre de panneaux avec le mappage UV. C'est une excellente opportunité pour le développement de nœuds personnalisés, car vous pourrez répéter un processus similaire plus facilement dans ce graphique ou dans d'autres workflows Dynamo.

![](<../images/6-1/2/custom node for uv mapping pt I - 01.jpg>)

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/6-1/2/UV-CustomNode.zip" %}

Commencez par créer un graphique à imbriquer dans un nœud personnalisé. Dans cet exemple, vous allez créer un graphique qui mappe des polygones d'une surface de base vers une surface cible, à l'aide de coordonnées UV. Ce processus de mappage UV est fréquemment utilisé, ce qui en fait un bon candidat pour un nœud personnalisé. Pour plus d’informations sur les surfaces et l’espace UV, reportez-vous à la page [Surface](../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/5-surfaces.md). Le graphique complet est _UVmapping\_Custom-Node.dyn_ à partir du fichier .zip téléchargé ci-dessus.

![](<../images/6-1/2/custom node for uv mapping pt I - 02.jpg>)

> 1. **Code Block :** utilisez cette ligne pour créer une plage de 10 nombres entre -45 et 45 `45..45..#10;`
> 2. **Point.ByCoordinates :** connectez la sortie du nœud **Code Block** aux entrées « x » et « y » et définissez la liaison sur Référence croisée. Vous devez maintenant avoir une grille de points.
> 3. **Plane.ByOriginNormal :** connectez la sortie _"Point"_ à l'entrée _"origin"_ pour créer un plan au niveau de chacun des points. Le vecteur normal par défaut de (0,0,1) est utilisé.
> 4. **Rectangle.ByWidthLength :** connectez les plans de l’étape précédente à l’entrée _« plan »_, puis utilisez un nœud **Code Block** avec une valeur de _10_ pour spécifier la largeur et la longueur.

Vous devez maintenant voir une grille de rectangles. Mappez ces rectangles sur une surface cible à l'aide des coordonnées UV.

![](<../images/6-1/2/custom node for uv mapping pt I - 03.jpg>)

> 1. **Polygon.Points :** connectez la sortie du nœud **Rectangle.ByWidthLength** de l’étape précédente à l’entrée _« polygon »_ pour extraire les points de coin de chaque rectangle. Il s'agit des points que vous allez mapper sur la surface cible.
> 2. **Rectangle.ByWidthLength :** utilisez un nœud **Code Block** avec une valeur de _100_ pour spécifier la largeur et la longueur d’un rectangle. Il s'agit de la limite de la surface de base.
> 3. **Surface.ByPatch :** connectez le nœud **Rectangle.ByWidthLength** de l’étape précédente à l’entrée _« closedCurve »_ pour créer une surface de base.
> 4. **Surface.UVParameterAtPoint :** connectez la sortie _"Point"_ du nœud **Polygon.Points** et la sortie _"Surface"_ du nœud **Surface.ByPatch** pour renvoyer le paramètre UV à chaque point.

Maintenant que vous avez une surface de base et un ensemble de coordonnées UV, importez une surface cible et mappez les points entre les surfaces.

![](<../images/6-1/2/custom node for uv mapping pt I - 04.jpg>)

> 1. **File Path :** sélectionnez le chemin d'accès au fichier de la surface à importer. Le type de fichier doit être .SAT. Cliquez sur le bouton _« Parcourir... »_ et accédez au fichier _UVmapping\_srf.sat_ à partir du fichier .zip téléchargé ci-dessus.
> 2. **Geometry.ImportFromSAT :** connectez le chemin d'accès au fichier pour importer la surface. Vous devez voir la surface importée dans l'aperçu de la géométrie.
> 3. **UV :** connectez la sortie du paramètre UV à un nœud _UV.U_ et à un nœud _UV.V_.
> 4. **Surface.PointAtParameter :** connectez la surface importée ainsi que les coordonnées u et v. Vous devez maintenant voir une grille de points 3D sur la surface cible.

La dernière étape consiste à utiliser les points 3D pour construire des corrections de surface rectangulaires.

![](<../images/6-1/2/custom node for uv mapping pt I - 05.jpg>)

> 1. **PolyCurve.ByPoints :** connectez les points de la surface pour construire une polycourbe à travers les points.
> 2. **Boolean :** ajoutez un nœud **Boolean** à l’espace de travail, connectez-le à l’entrée _« connectLastToFirst »_ et sélectionnez True pour fermer les polycourbes. Vous devez maintenant voir des rectangles mappés sur la surface.
> 3. **Surface.ByPatch :** connectez les polycourbes à l'entrée _"closedCurve"_ pour construire des corrections de surface.

### Partie II : Du graphique au nœud personnalisé

Sélectionnez les nœuds à imbriquer dans un nœud personnalisé, en choisissant les entrées et les sorties de votre nœud. Étant donné que votre nœud personnalisé doit être aussi flexible que possible, il doit être en mesure de mapper des polygones, pas seulement des rectangles.

Sélectionnez les nœuds suivants (en commençant par le nœud Polygon.Points), cliquez avec le bouton droit de la souris sur l’espace de travail et choisissez « Créer un nœud personnalisé ».

![](<../images/6-1/2/custom node for uv mapping pt II - 01.jpg>)

Dans la boîte de dialogue Propriétés du nœud personnalisé, attribuez un nom, une description et une catégorie au nœud personnalisé.

![](<../images/6-1/2/custom node for uv mapping pt II - 02.jpg>)

> 1. Nom : MapPolygonsToSurface
> 2. Description : mapper le ou les polygones d’une surface de base à une surface cible
> 3. Catégorie des packages complémentaires : Geometry.Curve

Le nœud personnalisé a considérablement nettoyé l'espace de travail. Notez que les entrées et les sorties ont été nommées en fonction des nœuds d'origine. Modifiez le nœud personnalisé pour rendre les noms plus descriptifs.

![](<../images/6-1/2/custom node for uv mapping pt II - 03.jpg>)

Cliquez deux fois sur le nœud personnalisé pour le modifier. Un espace de travail à l'arrière-plan jaune représentant l'intérieur du nœud s'affiche.

![](<../images/6-1/2/custom node for uv mapping pt II - 04.jpg>)

> 1. **Entrées :** remplacez les noms d'entrée par _baseSurface_ et _targetSurface_.
> 2. **Sorties :** ajoutez une sortie supplémentaire pour les polygones mappés.

Enregistrez le nœud personnalisé et revenez à l'espace de travail de base. Notez que le nœud **MapPolygonsToSurface** reflète les modifications apportées.

![](<../images/6-1/2/custom node for uv mapping pt II - 05.jpg>)

Vous pouvez également renforcer la robustesse du nœud personnalisé en ajoutant des **commentaires personnalisés**. Les commentaires peuvent vous aider à indiquer les types d'entrée et de sortie ou à expliquer la fonctionnalité du nœud. Des commentaires s'affichent lorsque l'utilisateur place le curseur sur une entrée ou une sortie d'un nœud personnalisé.

Cliquez deux fois sur le nœud personnalisé pour le modifier. Cette action permet de rouvrir l'espace de travail à l'arrière-plan jaune.

![](<../images/6-1/2/custom node for uv mapping pt II - 06.jpg>)

> 1. Commencez par modifier le **bloc de code** Input. Pour commencer un commentaire, saisissez "//" suivi du texte du commentaire. Tapez tout ce qui peut aider à clarifier le nœud : ici, c'est la _surface cible_ qui est décrite.
> 2. Définissez également la valeur par défaut pour _inputSurface_ en définissant le type d'entrée sur une valeur équivalente. Ici, la valeur par défaut est définie sur le jeu **Surface.ByPatch** d’origine.

Vous pouvez également appliquer les commentaires aux sorties.

![](<../images/6-1/2/custom node for uv mapping pt II - 07.jpg>)

> Modifiez le texte dans le bloc de code Output. Saisissez « // » suivi du texte du commentaire. Ici, une description plus détaillée est ajoutée pour clarifier les sorties de _Polygons_ et de _surfacePatches_.

![](<../images/6-1/2/custom node for uv mapping pt II - 08.jpg>)

> 1. Placez le curseur sur les entrées de nœud personnalisé pour afficher les commentaires.
> 2. Étant donné que la valeur par défaut est définie sur _inputSurface_, vous pouvez également exécuter la définition sans entrée de surface.
