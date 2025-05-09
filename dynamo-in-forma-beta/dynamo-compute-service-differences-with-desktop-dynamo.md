# Différences entre les services de calcul Dynamo et Dynamo Desktop

Cette page présente les différences à prendre en compte lors de l’écriture de programmes Dynamo à exécuter dans le contexte cloud du service de calcul Dynamo.

## Qu’est-ce que DaaS ?

DaaS, Dynamo as a Service, service de calcul Dynamo, etc. font tous référence à la même chose : le moteur d’exécution principal de Dynamo qui s’exécute dans un contexte cloud. Cela signifie que votre graphe ne s’exécute pas sur votre ordinateur. DaaS n’est actuellement accessible que via l’extension Dynamo Player pour Forma, où les utilisateurs peuvent charger et gérer des fichiers `.dyn` créés dans l’environnement de bureau, exécuter des fichiers `.dyn` partagés par leurs collègues via l’extension ou utiliser des routines `.dyn` préchargées fournies par Autodesk en tant qu’exemples.

Étant donné que vos graphes s’exécutent dans ce contexte cloud et non sur votre machine, DaaS ne peut actuellement pas utiliser directement les contextes hôtes traditionnels de Dynamo (Revit, Civil 3D, etc.). Si vous souhaitez utiliser les types de ces programmes dans votre graphe, vous devrez les sérialiser (les enregistrer) dans le graphe à l’aide du nœud `Data.Remember` ou d’autres techniques de sérialisation dans le graphe. Ces processus sont similaires aux workflows que vous devez utiliser lors de l’écriture de graphes pour la conception générative dans Revit.

## Quelle version de Dynamo exécute mon code ?

La version est basée sur la version 3.x et est mise à jour fréquemment sur la base de la branche principale open source de Dynamo.

## Quels packages/nœuds sont disponibles dans cette version de Dynamo ?

* La plupart des nœuds principaux, voir la section suivante pour connaître certaines limites spécifiques.
* Package `DynamoFormaBeta` pour interagir avec l’API Forma.
* `VASA` pour la voxélisation et une analyse efficace.
* `MeshToolKit` pour la manipulation du maillage. La boîte à outils de maillage est prête à l’emploi à partir de Dynamo 3.4.
* `RefineryToolkit` pour des algorithmes utiles pour les tests de conflit, les distances de vue, les chemins les plus courts, les isovists, etc.

## À quoi dois-je faire attention lorsque j’écris des graphes pour DaaS ?

* Les nœuds Python ne fonctionnent pas. _Actuellement_, ces nœuds ne s’exécuteront tout simplement pas.
* Les packages personnalisés ne peuvent pas être utilisés.
* La couche IU/vue des nœuds IU ne s’exécutera pas. Nous ne pensons pas qu’il s’agisse d’un problème pour les fonctionnalités de base, mais il est bon de le savoir si vous constatez un bogue lié à un nœud avec une interface utilisateur personnalisée.
* La fonctionnalité Windows uniquement ne fonctionne pas. Par exemple, si vous essayez d’utiliser le registre Windows ou WPF, ce dernier échouera.
* Les extensions de vue ne seront pas chargées.
* Les nœuds du système de fichiers ne seront pas très utiles. Les fichiers que vous référencez sur votre ordinateur local n’existeront pas lors de l’exécution dans DaaS.
* Les nœuds d’interopérabilité Excel/DSOffice ne fonctionnent pas. Les nœuds XML ouverts devraient fonctionner.
* Les requêtes réseau ne fonctionnent pas de manière générale, mais vous pouvez effectuer des appels à l’API Forma.

## Comment me souvenir de toutes ces indications ? Peuvent-elles changer ?

* À l’avenir, nous avons l’intention de fournir des outils dans Dynamo pour la version de bureau afin que votre graphe fonctionne de la même manière dans les deux contextes.

## Combien cela coûte-t-il ?

* Dans le cadre de cette version bêta, le temps de calcul n’est pas facturé.

## Comment démarrer ?

Pour commencer, consultez le [billet de blog](https://dynamobim.org/dynamo-as-a-service-powers-up-dynamo-player-in-forma/), la [série YouTube](https://www.youtube.com/playlist?list=PLY-ggSrSwbZqlbQG1i45bpT8clCJp08wD) ou les exemples de l’extension Forma. Ceux-ci vous aideront à :

* Accéder à Autodesk Forma.
* Installer DynamoFormaBeta pour la version de bureau de Dynamo et l’extension Dynamo dans Forma.
* Écrire votre premier graphe.

## Sécurité

* N’oubliez pas que vos graphes partagés sont stockés dans Forma.
* Le temps d’exécution maximal des graphes est actuellement inférieur à 30 minutes. Cette valeur est susceptible d’évoluer.
* Les demandes d’exécution sont limitées en termes de débit, de sorte que vous pouvez voir des erreurs si vous effectuez de nombreuses demandes de calcul en peu de temps.
