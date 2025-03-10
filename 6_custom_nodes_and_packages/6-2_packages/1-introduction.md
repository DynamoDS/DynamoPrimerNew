# Présentation des packages

Dynamo offre un grand nombre de fonctionnalités prêtes à l’emploi et dispose également d’une bibliothèque de packages complète qui permet d’étendre considérablement les capacités de Dynamo. Un package est un ensemble de nœuds personnalisés ou de fonctionnalités supplémentaires. Le gestionnaire de package Dynamo est un portail permettant à la communauté de télécharger tout package publié en ligne. Ces jeux d’outils sont développés par des tiers afin d’étendre les fonctionnalités clés de Dynamo, accessibles à tous, et prêts à être téléchargés en un clic.

![Site du gestionnaire de package](../images/6-2/1/dpm.jpg)

Un projet Open Source comme Dynamo s’appuie sur ce type d’implication de la communauté. Avec ses développeurs tiers dédiés, Dynamo peut étendre sa portée aux workflows dans différents secteurs d’activité. Par conséquent, l'équipe de Dynamo a entrepris de rationaliser le développement et la publication des packages (sujets abordés plus en détail dans les sections suivantes).

### Installation d’un package

La méthode la plus simple pour installer un package consiste à utiliser l’option de menu Packages de l’interface Dynamo. Passez à présent au vif du sujet et installez un package. Dans cet exemple rapide, vous allez installer un package très utilisé pour créer des panneaux quadrilatéraux sur une grille.

Dans Dynamo, accédez à _Packages > Gestionnaire de package..._

<figure><img src="../../.gitbook/assets/package-manager-menu.png" alt=""><figcaption></figcaption></figure>

Dans la barre de recherche, recherchez « quadrilatères à partir de la grille rectangulaire ». Au bout de quelques instants, l'ensemble des packages correspondants à cette demande de recherche apparaissent. Sélectionnez le premier package avec le nom correspondant.

Cliquez sur Installer pour ajouter ce package à votre bibliothèque, puis acceptez la confirmation. Terminé !

<figure><img src="../../.gitbook/assets/quads-from-rectangular-grid.png" alt=""><figcaption></figcaption></figure>

Vous avez maintenant un autre groupe appelé « buildz » dans la bibliothèque Dynamo. Son nom fait référence au développeur du package. Le nœud personnalisé est placé dans ce groupe. Vous pouvez commencer à l’utiliser immédiatement.

![](../images/6-2/1/packageintroduction-installingapackage03.jpg)

Utilisez un nœud **Code Block**pour définir rapidement une grille rectangulaire, générer le résultat sur un nœud **Polygon.ByPoints**, puis sur un nœud **Surface.ByPatch** pour afficher la liste des panneaux rectangulaires que vous venez de créer.

![](../images/6-2/1/packageintroduction-installingapackage04.jpg)

### Installation du dossier de package – DynamoUnfold

L'exemple ci-dessus est axé sur un package avec un nœud personnalisé, mais vous utilisez le même processus pour télécharger des packages avec plusieurs nœuds personnalisés et des fichiers de données de prise en charge. Démontrez-le maintenant avec un package plus complet : Dynamo Unfold.

Comme dans l’exemple ci-dessus, commencez par sélectionner _Packages > Gestionnaire de package.._.

Cette fois, recherchez _« DynamoUnfold »_, en un mot. Lorsque les packages s’affichent, cliquez sur Installer pour ajouter Dynamo Unfold à votre bibliothèque Dynamo.

<figure><img src="../../.gitbook/assets/unfold.png" alt=""><figcaption></figcaption></figure>

La bibliothèque Dynamo contient un groupe _DynamoUnfold_ avec plusieurs catégories et nœuds personnalisés.

![](../images/6-2/1/packageintroduction-installingpackagefolder02.jpg)

Examinez maintenant la structure de fichiers du package. 

1. Tout d’abord, accédez à Packages > Gestionnaire de package > Packages installés.
2. À côté de DynamoUnfold, sélectionnez le menu d’options <img src="../images/6-2/1/packageintroduction-verticaldotsmenu.jpg" alt="" data-size="line">.
3. Cliquez ensuite sur Afficher le répertoire racine pour ouvrir le dossier racine de ce package.

<figure><img src="../../.gitbook/assets/view-root-directory.png" alt=""><figcaption></figcaption></figure>

Cette action permet d’accéder au répertoire racine du package. Il contient trois dossiers et un fichier.

![](../images/6-2/1/packageintroduction-installingpackagefolder05.jpg)

> 1. Le dossier _bin_ contient les fichiers .dll. Ce package Dynamo a été développé à l’aide de la commande Zero Touch, de sorte que les nœuds personnalisés sont conservés dans ce dossier.
> 2. Le dossier _dyf_ contient les nœuds personnalisés. Ce package n’a pas été développé à l’aide de nœuds personnalisés Dynamo. Ce dossier est donc vide pour ce package.
> 3. Le dossier supplémentaire contient tous les fichiers supplémentaires, y compris les fichiers d'exemple.
> 4. Le fichier pkg est un fichier texte de base qui définit les paramètres du package. Pour l'instant, vous pouvez l'ignorer.

Lorsque vous ouvrez le dossier "extra", vous découvrez la présence de fichiers d'exemple téléchargés lors de l'installation. Les packages ne possèdent pas tous des fichiers d'exemple, mais lorsque ces derniers font partie d'un package, ils sont enregistrés ici.

Ouvrez « SphereUnfold ».

![](../images/6-2/1/rd2.jpg)

Après avoir ouvert le fichier et cliqué sur « Exécuter » dans le solveur, vous obtenez une sphère dépliée. Les fichiers d’exemple comme ceux-ci sont utiles pour apprendre à utiliser un nouveau package Dynamo.

