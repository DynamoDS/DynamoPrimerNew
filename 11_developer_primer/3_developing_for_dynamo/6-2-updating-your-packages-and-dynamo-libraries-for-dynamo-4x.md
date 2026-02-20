# Mise à jour des packages et des bibliothèques Dynamo pour Dynamo 4.x

### Introduction <a href="#introduction" id="introduction"></a>

Cette section contient des informations sur les problèmes que vous pouvez rencontrer lors de la migration de vos graphiques, modules et bibliothèques vers Dynamo 4.x. Contenu de Dynamo 4.0 :
* Améliorations importantes des performances
* Mises à jour de stabilité et de correction de bogues
* Modernisation du code base
* Suppression des API marquées comme obsolètes dans les versions 1.x
* Mise à jour majeure de l’environnement d’exécution de .NET 8 vers .NET 10
* PythonNet3 est désormais le moteur Python par défaut pour tous les nouveaux nœuds Python

L’activité de migration vers .NET 10 garantit que Dynamo reste aligné sur la feuille de route technologique de Microsoft, bien avant la fin de la prise en charge de .NET 8 en novembre 2026.

Au lancement de Dynamo 4.0, il vous sera demandé d’effectuer la mise à jour vers .NET 10 si ce n’est pas déjà fait. Les auteurs de packages doivent mettre à jour leurs projets vers la cible .NET 10 pour garantir une compatibilité totale.

Tous les nouveaux nœuds Python créés dans Dynamo 4.0 et versions ultérieures commencent par PythonNet3. Ne vous inquiétez pas de la rétrocompatibilité : pour ceux qui utilisent plusieurs versions de logiciels (p. ex. Revit ou Civil 3D 2025/2026), installez le package PythonNet3 Engine dans Dynamo 3.3–3.6 pour maintenir la compatibilité. Vous trouverez des informations complémentaires [ici](https://dynamobim.org/dynamo-core-4-0-release/).

L’API et les nœuds marqués comme obsolètes dans les versions 1.x ont été supprimés dans Dynamo 4.0. Vous pouvez consulter la [liste complète des modifications ici](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0).

### Compatibilité des modules <a href="#package-compatibility" id="package-compatibility"></a>

#### Utilisation des packages Dynamo 2.x et 3.x dans Dynamo 4.x 
Comme Dynamo 4.x s'exécute désormais dans l’environnement .NET 10, les packages qui ont été créés pour Dynamo 2.x (*avec .NET48*) et Dynamo 3.x (*avec .NET 8*) sont susceptibles de ne pas fonctionner dans Dynamo 4.x. Lorsque vous tentez de télécharger un package dans Dynamo 4.x qui a été publié à partir d’une version de Dynamo antérieure à la version 4.0, vous recevez un avertissement vous indiquant que le package provient d’une version antérieure de Dynamo.

**Cela ne signifie pas que le package ne fonctionnera pas.** Il s’agit simplement d’un avertissement indiquant que des problèmes de compatibilité peuvent survenir. En général, il est conseillé de vérifier si une version plus récente a été spécialement conçue pour Dynamo 4.x.

Vous pouvez également remarquer ce type d'avertissement dans vos fichiers journaux Dynamo au moment du chargement du module. Si tout fonctionne correctement, vous pouvez l'ignorer.

#### Utilisation des packages Dynamo 4.x dans Dynamo 2.x 

Il est très peu probable qu’un package créé pour Dynamo 4.x (*avec .Net 10*) fonctionne dans Dynamo 2.x. L’avertissement ci-dessous s’affiche également lorsque vous tentez d’installer des packages conçus pour Dynamo 4.x dans Dynamo 2.x.

![Avertissement de compatibilité des packages](images/6-2-packages-new-version-compatibility-warning.png)


#### Utilisation des packages Dynamo 4.x dans Dynamo 3.x 

Un package créé pour Dynamo 4.x (*avec .Net 10*) peut fonctionner dans Dynamo 3.x tant que toutes les API utilisées dans le package existent sous .NET 8. Mais le fonctionnement n’est en aucun cas garanti. L’avertissement ci-dessous s’affiche également lorsque vous tentez d’installer des packages conçus pour Dynamo 4.x dans Dynamo 3.x.

![Avertissement de compatibilité des packages](images/6-2-packages-compatibility-warning.png)

#### Meilleures pratiques pour les auteurs de packages 
Les meilleures pratiques consistent à établir plusieurs cibles pour votre projet, vers .NET 8 et .NET 10, en modifiant le fichier .csproj.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
Vous garantissez ainsi :
* la prise en charge des versions de Dynamo hébergées par Revit toujours sous .NET 8 ;
* la compatibilité avec la version autonome Dynamo 4.x sous .NET 10.