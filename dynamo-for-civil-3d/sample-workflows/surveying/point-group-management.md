# Gestion des groupes de points

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

L’utilisation de points et de groupes de points COGO dans Civil 3D est un élément essentiel de nombreux processus, du levé topographique aux figures Dynamo est particulièrement efficace en matière de gestion des données. Le présent exemple illustre un cas d’utilisation potentiel.  

## Objectif

> :dart: Créer un groupe de points pour chaque description de point COGO unique. 

## Concepts clés

> * Utilisation des listes
> * Regrouper des objets similaires avec le nœud **List.GroupByKey**
> * Afficher une sortie personnalisée dans le Lecteur Dynamo

## Compatibilité des versions

{% hint style="success" %} Ce graphique peut s’exécuter dans **Civil 3D 2020** et les versions ultérieures. {% endhint %}

## Ensemble de données

Commencez par télécharger les fichiers d’exemple ci-dessous, puis ouvrez le fichier DWG et le graphique Dynamo.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## Solution

Voici une présentation de la logique de ce graphique.

> 1. Obtenir tous les points COGO dans le document
> 2. Regrouper les points COGO par description
> 3. Créer des groupes de points
> 4. Générer un résumé dans le Lecteur Dynamo

Allons-y !

### Obtenir les points COGO

La première étape consiste à obtenir tous les groupes de points du document, puis tous les points COGO au sein de chaque groupe. Vous obtiendrez ainsi une _liste imbriquée_ ou « liste de listes », qui sera plus facile à utiliser ultérieurement si vous aplanissez tous les éléments pour en faire une liste unique à l’aide du nœud **List.Flatten**.

{% hint style="info" %} Si vous ne connaissez pas les listes, consultez la section [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention"). {% endhint %}

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>Obtention de tous les groupes de points et des points COGO </p></figcaption></figure>

### Regrouper les points par description

Maintenant que vous disposez de tous les points COGO, vous devez les séparer en groupes en fonction de leur description. C’est exactement ce que fait le nœud **List.GroupByKey**. Il regroupe tous les éléments qui partagent la même clé.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>Regroupement des points COGO par description</p></figcaption></figure>

### Créer des groupes de points

le plus dur est fait ! La dernière étape consiste à créer de nouveaux groupes de points Civil 3D à partir des points COGO regroupés.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>Création de nouveaux groupex de points</p></figcaption></figure>

### Générer un résumé

Lorsque vous exécutez le graphique, il n’y a rien à voir dans l’aperçu de l’arrière-plan de Dynamo, car vous ne travaillez pas avec une géométrie. Le seul moyen de savoir si le graphique a été exécuté correctement est donc de vérifier la fenêtre d’outils ou d’examiner les aperçus de sortie des nœuds. Cependant, si vous exécutez le graphique à l’aide du **Lecteur Dynamo**, vous pouvez fournir plus d’informations sur les résultats du graphique en générant un résumé des groupes de points qui ont été créés. Il vous suffit de cliquer avec le bouton droit de la souris sur un nœud et de lui attribuer la valeur _Est une sortie_. Dans ce cas, utilisez un nœud de **visualisation** renommé pour afficher les résultats.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>La définition d’un nœud comme <em>Est une sortie</em> affichera son contenu dans la sortie du Lecteur Dynamo.</p></figcaption></figure>

### Résultat

Voici un exemple d’exécution du graphique à l’aide du **Lecteur Dynamo**.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Exécution du graphique à l’aide du Lecteur Dynamo et visualisation des résultats dans la fenêtre d’outils</p></figcaption></figure>

{% hint style="info" %} Si vous ne connaissez pas le Lecteur Dynamo, consultez la section [lecteur-dynamo.md](../../dynamo-player.md "mention"). {% endhint %}

> :tada: Mission accomplie !

## Idées

Voici quelques suggestions pour élargir les possibilités offertes par ce graphique.

{% hint style="info" %} Modifiez le regroupement des points pour qu’il soit basé sur la **description complète** au lieu de la description brute. {% endhint %}

{% hint style="info" %} Regroupez les points selon d’autres **catégories prédéfinies** que vous choisissez (par exemple, « Prises de vue au sol », « Monuments », etc.) {% endhint %}

{% hint style="info" %} Créer automatiquement des surfaces triangulées pour les points de certains groupes. {% endhint %}
