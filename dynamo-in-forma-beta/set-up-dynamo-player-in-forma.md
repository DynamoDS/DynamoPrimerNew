# Configurer Dynamo Player dans Forma

Pour utiliser Dynamo avec Forma, vous avez deux options : Dynamo as a Service (DaaS) basé sur le cloud ou la version de bureau Dynamo. Chaque option a ses avantages en fonction de ce que vous voulez faire. Avant de commencer, réfléchissez donc bien à celle qui répond le plus à vos besoins. Cependant, n’oubliez pas que vous pouvez passer d’une option à l’autre à tout moment.

**Comparatif entre Dynamo as a Service et Dynamo Desktop**

<table><thead><tr><th>Dynamo as a Service</th><th>Bureau</th><th data-hidden></th></tr></thead><tbody><tr><td>Facilité d’installation</td><td>Processus d’installation en plusieurs étapes</td><td></td></tr><tr><td>Pas besoin d’installer Dynamo ou de l’ouvrir</td><td>Dynamo doit être ouvert</td><td></td></tr><tr><td>Ne reconnaît pas un graphe déjà ouvert dans Dynamo</td><td>Ouvrez un graphe ouvert dans Dynamo dans Player en un seul clic</td><td></td></tr><tr><td>Ne peut pas utiliser Python</td><td>Peut utiliser Python</td><td></td></tr><tr><td>Ne peut utiliser que les packages approuvés</td><td>Peut utiliser n’importe quel package</td><td></td></tr></tbody></table>

Sur cette page, nous allons vous expliquer comment configurer les deux options.

### Configuration de Dynamo as a Service

Dynamo dans Forma est actuellement disponible en version bêta ouverte en accès anticipé, ce qui signifie que les fonctionnalités et l’interface utilisateur peuvent changer fréquemment.

Commençons par installer Dynamo Player dans Forma.

1. Sur votre site Forma, accédez à **Extensions** dans la barre latérale gauche et cliquez sur **Add extension**. L’App Store Autodesk s’ouvre.
2. Recherchez Dynamo et ajoutez la version bêta de Dynamo Player. Lisez la clause de non-responsabilité et cliquez sur **Agree**.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Dynamo Player est maintenant disponible dans vos extensions. Cliquez dessus pour l’ouvrir.
4. Vous pouvez commencer à utiliser Dynamo Player !

### Configuration de Dynamo Desktop

Pour utiliser Dynamo Desktop, vous aurez besoin de Dynamo, soit en tant que sandbox autonome, soit connecté à Revit ou Civil 3D. Vous aurez également besoin du package DynamoFormaBeta.

#### Revit

Suivez ces instructions pour configurer Dynamo dans Revit et le package DynamoFormaBeta.

1. Assurez-vous que Revit 2024.1 ou une version ultérieure est installée.
2. Ouvrez Dynamo à partir de Revit en accédant à Gérer > Dynamo.
3. Dans Dynamo, installez le package DynamoFormaBeta. Accédez à Packages > Gestionnaire de packages, puis recherchez DynamoFormaBeta.
   1. Si vous utilisez Revit 2024, installez le package DynamoFormaBeta pour la version 2.x.
   2. Si vous utilisez Revit 2025, installez le package DynamoFormaBeta.

#### Civil 3D

Suivez ces instructions pour configurer Dynamo dans Civil 3D et le package DynamoFormaBeta.

1. Assurez-vous que Civil 3D 2024.1 ou une version ultérieure est installée.
2. Ouvrez Dynamo à partir de Civil 3D en accédant à Gérer > Dynamo.
3. Dans Dynamo, installez le package DynamoFormaBeta. Accédez à Packages > Gestionnaire de packages, puis recherchez DynamoFormaBeta.
   1. Si vous utilisez Civil 3D 2024, installez le package DynamoFormaBeta pour la version 2.x.
   2. Si vous utilisez Civil 3D 2025, installez le package DynamoFormaBeta.

#### Dynamo Sandbox

Suivez ces instructions pour installer Dynamo Sandbox et le package DynamoFormaBeta.

1. Téléchargez Dynamo version 2.18.0 ou ultérieure à partir de [builds Dynamo](https://dynamobuilds.com/). Pour une expérience optimale, choisissez la dernière des versions les plus stables, qui s’affiche en haut de la liste.
   1. Les versions Daily sont des versions de développement et peuvent inclure des fonctionnalités incomplètes ou en cours d’élaboration.
2. Extrayez Dynamo à l’aide de [7zip](https://www.7-zip.fr/) dans le dossier de votre choix.
3. Exécutez DynamoSandbox.exe à partir du dossier d’installation de Dynamo.
4. Dans Dynamo, installez le package DynamoFormaBeta. Accédez à Packages > Gestionnaire de packages, puis recherchez DynamoFormaBeta.
   1. Si vous utilisez Dynamo 2.x, installez le package DynamoFormaBeta pour la version 2.x.
   2. Si vous utilisez Dynamo 3.x, installez le package DynamoFormaBeta.

Une fois Dynamo installé, vous pouvez commencer à l’utiliser avec Forma. Lorsque vous exécutez l’option de bureau de Dynamo dans Forma, Dynamo doit être ouvert pour pouvoir utiliser l’extension Dynamo Player.

#### Accès à Dynamo Desktop dans Forma

Commençons par installer Dynamo Player dans Forma.

1. Sur votre site Forma, accédez à **Extensions** dans la barre latérale gauche et cliquez sur **Add extension**. L’App Store Autodesk s’ouvre.
2. Recherchez Dynamo et ajoutez la version bêta de Dynamo Player. Lisez la clause de non-responsabilité et cliquez sur **Agree**.

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Dynamo Player est maintenant disponible dans vos extensions. Cliquez dessus pour l’ouvrir.
4. Dans la partie supérieure, cliquez sur Desktop pour accéder à Dynamo Desktop.

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. Vous pouvez commencer à utiliser Dynamo Player ! Si vous avez déjà un graphe ouvert dans Dynamo, cliquez simplement sur Open sous **Connected graph** pour l’afficher dans Player.
