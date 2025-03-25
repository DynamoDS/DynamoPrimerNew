# Positionnement des services

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

La conception technique d’un projet de construction de logements typique implique de travailler avec plusieurs équipements souterrains, tels que des égouts sanitaires, des équipements d’évacuation des eaux pluviales, l’eau potable, etc. Cet exemple montre comment Dynamo peut être utilisé pour dessiner les raccordements d’une conduite principale à un terrain donné (c’est-à-dire une parcelle). Il est courant que chaque parcelle doive être raccordée à un service, ce qui représente une tâche fastidieuse pour placer tous les services. Dynamo peut accélérer le processus en dessinant automatiquement la géométrie nécessaire avec précision, et en fournissant des données d’entrée flexibles qui peuvent être adaptées aux normes locales.

## Objectif

> :dart: Placer les références de bloc de compteurs d’eau à des distances spécifiées de la ligne de parcelle, et tracer une ligne pour chaque raccordement perpendiculaire à la conduite principale.

## Concepts clés

> * Utiliser le nœud **Sélectionner un objet** pour la saisie utilisateur
> * Utiliser des systèmes de coordonnées
> * Utiliser des opérations géométriques telles que **Geometry.DistanceTo** et **Geometry.ClosestPointTo**
> * Créer des références de bloc
> * Contrôler les paramètres de liaison des objets

## Compatibilité des versions

{% hint style="success" %} Ce graphique peut s’exécuter dans **Civil 3D 2020** et les versions ultérieures. 
{% endhint %} 

## Ensemble de données

Commencez par télécharger les fichiers d’exemple ci-dessous, puis ouvrez le fichier DWG et le graphique Dynamo.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## Solution

Voici une présentation de la logique de ce graphique.

> 1. Obtenir la géométrie de la courbe pour la conduite principale
> 2. Obtenir la géométrie de la courbe pour une ligne de parcelle sélectionnée par l’utilisateur, en l’inversant si nécessaire
> 3. Générer les points d’insertion pour les compteurs
> 4. Obtenir les points de la conduite principale qui sont les plus proches de l’emplacement des compteurs
> 5. Créer des références de blocs et des lignes dans l’espace objet

Allons-y !

### Obtenir la géométrie de la conduite principale

La première étape consiste à intégrer la géométrie de la conduite principale dans Dynamo. Au lieu de sélectionner des lignes ou des polylignes individuelles, nous allons plutôt récupérer tous les objets d’une certaine couche et les regrouper sous la forme d’une polycourbe Dynamo.

