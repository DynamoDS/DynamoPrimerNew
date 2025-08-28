# Couleur

La couleur est un excellent type de donnée permettant de créer des visuels convaincants et de restituer la différence dans le résultat obtenu à partir de votre programme visuel. Lorsque vous travaillez avec des données abstraites et des nombres variables, il est parfois difficile de remarquer les changements. Cette application est idéale pour les couleurs.

### Création de couleurs

Dans Dynamo, les couleurs sont créées à l’aide des entrées ARVB. Cet acronyme correspond aux canaux Alpha, Rouge, Vert et Bleu. L’alpha représente la _transparence_ de la couleur, tandis que les trois autres couleurs sont utilisées comme couleurs primaires pour générer de concert l’ensemble du spectre de couleurs.

| Icône                                     | Nom (Syntaxe)                 | Entrées  | Sorties |
| ---------------------------------------- | ----------------------------- | ------- | ------- |
| \![](<../images/5-1/ColorbyARGB (2).jpg>) | Couleur ARVB (**Color.ByARGB**) | A,R,G,B | color   |

### Interrogation des valeurs de couleur

Les couleurs du tableau ci-dessous recherchent les propriétés utilisées pour définir la couleur : Alpha, Rouge, Vert et Bleu. Étant donné que le nœud Color.Components donne les quatre sorties différentes, ce nœud est préférable pour l'interrogation des propriétés d'une couleur.

| Icône                                              | Nom (Syntaxe)                     | Entrées | Sorties    |
| ------------------------------------------------- | --------------------------------- | ------ | ---------- |
| \![](<../images/5-1/ColorAlpha(1)(1) (2) (2).jpg>) | Alpha (**Color.Alpha**)           | color  | A          |
| ![](../images/5-1/ColorRed.jpg)                   | Rouge (**Color.Red**)               | color  | R          |
| \![](<../images/5-1/ColorGreen(1)(1) (2) (1).jpg>) | Vert (**Color.Green**)           | color  | G          |
| ![](../images/5-1/ColorBlue.jpg)                  | Bleu (**Color.Blue**)             | color  | B          |
| \![](<../images/5-1/ColorComponent (2).jpg>)       | Composants (**Color.Components**) | color  | A,R,G,B |

Les couleurs du tableau ci-dessous correspondent à l’**espace de couleurs HSB**. Diviser la couleur en teinte, en saturation et en luminosité permet probablement de l’interpréter de façon plus intuitive : quelle couleur choisir ? Quel niveau de saturation de la couleur choisir ? Et quel niveau d'intensité de la couleur choisir ? Il s'agit respectivement de la répartition de la teinte, de la saturation et de la luminosité.

| Icône                                         | Nom (Syntaxe)                     | Entrées | Sorties    |
| -------------------------------------------- | --------------------------------- | ------ | ---------- |
| ![](../images/5-1/ColorHue.jpg)              | Teinte (**Color.Hue**)               | color  | Teinte        |
| \![](<../images/5-1/ColorSaturation (2).jpg>) | Saturation (**Color.Saturation**) | color  | Saturation |
| \![](<../images/5-1/ColorBrightness (2).jpg>) | Luminosité (**Color.Brightness**) | color  | Luminosité |

### Intervalles de couleurs

