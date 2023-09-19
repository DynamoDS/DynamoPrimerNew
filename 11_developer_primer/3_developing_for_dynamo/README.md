# Développer pour Dynamo

Quel que soit le niveau d’expérience, la plateforme Dynamo est conçue pour que tous les utilisateurs soient des contributeurs. Il existe plusieurs options de développement qui ciblent des capacités et des niveaux de compétence différents, chacune avec ses forces et ses faiblesses selon l’objectif. Nous présentons ci-dessous les différentes options et comment les choisir.

![Trois environnements de développement](images/developing-for-dynamo.png)

> Trois environnements de développement : Visual Studio, Python Editor et Code Block DesignScript

#### Quelles sont les options proposées ? <a href="#what-are-my-options" id="what-are-my-options"></a>

Les options de développement de Dynamo sont principalement réparties en deux catégories : _pour_ Dynamo et _dans_ Dynamo. Les deux catégories peuvent être considérées comme : « dans » Dynamo implique un contenu créé à l’aide de l’environnement de développement intégré (IDE) de Dynamo pour être utilisé dans Dynamo, et « pour » Dynamo implique l’utilisation d’outils externes pour créer du contenu qui sera importé dans Dynamo pour être utilisé. Bien que ce guide soit axé sur le développement _pour_ Dynamo, les ressources pour tous les processus sont décrites ci-dessous.

#### Pour Dynamo <a href="#for-dynamo" id="for-dynamo"></a>

Ces nœuds permettent le plus haut degré de personnalisation. De nombreux packages sont créés avec cette méthode, et elle est nécessaire pour contribuer à la source de Dynamo. Le processus de génération sera abordé dans ce guide.

* Nœuds Zero-Touch
* Nœuds dérivés de NodeModel
* Extensions

