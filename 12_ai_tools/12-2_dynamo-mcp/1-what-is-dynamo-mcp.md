# Qu’est-ce que le serveur MCP Dynamo ?

**MCP** est l’abréviation de Model Context Protocol. Il s’agit d’une norme ouverte qui permet aux modèles d’IA de se connecter à des logiciels au moyen d’un protocole commun. Un serveur MCP expose les outils et les données d’un programme afin qu’un client IA puisse lire le contexte en temps réel, effectuer des actions et renvoyer des résultats structurés.

**DynamoMCP** est le serveur MCP intégré de Dynamo. Il connecte Autodesk Assistant à votre session Dynamo active (graphique, nœuds, fils et état de l’espace de travail), afin que l’aide de l’IA repose sur ce que vous êtes réellement en train de faire.

Pour les utilisateurs de Dynamo, cela signifie qu’Assistant peut faire plus que répondre à des questions générales. Il peut examiner le graphique ouvert, aider à résoudre les problèmes de nœuds et mettre à jour la zone de dessin en s’appuyant sur le contexte réel de la session, sans copier-coller manuel.

DynamoMCP est disponible dans Dynamo 4.2.0 et versions ultérieures.

### Comment utiliser DynamoMCP

Vous utilisez DynamoMCP par l’intermédiaire d’Autodesk Assistant : il n’y a ni configuration distincte ni interface autonome. Assistant est le point d’entrée pour poser des questions, obtenir de l’aide sur votre graphique et permettre à l’IA de travailler avec le contexte Dynamo en temps réel.

L’endroit où vous ouvrez Assistant dépend de l’environnement dans lequel vous travaillez :

- **Dynamo Sandbox** : les outils MCP de Dynamo sont disponibles dans Autodesk Assistant, au sein de Dynamo ;
- **Revit** : les outils MCP de Dynamo sont disponibles dans Autodesk Assistant, au sein de Revit.

Dans les deux cas, vous restez dans votre session en cours et passez par le même Assistant pour accéder à DynamoMCP.
### Ce que DynamoMCP peut faire

DynamoMCP fournit à Assistant un ensemble d’**outils**. Chaque outil effectue une tâche dans Dynamo, comme lire la zone de dessin, placer un nœud ou exécuter le graphique. Vous n’appelez jamais ces outils vous-même. Vous décrivez ce que vous voulez en langage courant, et Assistant choisit les outils dont il a besoin.

Il reste toutefois utile de connaître ces outils. Vous savez ainsi ce dont Assistant est capable, et vous formulez plus facilement des demandes qu’il peut traiter.

{% hint style="info" %}
Les outils dont le nom commence par `get_` se contentent de lire les informations de votre session. Tous les autres outils peuvent modifier votre graphique.
{% endhint %}

#### Lire le graphique actuel

Ces outils permettent à Assistant de voir sur quoi vous travaillez avant de répondre ou d’apporter une modification.

| Outil | Rôle |
| --- | --- |
| `get_workspace_info` | Répertorie le contenu de la zone de dessin : nœuds, fils, groupes, notes et détails de l’espace de travail. |
| `get_selection` | Indique les nœuds, les fils et les groupes sélectionnés. |
| `get_node_connections` | Indique à quoi un nœud est relié, port par port. |
| `get_node_property` | Indique les paramètres des nœuds, comme l’aperçu, le gel, l’aperçu épinglé et les étiquettes. |
| `get_node_lacing` | Indique le paramètre de combinaison d’un nœud. |
| `get_installation_info` | Indique votre version de Dynamo, l’hôte dans lequel vous travaillez et un lien vers les informations sur la nouvelle version. |

*Exemple d’invite : « Qu’y a-t-il dans ma zone de dessin en ce moment ? »*

#### Trouver le bon nœud

Dynamo comporte des milliers de nœuds, y compris ceux des packages installés. Ces outils aident Assistant à choisir le bon nœud au lieu de le deviner.

| Outil | Rôle |
| --- | --- |
| `get_nodes_info` | Effectue une recherche dans la bibliothèque de nœuds, y compris les nœuds de package, et renvoie le nœud exact à placer. |
| `get_node_help` | Renvoie l’aide d’un nœud et le chemin de son graphique d’exemple, s’il en existe un. |
| `get_type_schema` | Renvoie le format de données attendu pour un type Dynamo, tel que Point, Number ou Color. |

*Exemple d’invite : « Quel nœud dois-je utiliser pour obtenir l’aire d’une surface ? »*

#### Créer et modifier le graphique

Ces outils effectuent le travail dans la zone de dessin.

