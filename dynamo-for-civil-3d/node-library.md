# Bibliothèque de nœuds

Nous avons mentionné précédemment que les **nœuds** sont les blocs de construction principaux d’un graphique Dynamo et qu’ils sont organisés en groupes logiques dans la **bibliothèque**. Dans Dynamo for Civil 3D, il existe deux catégories (ou **étagères**) dans la bibliothèque qui contiennent des nœuds dédiés pour travailler avec des objets AutoCAD et Civil 3D, tels que des axes, des contours, des projets 3D, des références de bloc, etc. Le reste de la bibliothèque contient des nœuds qui sont plus génériques par nature et sont cohérents entre toutes les « variantes » de Dynamo (par exemple, Dynamo pour Revit, Dynamo Sandbox, etc.).

{% hint style="info" %} Consultez la section [2-library.md](../3\_user\_interface/2-library.md "mention") pour plus d’informations sur l’organisation des nœuds de la bibliothèque Dynamo principale. {% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Bibliothèque de nœuds dans Dynamo for Civil 3D</p></figcaption></figure>

> 1. Nœuds spécifiques pour l’utilisation d’objets AutoCAD et Civil 3D
> 2. Nœuds à usage général
> 3. Nœuds provenant de **packages** tiers que vous pouvez installer séparément

{% hint style="warning" %} En utilisant les nœuds trouvés sous les étagères AutoCAD et Civil 3D, votre graphique Dynamo ne fonctionnera que dans Dynamo for Civil 3D. Si un graphique Dynamo for Civil 3D est ouvert ailleurs (dans Dynamo pour Revit, par exemple), ces nœuds seront signalés par un avertissement et ne s’exécuteront pas.. {% endhint %}

{% hint style="info" %} **Pourquoi y a-t-il deux étagères distinctes pour AutoCAD et Civil 3D ?**

Cette organisation distingue les nœuds des objets natifs AutoCAD (lignes, polylignes, références de bloc, etc.) des nœuds des objets de Civil 3D (axes, projets 3D, surfaces, etc.). D’un point de vue technique, AutoCAD et Civil 3D sont distincts. AutoCAD est l’application de base, et Civil 3D est construit à partir de celle-ci. {% endhint %}

## Hiérarchie des nœuds

Pour travailler avec les nœuds AutoCAD et Civil 3D, il est important de bien comprendre la hiérarchie des objets dans chaque étagère. Vous vous souvenez de la taxinomie en biologie ? Règne, Embranchement, Classe, Ordre, Famille, Genre, Espèces ? Les objets AutoCAD et Civil 3D sont classés de la même manière. Voici quelques exemples qui illustrent cela.

### Objets Civil

Prenons l’exemple d’un axe.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Supposons que votre objectif soit de changer le nom de l’axe. À partir de là, le nœud suivant à ajouter est un nœud **CivilObject.SetName**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

À première vue, cela ne semble pas très intuitif. Qu’est-ce qu’un **CivilObject** , et pourquoi la bibliothèque n’a-t-elle pas de nœud **Alignment.SetName** ? La réponse est liée à la _réutilisation_ et à la _simplicité_. Si vous y réfléchissez bien, le processus de modification du nom d’un objet Civil 3D est le même, qu’il s’agisse d’un axe, d’un projet 3D, d’un profil ou d’autre chose. Ainsi, au lieu d’avoir des nœuds répétitifs qui font essentiellement la même chose (par exemple, **Alignment.SetName, Corridor.SetName, Profile.SetName**, etc.), il serait logique d’intégrer cette fonctionnalité dans un seul nœud. C’est exactement ce que fait **CivilObject.SetName** !

Il est également intéressant de réfléchir à cette question en termes de _relations_. Un axe et un projet 3D sont des **objets Civil**, tout comme une pomme et une poire sont des fruits. Les nœuds d’objet Civil s’appliquent à tout type d’objet Civil, de la même manière que vous pouvez utiliser un seul éplucheur pour éplucher une pomme et une poire. La cuisine deviendrait très encombrée si vous disposiez d’un éplucheur différent pour chaque type de fruit ! Ainsi, la bibliothèque de nœuds Dynamo est comme votre cuisine.

### Objets

Maintenant, allons plus loin. Supposons que vous souhaitiez modifier le calque de l’axe. Pour ce faire, vous devez utiliser le nœud **Object.SetLayer**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Pourquoi n’existe-t-il pas de nœud appelé **CivilObject.SetLayer** ? Les mêmes principes de réutilisation et de simplicité que nous avons évoqués précédemment s’appliquent ici. La propriété de _calque_ est commune à tous les objets dans AutoCAD pouvant être dessiné ou inséré, tel qu’une ligne, une polyligne, du texte, une référence de bloc, etc. Les objets Civil 3D tels que les axes et les projets 3D sont de la même catégorie. Par conséquent, tout nœud qui s’applique à un **objet** peut également être utilisé avec n’importe quel **objet Civil**.

