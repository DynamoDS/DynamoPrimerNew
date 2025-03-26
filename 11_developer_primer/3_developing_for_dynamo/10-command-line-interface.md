# Interface en ligne de commande Dynamo

***
      -o, -O, --OpenFilePath        Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox
***
      -c, -C, --CommandFilePath     Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox                      
***
      -v, -V, --Verbose             Instruct Dynamo to output all evaluations it performs to an XML file at this path
***                                        
      -g, -G, --Geometry            Instruct Dynamo to output geometry from all evaluations to a JSON file at this path
***
      -h, -H, --help                Get some help
***
      -i, -I, --Import              Instruct Dynamo to import an assembly as a node library. This argument should be a 
                                    file path to a single.dll - if you wish to import multiple dlls - list the dlls 
                                    separated by a space: -i 'assembly1.dll' 'assembly2.dll'
***
      --GeometryPath                Relative or absolute path to a directory containing ASM. When supplied, instead of 
                                    searching the hard disk for ASM, it will be loaded directly from this path
***
      -k, -K, --KeepAlive           Keepalive mode, leave the Dynamo process running until a loaded extension shuts it 
                                    down
***
      --HostName                    Identify Dynamo variation associated with the host
***
      -s, -S, --SessionId           Identify Dynamo host analytics session id
***
      -p, -P, --ParentId            Identify Dynamo host analytics parent id
***
      -x, -X, --ConvertFile         When used in combination with the 'O' flag, opens a .dyn file from the specified 
                                    path and converts it to .json. The file will have the .json extension and be 
                                    located in the same directory as the original file
***
      -n, -N, --NoConsole           Don't rely on the console window to interact with CLI in Keepalive mode
***
      -u, -U  --UserData            Specify user data folder to be used by PathResolver with CLI
***
      --CommonData                  Specify common data folder to be used by PathResolver with CLI
***
      --DisableAnalytics            Disables analytics in Dynamo for the process lifetime
***
      --CERLocation                 Specify the crash error report tool located on the disk
***
      --ServiceMode                 Specify the service mode startup


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
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
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
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v` : peut être utilisé lorsque Dynamo s’exécute en mode sans interface (après avoir utilisé `-o` pour ouvrir un fichier). Cet indicateur itérera tous les nœuds du graphe et videra leurs valeurs de sortie dans un simple fichier XML. Étant donné que l’indicateur `--ServiceMode` peut forcer Dynamo à exécuter plusieurs évaluations de graphes, le fichier de sortie contiendra des valeurs pour chaque évaluation qui se produit.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
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
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
Le fichier de géométrie JSON se présentera sous la forme suivante :
```
 TBD - Work in progress
```
`-h` : utilisez cette option pour obtenir une liste des options possibles
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
L’indicateur -i peut être utilisé plusieurs fois pour importer plusieurs assemblages que le graphe que vous essayez d’ouvrir doit exécuter.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

L’indicateur -l peut être utilisé pour exécuter Dynamo sous un paramètre régional différent. Mais en général, le paramètre régional n’affecte pas les résultats du graphe
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

L’indicateur --GeometryPath peut être utilisé pour faire pointer DynamoSandbox ou CLI vers un ensemble spécifique de fichiers binaires ASM. Utilisez-le comme suit :
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

ou
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
L’indicateur -k peut être utilisé pour laisser le processus Dynamo s’exécuter jusqu’à ce qu’une extension chargée l’arrête.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
L’indicateur --HostName peut être utilisé pour identifier la variation Dynamo associée à l’hôte.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
ou
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
L’indicateur -s peut être utilisé pour identifier l’ID de session d’analyse d’hôte Dynamo
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
L’indicateur -p peut être utilisé pour identifier l’ID parent d’analyse d’hôte Dynamo
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"