| Outil | Rôle |
| --- | --- |
| `create_nodes` | Place un ou plusieurs nœuds dans la zone de dessin, avec des valeurs de départ facultatives. |
| `set_node_value` | Définit la valeur des nœuds d’entrée tels que Number, String, Code Block et les listes déroulantes. |
| `connect_nodes` | Relie un port de sortie à un port d’entrée. |
| `disconnect_nodes` | Supprime les fils entre deux nœuds. |
| `delete_node` | Supprime un nœud de la zone de dessin. |
| `rename_node` | Renomme les nœuds d’entrée et les nœuds Watch pour que leur rôle soit clair. Les nœuds de fonction conservent leur nom d’origine. |
| `select_nodes` | Sélectionne des nœuds dans la zone de dessin pour vous montrer ceux dont parle Assistant. |
| `set_node_property` | Active ou désactive des paramètres de nœud : aperçu, gel, aperçu épinglé, étiquettes et entrée ou sortie. |
| `set_node_lacing` | Définit la combinaison sur Auto, Le plus court, Le plus long ou Produit vectoriel. |
| `set_node_port_list_at_level` | Définit le niveau de liste sur un port d’entrée. |

*Exemple d’invite : « Ajouter un curseur de nombre et le relier à l’entrée radius de mon cercle. »*

#### Organiser et documenter le graphique

Un graphique fonctionnel n’est pas forcément lisible. Ces outils gèrent la disposition, le regroupement, les couleurs et les notes.

| Outil | Rôle |
| --- | --- |
| `create_group` | Place un groupe autour d’un ensemble de nœuds. |
| `create_groups` | Crée plusieurs groupes en une seule étape. |
| `ungroup_by_guid` | Supprime un groupe. Les nœuds qu’il contient restent dans la zone de dessin. |
| `ungroup_by_title` | Supprime des groupes d’après leur titre. Les nœuds qu’ils contiennent restent dans la zone de dessin. |
| `get_available_group_styles` | Répertorie les styles de groupe disponibles, y compris les styles personnalisés définis dans vos préférences. |
| `assign_group_styles` | Applique des styles de groupe, comme Entrées, Actions, Sorties et Révision. |
| `get_notes` | Répertorie les notes du graphique. |
| `create_notes` | Ajoute des notes expliquant certaines parties du graphique. |
| `update_notes` | Modifie le texte d’une note ou la déplace. |
| `delete_notes` | Supprime des notes. |
| `set_note_state` | Épingle une note à un nœud pour que les deux se déplacent ensemble, ou la détache. |
| `auto_layout_workspace` | Réorganise les nœuds à l’aide de la disposition automatique intégrée de Dynamo. |
| `set_workspace_description` | Écrit une description du graphique dans les propriétés du fichier. |

*Exemple d’invite : « Regrouper ce graphique par entrées, actions et sorties avec un code couleur, puis ajouter de courtes notes. »*

#### Exécuter le graphique et vérifier les résultats

| Outil | Rôle |
| --- | --- |
| `run_workspace` | Exécute le graphique actuel. En mode automatique, le graphique est déjà réexécuté à chaque modification : cette étape est donc ignorée. |
| `get_workspace_warnings` | Répertorie les nœuds qui présentent des avertissements ou des erreurs, le texte du message et les prochaines étapes suggérées. Le graphique doit d’abord être exécuté. |
| `get_node_output_values_shapes` | Résume les valeurs de sortie d’un nœud ou de chaque nœud du graphique. |
| `get_node_output_value` | Lit la sortie complète d’un seul nœud, page par page, lorsque le résultat est trop volumineux pour être résumé. |

*Exemple d’invite : « Exécuter le graphique et m’expliquer pourquoi le nœud List.GetItemAtIndex affiche un avertissement. »*

#### Utiliser des fichiers de graphique

Assistant peut aussi travailler avec des graphiques enregistrés sur le disque, et pas seulement avec celui qui est ouvert.

| Outil | Rôle |
| --- | --- |
| `new_workspace` | Ouvre un nouveau graphique vide. Si le graphique actuel comporte des modifications non enregistrées, vous êtes d’abord invité à les enregistrer ou à les abandonner. |
| `save_workspace` | Enregistre le graphique actuel. Si aucun emplacement n’est indiqué, il est enregistré dans votre dossier Dynamo, sous Documents. |
| `edit_graph_from_path` | Ouvre un graphique enregistré pour le modifier, sans l’exécuter. |
| `insert_graph_from_path` | Fusionne un graphique enregistré dans votre zone de dessin actuelle. C’est ainsi que les graphiques d’exemple des nœuds sont importés. |
| `run_graph_from_path` | Exécute un graphique enregistré, avec les valeurs d’entrée facultatives que vous fournissez. |
| `run_graph_from_json` | Exécute un graphique fourni au format JSON Dynamo, sans fichier sur le disque. |
| `get_graph_info_from_path` | Lit les entrées, les sorties et les métadonnées d’un graphique enregistré sans l’ouvrir. |
| `get_graph_info_from_json` | Lit les mêmes informations à partir d’un graphique fourni au format JSON Dynamo. |
| `get_graphs_info_from_path` | Analyse un dossier de graphiques et indique le contenu de chacun d’eux. |
| `get_available_graphs_folders` | Répertorie les dossiers dont Dynamo a connaissance et qui peuvent contenir des graphiques. |

*Exemple d’invite : « Parcourir mon dossier de graphiques Dynamo et m’indiquer quels graphiques placent des murs. »*

{% hint style="warning" %}
Assistant peut modifier votre graphique. Enregistrez votre travail avant de demander une modification importante, et examinez le résultat avant d’écraser un fichier existant.
{% endhint %}
