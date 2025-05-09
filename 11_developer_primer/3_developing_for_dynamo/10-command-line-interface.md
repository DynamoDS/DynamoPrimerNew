# Interface de ligne de commande Dynamo

`-o, -O, --OpenFilePath` Demandez à Dynamo d’ouvrir un fichier de commandes et d’exécuter les commandes qu’il contient à cet emplacement. Cette option n’est prise en charge que lors de l’exécution à partir de DynamoSandbox.  

`-c, -C, --CommandFilePath` Demandez à Dynamo d’ouvrir un fichier de commandes et d’exécuter les commandes qu’il contient à cet emplacement. Cette option n’est prise en charge que lors de l’exécution à partir de DynamoSandbox.  

`-v, -V, --Verbose` Demandez à Dynamo de générer toutes les évaluations qu’il effectue dans un fichier XML à l’emplacement spécifié.  

`-g, -G, --Geometry` Demandez à Dynamo de générer la géométrie de toutes les évaluations dans un fichier JSON à cet emplacement.  

`-h, -H, --help` Obtenez de l’aide.  

`-i, -I, --Import` Demandez à Dynamo d’importer un assemblage en tant que bibliothèque de nœuds. Cet argument devrait être un chemin de fichier vers un `.dll` unique. Si vous souhaitez importer plusieurs `.dlls`, écrivez-les en les séparant par un espace : `-i 'assembly1.dll' 'assembly2.dll'`.  

`--GeometryPath` Chemin relatif ou absolu vers un répertoire contenant ASM. Lorsqu’il est fourni, au lieu de rechercher ASM sur le disque dur, il sera chargé directement à partir de ce chemin.  

`-k, -K, --KeepAlive` Mode Keepalive, laissez le processus Dynamo s’exécuter jusqu’à ce que le chargement d’une extension l’arrête.  

`--HostName` Identifiez la variation Dynamo associée à l’hôte.  

`-s, -S, --SessionId` Identifiez l’ID de session d’analyse de l’hôte Dynamo.  

`-p, -P, --ParentId` Identifiez l’ID parent d’analyse de l’hôte Dynamo.  

`-x, -X, --ConvertFile` Lorsqu’il est utilisé avec l’indicateur `-O`, il ouvre un fichier `.dyn` à partir du chemin spécifié et le convertit en `.json`. Le fichier porte l’extension `.json` et se trouve dans le même répertoire que le fichier d’origine.  

`-n, -N, --NoConsole` La fenêtre de la console ne permet pas d’interagir avec la CLI en mode Keepalive.  

`-u, -U, --UserData` Spécifiez le dossier de données utilisateur à utiliser par PathResolver avec la CLI.  

`--CommonData` Spécifiez le dossier de données commun à utiliser par PathResolver avec la CLI.  

`--DisableAnalytics` Désactive l’analyse dans Dynamo pendant toute la durée de vie du processus.  

`--CERLocation` Spécifiez l’emplacement de l’outil de rapport des erreurs d’échec sur le disque.  

`--ServiceMode` Spécifiez le démarrage du mode de service.  



#### Pourquoi 
 Vous pourriez vouloir contrôler Dynamo à partir de la ligne de commande pour différentes raisons, notamment : 
 
 * Automatisation de nombreuses exécutions de Dynamo
 * Test des graphes Dynamo (voir également l’option -c lors de l’utilisation de DynamoSandbox)
 * Exécution d’une séquence de graphes Dynamo dans un ordre déterminé
 * Écriture de fichiers de commandes qui exécutent plusieurs exécutions en ligne de commande
 * Écriture d’un autre programme pour contrôler et automatiser l’exécution des graphes Dynamo et tirer parti des résultats de ces calculs

#### What
L’interface en ligne de commande (DynamoCLI) est un complément de DynamoSandbox. Il s’agit d’un utilitaire en ligne de commande DOS/Terminal conçu pour fournir la commodité des arguments en ligne de commande pour exécuter Dynamo. Dans sa première implémentation, l’utilitaire ne s’exécute pas de manière autonome. Il doit être exécuté à partir du dossier qui stocke les fichiers binaires Dynamo, car il dépend des mêmes DLL de base que le bac à sable. Il ne peut pas interagir avec d’autres versions de Dynamo.

Il existe quatre façons d’exécuter la CLI : à partir d’une invite DOS, à partir de fichiers de commandes DOS et en tant que raccourci de bureau Windows dont le chemin est modifié pour inclure les indicateurs en ligne de commande spécifiés. La spécification du fichier DOS peut être complète ou relative, et les lecteurs mappés et la syntaxe de l’URL sont également pris en charge. Elle peut également être construite avec Mono et exécutée sous Linux ou Mac à partir du terminal.

