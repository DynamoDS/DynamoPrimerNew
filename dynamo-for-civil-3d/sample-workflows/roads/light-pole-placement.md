# Positionnement des lampadaires

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

L’un des nombreux cas d’utilisation de Dynamo est le placement dynamique d’objets discrets le long d’un modèle de projet 3D. Il arrive souvent que des objets doivent être placés à des emplacements indépendants des assemblages insérés le long du projet 3D, ce qui est une tâche très fastidieuse à accomplir manuellement. De plus, lorsque la géométrie horizontale ou verticale du projet 3D change, cela entraîne des travaux supplémentaires considérables.

## Objectif

> :dart: Placer des références de bloc de lampadaires le long d’un projet 3D aux valeurs d’abscisse curviligne spécifiées dans un fichier Excel. 

## Concepts clés

> * Lire des données à partir d’un fichier externe (Excel dans ce cas)
> * Organiser des données dans des dictionnaires
> * Utiliser les systèmes de coordonnées pour contrôler la position, l’échelle et la rotation
> * Placer des références de bloc
> * Visualiser la géométrie dans Dynamo

## Compatibilité des versions

{% hint style="success" %} Ce graphique peut s’exécuter dans **Civil 3D 2020** et les versions ultérieures. 
{% endhint %} 

## Ensemble de données

Commencez par télécharger les fichiers d’exemple ci-dessous, puis ouvrez le fichier DWG et le graphique Dynamo.

{% hint style="info" %}
 Il est préférable d’enregistrer le fichier Excel dans le même répertoire que le graphique Dynamo. 
{% endhint %} 

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs (1).dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## Solution

Voici une présentation de la logique de ce graphique.

> 1. Lire le fichier Excel et importer les données dans Dynamo
> 2. Obtenir des lignes caractéristiques du terrain à partir de la ligne de base du projet 3D spécifié
> 3. Générer des systèmes de coordonnées le long de la ligne caractéristique du terrain du projet au niveau des abscisses curvilignes souhaitées
> 4. Utiliser les systèmes de coordonnées pour placer les références de bloc dans l’espace objet

Allons-y !

### Obtenir des données Excel

Dans cet exemple de graphique, nous allons utiliser un fichier Excel pour stocker les données que Dynamo utilisera pour placer les références de bloc de lampadaire. Le tableau se présente comme suit.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Structure du tableau du fichier Excel</p></figcaption></figure>

{% hint style="info" %}
 L’utilisation de Dynamo pour lire des données à partir d’un fichier externe (tel qu’un fichier Excel) est une excellente stratégie, en particulier lorsque les données doivent être partagées avec d’autres membres de l’équipe. 
{% endhint %} 

Les données Excel sont importées dans Dynamo de la manière suivante. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Importation des données Excel dans Dynamo</p></figcaption></figure>

Maintenant que vous disposez des données, vous devez les diviser par colonne (_Projet 3D_, _Ligne de base_, _Code de point_, etc.) afin de pouvoir les utiliser dans le reste du graphique. Pour ce faire, vous pouvez utiliser le nœud **List.GetItemAtIndex** et spécifier le numéro d’index de chaque colonne souhaitée. Par exemple, la colonne _Projet 3D_ se trouve à l’index 0, la colonne _Ligne de base_ à l’index 1, etc.

Cela semble correct, n’est-ce pas ? Mais cette approche peut poser problème. Que se passe-t-il si l’ordre des colonnes dans le fichier Excel change par la suite ? Ou si une nouvelle colonne est ajoutée entre deux colonnes ? Dans ce cas, le graphique ne fonctionnera pas correctement et devra être mis à jour. Vous pouvez protéger le graphique en plaçant les données dans un **dictionnaire**, avec les en-têtes des colonnes Excel comme _clés_ et le reste des données comme _valeurs_.

