# Personnalisation

Bien que vous ayez précédemment étudié la modification d'un volume de construction de base, vous devez approfondir le lien Dynamo/Revit en modifiant un grand nombre d'éléments en une seule opération. La personnalisation à grande échelle devient plus complexe, car les structures de données nécessitent des opérations de listes plus avancées. Toutefois, les principes sous-jacents de leur exécution sont fondamentalement les mêmes. Examinez certaines possibilités d'effectuer des analyses à partir d'un ensemble de composants adaptatifs.

### Emplacement du point

Imaginez que vous avez créé une série de composants adaptatifs et que vous souhaitez modifier les paramètres en fonction de leurs emplacements de point.
Les points, par exemple, peuvent définir un paramètre d'épaisseur lié à la zone de l'élément. Ils peuvent aussi définir un paramètre d'opacité lié à l'exposition solaire tout au long de l'année. Dynamo permet la connexion de l'analyse aux paramètres en quelques étapes simples. Dans l'exercice ci-dessous, vous allez explorer une version de base.

![](<./images/5/customizing - point location.jpg>)

> Interrogez les points adaptatifs d'un composant adaptatif sélectionné à l'aide du nœud **AdaptiveComponent.Locations**. Cela vous permet de travailler avec une version abstraite d'un élément Revit pour l'analyse.

En extrayant l'emplacement des points des composants adaptatifs, vous pouvez exécuter une série d'analyses pour cet élément. Par exemple, un composant adaptatif à quatre points vous permet d'étudier l'écart par rapport au plan d'un panneau donné.

### Analyse de l'orientation du soleil

![](<./images/5/customizing - solar orientation analysis.jpg>)

> Utilisez la fonction de remappage pour mapper un jeu de données dans une plage de paramètres. Il s'agit d'un outil fondamental utilisé dans un modèle paramétrique, que vous allez découvrir dans l'exercice ci-dessous.

Avec Dynamo, vous pouvez utiliser les emplacements des points des composants adaptatifs pour créer un plan d'ajustement optimal pour chaque élément. Vous pouvez également interroger la position du soleil dans le fichier Revit et étudier l'orientation relative du plan par rapport au soleil en comparaison avec les autres composants adaptatifs. Étudiez ce cas dans l'exercice ci-dessous en créant un environnement de toiture algorithmique.

## Exercice

> Téléchargez le fichier d’exemple en cliquant sur le lien ci-dessous.
>
> Vous trouverez la liste complète des fichiers d'exemple dans l'annexe.

{% file src="./datasets/5/Revit-Customizing.zip" %}

Cet exercice fournit des informations sur les techniques présentées dans la section précédente. Dans ce cas, définissez une surface paramétrique à partir d'éléments Revit, instanciant des composants adaptatifs à quatre points, puis modifiez-les en fonction de l'orientation par rapport au soleil.

![](<./images/5/customizing - exercise 01.jpg>)

> 1. Sélectionnez d'abord deux arêtes avec le nœud _"Select Edge"_. Les deux arêtes sont les longues travées de l'atrium.
> 2. Combinez les deux arêtes dans une liste avec le nœud _List.Create_.
> 3. Créez une surface entre les deux arêtes avec un noeud _surface.ByLoft_.

![](<./images/5/customizing - exercise 02.jpg>)

> 1. À l’aide du nœud _Code Block_, définissez une plage comprise entre 0 et 1 avec 10 valeurs équidistantes : `0..1..#10;`
> 2. Connectez le nœud _Code Block_ aux entrées \*u \*et _v_ d’un nœud _Surface.PointAtParameter_, puis connectez le nœud _Surface.ByLoft_ à l’entrée _surface_. Cliquez avec le bouton droit de la souris sur le nœud et définissez la _liaison_ sur _Produit cartésien_. Cette action permet de créer une grille de points sur la surface.