Les packages Dynamo sont pris en charge par l’utilitaire. Toutefois, vous ne pouvez pas charger de nœuds personnalisés (dyf), mais uniquement des graphes autonomes (dyn).

Lors des tests préliminaires, l’utilitaire CLI prend en charge les versions localisées de Windows et vous pouvez spécifier des arguments filespec avec des caractères ASCII majuscules.

La CLI est accessible via l’application DynamoCLI.exe. Cette application permet à un utilisateur ou à une autre application d’interagir avec le modèle d’évaluation Dynamo en appelant DynamoCLI.exe à l’aide d’une chaîne de commande. La chaîne en question devrait se présenter comme suit :
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
Cette commande indiquera à Dynamo d’ouvrir le fichier spécifié à *"C:\\someReallyCoolDynamoFile.Dyn"*, sans dessiner d’interface utilisateur, puis de l’exécuter. Dynamo se ferme lorsque l’exécution du graphe est terminée. 

**Nouveautés de la version 2.1** : l’application DynamoWPFCLI.exe. Cette application prend en charge tout ce que l’application DynamoCLI.exe prend en charge avec en plus l’option de géométrie (-g). L’application DynamoWPFCLI.exe est spécifique à Windows uniquement.

#### Remarques importantes

* La méthode préférée pour interagir avec DynamoCLI consiste à utiliser une interface d’invite de commande.
* À ce stade, vous devez exécuter DynamoCLI à partir depuis son emplacement d’installation dans le dossier [Version de Dynamo]. La CLI doit accéder aux mêmes fichiers .dll que Dynamo. Elle ne doit donc pas être déplacée.
* Vous devriez être en mesure d’exécuter des graphes actuellement ouverts dans Dynamo, mais cela peut entraîner des effets secondaires inattendus.
* Tous les chemins d’accès de fichiers sont entièrement compatibles avec DOS, donc les chemins relatifs et complets devraient fonctionner, mais assurez-vous de placer vos chemins entre guillemets *"C:path\\to\\file.dyn"* 
* DynamoCLI est une nouvelle fonctionnalité et actuellement en cours de développement. Actuellement, la **CLI ne charge qu’un seul sous-ensemble** de bibliothèques Dynamo standard, tenez-en compte si un graphe ne s’exécute pas correctement. Ces bibliothèques sont spécifiées [ici](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) 
* Actuellement, aucune sortie std n’est fournie si aucune erreur n’est rencontrée. Dans ce cas, la CLI se fermera simplement une fois l’exécution terminée.
 
#### Comment

`-o` : vous pouvez ouvrir Dynamo en pointant vers un fichier .dyn, dans un mode sans interface qui exécutera le graphe.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

`-v` : peut être utilisé lorsque Dynamo s’exécute en mode sans interface (après avoir utilisé `-o` pour ouvrir un fichier). Cet indicateur itérera tous les nœuds du graphe et videra leurs valeurs de sortie dans un simple fichier XML. Étant donné que l’indicateur `--ServiceMode` peut forcer Dynamo à exécuter plusieurs évaluations de graphes, le fichier de sortie contiendra des valeurs pour chaque évaluation qui se produit.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
Le fichier XML de sortie se présentera sous la forme suivante :
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` : peut être utilisé lorsque Dynamo s’exécute en mode sans interface (après avoir utilisé `-o` pour ouvrir un fichier). Cet indicateur générera le graphe, puis videra la géométrie résultante dans un fichier JSON. 

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
Le fichier de géométrie JSON se présentera sous la forme suivante :

 TBD - Travail en cours

`-h` : utilisez cette option pour obtenir une liste des options possibles

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

L’indicateur -i peut être utilisé plusieurs fois pour importer plusieurs assemblages que le graphe que vous essayez d’ouvrir doit exécuter.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

L’indicateur -l peut être utilisé pour exécuter Dynamo sous un paramètre régional différent. Mais en général, le paramètre régional n’affecte pas les résultats du graphe

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

L’indicateur --GeometryPath peut être utilisé pour faire pointer DynamoSandbox ou CLI vers un ensemble spécifique de fichiers binaires ASM. Utilisez-le comme suit :

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

ou

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

L’indicateur -k peut être utilisé pour laisser le processus Dynamo s’exécuter jusqu’à ce qu’une extension chargée l’arrête.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

L’indicateur --HostName peut être utilisé pour identifier la variation Dynamo associée à l’hôte.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

ou

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

L’indicateur -s peut être utilisé pour identifier l’ID de session d’analyse d’hôte Dynamo

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

L’indicateur -p peut être utilisé pour identifier l’ID parent d’analyse d’hôte Dynamo

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
