# La connexion de Civil 3D

<figure><img src="../.gitbook/assets/DynamoSwissKnife-WhiteBackground_edit (2).jpg" alt="" width="563"><figcaption></figcaption></figure>

Dynamo pour Civil 3D offre le paradigme de la _programmation visuelle_ aux ingénieurs et aux concepteurs travaillant sur des projets d’infrastructures civiles. Vous pouvez considérer Dynamo comme une sorte d’outil multiple numérique pour les utilisateurs de Civil 3D. Peu importe la tâche à accomplir, il dispose de l’outil adéquat pour la réaliser. Son interface intuitive vous permet de créer des routines puissantes et personnalisables sans écrire une seule ligne de code. Vous n’avez pas besoin d’_être_ programmeur pour utiliser Dynamo, mais vous devez être capable de _penser_ avec la logique d’un programmeur. Associé aux autres chapitres du guide, ce chapitre vous aidera à développer vos compétences en logique afin que vous puissiez réaliser n’importe quelle tâche avec une pensée computationnelle.

## Historique

Dynamo a été introduit pour la première fois dans Civil 3D 2020 et a continué à évoluer depuis. Initialement installé séparément via une mise à jour logicielle, il est désormais fourni avec toutes les versions de Civil 3D. Selon la version de Civil 3D que vous utilisez, vous pouvez remarquer que l’interface de Dynamo est légèrement différente des exemples présentés dans ce chapitre. En effet, celle-ci a fait l’objet d’une refonte importante dans Civil 3D 2023.

<figure><img src="../.gitbook/assets/c3d-ui-old.png" alt=""><figcaption><p>Interface de Dynamo, Civil 3D (2020 – 2022)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/c3d-ui-new.png" alt=""><figcaption><p>Interface de Dynamo, Civil 3D (2023 – aujourd’hui)</p></figcaption></figure>

Nous vous recommandons de consulter le [blog de Dynamo](https://dynamobim.org/blog/) pour obtenir les informations les plus récentes concernant le développement de Dynamo. Le tableau ci-dessous résume les principales étapes du cycle de vie de Dynamo pour Civil 3D. 

<table data-full-width="false"><thead><tr><th width="180">Version de Civil 3D</th><th width="161">Version de Dynamo</th><th>Remarques</th></tr></thead><tbody><tr><td>2024.1</td><td>2,18</td><td></td></tr><tr><td>2024</td><td>2.17</td><td>Mise à jour de l’interface utilisateur du Lecteur Dynamo</td></tr><tr><td>2023.2</td><td>2.15</td><td></td></tr><tr><td>2023</td><td>2.13</td><td>Mise à jour de l’interface utilisateur de Dynamo</td></tr><tr><td>2022.1</td><td>2.12</td><td><ul><li>Ajout de paramètres de stockage des données de liaison d’objet</li><li>Nouveaux nœuds pour le contrôle de la liaison des objets</li></ul></td></tr><tr><td>2022</td><td>2.10</td><td><ul><li>Inclus dans l’installation principale de Civil 3D</li><li>Transition d’IronPython vers Python.NET</li></ul></td></tr><tr><td>2021</td><td>2.5</td><td></td></tr><tr><td>2020.2</td><td>2.4</td><td></td></tr><tr><td>2020 Update 2</td><td>2.4</td><td>Nouveaux nœuds ajoutés</td></tr><tr><td>2020.1</td><td>2.2</td><td></td></tr><tr><td>2020</td><td>2.1</td><td>Version initiale</td></tr></tbody></table>