Cette grille de points sert de points de contrôle pour une surface définie de manière paramétrique. Vous devez extraire les positions u et v de chacun de ces points afin de pouvoir les relier à une formule paramétrique et conserver la même structure de données. Pour ce faire, vous pouvez interroger les emplacements des paramètres des points que vous venez de créer.

![](<./images/5/customizing - exercise 03.jpg>)

> 1. Ajoutez un nœud _Surface.ParameterAtPoint_ à la zone de dessin et connectez les entrées comme indiqué ci-dessus.
> 2. Interrogez les valeurs _u_ de ces paramètres avec le nœud UV.U.
> 3. Interrogez les valeurs _v_ de ces paramètres avec le nœud UV.V.
> 4. Les sorties montrent les valeurs _u_ et _v_ correspondantes pour chaque point de la surface. Vous disposez à présent d'une plage de _0_ à _1_ pour chaque valeur dans la structure de données appropriée. Vous êtes prêts à appliquer un algorithme paramétrique.

![](<./images/5/customizing - exercise 04.jpg>)

> 1. Ajoutez un nœud _Block Code_ dans la zone de dessin et entrez le code : `Math.Sin(u*180)*Math.Sin(v*180)*w;`. Il s’agit d’une fonction paramétrique qui crée un sinus à partir d’une surface plane.
> 2. Connectez _UV.U_ à l’entrée _u_ et UV.V à l’entrée _v_.
> 3. Étant donné que l'entrée _w_ représente l'_amplitude_ de la forme, joignez-y un _curseur de numérotation_.

![](<./images/5/customizing - exercise 05.jpg>)

> 1. La liste des valeurs définies par l'algorithme est maintenant disponible. Utilisez cette liste de valeurs pour déplacer les points vers le haut dans la direction _+Z_. À l’aide de _Geometry.Translate_, connectez le nœud \*Code Block \*à _zTranslation_ et le nœud _Surface.PointAtParameter_ à l’entrée _geometry_. Les nouveaux points doivent s'afficher dans l'aperçu Dynamo.
> 2. Enfin, créez une surface avec le nœud _NurbsSurface.ByPoints_ en connectant le nœud de l'étape précédente à l'entrée des points. Vous obtenez une surface paramétrique. N'hésitez pas à déplacer le curseur pour observer la taille du monticule diminuer ou augmenter.

En ce qui concerne la surface paramétrique, vous devez définir un moyen de la paneliser afin de mettre en réseau les composants adaptatifs à quatre points. Étant donné que Dynamo ne dispose pas de fonctionnalités prêtes à l'emploi pour la panelisation des surfaces, contactez la communauté pour des packages Dynamo utiles.

![](<./images/5/customizing - exercise 06.jpg>)

> 1. Accédez à _Packages > Rechercher un package..._
> 2. Recherchez _« LunchBox »_ et installez _« LunchBox for Dynamo »_. Il s'agit d'un ensemble d'outils très utiles pour les opérations de géométrie telles que celle-ci.

> 1. Après le téléchargement, vous disposez maintenant d'un accès complet à la suite LunchBox. Recherchez _"Quad Grid"_ et sélectionnez _"LunchBox Quad Grid By Face"_. Connectez la surface paramétrique à l'entrée _surface_ et définissez les divisions _U_ et _V_ sur _15_. Vous devriez voir apparaître une surface à quatre panneaux dans l'aperçu Dynamo.

> Si vous souhaitez en savoir plus sur la configuration, cliquez deux fois sur le nœud _Lunch Box_ pour voir comment il est créé.

> Dans Revit, examinez rapidement le composant adaptatif utilisé ici. Pas besoin de suivre, mais c'est le panneau de toit à instancier. Il s'agit d'un composant adaptatif à quatre points, qui est une représentation grossière d'un système ETFE. L'ouverture du vide au centre se trouve sur un paramètre appelé _"ApertureRatio"_.