> Le guide contient des instructions sur l’[importation de bibliothèques Zero-Touch](https://primer2.dynamobim.org/v/fr/6_custom_nodes_and_packages/6-2_packages/5-zero-touch).

Visual Studio est utilisé comme environnement de développement pour les nœuds Zero Touch et NodeModel dans les explications ci-dessous.

![Interface Visual Studio](images/vs-devenv.jpg)

> L’interface Visual Studio avec un projet que nous allons développer

#### Dans Dynamo <a href="#in-dynamo" id="in-dynamo"></a>

Bien que ces processus existent dans l’espace de travail de programmation visuelle et soient relativement simples, ils constituent tous des options viables pour personnaliser Dynamo. Le guide couvre ces aspects de manière détaillée et fournit des conseils et des meilleures pratiques en matière d’utilisation de scripts dans le chapitre [Stratégies d’utilisation de scripts](../../9\_best\_practices/2-scripting-strategies.md).

*   Les blocs de code exposent DesignScript dans l’environnement de programmation visuel, ce qui permet des flux de travail de nœud et de script de texte flexibles. Une fonction dans un bloc de code peut être appelée par n’importe quel élément de l’espace de travail.

    > Téléchargez un exemple de bloc de code (cliquez avec le bouton droit de la souris et choisissez Enregistrer sous) ou découvrez une présentation détaillée dans le [guide](https://primer2.dynamobim.org/v/fr/8_coding_in_dynamo/8-1_code-blocks-and-design-script/1-what-is-a-code-block).
*   Les nœuds personnalisés sont des conteneurs destinés à des ensembles de nœuds ou même à des graphiques entiers. Ils sont un moyen efficace de collecter les routines fréquemment utilisées et de les partager avec la communauté.

    > Téléchargez un exemple de nœud personnalisé (cliquez avec le bouton droit de la souris et choisissez Enregistrer sous) ou découvrez une présentation détaillée dans le [guide](https://primer2.dynamobim.org/v/fr/6_custom_nodes_and_packages/6-1_custom-nodes/1-introduction).
*   Les nœuds Python sont une interface de script dans l’espace de travail de programmation visuelle, similaire aux blocs de code. Les bibliothèques Autodesk.DesignScript utilisent une notation par points similaire à DesignScript.

    > Téléchargez un exemple de nœud Python (cliquez avec le bouton droit de la souris et choisissez Enregistrer sous) ou découvrez une présentation détaillée dans le [guide](https://primer2.dynamobim.org/v/fr/8_coding_in_dynamo/8-3_python).

Le développement dans l’espace de travail de Dynamo est un outil puissant qui permet d’obtenir un retour immédiat.

![Développement dans l’espace de travail Dynamo avec le nœud Python](images/python-example.jpg)

> Développement dans l’espace de travail Dynamo avec le nœud Python

#### Quels sont les avantages/inconvénients de chacun ? <a href="#what-are-the-advantagesdisadvantages-of-each" id="what-are-the-advantagesdisadvantages-of-each"></a>

Les options de développement de Dynamo ont été conçues pour répondre à la complexité d’un besoin de personnalisation. Que ce soit pour écrire un script récursif en Python ou pour générer une interface utilisateur de nœud entièrement personnalisée, il existe des options d’implémentation de code qui n’impliquent que ce qui est nécessaire pour être opérationnel.

**Blocs de code, nœud Python et nœuds personnalisés dans Dynamo**

Ces options sont simples pour écrire du code dans l’environnement de programmation visuelle Dynamo. L’espace de programmation visuelle Dynamo permet d’accéder à Python, DesignScript et de contenir plusieurs nœuds dans un nœud personnalisé.

![Bloc de code, script Python et nœud personnalisé](images/Development-Icons.png)

Avec ces méthodes, vous pouvez :

* commencer à écrire des scripts Python ou DesignScript avec peu ou pas de configuration ;
* importer des bibliothèques Python dans Dynamo ;
* partager des blocs de code, des nœuds Python et des nœuds personnalisés avec la communauté Dynamo dans un package.

**Nœuds Zero-Touch**

Zero Touch fait référence à une méthode pointer-cliquer simple permettant d’importer des bibliothèques C#. Dynamo lit les méthodes publiques d’un fichier `.dll` et les convertit en nœuds Dynamo. Vous pouvez utiliser le Zero-Touch pour développer vos propres nœuds et packages personnalisés.

![Nœuds Zero-Touch](images/ZTImport.png)

Avec cette méthode, vous pouvez :

* importer une bibliothèque qui n’a pas nécessairement été développée pour Dynamo et créer automatiquement une suite de nouveaux nœuds, comme l’[exemple d’A-Forge](../../6\_custom\_nodes\_and\_packages/6-2\_packages/5-zero-touch.md#case-study-importing-aforge) dans le guide ;
* écrire des méthodes C# et les utiliser facilement comme nœuds dans Dynamo ;
* partager une bibliothèque C# en tant que nœuds avec la communauté Dynamo dans un package.

**Nœuds dérivés de NodeModel**

Ces nœuds permettent d’approfondir la structure de Dynamo. Ils sont basés sur la classe `NodeModel` et écrits en C#. Bien que cette méthode soit la plus souple et la plus puissante, la plupart des aspects du nœud doivent être définis de manière explicite et les fonctions doivent se trouver dans un ensemble séparé.

![Nœuds dérivés de NodeModel](images/Development-Icons-NodeModel.png)

Avec cette méthode, vous pouvez :

* créer une interface utilisateur de nœuds entièrement personnalisable avec des curseurs, des images, des couleurs, etc. (p. ex. un nœud ColorRange) ;
* accéder et agir sur la zone de dessin Dynamo ;
* personnaliser la combinaison ;
* charger dans Dynamo comme un package.

#### Présentation du contrôle des versions de Dynamo et des modifications apportées à l’API (1.x → 2.x) <a href="#understanding-dynamo-versioning-and-api-changes-1x-2x" id="understanding-dynamo-versioning-and-api-changes-1x-2x"></a>

Comme Dynamo est régulièrement mis à jour, des modifications peuvent être apportées à une partie de l’API utilisée par un package. Il est important de suivre ces modifications pour s’assurer que les packages existants continuent à fonctionner correctement.

Les modifications de l’API sont répertoriées sur le [Wiki de Github sur Dynamo](https://github.com/DynamoDS/Dynamo/wiki/API-Changes). Cela couvre les modifications apportées à DynamoCore, aux bibliothèques et aux espaces de travail.

![Document des modifications de l’API Dynamo](images/api-changes.jpg)

Le passage du format de fichier XML au format JSON dans la version 2.0 est un exemple de changement important à venir. Les nœuds dérivés de NodeModel auront désormais besoin d’un [constructeur JSON](https://github.com/DynamoDS/Dynamo/wiki/Write-a-Json-Constructor-for-a-NodeModel-Node), sinon ils ne s’ouvriront pas dans Dynamo 2.0.

La documentation de l’API Dynamo couvre actuellement les fonctionnalités de base : [http://dynamods.github.io/DynamoAPI](http://dynamods.github.io/DynamoAPI)

![Documentation de l’API](images/api-docs.jpg)

#### Autorisation de distribuer des binaires dans un package <a href="#permission-to-distribute-binaries-in-a-package" id="permission-to-distribute-binaries-in-a-package"></a>

Faites attention aux fichiers .dll inclus dans un package qui sont téléchargés dans le gestionnaire de package. Si l’auteur du package n’a pas créé le fichier .dll, il doit disposer des droits nécessaires pour le partager.

Si un package comprend des binaires, les utilisateurs doivent être avertis lors du téléchargement que celui-ci en contient.
