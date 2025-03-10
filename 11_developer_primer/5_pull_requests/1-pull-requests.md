# Demandes de tirage

Dynamo repose sur la créativité et l’engagement de sa communauté, et l’équipe Dynamo encourage les contributeurs à explorer les possibilités, à tester les idées et à demander l’avis de la communauté. Bien que l’innovation soit fortement encouragée, les modifications ne seront fusionnées que si elles facilitent l’utilisation de Dynamo et répondent aux instructions définies dans ce document. Les modifications ayant des avantages limités ne seront pas fusionnées.

#### Attentes relatives aux demandes de tirage <a href="#pull-request-expectations" id="pull-request-expectations"></a>

L’équipe Dynamo s’attend à ce que les demandes de tirage suivent ces quelques règles :

* respecter les [normes de codage](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) et les [normes d’attribution de noms de nœud ;](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards)
* intégrer les tests unitaires lors de l’ajout de nouvelles fonctionnalités ;
* lors de la correction d’un bogue, ajouter un test unitaire qui illustre le dysfonctionnement actuel ;
* garder la discussion centrée sur une seule demande. Créez une nouvelle question si un nouveau sujet ou un sujet connexe est abordé.

Quelques indications sur ce qu’il ne faut pas faire :

* nous surprendre avec des demandes de tirage importantes. il est préférable de soumettre une demande et d’entamer une discussion afin que nous puissions nous mettre d’accord sur une direction à prendre avant d’investir beaucoup de temps ;
* faire un commit de code que vous n’avez pas écrit. Si vous trouvez du code que vous pensez pouvoir ajouter à Dynamo, soumettez une demande et lancez une discussion avant de continuer ;
* soumettre des demandes de tirage qui modifient les fichiers ou en-têtes associés aux licences. Si vous pensez qu’il y a un problème avec ces derniers, soumettez une demande et nous nous ferons un plaisir d’en discuter ;
* apporter des ajouts à l’API sans soumettre une demande et sans en discuter d’abord avec nous.

#### Remplissage du modèle de demande de tirage <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

Lors de la soumission d’une demande de tirage, utilisez le [modèle de demande de tirage par défaut](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Avant de soumettre votre demande de tirage, assurez-vous que l’objectif est clairement décrit et que toutes les affirmations suivantes sont exactes :

* le code base est de meilleur qualité après cette demande de tirage ;
* est documenté selon les [normes](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) ;
* le niveau de test inclus dans cette demande de tirage est approprié ;
* les éventuelles chaînes de caractères destinées à l’utilisateur sont extraites dans des fichiers `*.resx` ;
* tous les tests sont réussis grâce à l’intégration continue en libre-service ;
* capture d’écran des modifications apportées à l’interface utilisateur, le cas échéant ;
* les modifications apportées à l’API sont conformes à la procédure de [gestion des versions sémantique](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) et sont décrites dans le document [Modifications apportées à l’API](https://github.com/DynamoDS/Dynamo/wiki/API-Changes).

Un réviseur approprié sera affecté à votre demande de tirage par l’équipe Dynamo.

#### Processus d’examen des demandes de tirage <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Une fois la demande soumise, vous pouvez être amené à participer au processus d’examen. Veuillez tenir compte des critères d’examen suivants :

* l’équipe Dynamo se réunit une fois par mois pour examiner les demandes de tirage, de la plus ancienne à la plus récente ;
* si une demande de tirage examinée nécessite des modifications de la part du propriétaire, ce dernier dispose de 30 jours pour y répondre. Si la demande de tirage n’a connu aucune activité lors de la session suivante, elle sera soit fermée par l’équipe, soit, selon son utilité, prise en charge par quelqu’un de l’équipe ;
* les demandes de tirage doivent utiliser le modèle de demande de tirage par défaut de Dynamo ;
* les demandes de tirage pour lesquelles les modèles de demande de tirage de Dynamo ne sont pas entièrement remplis avec toutes les déclarations satisfaites ne seront pas examinées.

#### Sélection des commits de Dynamo Revit <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Étant donné qu’il existe plusieurs versions de Revit sur le marché, il se peut que vous soyez obligé de sélectionner vos modifications dans des branches release spécifiques de DynamoRevit afin que les différentes versions de Revit puissent bénéficier des nouvelles fonctionnalités. Pendant le processus d’examen, les contributeurs seront responsables de la sélection de leurs commits examinés dans les autres branches de DynamoRevit comme spécifié par l’équipe Dynamo.
