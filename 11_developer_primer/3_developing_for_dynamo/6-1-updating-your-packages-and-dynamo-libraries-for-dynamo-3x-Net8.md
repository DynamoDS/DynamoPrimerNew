# Mise à jour des packages et des bibliothèques Dynamo pour Dynamo 3.x et NET8

### Introduction <a href="#introduction" id="introduction"></a>

Cette section contient des informations sur les problèmes que vous pouvez rencontrer lors de la migration de vos graphiques, modules et bibliothèques vers Dynamo 3.x.

Dynamo 3.0 est une version majeure et certaines API ont été modifiées ou supprimées. Le changement le plus important qui est susceptible de vous affecter en tant que développeur ou utilisateur de Dynamo 3.x est le passage à .NET8.

Dotnet/.NET est le runtime qui alimente le langage C# dans lequel Dynamo est écrit. Nous avons mis à jour cette version de runtime vers une version plus récente, ainsi que le reste de l'écosystème Autodesk.

Pour en savoir plus, consultez [notre billet de blog](https://dynamobim.org/dynamo-on-net-8/).
***

### Compatibilité des modules <a href="#package-compatibility" id="package-compatibility"></a>

#### Utilisation des modules Dynamo 2.x dans Dynamo 3.x 
Comme Dynamo 3.x s'exécute désormais sur le runtime .NET8, les modules qui ont été créés pour Dynamo 2.x (*à l'aide de .NET48*) sont susceptibles de ne pas fonctionner dans Dynamo 3.x. Lorsque vous tentez de télécharger un module dans Dynamo 3.x qui a été publié à partir d'une version de Dynamo antérieure à la version 3.0, vous recevez un avertissement vous indiquant que le module provient d'une version antérieure de Dynamo. 

**Cela ne signifie pas que le module ne fonctionnera pas.** Il s'agit simplement d'un avertissement indiquant que des problèmes de compatibilité peuvent survenir. En général, il est conseillé de vérifier si une version plus récente a été spécialement conçue pour Dynamo 3.x.

Vous pouvez également remarquer ce type d'avertissement dans vos fichiers journaux Dynamo au moment du chargement du module. Si tout fonctionne correctement, vous pouvez l'ignorer.

#### Utilisation des modules Dynamo 3.x dans Dynamo 2.x 

Il est très peu probable qu'un module créé pour Dynamo 3.x (*à l'aide de .Net8*) fonctionne sur Dynamo 2.x. Un avertissement s'affiche également lors du téléchargement de modules créés pour des versions plus récentes de Dynamo lorsque vous utilisez une version antérieure.


