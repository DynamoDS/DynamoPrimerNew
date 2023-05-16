# Exemples

Si vous cherchez des exemples sur la façon de développer pour Dynamo, consultez les ressources ci-dessous :

#### Exemples de dépôts <a href="#sample-repositories" id="sample-repositories"></a>

Ces exemples sont des modèles Visual Studio que vous pouvez utiliser pour démarrer votre propre projet :

* [**ZeroTouchEssentials**](https://github.com/DynamoDS/ZeroTouchEssentials)** :** modèle pour les nœuds ZeroTouch de base.
  * Renvoie plusieurs sorties : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15-L24)
  * Utiliser un objet de géométrie natif de Dynamo : [code](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86-L89)
  * Propriété d’exemple (nœud de requête) : [code](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L48)
* [**HelloDynamo**](https://github.com/teocomi/HelloDynamo)** :** modèles pour les nœuds NodeModel de base et la personnalisation des vues.
  * Modèle NodeModel de base : [HelloNodeModel.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloNodeModel.cs)
    * Définir les attributs des nœuds (noms des entrées/sorties, descriptions, types) : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L15)
    * Renvoie un nœud nul s’il n’y a pas d’entrées : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L34-L36)
    * Créer un appel de fonction : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloNodeModel.cs#L39)
  * Modèle de personnalisation de la vue NodeModel de base : [HelloGui.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGui.cs), [HelloGuiNodeView.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs), [Slider.xaml](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml), [Slider.xaml.cs](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
    * Signaler à l’interface utilisateur qu’un élément doit être mis à jour : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGui.cs#L27)
    * Personnaliser le NodeModel : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/HelloGuiNodeView.cs#L11)
    * Définir les attributs de curseur : [code](https://github.com/teocomi/HelloDynamo/blob/6c5333d731d58043c12e84cd3244cdbafbe74934/HelloDynamo/HelloNodeModel/Slider.xaml#L10)
    * Déterminer la logique d’interaction pour le curseur : [code](https://github.com/teocomi/HelloDynamo/blob/master/HelloDynamo/HelloNodeModel/Slider.xaml.cs)
* [**DynamoSamples**](https://github.com/DynamoDS/DynamoSamples)** :** modèles pour ZeroTouch, interface utilisateur personnalisée, tests et extensions de vue.
  * [Exemples d’interface utilisateur](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryUI)
    * Créer un nœud d’interface utilisateur de base et personnalisé : [CustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/CustomNodeModel.cs)
    * Créer un menu déroulant : [DropDown.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/DropDown.cs)
  * [Tests](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryTests)
    * Tests système : [HelloDynamoSystemTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoSystemTests.cs)
    * Tests ZeroTouch : [HelloDynamoZeroTouchTests.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryTests/HelloDynamoZeroTouchTests.cs)
  * [Exemples ZeroTouch](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleLibraryZeroTouch/Examples) :
    * Exemple de nœuds ZeroTouch, dont un qui implémente `IGraphicItem` pour affecter le rendu de la géométrie : [BasicExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/BasicExample.cs)
    * Exemple de nœuds ZeroTouch pour colorer la géométrie en utilisant `IRenderPackage`: [ColorExample.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/ColorExample.cs)
  * [Exemples d’extension de vue](https://github.com/DynamoDS/DynamoSamples/tree/master/src/SampleViewExtension) : implémentation IViewExtension qui affiche une fenêtre non modale lorsque l’utilisateur clique sur son élément MenuItem.
* [**NodeModelsEssentials**](https://github.com/nonoesp/DynamoNodeModelsEssentials)** :** modèles pour le développement avancé de packages Dynamo à l’aide de NodeModel.
  * Exemples essentiels :
    * [Erreur](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsError.cs)
    * [MultiOperation](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiOperation.cs)
    * [Multiplier](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsMultiply.cs)
    * [Délai](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/EssentialsTimeout.cs)
  * Exemples de géométrie :
    * [CustomPreview](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryCustomPreview.cs)
    * [SurfaceFrom4Points](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometrySurfaceFrom4Points.cs)
    * [UVPlanesOnSurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryUVPlanesOnSurface.cs)
    * [WobblySurface](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/GeometryWobblySurface.cs)
  * Exemples d’interface utilisateur :
    * [Bouton](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButton.cs)
    * [ButtonFunction](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIButtonFunction.cs)
    * [CopyableWatch](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UICopyableWatch.cs)
    * [Tiroir](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISlider.cs)
    * [SliderBound](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UISliderBound.cs)
    * [Etat](https://github.com/nonoesp/DynamoNodeModelsEssentials/blob/master/src/Essentials/NodeModelsEssentials/UIState.cs)

[**DynamicText**](https://github.com/DynamoDS/DynamoText)** :** bibliothèque ZeroTouch permettant de créer du texte dans Dynamo.

#### Étude de cas <a href="#case-studies" id="case-studies"></a>

Les développeurs tiers ont apporté des contributions significatives et intéressantes à la plateforme, dont beaucoup sont également en open source. Les projets suivants sont des exemples exceptionnels de ce qu’il est possible de faire avec Dynamo.

**Ladybug** est une bibliothèque Python qui permet de charger, d’analyser et de modifier les fichiers EneregyPlus Weather (epw).

[https://github.com/ladybug-tools/ladybug](https://github.com/ladybug-tools/ladybug)

**Honeybee** est une bibliothèque Python qui permet de créer, d’exécuter et de visualiser les résultats d’analyses de la lumière du jour (RADIANCE) et de l’énergie (EnergyPlus/OpenStudio).

[https://github.com/ladybug-tools/honeybee](https://github.com/ladybug-tools/honeybee)

**Bumblebee** est un plugin pour l’interopérabilité entre Excel et Dynamo (GPL).

[https://github.com/ksobon/Bumblebee](https://github.com/ksobon/Bumblebee)

**Clockwork** est une collection de nœuds personnalisés pour les activités liées à Revit ainsi que pour d’autres opérations tels que la gestion de listes, les opérations mathématiques, les opérations de chaîne, les opérations géométriques (principalement des zones de délimitation, des maillages, des plans, des points, des surfaces, des UV et des vecteurs) et la création de panneaux.

[https://github.com/andydandy74/ClockworkForDynamo](https://github.com/andydandy74/ClockworkForDynamo)
