# Cycle de vie et cadence de mise à jour du service Dynamo Compute



Ce document décrit la cadence de mise à jour et la politique d’assistance de Dynamo Cloud Compute. La solution peut également être désignée dans le présent document de manière interchangeable comme « le service ».

Le document décrit comment les versions de moteur sont gérées, quand les mises à jour ont lieu et ce à quoi les utilisateurs peuvent s’attendre lors de l’exécution de graphiques Dynamo dans le cloud.

---

## Cadence de mise à jour

Pour répondre aux différents besoins des utilisateurs, Dynamo Cloud Compute gère **deux pistes de moteur distinctes**. Chaque piste a un objectif spécifique et suit son propre calendrier de mise à jour :

### Moteur stable (production)

Le moteur stable est conçu pour la fiabilité et la cohérence dans les environnements de production. Il est basé sur la dernière version stable de DynamoCore Runtime et mis à jour lorsque les versions officielles de Dynamo sont accessibles aux utilisateurs de Dynamo Desktop. Dans un premier temps, nous suivrons la cadence de mise à jour de DynamoRevit.

Cette piste est destinée aux charges de travail de production où la fiabilité et la prévisibilité sont essentielles. Lorsque vous utilisez le moteur stable, vous pouvez vous attendre à ce que les mises à jour s’alignent sur le calendrier des versions publiques de Dynamo, ce qui vous donne le temps de vous préparer aux modifications et de tester vos graphiques avant que celles-ci n’aient une incidence sur vos flux de travail.

### Moteur de prévisualisation (prévisualisation/bac à sable quotidien)

Le moteur de prévisualisation permet d’accéder en avant-première aux derniers développements de Dynamo. Il est basé sur la dernière version de développement de DynamoCore Runtime et mis à jour fréquemment au fur et à mesure que de nouvelles fonctionnalités et corrections de bugs sont fusionnées.

Ce canal est idéal pour les utilisateurs qui souhaitent tester les modifications à venir, tester de nouvelles fonctionnalités avant leur sortie officielle ou vérifier que leurs graphiques continueront à fonctionner avec les futures versions de Dynamo. Le moteur de prévisualisation vous permet de garder une longueur d’avance sur les modifications et de fournir des commentaires à l’équipe Dynamo.

---


## Calendrier de prise en charge

Comprendre combien de temps chaque version de moteur reste prise en charge vous aide à planifier les fenêtres de maintenance et les mises à jour des graphiques.

### Moteur stable

Le moteur stable est mis à jour lorsque Dynamo publie une nouvelle version stable de Dynamo Core dans Revit. Chaque version stable reste disponible et prise en charge jusqu’à ce que la prochaine version stable soit déployée sur le service.

Par exemple, si le service exécute actuellement Dynamo 3.6 (stable), il continuera d’exécuter cette version jusqu’à ce que Dynamo 4.0 soit généralement disponible pour les utilisateurs (généralement lorsqu’il sera livré dans Revit). À ce moment-là, le service sera mis à jour vers Dynamo 4.0 (stable).

Cette approche garantit que le service cloud reste synchronisé avec ce que la majorité des utilisateurs expérimentent dans les environnements de bureau.

### Moteur de prévisualisation

Le moteur de prévisualisation est mis à jour en permanence à partir de la dernière branche de développement de Dynamo. Au fur et à mesure que le développement de chaque version progresse, le moteur de prévisualisation effectue le suivi de ces modifications.

Par exemple, alors que Dynamo 4.1 est en cours de développement, le moteur d’aperçu peut être étiqueté « Dynamo Cloud Compute Service 4.1 ». Une fois le développement passé à la version 4.2, le moteur de prévisualisation commencera à suivre ces modifications et pourra être renommé « Dynamo Cloud Compute Service 4.2 ».

Étant donné que le moteur de prévisualisation est fréquemment mis à jour, attendez-vous à des modifications avec rupture de compatibilité occasionnelles ou à des fonctionnalités expérimentales. Il est préférable de l’utiliser pour les tests et la validation plutôt que pour les flux de production.

---

## Choisir le bon moteur

Lors du choix du moteur à utiliser :

- **Choisissez le moteur stable** si vous avez besoin d’un comportement prévisible et testé pour les flux de production ou si vous déployez des graphiques auprès d’utilisateurs finaux qui attendent des résultats cohérents.

- **Choisissez le moteur de prévisualisation** si vous souhaitez tester de nouvelles fonctionnalités en avance, valider que vos graphiques fonctionneront avec les versions à venir ou nous faire part de vos commentaires sur le développement de Dynamo.

Les deux moteurs exécutent le même moteur d’exécution Dynamo de base. La différence réside dans le moment et la fréquence de réception des mises à jour. 

---

> **Remarque : service en version bêta**  
 Dynamo Cloud Compute est actuellement en version bêta. Les délais de prise en charge et les stratégies de mise à jour décrits dans ce document représentent nos intentions actuelles tandis que nous expérimentons et affinons le service. Il ne s’agit pas de garanties et ces éléments peuvent être modifiés en fonction des commentaires des utilisateurs et de l’expérience opérationnelle.



