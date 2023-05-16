# Publier un package

### Publier un package <a href="#publish-a-package" id="publish-a-package"></a>

Les packages sont un moyen pratique de stocker et de partager des nœuds avec la communauté Dynamo. Un paquet peut contenir tout ce qui est nécessaire, des nœuds personnalisés créés dans l’espace de travail Dynamo aux nœuds dérivés de NodeModel. Les packages sont publiés et installés à l’aide du gestionnaire de package. En plus de cette page, le [guide](https://primer2.dynamobim.org/6_custom_nodes_and_packages/6-2_packages/1-introduction) contient un guide général sur les packages.

#### Qu’est-ce qu’un gestionnaire de package ? <a href="#what-is-a-package-manager" id="what-is-a-package-manager"></a>

Le gestionnaire de package Dynamo est un registre de logiciels (similaire à npm) accessible à partir de Dynamo ou d’un navigateur web. Le gestionnaire de package comprend l’installation, la publication, la mise à jour et la visualisation des packages. Comme npm, il conserve différentes versions des packages. Il permet également de gérer les dépendances de votre projet.

Dans le navigateur, recherchez des packages et consultez les statistiques : [https://dynamopackages.com/](https://dynamopackages.com)

* Dans Dynamo, le gestionnaire de package inclut les packages d’installation, de publication et de mise à jour.

![Recherche de packages](images/dynamopackagemanager.jpg)

> 1. Rechercher des packages en ligne : `Packages > Search for a Package...`
> 2. Afficher/modifier les packages installés : `Packages > Manage Packages...`
> 3. Publier un nouveau package : `Packages > Publish New Package...`

#### Publier un package <a href="#publishing-a-package" id="publishing-a-package"></a>

Les packages sont publiés à partir du gestionnaire de package dans Dynamo. Le processus recommandé consiste à publier localement, tester le package, puis publier en ligne pour le partager avec la communauté. En utilisant l’étude de cas NodeModel, nous allons suivre les étapes nécessaires pour publier le nœud RectangularGrid en tant que package localement et ensuite en ligne.

Lancez Dynamo et sélectionnez `Packages > Publish New Package...` pour ouvrir la fenêtre `Publish a Package`.

![Publier un package](images/dyn-publish-package-add-files.jpg)

> 1. Sélectionnez `Add file...` pour rechercher les fichiers à ajouter au package
> 2. Sélectionnez les deux fichiers `.dll` dans l’étude de cas NodeModel
> 3. Sélectionnez `Ok`

Une fois les fichiers ajoutés au contenu du package, donnez-lui un nom, une description et une version. Publier un package avec Dynamo crée automatiquement un fichier `pkg.json`.

![Paramètres du package](images/dyn-publish-package.jpg)

> Un package prêt à être publié.
>
> 1. Renseignez les informations requises pour le nom, la description et la version.
> 2. Publiez en cliquant sur « Publier localement » et sélectionnez le dossier de package de Dynamo : `AppData\Roaming\Dynamo\Dynamo Core\1.3\packages` pour que le nœud soit disponible dans Core. Publiez toujours localement jusqu’à ce que le package soit prêt à être partagé.

Après avoir publié un package, les nœuds seront disponibles dans la bibliothèque Dynamo sous la catégorie `CustomNodeModel`.

![Package dans la bibliothèque Dynamo](images/dyn-publish-package-library.jpg)

> 1. Le package que nous venons de créer dans la bibliothèque Dynamo

Une fois que le package est prêt à être publié en ligne, ouvrez le gestionnaire de package et choisissez `Publish` puis `Publish Online`.

![Publier un package dans le gestionnaire de package](images/dyn-publish-package-directory.jpg)

> 1. Pour voir comment Dynamo a formaté le package, cliquez sur les trois points verticaux à droite de « CustomNodeModel » et choisissez « Afficher le répertoire racine ».
> 2. Sélectionnez `Publish`, puis `Publish Online` dans la fenêtre « Publier un package Dynamo ».
> 3. Pour supprimer un package, sélectionnez `Delete`.

#### Comment mettre à jour un package ? <a href="#how-do-i-update-a-package" id="how-do-i-update-a-package"></a>

La mise à jour d’un package est un processus similaire à la publication. Ouvrez le gestionnaire de package et sélectionnez `Publish Version...` sur le package à mettre à jour, puis entrez une version supérieure.

![Publier une version de package](images/dyn-publish-package-version.jpg)

> 1. Sélectionnez `Publish Version` pour mettre à jour un package existant avec de nouveaux fichiers dans le répertoire racine, puis choisissez s’il doit être publié localement ou en ligne.

#### Client Web du gestionnaire de package <a href="#package-manager-web-client" id="package-manager-web-client"></a>

Le client Web du gestionnaire de package est utilisé exclusivement pour rechercher et afficher des données de package, telles que le contrôle des versions et les statistiques de téléchargement.

Le client Web du gestionnaire de package est accessible à l’adresse suivante : [https://dynamopackages.com/](https://dynamopackages.com)

![Client Web du gestionnaire de package](images/packagemanager-browser.jpg)
