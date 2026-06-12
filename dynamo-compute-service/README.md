# Dynamo Cloud Compute



Dynamo Cloud Compute apporte la puissance de l’environnement d’exécution de programmation visuelle de Dynamo dans le cloud. Au lieu d’exécuter vos graphiques sur votre ordinateur local, le service de calcul les exécute dans un environnement cloud sécurisé et renvoie les résultats.

## Définition de Dynamo

Dynamo est un langage de programmation visuel et un environnement de création qui vous permet de créer des programmes en connectant des nœuds entre eux dans un graphique. Le moteur d’exécution Dynamo exécute ces graphiques, ce qui vous permet d’automatiser des tâches complexes, de générer de la géométrie et d’assurer l’intégration avec d’autres logiciels.

## Fonctionnement du service de calcul

Lorsque vous utilisez Dynamo par le biais d’un client basé sur le cloud (tel que Dynamo Player dans Forma), vos fichiers de graphiques `.dyn` sont envoyés au service de calcul pour exécution. Le service :

1. Reçoit votre graphique et tous les paramètres d’entrée
2. Exécute votre graphique dans un environnement cloud isolé
3. Renvoie les résultats à l’application cliente

Cette approche basée sur le cloud signifie que vous pouvez exécuter des graphiques Dynamo sans installer Dynamo localement, et vous pouvez tirer parti de la puissance du cloud computing pour des opérations complexes.

## Pourquoi utiliser Dynamo Cloud Compute ?

Dynamo Cloud Compute vous facilite la tâche lorsque vous souhaitez :

**Exécuter des graphiques sans installation sur l’ordinateur** : exécutez des graphiques Dynamo directement à partir d’applications Web sans demander aux utilisateurs d’installer Dynamo Desktop sur leur ordinateur.

**Collaborer et partager** : partagez des graphiques avec les membres de l’équipe qui peuvent les exécuter par le biais d’interfaces Web telles que Forma, ce qui facilite la distribution des flux de travail automatisés dans votre entreprise.

**Tirer parti du cloud computing** : tirez parti de l’infrastructure cloud pour les opérations de calcul intensif qui peuvent prendre plus de temps sur les machines locales.

**Standardiser l’environnement d’exécution** : assurez un comportement cohérent entre les différents utilisateurs et machines en exécutant des graphiques dans un environnement cloud contrôlé.

**Se connecter à Forma** : interagissez avec l’API Forma à l’aide de Dynamo. [Voir cet article de blog pour en savoir plus.](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/)

## Caractéristiques principales

**Exécution dans le cloud** : vos graphiques sont exécutés dans le cloud, et non sur votre machine locale. Cela signifie que :
- Vous n’avez pas besoin d’installer Dynamo Desktop pour exécuter des graphiques
- Vous avez accès aux ressources de cloud computing
- Vous bénéficiez d’un environnement d’exécution cohérent entre les différents utilisateurs

**Sécurité** : le service exécute les graphiques de chaque utilisateur dans des environnements isolés afin de garantir la sécurité et la confidentialité des données. Vos graphiques et vos données sont séparés des autres utilisateurs.

**Traitement asynchrone** : l’exécution du graphique se fait de manière asynchrone. Les clients soumettent une tâche et peuvent vérifier son état jusqu’à ce qu’elle soit terminée. Cela permet d’effectuer des calculs de longue durée sans bloquer votre flux de travail.

## Disponibilité actuelle

Dynamo Cloud Compute est actuellement disponible via :
- **Dynamo Player dans la bêta ouverte de Forma** : chargez, partagez et exécutez des graphiques Dynamo directement dans l’interface Web d’Autodesk Forma.

## En savoir plus

- [Différences entre Dynamo Cloud Compute et Dynamo Desktop](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md) : différences importantes à prendre en compte lors de l’écriture de graphiques pour l’exécution dans le cloud
- [Cycle de vie du moteur](engine-lifecycle.md) : informations sur les versions de moteur prises en charge et leur cycle de vie

-----


> **Remarque : service en version bêta**  
 Dynamo Cloud Compute est actuellement en version bêta. Les délais de prise en charge et les stratégies de mise à jour décrits dans ce document représentent nos intentions actuelles tandis que nous expérimentons et affinons le service. Il ne s’agit pas de garanties et ces éléments peuvent être modifiés en fonction des commentaires des utilisateurs et de l’expérience opérationnelle.