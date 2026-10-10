# Attentes concernant les tests

Cette page décrit ce qui est attendu lors des tests du nouveau code ajouté à Dynamo.

Vous avez donc un nouveau nœud à ajouter. Très bien. Il est temps d’ajouter quelques tests. Cette étape est recommandée pour deux raisons principales.

1. Cela aide à découvrir les cas où il ne fonctionne pas.
2. Lorsque quelqu’un d’autre modifie quelque chose qui brise votre nœud, les tests devraient échouer. De cette façon, la personne qui a fait échouer les tests peut le réparer. Si les tests n’échouent pas, alors c’est en grande partie votre problème, et vous devez gérer les utilisateurs dont les modèles ne fonctionnent plus.

Il existe deux grands types de tests dans Dynamo : les tests unitaires et les tests système.

## Tests unitaires

Les tests unitaires doivent tester le moins possible. Si vous avez créé un nœud qui calcule les racines carrées via un nœud C# Zero-Touch, le test doit tester uniquement cette DLL et effectuer des appels C# directement vers ce code. Les tests unitaires ne doivent même pas inclure Dynamo.

Ils doivent comprendre :

* Tests positifs (le code fait ce qu’il doit faire).
* Tests négatifs (le code ne casse pas lorsqu’on lui fournit des données aberrantes).
* Tests de régression (lorsqu’un bogue est détecté dans votre code, écrivez un test pour vous assurer qu’il ne se reproduit pas).

Ils doivent être courts, rapides et fiables. La majorité des tests devraient être des tests unitaires.

## Tests système

Les tests système fonctionnent sur plusieurs composants et testent la façon dont ils s’accordent. Ils doivent être utilisés et élaborés avec soin. 

Pourquoi ? Leur exécution est coûteuse. Il faut faire en sorte que la suite de tests s’exécute avec le moins de latence possible.

Lors de l’écriture d’un test système, la première chose à faire est de regarder [ce diagramme](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) et de déterminer les sous-systèmes que vous couvrez.

Idéalement, il doit y avoir une série progressive de tests couvrant des ensembles croissants de blocs en interaction, de sorte que lorsque les tests commencent à échouer, vous puissiez découvrir très rapidement où se trouve le problème.

Exemples d’éléments qui nécessitent des tests système :

* Un nouveau type de nœud Revit qui stocke plusieurs éléments dans Trace au lieu d’un seul.
* Un nouveau nœud Watch qui affiche les données différemment.

Exemple d’éléments qui ne nécessitent pas de tests système :

* Un nouveau nœud mathématique.
* Une bibliothèque de traitement de chaînes.

Les tests système doivent :

* Vérifier que le comportement est correct.
* Vérifier l’absence de comportements pathologiques, p. ex. l’absence d’exceptions.