\![](<../images/6-2/1/packageintroduction-installingpackagefolder07 (1) (2).jpg>)

### Parcourir et afficher les informations sur le package

Dans le gestionnaire de package, vous pouvez rechercher des packages à l’aide des options de tri et de filtrage de l’onglet Rechercher des packages. Plusieurs filtres sont disponibles pour le programme hôte, l’état (nouveau, obsolète ou non obsolète) et si le package possède des dépendances.

En triant les packages, vous pouvez identifier les packages les mieux notés ou les plus téléchargés, ou trouver les packages ayant fait l’objet de mises à jour récentes. 

Vous pouvez également obtenir plus de détails sur chaque paquet en cliquant sur Afficher les détails. Cela ouvre un panneau latéral dans le Gestionnaire de package, où vous pouvez trouver des informations telles que la version et les dépendances, l’URL du site Web ou du dépôt, les informations sur la licence, etc.

### Site Web du gestionnaire de package Dynamo

Une autre façon de découvrir les packages Dynamo est d’explorer le site Web du [gestionnaire de package Dynamo](http://dynamopackages.com). Vous y trouverez les dépendances des packages et les informations sur la compatibilité hôte/version fournies par les auteurs de chaque package. Vous pouvez également télécharger les fichiers de package à partir du gestionnaire de package Dynamo, mais le processus de Dynamo est plus simple.

![](../images/6-2/1/dpm2.jpg)

### Où sont les fichiers de package stockés localement ?

Si vous souhaitez voir où vos fichiers de package sont conservés, dans la barre de navigation supérieure, cliquez sur Dynamo > Préférences > Paramètres de package > Emplacement des fichiers de nœuds et de packages. Vous pouvez trouver le répertoire de votre dossier racine actuel à partir d’ici.

![](../images/6-2/1/packageintroduction-installingpackagefolder08.jpg)

Par défaut, les packages sont installés dans un emplacement semblable à celui du dossier suivant : _C:/Utilisateurs/[nom d’utilisateur]/AppData/Itinérance/Dynamo/[Version Dynamo]_.

### Configuration d’un emplacement partagé pour les packages dans un bureau

Pour les utilisateurs qui se demandent s’il est possible de déployer Dynamo (sous quelque forme que ce soit) avec des packages pré-attachés : pour résoudre ce problème et permettre un contrôle centralisé pour tous les utilisateurs avec des installations Dynamo, ajoutez un chemin d’accès au package personnalisé à chaque installation.

**Ajout d’un dossier réseau dans lequel le responsable BIM ou d’autres personnes pourraient superviser le stockage du dossier avec des packages approuvés par le bureau**  

Dans l’interface utilisateur d’une application individuelle, accédez à *Dynamo -> Préférences -> Paramètres de packages -> Emplacements des fichiers de nœuds et de packages*. Dans la boîte de dialogue, appuyez sur le bouton « Ajouter un chemin » et accédez à l’emplacement réseau de la ressource de package partagée. 
 
Comme il s’agit d’un processus automatisé, il faut ajouter des informations au fichier de configuration installé avec Dynamo :  
 `C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]\DynamoSettings.xml`

Par défaut, la configuration de Dynamo for Revit est la suivante :
 
 
`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`</CustomPackageFolders>`

L’ajout d’un emplacement personnalisé ressemble à ce qui suit :  

`<CustomPackageFolders>`  

`<string>C:\Users\[Username]\AppData\Roaming\Dynamo\Dynamo Revit\[Dynamo Version]</string>`  

`<string>N:\OfficeFiles\Dynamo\Packages_Limited</string>`  

`</CustomPackageFolders>`


Pour contrôler la gestion centralisée de ce dossier, vous pouvez simplement le mettre en lecture seule.

### Charger des packages avec des binaires à partir d’un emplacement réseau

#### Scénario

Une organisation peut vouloir harmoniser les packages installés par différents postes de travail et utilisateurs. Pour ce faire, vous pouvez installer ces packages sous *Dynamo -> Préférences -> Paramètres de package -> Chemins d’accès des nœuds et packages*, en sélectionnant un dossier réseau en tant qu’emplacement d’installation et en demandant aux stations de travail d’ajouter ce chemin à `Manage Node and Package Paths`.

#### Problème

Bien que le scénario fonctionne correctement pour les packages qui contiennent uniquement des nœuds personnalisés, il peut ne pas fonctionner pour les packages contenant des binaires, comme les nœuds Zero-Touch. Ce problème est dû à des [mesures de sécurité](https://stackoverflow.com/questions/5328274/load-assembly-from-network-location) que .NET Framework impose aux ensembles chargés lorsqu’ils proviennent d’un emplacement réseau. Malheureusement, l’utilisation de l’élément de configuration `loadFromRemoteSources`, comme suggéré dans le thread cible, n’est pas une solution possible pour Dynamo, car il est distribué en tant que composant et non pas en tant qu’application.

#### Alternative

Une solution de contournement possible est d’utiliser un lecteur réseau mappé pointant vers l’emplacement réseau et de faire en sorte que les postes de travail fassent référence à ce chemin à la place. Les étapes à suivre pour créer un lecteur réseau mappé sont décrites [ici](https://support.microsoft.com/en-us/help/4026635/windows-10-map-a-network-drive).

### Repousser les limites des packages

La communauté Dynamo est en constante évolution. Explorez le gestionnaire de package Dynamo de temps en temps : vous y découvrirez de nouvelles avancées intéressantes. Dans les sections suivantes, vous allez examiner les packages de manière plus approfondie, du point de vue de l'utilisateur final à la création de votre propre package Dynamo.