L’intervalle de couleurs est semblable au nœud **Remap Range** de l’exercice [\#part-ii-from-logic-to-geometry](3-logic.md#part-ii-from-logic-to-geometry "mention") : il remappe une liste de nombres dans un autre domaine. Au lieu d’effectuer le mappage vers un domaine _nombre_, il mappe vers un _dégradé de couleurs_ basé sur des numéros d’entrée allant de 0 à 1.

Le nœud actuel fonctionne bien, mais il peut être un peu délicat de tout faire fonctionner la première fois. La meilleure façon de se familiariser avec le dégradé de couleurs est de le tester de manière interactive. Vous allez faire un exercice rapide pour découvrir comment configurer un dégradé avec des couleurs de sortie correspondant aux nombres.

![](../images/5-3/5/color-colorrange.jpg)

> 1. Définir trois couleurs : à l’aide d’un nœud **Code Block**, définissez _rouge, vert_ et _bleu_ en connectant les combinaisons appropriées de _0_ et _255_.
> 2. **Créer une liste :** fusionnez les trois couleurs dans une liste.
> 3. Définir les index : créez une liste pour définir les positions des poignées de chaque couleur (de 0 à 1). Notez la valeur 0.75 pour le vert. La couleur verte est ainsi placée aux trois quarts du dégradé horizontal dans le curseur de l’intervalle de couleurs.
> 4. **Code Block** : valeurs d’entrée (entre 0 et 1) pour convertir en couleurs.

### Aperçu des couleurs

Le nœud **Display.ByGeometry** permet de colorer la géométrie dans la fenêtre Dynamo. Ce nœud est utile pour séparer différents types de géométrie, présenter un concept paramétrique ou définir une légende d’analyse pour la simulation. Les entrées sont simples : geometry et color. Pour créer un dégradé comme l’image ci-dessus, l’entrée color est connectée au nœud **Color** **Range**.

![](../images/5-3/5/color-colorpreview.jpg)

### Couleur sur les surfaces

Le nœud **Display.BySurfaceColors** permet de mapper des données sur une surface grâce à la couleur. Cette fonctionnalité présente de nombreuses possibilités pour visualiser des données obtenues par analyse discrète, comme le soleil, l’énergie et la proximité. Dans Dynamo, l'application d'une couleur à une surface revient à appliquer une texture à un matériau dans d'autres environnements de CAO. Dans le court exercice ci-dessous, vous allez découvrir comment utiliser cet outil.

![](../images/5-3/5/12\(1\).jpg)

## Exercice

### Hélice de base avec couleurs

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-3/5/Building Blocks of Programs - Color.dyn" %}

Cet exercice est axé sur le contrôle paramétrique de la couleur parallèlement à la géométrie. La géométrie est une hélice de base, définie ci-dessous à l’aide d’un nœud **Code Block**. C’est une méthode simple et rapide servant à créer une fonction paramétrique. Étant donné que vous vous intéressez à la couleur (plutôt qu’à la géométrie), le bloc de code vous permet de créer efficacement l’hélice sans encombrer la zone de dessin. Vous utiliserez le bloc de code plus fréquemment lorsque le guide traitera de matériaux plus avancés.

![](../images/5-3/5/color-basichelixwithcolors01.jpg)

> 1. **Bloc de code :** définissez les deux blocs de code avec les formules susmentionnées. Il s’agit d’une méthode paramétrique rapide de création d’une clothoïde.
> 2. **Point.ByCoordinates** : connectez les trois sorties du nœud Code Block aux coordonnées du nœud.

Un réseau de points est maintenant visible, créant une hélice. L’étape suivante consiste à créer une courbe passant par les points afin de pouvoir visualiser l’hélice.

![](../images/5-3/5/color-basichelixwithcolors02.jpg)

> 1. **PolyCurve.ByPoints :** connectez la sortie **Point.ByCoordinates** à l’entrée _points_ du nœud. Vous obtenez une courbe hélicoïdale.
> 2. **Curve.PointAtParameter** : connectez la sortie **PolyCurve.ByPoints** à l’entrée _curve_. L’objectif de cette étape est de créer un point d’attraction paramétrique qui glisse le long de la courbe. Puisque la courbe évalue un point au paramètre, vous devez entrer une valeur _param_ comprise entre 0 et 1.
> 3. **Number Slider :** après l’ajout à la zone de dessin, remplacez la valeur _min_ par _0.0_, la valeur _max_ par _1.0_ et la valeur _step_ par _.01_. Connectez la sortie du curseur à l’entrée _param_ pour **Curve.PointAtParameter**. Un point apparaît désormais sur la longueur de l’hélice, représenté par un pourcentage du curseur (0 au point de départ, 1 au point d’arrivée).

Une fois le point de référence créé, vous allez maintenant comparer la distance entre le point de référence et les points d'origine définissant l'hélice. Cette valeur de distance détermine la géométrie ainsi que la couleur.

![](../images/5-3/5/color-basichelixwithcolors03.jpg)

> 1. **Geometry.DistanceTo :** connectez la sortie **Curve.PointAtParameter** à l’_entrée_. Connectez **Point.ByCoordinates** à l’entrée geometry.
> 2. **Watch :** la sortie obtenue affiche une liste des distances entre chaque point hélicoïdal et le point de référence le long de la courbe.

L’étape suivante consiste à piloter les paramètres avec la liste des distances entre les points hélicoïdaux et le point de référence. Ces valeurs de distance permettent de définir les rayons d’une série de sphères le long de la courbe. Pour conserver une taille adaptée aux sphères, vous devez _remapper_ les valeurs de distance.

![](../images/5-3/5/color-basichelixwithcolors04.jpg)

> 1. **Math.RemapRange :** connectez la sortie **Geometry.DistanceTo** à l’entrée numbers.
> 2. **Bloc de code :** connectez un bloc de code avec une valeur de _0.01_ à l’entrée _newMin_ et un bloc de code avec une valeur de _1_ à l’entrée _newMax_.
> 3. **Watch :** connectez la sortie **Math.RemapRange** à un nœud et la sortie **Geometry.DistanceTo** à un autre nœud. Comparez les résultats.

Cette étape a permis de remapper la liste de distance pour qu’elle soit plus petite. Vous pouvez modifier les valeurs _newMin_ et _newMax_ comme bon vous semble. Les valeurs sont remappées et auront le même _rapport de distribution_ sur le domaine.

![](../images/5-3/5/color-basichelixwithcolors05.jpg)

> 1. **Sphere.ByCenterPointRadius :** connectez la sortie **Math.RemapRange** à l’entrée _radius_ et la sortie **Point.ByCoordinates** d’origine à l’entrée _centerPoint_.

Modifiez la valeur du curseur de numérotation et observez la mise à jour de la taille des sphères. Vous avez désormais un gabarit paramétrique.

![](../images/5-3/5/color-basichelixwithcolors06.gif)

La taille des sphères montre le réseau paramétrique défini par un point de référence le long de la courbe. Utilisez le même concept pour le rayon des sphères afin de contrôler leur couleur.

![](../images/5-3/5/color-basichelixwithcolors07.jpg)

> 1. **Color Range :** à ajouter en haut de la zone de dessin. Lorsque vous passez le curseur sur l’entrée _value_, vous remarquez que les nombres demandés sont compris entre 0 et 1. Vous devez remapper les numéros de la sortie **Geometry.DistanceTo** afin qu’ils soient compatibles avec ce domaine.
> 2. **Sphere.ByCenterPointRadius :** pour le moment, désactivez l’aperçu sur ce nœud (_cliquez avec le bouton droit de la souris > Aperçu_).

![](../images/5-3/5/color-basichelixwithcolors08.jpg)

> 1. **Math.RemapRange :** ce processus devrait vous sembler familier. Connectez la sortie **Geometry.DistanceTo** à l’entrée numbers.
> 2. **Bloc de code :** comme lors d’une étape précédente, créez une valeur de _0_ pour l’entrée _newMin_ et une valeur de _1_ pour l’entrée _newMax_. Dans ce cas, vous pouvez définir deux sorties à partir d’un bloc de code.
> 3. **Color Range :** connectez la sortie **Math.RemapRange** à l’entrée _value_.

![](../images/5-3/5/color-basichelixwithcolors09.jpg)

> 1. **Color.ByARGB :** cette action permet de créer deux couleurs. Bien que ce processus puisse paraître délicat, il est identique aux couleurs RVB d’un autre logiciel. Vous allez simplement utiliser la programmation visuelle pour le faire.
> 2. **Bloc de code :** créez deux valeurs de _0_ et _255_. Connectez les deux sorties aux deux entrées **Color.ByARGB** conformément à l’image ci-dessus (ou créez vos deux couleurs préférées).
> 3. **Color Range :** l’entrée _colors_ demande une liste de couleurs. Vous devez créer cette liste à partir des deux couleurs créées à l’étape précédente.
> 4. **List.Create :** fusionnez les deux couleurs dans une liste. Connectez la sortie à l’entrée _colors_ pour **Color Range**.

![](../images/5-3/5/color-basichelixwithcolors10.jpg)

> 1. **Display.ByGeometryColor :** connectez **Sphere.ByCenterPointRadius** à l’entrée _geometry_ et _Color Range_ à l’entrée _color_. Vous avez maintenant un dégradé lisse sur le domaine de la courbe.

Si vous modifiez la valeur de **Number Slider** lors d’une étape précédente de la configuration, les couleurs et les tailles sont mises à jour. Dans ce cas, les couleurs et la taille du rayon sont directement liées : vous avez désormais un lien visuel entre deux paramètres !

![](../images/5-3/5/color-basichelixwithcolors11.gif)

### Exercice - Couleur sur surfaces

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="../datasets/5-3/5/BuildingBlocks of Programs - ColorOnSurface.zip" %}

