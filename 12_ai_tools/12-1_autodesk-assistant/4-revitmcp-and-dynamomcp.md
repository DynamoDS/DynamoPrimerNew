# RevitMCP et DynamoMCP

Autodesk Assistant tire ses capacités des serveurs MCP. Chaque serveur fournit à Assistant un ensemble d’outils, et chaque outil effectue une tâche dans le produit auquel il appartient.

Lorsque vous travaillez dans Revit, deux de ces serveurs sont importants : **RevitMCP** et **DynamoMCP**. Ces serveurs ne sont pas des options concurrentes, et vous n’avez pas à choisir entre eux. Ils interviennent à des niveaux différents, et Assistant utilise celui qui correspond à votre demande.

### RevitMCP : opérations directes sur le modèle

RevitMCP, fourni sous la forme du [serveur MCP public Revit](https://help.autodesk.com/cloudhelp/2027/FRA/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm), encapsule une sélection d’opérations de l’API Revit en tant qu’outils MCP. Chaque outil correspond à une action définie dans le modèle : recherche d’éléments, vérification des paramètres et des décomptes, modification de paramètres par lots et capture d’instantanés de vues.

C’est la solution la plus directe pour une tâche que le jeu d’outils couvre déjà. Une demande donne lieu à un seul appel, et le modèle est modifié immédiatement. Il n’y a ni graphique à créer ni opération à exécuter ensuite.

### DynamoMCP : outils qui construisent l’opération

DynamoMCP intervient à un niveau supérieur. Ses outils n’effectuent pas directement d’actions de modélisation. Ils créent plutôt un graphique Dynamo : ils placent des nœuds, les relient, définissent des valeurs et exécutent le résultat. Reportez-vous à la rubrique [Qu’est-ce que le serveur MCP Dynamo ?](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md) pour obtenir la liste complète des outils.

Ce point est important en raison de la nature même d’un nœud. Chaque nœud de la bibliothèque Dynamo correspond lui-même à un ou plusieurs appels d’API déjà écrits, testés et documentés. Les nœuds de package et les nœuds Python sont disponibles de la même manière. Ainsi, un petit jeu d’outils de création de graphiques donne accès à l’intégralité de la bibliothèque Dynamo.

Résultat : les outils de DynamoMCP donnent accès à de nouvelles capacités plutôt que de se limiter aux capacités existantes. Au lieu d’une liste d’actions figée, Assistant assemble l’action demandée à partir de nœuds. En pratique, les outils MCP qui génèrent des graphiques se comportent comme des outils qui en créent d’autres.

### La différence en pratique

Imaginons que vous demandiez des étiquettes de porte sur un niveau.

- Avec RevitMCP, le travail se fait par un appel direct, et les étiquettes apparaissent.
- Avec DynamoMCP, Assistant place les nœuds qui collectent les portes, les filtrent selon le niveau indiqué et créent les étiquettes, puis il exécute le graphique.

Dans les deux cas, les portes sont étiquetées. La seconde méthode vous laisse en plus quelque chose dans la zone de dessin.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| Ce que fait un outil | Effectue une opération Revit définie | Crée et exécute un graphique Dynamo |
| Portée | Les opérations incluses dans le jeu d’outils | Tout nœud de votre bibliothèque, y compris les nœuds de package et les nœuds Python |
| Ce que vous obtenez | La modification du modèle | La modification du modèle, ainsi que le graphique qui l’a produite |
| Idéal pour | Une tâche unique et bien définie | Une logique que vous souhaitez examiner, ajuster, réutiliser ou partager |

### L’intérêt d’un graphique

Un graphique conserve la trace du travail, et pas seulement son résultat. Une fois qu’il est dans la zone de dessin, vous pouvez :

- le lire et voir exactement ce qui a été fait ;
- modifier une valeur et l’exécuter de nouveau, sans redemander l’intégralité de la tâche ;
- l’enregistrer et l’exécuter dans le projet suivant, ou le transmettre à un collègue ;
- le publier dans le Lecteur Dynamo pour que les personnes qui n’utilisent pas Dynamo puissent l’exécuter.

C’est la première demande qui prend du temps. Chaque exécution ultérieure ne coûte presque rien.

{% hint style="info" %}
Dans Dynamo Sandbox, Assistant n’a accès qu’à DynamoMCP, car il n’y a pas de modèle Revit sur lequel agir. Dans Revit, les deux sont disponibles.
{% endhint %}

### Formuler votre demande

Il n’est pas nécessaire de nommer un serveur dans votre invite. Il suffit de décrire le résultat attendu : Assistant sélectionne les outils.

Si vous voulez spécifiquement un graphique, dites-le. Des invites telles que *« Créer un graphique qui étiquette les portes du niveau 2 »* indiquent à Assistant que vous attendez la version réutilisable, et pas seulement les portes étiquetées.

### Pour en savoir plus

- [Autodesk Assistant (aperçu technique)](https://help.autodesk.com/cloudhelp/2027/FRA/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) : ce que fait Assistant dans Revit, dans l’aide de Revit 2027 ;
- [Serveur MCP public Revit (aperçu technique)](https://help.autodesk.com/cloudhelp/2027/FRA/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) : le volet Revit de MCP, dans l’aide de Revit 2027 ;
- [Autodesk Assistant : présentation et questions fréquentes](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) : les produits qui incluent Assistant et son utilisation.