{% hint style="info" %}
 Si vous ne connaissez pas la géométrie des courbes Dynamo, consultez la section [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Récupération des objets de Civil 3D et regroupement de l’ensemble en une seule polycourbe</p></figcaption></figure>

### Obtenir la géométrie de la ligne de parcelle

Ensuite, vous devez obtenir la géométrie d’une ligne de parcelle sélectionnée dans Dynamo afin de pouvoir l’utiliser. L’outil approprié est le nœud **Sélectionner un objet**, qui permet à l’utilisateur du graphique de sélectionner un objet spécifique dans Civil 3D.

Vous devez également gérer un problème potentiel qui pourrait survenir. La ligne de parcelle a un point de départ et un point d’arrivée, ce qui signifie qu’elle a une direction. Pour que le graphique produise des résultats cohérents, il faut que toutes les lignes de parcelle aient une direction cohérente. Vous pouvez tenir compte de cette condition directement dans la logique du graphique, ce qui rend ce dernier plus résilient. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>Sélection d’une ligne de parcelle et vérification de la bonne direction</p></figcaption></figure>

> 1. Obtenez les points de départ et d’arrivée de la ligne de parcelle.
> 2. Mesurez la distance entre chaque point et la conduite principale, puis déterminez quelle est la plus grande distance.
> 3. Faites en sorte que le point de départ de la ligne soit le plus proche de la conduite principale. Si ce n’est pas le cas, inversez la direction de la ligne de parcelle. Dans le cas contraire, renvoyez simplement la ligne de parcelle d’origine.

### Générer des points d’insertion

Il est temps de déterminer l’emplacement des compteurs. Généralement, l’emplacement est déterminé par les exigences des autorités locales. Vous fournirez donc simplement des valeurs d’entrée qui peuvent être modifiées pour s’adapter à diverses conditions. Vous allez utiliser un **système de coordonnées** le long de la ligne de parcelle comme référence pour la création des points. Il est ainsi très facile de définir des décalages par rapport à la ligne de parcelle, quelle que soit son orientation.

{% hint style="info" %}
 Si vous ne connaissez pas les systèmes de coordonnées, consultez la section [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention"). 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>Création des points d’insertion des compteurs</p></figcaption></figure>

### Obtenir des points de raccordement

Vous devez maintenant déterminer les points de la conduite principale les plus proches de l’emplacement des compteurs. Cela vous permettra de dessiner les raccordements aux services dans l’espace objet de manière à ce qu’ils soient toujours perpendiculaires à la conduite principale. Le nœud **Geometry.ClosestPointTo** est la solution idéale.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>Obtention de points perpendiculaires sur la conduite principale</p></figcaption></figure>

> 1. Polycourbe de la conduite principale
> 2. Points d’insertion des compteurs

### Création d’objets

La dernière étape consiste à créer des objets dans l’espace objet. Vous utiliserez les points d’insertion que vous avez générés précédemment pour créer des références de bloc, puis vous utiliserez les points sur la conduite principale pour tracer des lignes vers les raccordements aux services.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### Résultat

Lorsque vous exécutez le graphique, vous devriez voir apparaître de nouvelles références de bloc et des lignes de raccordement aux services dans l’espace objet. Essayez de modifier certaines entrées et observez l’actualisation automatique de l’ensemble !

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Ajustement des paramètres d’entrée dans Dynamo et visualisation immédiate des résultats dans Civil 3D</p></figcaption></figure>

### Bonus : activer le placement séquentiel

Vous avez peut-être remarqué qu’après avoir placé les objets pour une ligne de parcelle, la sélection d’une autre ligne de parcelle entraîne le « déplacement » des objets.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>Comportement lorsque la liaison d’objet est activée</p></figcaption></figure>

C’est le comportement par défaut de Dynamo, et il est très utile dans de nombreux cas. Cependant, il se peut que vous souhaitiez placer plusieurs raccordements aux services de manière séquentielle et que Dynamo crée de nouveaux objets à chaque exécution au lieu de modifier les objets d’origine. Vous pouvez contrôler ce comportement en modifiant les paramètres de liaison d’objet.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Paramètres de liaison d’objet de Dynamo</p></figcaption></figure>

{% hint style="info" %}
 Pour plus d’informations, consultez la section [object-binding.md](../../advanced-topics/object-binding.md "mention"). 
{% endhint %} 

La modification de ce paramètre forcera Dynamo à « oublier » les objets qu’il crée à chaque exécution. Voici un exemple d’exécution du graphique avec la liaison d’objet désactivée en utilisant le **Lecteur Dynamo**.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Exécution du graphique à l’aide du Lecteur Dynamo et visualisation des résultats dans Civil 3D</p></figcaption></figure>

{% hint style="info" %}
 Si vous ne connaissez pas le Lecteur Dynamo, consultez la section [lecteur-dynamo.md](../../dynamo-player.md "mention"). 
{% endhint %} 

> :tada: Mission accomplie !

## Idées

Voici quelques suggestions pour élargir les possibilités offertes par ce graphique.

{% hint style="info" %}
 Placez **plusieurs raccordements aux services** simultanément au lieu de sélectionner chaque ligne de parcelle. 
{% endhint %} 

{% hint style="info" %}
 Ajustez les données d’entrée pour remplacer les compteurs d’eau par des **regards de canalisation**. 
{% endhint %} 

{% hint style="info" %}
 **Ajoutez un bouton bascule** pour permettre de placer un seul raccordement aux services sur un côté particulier de la ligne de parcelle plutôt que sur les deux côtés. 
{% endhint %} 