> 1. Étant donné que vous allez instancier un grand nombre de géométries dans Revit, veillez à définir le solveur Dynamo sur _Manuel_.
> 2. Ajoutez un nœud _Family Types_ à la zone de dessin et sélectionnez _"ROOF-PANEL-4PT"_.
> 3. Ajoutez un nœud _AdaptiveComponent.ByPoints_ à la zone de dessin, connectez les points _Panel Pts_ de la sortie _"LunchBox Quad Grid by Face"_ à l'entrée _points_. Connectez le nœud _Family Types_ à l'entrée _FamilySymbol_.
> 4. Cliquez sur _Exécuter_. La création de la géométrie nécessite un peu de _temps_ sur Revit. Si l'opération est trop longue, réduisez la valeur du bloc de code qui est égale à _15_. Cela permet de réduire le nombre de panneaux sur le toit.

_Remarque : si Dynamo prend trop de temps pour calculer les nœuds, vous pouvez utiliser la fonctionnalité "geler" du nœud pour interrompre l'exécution des opérations Revit lorsque vous développez votre graphique. Pour plus d'informations sur le gel des nœuds, consultez la section "Gel" du chapitre Solides._

> Dans Revit, vous avez le réseau de panneaux sur le toit.

> En zoomant, vous pouvez mieux observer leurs qualités de surface.

### Analyse

> 1. À partir de l'étape précédente, reprenez l'ouverture de chaque panneau en fonction de son exposition au soleil. Dans Revit, en zoomant et en sélectionnant un panneau, vous pouvez constater la présence d'un paramètre appelé _"Aperture Ratio"_ dans la barre des propriétés. La famille est configurée de façon à ce que l'ouverture s'étende, approximativement, de _0,05_ à _0,45_.

> 1. Si vous activez la trajectoire d'ensoleillement, vous pouvez voir l'emplacement actuel du soleil dans Revit.

> 1. Vous pouvez référencer l'emplacement du soleil à l'aide du nœud _SunSettings.Current_.

1. Connectez l'entrée SunSettings à _Sunsetting.SunDirection_ pour obtenir le vecteur solaire.
2. À partir des points _Panel Pts_ utilisés pour créer les composants adaptatifs, ayez recours à _Plane.ByBaestFitThroughPoints_ pour obtenir une approximation du plan du composant.
3. Interrogez la _normale_ de ce plan.
4. Utilisez le _produit scalaire_ pour calculer l'orientation du soleil. Le produit scalaire est une formule qui détermine la mesure selon laquelle deux vecteurs sont parallèles ou antiparallèles. Prenez donc la normale du plan de chaque composant adaptatif et comparez-la au vecteur solaire pour simuler l'orientation du soleil de façon approximative.
5. Prenez la _valeur absolue_ du résultat. Cela permet de garantir que le produit scalaire est précis si la normale du plan est orientée vers l'arrière.
6. Cliquez sur _Exécuter_.

> 1. Le _produit scalaire_ contient une grande quantité de nombres. Pour utiliser leur distribution relative, vous devez condenser les nombres dans la plage appropriée du paramètre _"Aperture Ratio"_ à modifier.

1. L'outil _Math.RemapRange_ est idéal pour cela. Il prend une liste d'entrée et remappe ses limites en deux valeurs cibles.
2. Définissez les valeurs cibles sur _0,15_ et _0,45_ dans un _bloc de code_.
3. Cliquez sur _Exécuter_.

> 1. Connectez les valeurs remappées à un nœud _Element.SetParameterByName_.

1. Connectez la chaîne _"Aperture Ratio"_ à l'entrée _parameterName_.
2. Connectez les _composants adaptatifs_ à l'entrée _element_.
3. Cliquez sur _Exécuter_.

> Dans Revit, à partir d'une distance, vous pouvez définir l'impact de l'orientation du soleil sur l'ouverture des panneaux ETFE.

> En zoomant, vous pouvez voir que plus les panneaux ETFE font face au soleil et plus ils sont fermés. L'objectif ici est de réduire la surchauffe de l'exposition au soleil. Si vous voulez laisser davantage de lumière en fonction de l'exposition solaire, vous devez simplement définir le domaine sur _Math.RemapRange_.
