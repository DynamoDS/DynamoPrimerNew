# Générer Dynamo à partir de la source

La source de Dynamo est hébergée sur Github pour que tout le monde puisse la cloner et y apporter sa contribution. Dans ce chapitre, nous verrons comment cloner le dépôt à l’aide de git, compiler les fichiers sources avec Visual Studio, exécuter et déboguer une génération locale, et extraire toutes les nouvelles modifications de Github.

### Localiser les dépôts Dynamo sur Github <a href="#locating-the-dynamo-repositories-on-github" id="locating-the-dynamo-repositories-on-github"></a>

Github est un service d’hébergement basé sur [git](https://docs.github.com/fr/get-started/quickstart/git-and-github-learning-resources), un système de contrôle de version permettant de suivre les modifications et de coordonner le travail entre les personnes. Git est un outil que nous pouvons utiliser pour télécharger les fichiers sources de Dynamo, et les maintenir à jour avec quelques commandes. L’utilisation de cette méthode permet d’éviter le travail fastidieux et inutile consistant à télécharger et à remplacer manuellement les fichiers sources à chaque mise à jour. Le système de contrôle de version git suit toutes les différences entre un dépôt de code local et un dépôt de code distant.

La source de Dynamo est hébergée sur Github DynamoDS dans ce dépôt : [https://github.com/DynamoDS/Dynamo](https://github.com/DynamoDS/Dynamo)

![Les fichiers sources de Dynamo](images/github.jpg)
> Les fichiers source de Dynamo.
>
> 1. Cloner ou télécharger l’intégralité du dépôt
> 2. Afficher d’autres dépôts DynamoDS
> 3. Fichiers source de Dynamo
> 4. Fichiers spécifiques à Git

### Tirer le dépôt Dynamo à l’aide de git <a href="#pulling-the-dynamo-repository-using-git" id="pulling-the-dynamo-repository-using-git"></a>

Avant de pouvoir cloner le dépôt, nous devons installer git. Suivez ce [guide](https://docs.github.com/fr/get-started/quickstart/set-up-git#setting-up-git) pour connaître les étapes à suivre pour l’installation et savoir comment configurer un nom d’utilisateur et une adresse email GitHub. Pour cet exemple, nous utiliserons git en ligne de commande. Ce guide part du principe que vous utilisez Windows, mais vous pouvez également utiliser git sur Mac ou Linux pour cloner la source de dynamo.

Nous avons besoin d’une URL pour le dépôt Dynamo à cloner. Celle-ci se trouve en cliquant sur le bouton « Cloner ou télécharger » sur la page du dépôt. Copiez l’URL pour la coller dans l’invite de commande.

![Clonage d’un dépôt](images/github-clone.png)
> 1. Sélectionnez « Cloner ou télécharger »
> 2. Copiez l’URL

Une fois git installé, nous pouvons cloner le dépôt Dynamo. Commencez par ouvrir l’invite de commande. Utilisez ensuite la commande de changement de répertoire `cd` pour naviguer jusqu’au dossier dans lequel les fichiers sources doivent être clonés. Dans ce cas, nous avons créé un dossier appelé `Github` dans `Documents`.

`cd C:\Users\username\Documents\GitHub`

> Remplacez « username » par votre nom d’utilisateur

![Invite de commande](images/cli-1.jpg)

Dans l’étape suivante, nous allons lancer une commande git pour cloner le dépôt Dynamo à l’emplacement que nous avons spécifié. L’URL de la commande est obtenue en cliquant sur le bouton « Cloner ou télécharger » sur Github. Exécutez cette commande dans le terminal de commande. Notez que cela clonera la branche master du dépôt Dynamo qui est le code le plus à jour de Dynamo, et contiendra la dernière version du code de Dynamo. Cette branche change quotidiennement.

`git clone https://github.com/DynamoDS/Dynamo.git`

![Résultats de l’opération de clonage de Git](images/cli-2.jpg)

Nous savons que git fonctionne si l’opération de clonage s’est terminée avec succès. Dans l’explorateur de fichiers, naviguez jusqu’au répertoire où vous avez cloné pour voir les fichiers sources. La structure du répertoire doit être identique à la branche master du dépôt Dynamo sur Github.

![Fichiers source de Dynamo](images/source-files.jpg)

> 1. Fichiers source de Dynamo
> 2. Fichiers Git

### Générer le dépôt à l’aide de Visual Studio <a href="#building-the-repository-using-visual-studio" id="building-the-repository-using-visual-studio"></a>

Les fichiers sources étant maintenant clonés sur notre machine locale, nous pouvons générer un fichier exécutable pour Dynamo. Pour ce faire, nous devons configurer l’IDE Visual Studio et nous assurer que le .NET Framework et DirectX sont installés.

* Téléchargez et installez [Microsoft Visual Studio Community 2015](https://my.visualstudio.com/Downloads/Results), un IDE (environnement de développement intégré) gratuit et complet (les versions ultérieures peuvent également fonctionner).
* Téléchargez et installez [Microsoft .NET Framework 4.5](https://www.microsoft.com/fr-fr/download/details.aspx?id=30653) ou une version ultérieure.
* Installez Microsoft DirectX à partir du dépôt local Dynamo (`Dynamo\tools\install\Extra\DirectX\DXSETUP.exe`).

> .NET et DirectX peuvent être déjà installés.

> **Remarque :** changement majeur : **Visual Studio 2022 Preview / Visual Studio 2026 Insider** requis
> 
> À partir de la fin de 2025, Dynamo mettra en œuvre la structure `dotnet10.0`. Pour développer dans cette structure, vous aurez besoin de Visual Studio 2022 Preview ou Visual Studio 2026 Insider (ou versions ultérieures), car les versions stables ne prennent pas encore en charge .NET 10.0.
> 
> **Installation de Visual Studio 2022 Preview / 2026 Insider avec votre installation existante :**
> 1. Ouvrez le **programme d’installation de Visual Studio** (recherchez-le dans votre menu Démarrer).
> 2. Cliquez sur **Mettre à jour** pour vous assurer de disposer de la version la plus récente du programme d’installation.
> 3. Accédez à l’onglet **Disponible**.
> 4. Recherchez **Visual Studio 2022 Preview / 2026 Insider** (Community, Professional ou Enterprise).
> 5. Cliquez sur **Installer** pour l’ajouter avec votre installation Visual Studio existante.
> 
![Visual Studio Preview](images/vs-preview.png) ![Visual Studio 2026 Insider](images/vs-2026-insiders.png)

Une fois l’installation terminée, nous pouvons lancer Visual Studio et ouvrir la solution `Dynamo.All.sln` située dans `Dynamo\src`.

![Ouverture du fichier solution](images/vs-open-dynamo.jpg)

> 1. Sélectionnez `File > Open > Project/Solution`
> 2. Naviguez jusqu’au dépôt Dynamo et ouvrez le dossier `src`
> 3. Sélectionnez le fichier solution `Dynamo.All.sln`
> 4. Sélectionnez `Open`

Avant de pouvoir générer la solution, vous devez spécifier quelques paramètres. Nous devrions d’abord générer une version de débogage de Dynamo afin que Visual Studio puisse recueillir plus d’informations pendant le débogage pour nous aider à développer, et nous voulons cibler AnyCPU.

![Paramètres de la solution](images/vs-dynamo-build-settings.jpg)

> Ils deviendront des dossiers dans le dossier `bin`
>
> 1. Dans cet exemple, nous avons choisi `Debug` comme configuration de la solution
> 2. Définissez la plateforme de solution sur `Any CPU`

Une fois le projet ouvert, nous pouvons générer la solution. Ce processus créera un fichier DynamoSandbox.exe que vous pouvez exécuter.

![Générer la solution](images/vs-build-dynamo.jpg)

> La génération du projet restaurera les dépendances NuGet.
>
> 1. Sélectionnez `Build > Build Solution`
> 2. Vérifiez que la génération a réussi dans la fenêtre de sortie, elle devrait ressembler à `==== Build: 69 succeeded, 0 failed, 0 up-to-date, 0 skipped ====`

### Exécution d’une génération locale <a href="#running-a-local-build" id="running-a-local-build"></a>

Si Dynamo est généré correctement, un dossier `bin` est créé dans le dépôt Dynamo avec le fichier DynamoSandbox.exe. Dans notre cas, nous générons avec l’option Debug, donc le fichier exécutable est situé dans `bin\AnyCPU\Debug`. L’exécution de cette commande ouvrira une version locale de Dynamo.

![Exécutable DynamoSandbox](images/ex-dynamosandbox.jpg)

> 1. L’exécutable DynamoSandbox que nous venons de générer. Exécutez cette commande pour démarrer Dynamo.

Nous sommes maintenant presque prêts à commencer à développer pour Dynamo.

Pour des instructions sur la génération de Dynamo pour d’autres plate-formes (par exemple Linux ou OS X), visitez cette [page wiki](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-on-Linux,-Mac).

### Déboguer une version locale à l’aide de Visual Studio <a href="#debugging-a-local-build-using-visual-studio" id="debugging-a-local-build-using-visual-studio"></a>

Le débogage est un processus qui permet d’identifier, d’isoler et de corriger un bogue ou un problème. Une fois que Dynamo a été généré à partir de la source, nous pouvons utiliser plusieurs outils dans Visual Studio pour déboguer une application en cours d’exécution, le complément DynamoRevit par exemple. Nous pouvons analyser son code source pour trouver l’origine d’un problème, ou observer le code en cours d’exécution. Pour une explication plus détaillée sur la manière de déboguer et de parcourir le code dans Visual Studio, reportez-vous à la [documentation de Visual Studio](https://docs.microsoft.com/fr-fr/visualstudio/debugger/navigating-through-code-with-the-debugger).

Pour l’application Dynamo autonome, DynamoSandbox, nous allons couvrir deux options de débogage :

* générer et démarrer Dynamo directement à partir de Visual Studio ;
* associer Visual Studio à un processus en cours d’exécution de Dynamo.

Démarrer Dynamo à partir de Visual Studio génère à nouveau la solution pour chaque session de débogage si nécessaire, donc si nous avons fait des changements à la source, ils seront incorporés lors du débogage. La solution `Dynamo.All.sln` étant toujours ouverte, sélectionnez `Debug`, `AnyCPU` et `DynamoSandbox` dans les menus déroulants, puis cliquez sur `Start`. Cela va générer Dynamo et démarrer un nouveau processus (DynamoSandbox.exe), et y attacher le débogueur de Visual Studio.

![Génération et démarrage d’une application à partir de Visual Studio](images/vs-debug-options.jpg)

> Générer et démarrer une application à partir de Visual Studio
>
> 1. Définissez la configuration sur `Debug`
> 2. Définissez la plate-forme sur `Any CPU`
> 3. Définissez le projet de démarrage sur `DynamoSandbox`
> 4. Cliquez sur `Start` pour commencer le processus de débogage

Il est également possible de déboguer un processus Dynamo déjà en cours d’exécution pour résoudre un problème lié à un graphique ouvert ou à un package spécifique. Pour ce faire, nous devons ouvrir les fichiers sources du projet dans Visual Studio et nous attacher à un processus Dynamo en cours d’exécution en utilisant l’élément de menu de débogage `Attach to Process`.

![Boîte de dialogue Attacher au processus](images/vs-attach-dynamosandbox.jpg)

> Attacher un processus en cours d’exécution à Visual Studio
>
> 1. Sélectionnez `Debug > Attach to Process...`
> 2. Choisissez `DynamoSandbox.exe`
> 3. Sélectionnez `Attach`

Dans les deux cas, nous attachons le débogueur à un processus que nous souhaitons déboguer. Nous pouvons définir des points d’arrêt dans le code avant ou après le démarrage du débogueur, ce qui entraînera une pause du processus immédiatement avant l’exécution de cette ligne de code. Si une exception non interceptée est levée lors du débogage, Visual Studio accède à l’emplacement où elle s’est produite dans le code source. Cette méthode est efficace pour rechercher des blocages simples, des exceptions non gérées et pour comprendre le flux d’exécution d’une application.

![Définition d’un point d’arrêt](images/vs-debug-dynamocore.jpg)

> Lors du débogage de DynamoSandbox, nous avons placé un point d’arrêt dans le constructeur du nœud Color.ByARGB qui provoque une pause dans le processus Dynamo lorsque le nœud est instancié. Si ce nœud lance une exception ou provoque le blocage de Dynamo, nous pouvons parcourir chaque ligne du constructeur pour déterminer où le problème se produit.
>
> 1. Le point d’arrêt
> 2. La pile d’appels montre la fonction en cours d’exécution et les appels de fonction précédents

Dans la section suivante, **Générer DynamoRevit à partir de la source**, nous aborderons un exemple spécifique de débogage et expliquerons comment définir des points d’arrêt, parcourir le code et lire la pile d’appels.

### Tirer la dernière version<a href="#pulling-latest-build" id="pulling-latest-build"></a>

Puisque la source de Dynamo est hébergée sur Github, la façon la plus simple de garder les fichiers source locaux à jour est de tirer les changements en utilisant les commandes git.

En utilisant la ligne de commande, définissez le répertoire courant du dépôt Dynamo :

`cd C:\Users\username\Documents\GitHub\Dynamo`

> Remplacez `"username"` par votre nom d’utilisateur

Utilisez la commande suivante pour tirer les dernières modifications :

`git pull origin master`

![Mise à jour du dépôt local](images/cli-pull-changes.jpg)

> 1. Nous pouvons voir ici que le dépôt local a été mis à jour avec les modifications apportées par le dépôt distant.

En plus de tirer des mises à jour, il y a quatre autres flux de travail git avec lesquels il faut se familiariser.

* **Dupliquer** le dépôt Dynamo permet de créer une copie séparée de l’original. Tout changement effectué ici n’affectera pas le dépôt original et les mises à jour peuvent être récupérées ou soumises avec des demandes de tirage. Dupliquer n’est pas une commande git, mais un flux de travail que github ajoute. Le modèle de duplication et de tirage est l’un des flux de travail les plus courants pour contribuer à des projets open source en ligne. Cela vaut la peine de l’apprendre si vous voulez contribuer à Dynamo.
* Une **branche** permet de travailler sur des expériences ou de nouvelles fonctionnalités isolées des autres travaux dans les branches. Cela facilite l’envoi de demandes de tirage.
* Effectuez des **commits** fréquemment, après avoir terminé une unité de travail et après une modification qui pourrait être annulée. Un commit enregistre les changements dans le dépôt et sera visible lors d’une demande de tirage dans le dépôt principal de Dynamo.
* Créez des **demandes de tirage** lorsque les changements sont prêts à être officiellement proposés au dépôt principal de Dynamo.

L’équipe Dynamo a des instructions spécifiques sur la création de demandes de tirage. Reportez-vous à la section Demandes de tirage de cette documentation pour obtenir des informations plus détaillées sur les éléments à traiter.

Reportez-vous à cette [page de documentation](https://git-scm.com/docs) pour obtenir une liste de référence des commandes git.