Tout d’abord, créez (ou référencez) une surface à utiliser comme entrée pour le nœud **Display.BySurfaceColors**. Dans cet exemple, vous allez effectuer un lissage entre une courbe sinus et cosinus.

![](../images/5-3/5/color-coloronsurface01.jpg)

> 1. Ce groupe de nœuds crée des points le long de l’axe Z, puis les déplace selon les fonctions de sinus et cosinus. Les deux listes de points sont ensuite utilisées pour générer des courbes NURBS.
> 2. **Surface.ByLoft** : générez une surface interpolée entre la liste des courbes NURBS.

![](../images/5-3/5/color-coloronsurface02.jpg)

> 1. **File Path** : sélectionnez un fichier image à échantillonner pour les données de pixel en aval.
> 2. Utilisez **File.FromPath** pour convertir le chemin d’accès au fichier en fichier, puis le transmettre à **Image.ReadFromFile** pour générer une image à des fins d’échantillonnage.
> 3. **Image.Pixels** : entrez une image et indiquez une valeur d’exemple à utiliser le long des dimensions x et y de l’image.
> 4. **Curseur** : fournit des valeurs d’exemple pour **Image.Pixels**.
> 5. **Display.BySurfaceColors** : mappez le réseau de valeurs de couleur sur la surface le long de X et Y respectivement.

Aperçu rapproché de la surface de sortie avec résolution des échantillons de 400 x 300

![](../images/5-3/5/color-coloronsurface03.jpg)
