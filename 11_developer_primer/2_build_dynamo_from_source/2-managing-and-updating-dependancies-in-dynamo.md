# Comment mettre à jour ou ajouter une nouvelle dépendance à Dynamo

### Quand ce wiki s’applique-t-il ?
Lorsque vous travaillez sur une nouvelle fonctionnalité ou que vous mettez simplement à jour une dépendance existante, vous devez prendre en compte les points suivants avant d’importer une nouvelle dépendance dans le référentiel Dynamo.

### Considérations
1. Quelle est la licence de la dépendance nouvelle ou mise à jour ? Seules certaines licences open source sont approuvées sans passer par le service juridique d’ADSK.
    * Après avoir identifié la licence, assurez-vous que la dépendance et la version sont enregistrées dans le wiki interne.
    * Si la licence est `LGPL`, `GPL` ou `Apache`, vous devez copier le fichier de licence dans le sous-dossier _« Licences open source »_ du build Dynamo.
    * Si la licence est `LGPL`, vous devez charger le code source complet de tous les composants tiers, ainsi que des copies texte de leurs licences open source appropriées dans [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. S’il s’agit d’une mise à jour, le type de licence a-t-il changé depuis la version précédente ?
3. La dépendance est-elle multiplateforme ? 
    * Comporte-t-elle des composants natifs (comme `CEFSharp` ou `ImageMagick`) ? Cela rendra plus difficile le déploiement multiplateforme
    * Comporte-t-elle des références Windows uniquement ? Si c’est le cas, il ne doit pas s’agir d’une dépendance de DynamoCore ou d’autres parties multiplateformes de Dynamo (la couche du modèle).
4. La dépendance est-elle correctement regroupée dans le dossier bin lors du build avec toutes les dépendances requises ?
    * S’il s’agit d’une mise à jour, certains fichiers seront-ils supprimés à la suite de la mise à jour ? Cette version de Dynamo est-elle destinée à une version ponctuelle des produits hôtes ? Si c’est le cas, vous devrez conserver les anciens binaires jusqu’à une année de lancement mondial pour prendre en charge les programmes d’installation de correctifs. Consultez [ce lien](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. La dépendance ou son arborescence de dépendances est-elle en conflit avec d’autres dépendances existantes dans Dynamo ?
6. ?? La dépendance ou son arborescence de dépendances est-elle en conflit avec les dépendances existantes dans les produits qui intègrent Dynamo dans le processus (Revit, Civil, etc.) - **Ce point est important, car ces problèmes ne peuvent être découverts qu’au moment de l’intégration, à moins que le travail ne soit effectué en amont.**