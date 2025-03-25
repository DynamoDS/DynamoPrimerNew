# Zone de dégagement

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

L’élaboration d’enveloppes cinématiques pour la validation du dégagement est une partie importante de la conception ferroviaire. Dynamo peut être utilisé pour générer des solides pour l’enveloppe au lieu de créer et de gérer des éléments de profil type de projets 3D complexes pour effectuer le travail.

## Objectif

> :dart: Utiliser un bloc de contour du véhicule pour générer des solides 3D de zone de dégagement le long d’un projet 3D.

## Concepts clés

> * Utiliser des lignes caractéristiques du terrain du projet 3D
> * Transformer la géométrie entre systèmes de coordonnées
> * Créer des solides par lissage
> * Contrôler le comportement des nœuds à l’aide des paramètres de combinaison

## Compatibilité des versions

{% hint style="success" %} Ce graphique peut s’exécuter dans **Civil 3D 2020** et les versions ultérieures. 
{% endhint %} 

## Ensemble de données

Commencez par télécharger les fichiers d’exemple ci-dessous, puis ouvrez le fichier DWG et le graphique Dynamo.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope (1).dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## Solution

Voici une présentation de la logique de ce graphique.

> 1. Obtenir des lignes caractéristiques du terrain à partir de la ligne de base du projet 3D spécifié
> 2. Générer des systèmes de coordonnées le long de la ligne caractéristique du terrain du projet avec l’espacement souhaité
> 3. Transformer la géométrie du bloc de profil en systèmes de coordonnées
> 4. Lisser un solide entre les contours
> 5. Créer les solides dans Civil 3D

Allons-y !

### Obtenir les données du projet 3D

La première étape consiste à obtenir les données du projet 3D. Sélectionnez le modèle de projet 3D par son nom, obtenez une ligne de base spécifique dans le projet 3D, puis une ligne caractéristique du terrain dans la ligne de base par son code de point.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>Sélection du projet 3D, de la ligne de base et de la ligne caractéristique du terrain</p></figcaption></figure>

### Générer des systèmes de coordonnées

Vous allez maintenant générer des **systèmes de coordonnées** le long des lignes caractéristiques du terrain du projet 3D entre une abscisse curviligne de départ et de fin donnée. Ces systèmes de coordonnées seront utilisés pour aligner la géométrie du bloc de contour du véhicule sur le projet 3D.

{% hint style="info" %}
 Si vous ne connaissez pas les systèmes de coordonnées, consultez la section [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>Obtention de systèmes de coordonnées le long des lignes caractéristiques du terrain du projet 3D</p></figcaption></figure>

> 1. Remarquez le symbole **XXX** dans le coin inférieur droit du nœud. Cela signifie que les paramètres de combinaison du nœud sont définis sur _Produit cartésien_, ce qui est nécessaire pour générer les systèmes de coordonnées avec les mêmes valeurs d’abscisse curviligne pour les deux lignes caractéristiques du terrain.

{% hint style="info" %}
 Si vous ne connaissez pas la combinaison de nœuds, consultez la section [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention"). 
{% endhint %} 

### Transformer la géométrie du bloc

Vous devez maintenant créer un réseau de contours du véhicule le long des lignes caractéristiques du terrain. Vous allez transformer la géométrie à partir de la définition du bloc de contour du véhicule à l’aide du nœud **Geometry.Transform**. Il s’agit d’un concept difficile à visualiser, c’est pourquoi, avant d’examiner les nœuds, observez le graphique ci-dessous qui montre ce qui va se passer.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>Visualisation de la transformation de la géométrie entre les systèmes de coordonnées</p></figcaption></figure>

Il s’agit donc essentiellement de prendre la géométrie Dynamo d’une _seule_ définition de bloc unique et de la déplacer ou de la faire pivoter, tout en créant un réseau le long de la ligne caractéristique du terrain. Génial, non ? Voici à quoi ressemble la séquence de nœuds.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. Cela permet d’obtenir la définition du bloc à partir du document.
> 2. Ces nœuds obtiennent la géométrie Dynamo des objets du bloc.
> 3. Ces nœuds définissent essentiellement le système de coordonnées _à partir duquel_ la géométrie est transformée.
> 4. Enfin, ce nœud effectue le travail de transformation de la géométrie.
> 5. Notez que ce nœud présente _la plus longue_ combinaison.

Voici le résultat dans Dynamo.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>Géométrie du bloc de contour du véhicule après transformation</p></figcaption></figure>

### Générer des solides

Bonne nouvelle : le plus dur est fait. Il ne vous reste plus qu’à générer des solides entre les contours. Cette opération est facile à réaliser avec le nœud **Solid.ByLoft**.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

Voici le résultat. N’oubliez pas qu’il s’agit de solides Dynamo. Vous devez donc encore les créer dans Civil 3D.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>Solides Dynamo après lissage</p></figcaption></figure>

### Créer des solides dans Civil 3D

La dernière étape consiste à créer les solides générés dans l’espace objet. Vous leur donnerez également une couleur pour qu’ils soient faciles à voir.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Création des solides dans Civil 3D</p></figcaption></figure>

### Résultat

Voici un exemple d’exécution du graphique à l’aide du **Lecteur Dynamo**.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Exécution du graphique à l’aide du Lecteur Dynamo et visualisation des résultats dans Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Si vous ne connaissez pas le Lecteur Dynamo, consultez la section [lecteur-dynamo.md](../../dynamo-player.md "mention"). 
{% endhint %} 

> :tada: Mission accomplie !

## Idées

Voici quelques suggestions pour élargir les possibilités offertes par ce graphique.

{% hint style="info" %}
 Ajoutez la possibilité d’utiliser **différents intervalles d’abscisses curvilignes** séparément pour chaque voie. 
{% endhint %} 

{% hint style="info" %}
 **Divisez les solides** en segments plus petits qui peuvent être analysés individuellement pour détecter d’éventuels conflits. 
{% endhint %} 

{% hint style="info" %}
 Vérifiez si les solides de l’enveloppe **croisent des objets** et coloriez ceux qui entrent en conflit. 
{% endhint %} 
