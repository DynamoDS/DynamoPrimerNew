# Interface utilisateur

### Aperçu de l'interface utilisateur

L’interface utilisateur de Dynamo est organisée en cinq zones principales. Nous allons faire ici une présentation succincte et expliquer plus en détail l’espace de travail et la bibliothèque dans les sections suivantes.

![](<images/user interface - ui.jpg>)

> 1. Menus
> 2. Barre d'outils
> 3. Bibliothèque
> 4. Espace de travail
> 5. Barre d’exécution

### Menus

![](<images/user interface - menu.jpg>)

Voici les menus des fonctionnalités de base de l’application Dynamo. Comme la plupart des logiciels Windows, les deux premiers menus concernent la gestion des fichiers, les opérations de sélection et la modification du contenu. Les autres menus sont plus spécifiques de Dynamo.

#### Menus Dynamo

Des informations générales et des paramètres sont disponibles dans le menu déroulant **Dynamo**.

![](<images/user interface - dynamo menu.jpg>)

> 1. À propos de : découvrez la version de Dynamo installée sur votre ordinateur.
> 2. Accord sur la collecte des données d’utilisation : permet d’accepter ou de refuser le partage de vos données utilisateur pour améliorer Dynamo.
> 3. Préférences : inclut des paramètres tels que la précision décimale de l’application et la qualité du rendu de la géométrie.
> 4. Quitter Dynamo.

#### Aide

Si vous êtes bloqué, consultez le menu **Aide**. Vous pouvez accéder à l’un des sites Web de référence Dynamo via votre navigateur Internet.

![](<images/user interface - help menu.jpg>)

> 1. Mise en route : brève introduction à l'utilisation de Dynamo.
> 2. Guides interactifs.
> 3. Exemples : fichiers d'exemple de référence.
> 4. Dictionnaire Dynamo : ressource avec documentation sur tous les nœuds.
> 5. Site Web de Dynamo : affichez le projet Dynamo sur GitHub.
> 6. Wiki du projet Dynamo : permet de consulter le wiki pour en savoir plus sur le développement à l’aide de l’API Dynamo, qui prend en charge les bibliothèques et les outils.
> 7. Afficher la page de démarrage : permet de revenir à la page de démarrage de Dynamo lorsque vous vous trouvez dans un document.
> 8. Signaler un bogue : permet d'ouvrir un problème sur GitHub.

### Barre d'outils

La barre d’outils de Dynamo contient une série de boutons permettant d’accéder rapidement aux fichiers et aux commandes Annuler \[Ctrl + Z] et Rétablir \[Ctrl + Y]. À l'extrémité droite se trouve un autre bouton qui permet d'exporter un cliché de l'espace de travail, ce qui est extrêmement utile pour la documentation et le partage.

* ![](<images/user interface - new file.jpg>) Nouveau : permet de créer un fichier .dyn.
* ![](<images/user interface - open (1).jpg>) Ouvrir : permet d’ouvrir un fichier .dyn (espace de travail) ou .dyf (nœud personnalisé).
* ![](<images/user interface - save.jpg>) Enregistrer/Enregistrer sous : permet d’enregistrer le fichier .dyn ou .dyf actif.
* ![](<images/user interface - undo.jpg>) Annuler : permet d’annuler la dernière action.
* ![](<images/user interface - redo.jpg>) Rétablir : permet de rétablir l’action suivante.
* ![](<images/user interface - screenshot.jpg>) Exporter l’espace de travail en tant qu’image : permet d’exporter l’espace de travail visible en tant que fichier PNG.

### Bibliothèque

La bibliothèque Dynamo est un ensemble de bibliothèques fonctionnelles, chaque bibliothèque contenant des nœuds regroupés par catégorie. Elle se compose de bibliothèques de base qui sont ajoutées lors de l’installation par défaut de Dynamo. Au fur et à mesure de la présentation de son utilisation, vous allez découvrir comment étendre la fonctionnalité de base avec des nœuds personnalisés et d’autres packages. La section [2-library.md](2-library.md "mention") fournit des instructions plus détaillées sur son utilisation.

![](<images/user interface - library.jpg>)

### Espace de travail

L’espace de travail est l’endroit où vous composez vos programmes visuels, vous pouvez également modifier son paramètre d’aperçu pour afficher les géométries 3D à partir d’ici. Pour plus d’informations, reportez-vous à [1-workspace.md](1-workspace.md "mention").

![](<images/user interface - workspace.gif>)

### Barre d'exécution

Exécutez votre script Dynamo à partir d’ici. Cliquez sur l’icône déroulante du bouton Exécution pour passer d’un mode à l’autre.

![](<images/user interface - execution bar.gif>)

* Automatique : exécute votre script automatiquement. Les modifications sont mises à jour en temps réel.
* Manuel : le script s’exécute uniquement lorsque vous cliquez sur le bouton « Exécuter ». Utile pour modifier des scripts complexes.
* Périodique : cette option est grisée par défaut. Disponible uniquement lorsque le nœud DateTime.Now est utilisé. Vous pouvez définir l’exécution automatique du graphique à un intervalle spécifié.

![](<images/user interface - execution bar DateTime node.jpg>)