{% hint style="info" %}
 Si vous ne connaissez pas les dictionnaires, consultez la section [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>Insertion des données Excel dans un dictionnaire</p></figcaption></figure>

Le graphique est ainsi plus résilient, car il est possible de modifier l’ordre des colonnes dans Excel. Tant que les en-têtes de colonne restent les mêmes, les données peuvent simplement être extraites du dictionnaire à l’aide de sa _clé_ (c’est-à-dire l’en-tête de colonne), ce que vous ferez ensuite.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>Récupération des données du dictionnaire</p></figcaption></figure>

### Obtenir des lignes caractéristiques du terrain du projet 3D

Maintenant que les données Excel sont importées et prêtes à l’emploi, commencez à les utiliser pour obtenir des informations de Civil 3D sur les modèles de projet 3D.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. Sélectionnez le modèle de projet 3D par son nom.
> 2. Obtenez une ligne de base spécifique dans le projet 3D.
> 3. Obtenez une ligne caractéristique du terrain dans la ligne de base à l’aide de son code de point.

### Générer des systèmes de coordonnées

Vous allez maintenant générer des **systèmes de coordonnées** le long des lignes caractéristiques du terrain du projet 3D aux valeurs d’abscisse curviligne spécifiées dans le fichier Excel. Ces systèmes de coordonnées seront utilisés pour définir la position, la rotation et l’échelle des références de bloc du lampadaire.

{% hint style="info" %}
 Si vous ne connaissez pas les systèmes de coordonnées, consultez la section [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>Obtention de systèmes de coordonnées le long des lignes caractéristiques du terrain du projet 3D</p></figcaption></figure>

Notez l’utilisation d’un bloc de code pour faire pivoter les systèmes de coordonnées en fonction du côté de la ligne de base sur lequel ils se trouvent. Cela pourrait être réalisé en utilisant une séquence de plusieurs nœuds, mais ceci est un bon exemple d’une situation où il est plus facile de l’écrire.

{% hint style="info" %}
 Si vous ne connaissez pas les blocs de code, consultez la section [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention"). 
{% endhint %} 

### Créer des références de bloc

Vous approchez du but ! Vous disposez de toutes les informations nécessaires pour placer les références de bloc. La première chose à faire est d’obtenir les définitions des blocs que vous souhaitez à l’aide de la colonne _BlockName_ dans le fichier Excel.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>Obtention des définitions de bloc souhaitées à partir du document</p></figcaption></figure>

La dernière étape consiste à créer les références de bloc.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>Création de références de bloc dans l’espace objet</p></figcaption></figure>

### Résultat

Lorsque vous exécutez le graphique, vous devriez voir apparaître de nouvelles références de bloc dans l’espace objet le long du projet 3D. Et voici la partie la plus intéressante : si le mode d’exécution du graphique est défini sur Automatique et que vous modifiez le fichier Excel, les références de bloc sont mises à jour automatiquement !

{% hint style="info" %}
 Pour en savoir plus sur les modes d’exécution des graphiques, consultez la section [3_user_interface](../../../3\_user\_interface/ "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Mise à jour du fichier Excel et visualisation rapide des résultats dans Civil 3D</p></figcaption></figure>

Voici un exemple d’exécution du graphique à l’aide du **Lecteur Dynamo**.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Exécution du graphique à l’aide du Lecteur Dynamo et visualisation des résultats dans Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Si vous ne connaissez pas le Lecteur Dynamo, consultez la section [lecteur-dynamo.md](../../dynamo-player.md "mention"). 
{% endhint %} 

> :tada: Mission accomplie !

### Bonus : visualiser dans Dynamo

Il peut être utile de visualiser la géométrie du projet 3D dans Dynamo pour fournir du contexte. Les solides du projet 3D de ce modèle sont déjà extraits dans l’espace objet. Importez-les dans Dynamo. 

Cependant, il y a autre chose que vous devez prendre en compte. Les solides sont un type de géométrie relativement « lourd », ce qui signifie que cette opération ralentira le graphique. L’idéal serait de disposer d’un moyen simple permettant de _choisir_ d’afficher ou non les solides. La solution la plus évidente est de débrancher le nœud **Corridor.GetSolids**, mais cela produira des avertissements pour tous les nœuds en aval, ce qui est un peu gênant. C’est une situation dans laquelle le nœud **ScopeIf** se révèle particulièrement utile.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. Remarquez que le nœud **Object.Geometry** a une barre grise en bas. Cela signifie que l’aperçu du nœud est désactivé (accessible par un clic droit sur le nœud), ce qui permet à **GeometryColor.ByGeometryColor** d’éviter de se « battre » avec d’autres géométries pour la priorité d’affichage dans l’aperçu de l’arrière-plan.
> 2. Le nœud **ScopeIf** vous permet d’exécuter de manière sélective une branche entière de nœuds. Si l’entrée du _test_ est définie sur False, tous les nœuds connectés au nœud **ScopeIf** ne s’exécuteront pas.

Voici le résultat dans l’aperçu de arrière-plan de Dynamo.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Visualisation de la géométrie du projet 3D dans Dynamo</p></figcaption></figure>

## Idées

Voici quelques suggestions pour élargir les possibilités offertes par ce graphique.

{% hint style="info" %}
 Ajoutez une colonne de **rotation** au fichier Excel et utilisez-la pour commander la rotation des systèmes de coordonnées. 
{% endhint %} 

{% hint style="info" %}
 Ajoutez des **décalages horizontaux ou verticaux** au fichier Excel de sorte que les lampadaires puissent dévier de la ligne caractéristique du terrain du projet 3D si nécessaire. 
{% endhint %} 

{% hint style="info" %}
 Au lieu d’utiliser un fichier Excel avec des valeurs d’abscisse curviligne, générez les valeurs d’abscisse curviligne **directement dans Dynamo** à l’aide d’une abscisse curviligne de départ et d’un espacement standard. 
{% endhint %} 
