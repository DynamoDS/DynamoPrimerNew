# Liaison d’objet

Dynamo for Civil 3D contient un mécanisme très puissant qui « se souvient » des objets créés par chaque nœud. Ce mécanisme est appelé **Liaison d’objet** et permet à un graphique Dynamo de produire des résultats cohérents chaque fois qu’il est exécuté dans le même document. Bien que cela soit souhaitable dans de nombreuses situations, il est possible que vous vouliez avoir plus de contrôle sur le comportement de Dynamo dans d’autres cas. Cette section vous aidera à comprendre le fonctionnement de la liaison d’objet et comment en tirer parti.

## Exemple

Prenons l’exemple de ce graphique qui crée un cercle dans l’espace objet sur le calque courant.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Graphique simple pour créer un cercle</p></figcaption></figure>

Remarquez ce qui se passe lorsque le rayon est modifié.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Modifier la valeur du rayon dans Dynamo</p></figcaption></figure>

Voilà une démonstration de la liaison d’objet. Par défaut, Dynamo _modifie_ le rayon du cercle, plutôt que de créer un nouveau cercle à chaque fois que la valeur du rayon change. Ceci est dû au fait que le nœud **Object.ByGeometry** « se souvient » qu’il a créé ce cercle _spécifique_ à chaque exécution du graphique. De plus, Dynamo stocke cette information de sorte que lors de la prochaine ouverture du document Civil 3D et de l’exécution du graphique, celui-ci aura exactement le même comportement.

## Autre exemple

Voyons un exemple dans lequel vous voudriez changer le comportement par défaut des liaisons d’objet dans Dynamo. Supposons que vous souhaitiez créer un graphique qui place un texte au milieu d’un cercle. Vous souhaitez également que ce graphique puisse être exécuté à plusieurs reprises et qu’il place un nouveau texte à chaque fois, quel que soit le cercle sélectionné. Voici à quoi ce graphique pourrait ressembler.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Graphique simple qui place un texte au centre d’un cercle sélectionné</p></figcaption></figure>

Cependant, ceci se produit lorsque vous sélectionnez un autre cercle.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Comportement par défaut de Dynamo lors de la sélection d’un nouveau cercle</p></figcaption></figure>

Il semble que le texte soit supprimé et recréé à chaque exécution du graphique. En réalité, la position du texte est _modifiée_ en fonction du cercle sélectionné. Il s’agit donc du même texte, mais à un autre endroit ! Afin de créer un nouveau texte à chaque fois, vous devez modifier les paramètres de liaison d’objet de Dynamo pour qu’aucune donnée de liaison ne soit conservée (consultez la partie [\#binding-settings](object-binding.md#binding-settings "mention") ci-dessous).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Paramètres de liaison d’objet</p></figcaption></figure>

Après avoir effectué ce changement, vous obtenez le comportement que vous recherchez.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Comportement avec la liaison d’objet désactivée</p></figcaption></figure>

## Paramètres de liaison

Dynamo for Civil 3D permet de modifier le comportement par défaut des liaison d’objet via les paramètres **Stockage des données de liaison** du menu de **Dynamo**.

{% hint style="info" %} Notez que les options de stockage des données de liaison sont disponibles dans **Civil 3D 2022.1** et dans les versions ultérieures. {% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Toutes les options sont activées par défaut. Voici un résumé des effets de chaque option.

### Option 1 : aucune donnée de liaison conservée

Lorsque cette option est activée, Dynamo « oublie » les objets qu’il a créés lors de la dernière exécution du graphique. Le graphique peut être exécuté dans n’importe quel dessin, dans n’importe quelle situation, et il créera des objets à chaque fois.

{% hint style="info" %} **Quand l’utiliser**

Utilisez cette option lorsque vous souhaitez que Dynamo « oublie » tout ce qu’il a fait lors des exécutions précédentes et crée de nouveaux objets à chaque fois. {% endhint %}

### Option 2 : stocker dans un graphique pour Dynamo

Cette option signifie que les métadonnées de liaison d’objet seront sérialisées dans le graphique (fichier .dyn) lors de son enregistrement. Si vous fermez/rouvrez le graphique et l’exécutez dans le **même dessin**, tout devrait fonctionner comme vous l’avez laissé. Si vous exécutez le graphique dans un **autre dessin**, les données de liaison seront supprimées du graphique et de nouveaux objets seront créés. Cela signifie que si vous ouvrez le dessin d’origine et exécutez à nouveau le graphique, de nouveaux objets seront créés en plus des anciens.

{% hint style="info" %} **Quand l’utiliser**

Utilisez cette option lorsque vous souhaitez que Dynamo « se souvienne » des objets qu’il a créés la dernière fois qu’il s’est exécuté dans un **dessin spécifique**. {% endhint %}

{% hint style="warning" %} Cette option convient mieux aux situations où il est possible de maintenir un rapport 1:1 entre un **dessin spécifique** et un graphique Dynamo. Les options 1 et 3 sont mieux adaptées aux graphiques conçus pour s’exécuter sur plusieurs dessins. {% endhint %}

### Option 3 : stocker dans un dessin pour Dynamo

Cette option est similaire à l’option 2, sauf que les données de liaison d’objet sont sérialisées dans le dessin au lieu du graphique (fichier .dyn). Si vous fermez/rouvrez le graphique et l’exécutez dans le **même dessin**, tout devrait fonctionner comme vous l’avez laissé. Si vous exécutez le graphique dans un **autre dessin**, les données de liaison sont conservées dans le dessin d’origine, car elles sont enregistrées dans le dessin et non dans le graphique.

{% hint style="info" %} **Quand l’utiliser**

Utilisez cette option lorsque vous souhaitez utiliser le même graphique sur **plusieurs dessins** et que Dynamo « se souvienne » de ce qu’il a fait dans chacun d’eux. {% endhint %}

### Option 4 : stocker dans un dessin pour le Lecteur Dynamo

La première chose à noter avec cette option est qu’elle n’a aucun effet sur la façon dont le graphique interagit avec le dessin lorsqu’il est exécuté via l’interface principale de Dynamo. Cette option s’applique _uniquement_ lorsque le graphique est exécuté à l’aide du Lecteur Dynamo.

{% hint style="info" %} Si vous ne connaissez pas le Lecteur Dynamo, consultez la section [dynamo-player.md](../dynamo-player.md "mention"). {% endhint %}

Si vous exécutez le graphique en utilisant l’interface principale de Dynamo, puis fermez et exécutez le même graphique en utilisant le Lecteur Dynamo, il créera de nouveaux objets en plus de ceux qu’il a créés auparavant. Cependant, une fois que le Lecteur Dynamo a exécuté le graphique une fois, il sérialisera les données de liaison d’objet dans le dessin. Ainsi, si vous exécutez le graphique plusieurs fois via le Lecteur Dynamo, il mettra à jour les objets au lieu d’en créer de nouveaux. Si vous exécutez le graphique via le Lecteur Dynamo sur un **autre dessin**, les données de liaison sont toujours conservées dans le dessin d’origine, car elles sont enregistrées dans le dessin et non dans le graphique.

{% hint style="info" %} **Quand l’utiliser**

Utilisez cette option lorsque vous souhaitez exécuter un graphique à l’aide du Lecteur Dynamo sur plusieurs dessins et que vous souhaitez qu’il « se souvienne » de ce qu’il a fait dans chacun d’eux. {% endhint %}
