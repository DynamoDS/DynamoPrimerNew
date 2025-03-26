# Intégration Dynamo 

Vous venez d’accéder à la documentation d’intégration du langage de programmation visuel Dynamo.

Ce guide aborde différents aspects de l’hébergement de Dynamo dans votre application pour permettre à vos utilisateurs d’interagir avec votre application à l’aide de la programmation visuelle.

Sommaire :
* [Introduction](#dynamo-integration) Aperçu détaillé du contenu de ce guide et définition de Dynamo.
* [Point d’entrée Dynamo personnalisé](#dynamo-custom-entry-point) Comment créer un DynamoModel et par où commencer.
* [Liaison et traçage d’éléments](#-element-binding-and-trace) Utilisation du mécanisme de traçage de Dynamo pour lier les nœuds du graphe à leurs résultats dans votre hôte.
* [Nœuds de sélection Dynamo Revit](#-dynamo-revit-selection-nodes) Comment implémenter des nœuds qui permettent aux utilisateurs de sélectionner des objets ou des données de votre hôte et de les transmettre en tant qu’entrées au graphe Dynamo. 
* [Vue d’ensemble des packages intégrés Dynamo](#dynamo-built-in-packages-overview) Description de la bibliothèque standard Dynamo et de l’utilisation du mécanisme sous-jacent pour expédier des paquets avec votre intégration.


##### Un peu de terminologie :
Dans ces documents, nous utiliserons les termes script, graphe et programme Dynamo indistinctement pour désigner le code créé par les utilisateurs dans Dynamo.

## Point d’entrée Dynamo personnalisé
#### Exemple Dynamo Revit 

[https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534](https://github.com/DynamoDS/DynamoRevit/blob/master/src/DynamoRevit/DynamoRevit.cs#L534)

Le modèle `DynamoModel` constitue le point d’entrée d’un hébergement d’application Dynamo. Il représente une application Dynamo. Le modèle est l’objet racine de niveau supérieur qui contient des références aux autres structures de données et objets importants qui composent l’application Dynamo et la machine virtuelle DesignScript. 

Un objet de configuration est utilisé pour définir des paramètres communs sur un modèle `DynamoModel` lors de sa construction. 

Les exemples présentés dans ce document sont tirés de l’implémentation de DynamoRevit, qui est une intégration dans laquelle Revit héberge un modèle `DynamoModel` en tant que complément. (Architecture de plug-in pour Revit). Lorsque ce complément se charge, il démarre un modèle `DynamoModel`, puis l’affiche à l’utilisateur avec une vue `DynamoView` et un modèle de vue `DynamoViewModel`. 

Dynamo est un projet c# .net, et pour l’utiliser en cours de traitement dans votre application, vous devez être en mesure d’héberger et d’exécuter du code .net.

DynamoCore est un moteur de calcul multiplateforme et une collection de modèles principaux, qui peuvent être construits avec .net ou mono (à l’avenir .net core). Toutefois, DynamoCoreWPF contient les composants d’interface utilisateur de Dynamo spécifiques à Windows et ne se compile pas sur d’autres plates-formes.

### Étapes pour personnaliser votre point d’entrée Dynamo 

Pour initialiser le modèle `DynamoModel`, les intégrateurs devront suivre ces étapes à partir d’un endroit du code de l’hôte.

### Préchargez les fichiers DLL Dynamo partagés à partir de l’hôte.  

Actuellement, la liste de D4R n’inclut que `Revit\SDA\bin\ICSharpCode.AvalonEdit.dll.` Cela permet d’éviter les conflits de versions de bibliothèque entre Dynamo et Revit. Exemple En cas de conflits sur `AvalonEdit`, la fonction du bloc de code peut être totalement interrompue. Le problème est signalé dans Dynamo 1.1.x à https://github.com/DynamoDS/Dynamo/issues/7130 et peut également être reproduit manuellement. Si les intégrateurs détectent des conflits de bibliothèques entre la fonction hôte et Dynamo, il est suggéré d’effectuer cette action dans un premier temps. Cela est parfois nécessaire pour empêcher d’autres plug-ins ou l’application hôte elle-même de charger une version incompatible d’une dépendance partagée. Une meilleure solution consiste à résoudre le conflit de versions en alignant la version ou, si possible, à utiliser une redirection de liaison .net dans le fichier app.config de l’hôte. 
 

### Chargement d’ASM 

#### Qu’est-ce qu’ASM et LibG ?

ASM est la bibliothèque de géométrie ADSK sur laquelle Dynamo est construit.

LibG est un wrapper .Net convivial autour du noyau de géométrie ASM. LibG partage son schéma de gestion de versions avec ASM : il utilise le même numéro de version majeure et mineure qu’ASM pour indiquer qu’il s’agit du wrapper correspondant d’une version d’ASM particulière. Lorsqu’une version ASM est donnée, la version correspondante de LibG doit être la même. Dans la plupart des cas, LibG devrait fonctionner avec toutes les versions d’ASM d’une version majeure particulière. Par exemple, LibG 223 devrait pouvoir charger n’importe quelle version d’ASM 223.


#### Chargement d’ASM par Dynamo Sandbox 

Dynamo Sandbox est conçu pour pouvoir fonctionner avec plusieurs versions d’ASM. Pour cela, plusieurs versions de LibG sont regroupées et livrées avec Core. Dynamo Shape Manager intègre une fonctionnalité permettant de rechercher les produits Autodesk livrés avec ASM, de sorte que Dynamo peut charger ASM à partir de ces produits et faire en sorte que les nœuds de géométrie fonctionnent sans être explicitement chargés dans une application hôte. À l’heure actuelle, la liste des produits est la suivante : 
```
private static readonly List<string> ProductsWithASM = new List<string>() 

 { "Revit", "Civil", "Robot Structural Analysis", "FormIt" }; 
```
Dynamo effectuera une recherche dans le registre Windows et déterminera si les produits Autodesk de cette liste sont installés sur la machine de l’utilisateur. Si l’un d’entre eux est installé, le programme recherchera les fichiers binaires ASM, en obtiendra la version et recherchera une version LibG correspondante dans Dynamo.  

Compte tenu de la version d’ASM, l’API ShapeManager suivante choisira l’emplacement du préchargeur LibG correspondant à charger. Si une version correspond exactement, elle sera utilisée. Autrement, la version de LibG inférieure la plus proche, mais avec la même version majeure, sera chargée.  

Exemple Si Dynamo est intégré à une version de développement Revit où il existe une version ASM 225.3.0 plus récente, Dynamo essaiera d’utiliser LibG 225.3.0 si elle existe. Autrement, le programme essaiera d’utiliser la version majeure la plus proche inférieure à son premier choix, c’est-à-dire 225.0.0. 

`public static string GetLibGPreloaderLocation(Version asmVersion, string dynRootFolder)` 

#### Chargement d’ASM à partir de l’hôte par l’intégration en cours de processus Dynamo 

Revit est le premier élément de la liste de recherche de produits ASM, ce qui signifie que, par défaut, `DynamoSandbox.exe` tentera d’abord de charger ASM à partir de Revit. Nous voulons quand même nous assurer que la session de travail D4R intégrée charge ASM à partir de l’hôte Revit actuel : par exemple, si l’utilisateur possède à la fois R2018 et R2020 sur son ordinateur, lors du lancement de D4R à partir de R2020, D4R doit utiliser ASM 225 à partir de R2020 au lieu d’ASM 223 à partir de R2018. Les intégrateurs devront implémenter des appels similaires à ceux indiqués ci-après pour forcer le chargement de leur version spécifiée. 

```
internal static Version PreloadAsmFromRevit() 

{ 

     var asmLocation = AppDomain.CurrentDomain.BaseDirectory; 
     Version libGVersion = findRevitASMVersion(asmLocation); 
     var dynCorePath = DynamoRevitApp.DynamoCorePath; 
     var preloaderLocation = DynamoShapeManager.Utilities.GetLibGPreloaderLocation(libGVersion, dynCorePath); 
     Version preLoadLibGVersion = PreloadLibGVersion(preloaderLocation); 
     DynamoShapeManager.Utilities.PreloadAsmFromPath(preloaderLocation, asmLocation); 
     return preLoadLibGVersion; 

} 
```

#### Chargement d’ASM par Dynamo à partir d’un chemin personnalisé 

Récemment, nous avons ajouté la possibilité pour `DynamoSandbox.exe` et `DynamoCLI.exe` de charger une version particulière d’ASM. Pour ignorer le comportement normal de recherche dans le Registre, vous pouvez utiliser l’indicateur `�gp` pour forcer Dynamo à charger ASM à partir d’un chemin particulier. 

`DynamoSandbox.exe -gp �somePath/To/ASMDirectory/� `

  



### Créer une StartConfiguration 

La StartupConfiguration est utilisée pour être transmise en tant que paramètre d’initialisation de DynamoModel, ce qui indique qu’elle contient presque toutes les définitions de la façon dont vous souhaitez personnaliser les paramètres de votre session Dynamo. En fonction de la façon dont les propriétés suivantes sont définies, l’intégration de Dynamo peut varier entre les différents intégrateurs. Par exemple, différents intégrateurs peuvent définir différents chemins de modèle Python ou différents formats de nombres affichés. 

Elle se compose des éléments suivants : 

* DynamoCorePath // Où se trouvent les fichiers binaires DynamoCore de chargement

* DynamoHostPath // Où se trouvent les fichiers binaires d’intégration Dynamo

* GeometryFactoryPath // Où se trouvent les fichiers binaires LibG chargés

* PathResolver // Objet qui permet de résoudre divers fichiers

* PreloadLibraryPaths // Où se trouvent les fichiers binaires des nœuds préchargés, par exemple DSOffice.dll 

* AdditionalNodeDirectories // Où se trouvent des fichiers binaires de nœuds supplémentaires
 
* AdditionalResolutionPaths // Chemins de résolution d’assemblage supplémentaires pour d’autres dépendances qui peuvent être requises lors du chargement des bibliothèques 

* UserDataRootFolder // Dossier de données utilisateur, p. ex. `"AppData\Roaming\Dynamo\Dynamo Revit"` 

* CommonDataRootFolder // Dossier par défaut pour l’enregistrement de définitions personnalisées, d’échantillons, etc.

* Context // Nom d’hôte de l’intégrateur + version `(Revit<BuildNum>)`

* SchedulerThread // Thread du planificateur d’intégrateur implémentant `ISchedulerThread`. Pour la plupart des intégrateurs, il s’agit du thread de l’interface utilisateur principale ou de n’importe quel thread à partir duquel ils peuvent accéder à leur API.

* StartInTestMode // Si la session en cours est une session d’automatisation de test (modifie un ensemble de comportements Dynamo) ne l’utilisez pas, à moins que vous ne soyez en train d’écrire des tests.

* AuthProvider // L’implémentation de l’intégrateur d’IAuthProvider, p. ex. l’implémentation de RevitOxygenProvider est dans Greg.dll, qui s’utilise pour l’intégration du chargement de packageManager. 

### Préférences 

Le chemin des paramètres de préférences par défaut est géré par `PathManager.PreferenceFilePath`, p. ex. `"AppData\\Roaming\\Dynamo\\Dynamo Revit\\2.5\\DynamoSettings.xml"`. Les intégrateurs peuvent décider s’ils souhaitent également envoyer un fichier de paramètres personnalisés de préférences à un emplacement qui doit être en accord avec le gestionnaire de chemins. Les propriétés de paramètres de préférence suivantes sont sérialisées : 

* IsFirstRun // Indique si cette version de Dynamo est exécutée pour la première fois, p. ex. utilisée pour déterminer s’il est nécessaire d’afficher le message d’acceptation/refus de GA. S’utilise également pour déterminer s’il est nécessaire de migrer l’ancien paramètre de préférences Dynamo lors du lancement d’une nouvelle version de Dynamo, afin que les utilisateurs bénéficient d’une expérience fluide 

* IsUsageReportingApproved // Indique si les rapports d’utilisation sont approuvés ou non 

* IsAnalyticsReportingApproved // Indique si les rapports d’analyse sont approuvés ou non 

* LibraryWidth // La largeur du panneau gauche de la bibliothèque Dynamo 

* ConsoleHeight // La hauteur de l’affichage de la console 

* ShowPreviewBubbles // Indique si les bulles d’aperçu doivent être affichées 

* ShowConnector // Indique si les connecteurs sont affichés 

* ConnectorType // Indique le type de connecteur : Bézier ou Polyligne 

* BackgroundPreviews // Indique l’état actif de l’aperçu de l’arrière-plan spécifié 

* RenderPrecision // Le niveau de précision du rendu. Une valeur plus faible génère des maillages avec moins de triangles. Plus la valeur est élevée, plus la géométrie sera lisse dans l’aperçu de l’arrière-plan. Le chiffre 128 convient pour générer rapidement la géométrie de l’aperçu.

* ShowEdges // Indique si les arêtes de surfaces et de solides seront rendues 

* ShowDetailedLayout // NE S’UTILISE PAS

* WindowX, WindowY // Dernières coordonnées X, Y de la fenêtre Dynamo 

* WindowW, WindowH // Dernière largeur, hauteur de la fenêtre Dynamo 

* UseHardwareAcceleration // Dynamo doit-il utiliser l’accélération matérielle si elle est prise en charge ? 

* NumberFormat // La précision décimale utilisée pour afficher les nombres dans la bulle d’aperçu toString(). 

* MaxNumRecentFiles // Le nombre maximal de chemins de fichier récents à enregistrer 

* RecentFiles // Liste des chemins d’accès aux fichiers récemment ouverts. Toute modification aura une incidence directe sur la liste des fichiers récents dans la page de démarrage de Dynamo 

* BackupFiles // Liste des chemins d’accès aux fichiers de sauvegarde 

* CustomPackageFolders // Liste de dossiers contenant des fichiers binaires Zero-Touch et des chemins d’accès aux répertoires qui seront analysés à la recherche de packages et de nœuds personnalisés.

* PackageDirectoriesToUninstall // Liste des packages utilisés par le gestionnaire de packages pour déterminer quels packages sont marqués pour être supprimés. Ces chemins seront supprimés si possible lors du démarrage de Dynamo.

* PythonTemplateFilePath // Chemin d’accès au fichier Python (.py) à utiliser comme modèle de départ lors de la création d’un nouveau nœud PythonScript. Il peut être utilisé pour configurer un modèle Python personnalisé pour votre intégration.

* BackupInterval // Indique combien de temps (en millisecondes) le graphe sera automatiquement sauvegardé 

* BackupFilesCount // Indique le nombre de sauvegardes qui seront effectuées 

* PackageDownloadTouAccepted // Indique si l’utilisateur a accepté les conditions générales d’utilisation pour le téléchargement de packages à partir du gestionnaire de packages 

* OpenFileInManualExecutionMode // Indique l’état par défaut de la case à cocher « Ouvrir en mode manuel » dans OpenFileDialog 

* NamespacesToExcludeFromLibrary // Indique les espaces de noms (le cas échéant) qui ne doivent pas être affichés dans la bibliothèque de nœuds Dynamo. Format de chaîne : « [nom de la bibliothèque]:[espace de noms complet] » 

Exemple de paramètres de préférences sérialisés : 

``` xml 
<PreferenceSettings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"> 

<IsFirstRun>false</IsFirstRun> 

<IsUsageReportingApproved>false</IsUsageReportingApproved> 

<IsAnalyticsReportingApproved>false</IsAnalyticsReportingApproved> 

<LibraryWidth>204</LibraryWidth> 

<ConsoleHeight>0</ConsoleHeight> 

<ShowPreviewBubbles>true</ShowPreviewBubbles> 

<ShowConnector>true</ShowConnector> 

<ConnectorType>BEZIER</ConnectorType> 

<BackgroundPreviews> 

<BackgroundPreviewActiveState> 

<Name>IsBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

<BackgroundPreviewActiveState> 

<Name>IsRevitBackgroundPreviewActive</Name> 

<IsActive>true</IsActive> 

</BackgroundPreviewActiveState> 

</BackgroundPreviews> 

<IsBackgroundGridVisible>true</IsBackgroundGridVisible> 

<RenderPrecision>128</RenderPrecision> 

<ShowEdges>false</ShowEdges> 

<ShowDetailedLayout>true</ShowDetailedLayout> 

<WindowX>553</WindowX> 

<WindowY>199</WindowY> 

<WindowW>800</WindowW> 

<WindowH>676</WindowH> 

<UseHardwareAcceleration>true</UseHardwareAcceleration> 

<NumberFormat>f3</NumberFormat> 

<MaxNumRecentFiles>10</MaxNumRecentFiles> 

<RecentFiles> 

<string></string> 

</RecentFiles> 

<BackupFiles> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\backup\backup.DYN</string> 

</BackupFiles> 

<CustomPackageFolders> 

<string>..AppData\Roaming\Dynamo\Dynamo Revit\2.5</string> 

</CustomPackageFolders> 

<PackageDirectoriesToUninstall /> 

<PythonTemplateFilePath /> 

<BackupInterval>60000</BackupInterval> 

<BackupFilesCount>1</BackupFilesCount> 

<PackageDownloadTouAccepted>true</PackageDownloadTouAccepted> 

<OpenFileInManualExecutionMode>false</OpenFileInManualExecutionMode> 

<NamespacesToExcludeFromLibrary> 

<string>ProtoGeometry.dll:Autodesk.DesignScript.Geometry.TSpline</string> 

</NamespacesToExcludeFromLibrary> 

</PreferenceSettings> 
``` 
 

* Extensions // Liste d’extensions implémentant IExtension, si la valeur est nulle, Dynamo charge les extensions à partir du chemin par défaut (dossier `extensions` sous le dossier Dynamo) 

* IsHeadless // Indique si Dynamo est lancé sans interface utilisateur, effets Analyses 

* UpdateManager // Implémentation d’UpdateManager par l’intégrateur, voir la description ci-dessus 

* ProcessMode // Équivalent à TaskProcessMode, Synchrone en mode test, sinon Asynchrone. Contrôle le comportement du planificateur. Les environnements monothreads peuvent également définir cette option sur synchrone.

Utiliser la StartConfiguration cible pour lancer `DynamoModel`

Après la transmission de StartConfig pour lancer `DynamoModel`, DynamoCore supervise les spécificités réelles pour s’assurer que la session Dynamo est correctement initialisée avec les détails spécifiés. Chaque intégrateur devra effectuer des étapes de configuration supplémentaires après l’initialisation de `DynamoModel`, p. ex. dans D4R, il faut s’abonner aux événements pour surveiller les transactions de l’hôte Revit ou les mises à jour de documents, la personnalisation des nœuds Python, etc. 

 ### Passons déjà à la partie de « programmation visuelle »

Pour initialiser `DynamoViewModel` et `DynamoView`, vous devez d’abord construire un `DynamoViewModel`. Pour cela, vous pouvez utiliser la méthode statique `DynamoViewModel.Start`. Voir ci-dessous :

``` c#

    viewModel = DynamoViewModel.Start(
                    new DynamoViewModel.StartConfiguration()
                    {
                        CommandFilePath = commandFilePath,
                        DynamoModel = model,
                        Watch3DViewModel = 
                            HelixWatch3DViewModel.TryCreateHelixWatch3DViewModel(
                                null,
                                new Watch3DViewModelStartupParams(model), 
                                model.Logger),
                        ShowLogin = true
                    });
     
     var view = new DynamoView(viewModel);

```

`DynamoViewModel.StartConfiguration` propose beaucoup moins d’options que la configuration du modèle. Elles sont pour la plupart explicites : `CommandFilePath` peut être ignoré à moins que vous n’écriviez un cas de test.


Le paramètre `Watch3DViewModel` contrôle la façon dont l’aperçu d’arrière-plan et les nœuds watch3D affichent la géométrie 3D. Vous pouvez utiliser votre propre implémentation si vous implémentez les interfaces requises.

Pour construire la `DynamoView`, seul `DynamoViewModel` est requis. La vue est un contrôle de fenêtre et peut être affichée à l’aide de WPF.

 ### Exemple DynamoSandbox.exe :

 DynamoSandbox.exe est un environnement de développement permettant de tester, d’utiliser et d’expérimenter avec DynamoCore. Il s’agit d’un excellent exemple à consulter pour voir comment les composants `DynamoCore` et `DynamoCoreWPF` sont chargés et configurés. Vous pouvez trouver certains des points d’entrée [ici](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoSandbox/DynamoCoreSetup.cs#L37)

 ## Liaison et traçage d’éléments

#### vue d’ensemble

*Trace* est un mécanisme de DynamoCore, qui est capable de sérialiser des données dans le fichier .dyn (fichier Dynamo). Important : ces données sont liées aux sites d’appel des nœuds dans le graphe Dynamo.

Lorsqu’un graphe Dynamo est ouvert à partir du disque, les données de traçage qui y sont enregistrées sont réassociées aux nœuds du graphe.

#### glossaire :
* Mécanisme de traçage :
    
  * Implémente la liaison d’éléments dans Dynamo
  * Le mécanisme de traçage peut être utilisé pour s’assurer que les objets sont reliés à la géométrie qu’ils ont créée
  * Le site d’appel et le mécanisme de traçage gèrent la fourniture d’un GUID persistant que l’implémenteur de nœud peut utiliser pour relier les liens

* Site d’appel

  * L’exécutable contient plusieurs sites d’appels. Ces sites d’appels sont utilisés pour expédier l’exécution vers les différents endroits d’où ils doivent être expédiés :
    * Bibliothèque C#
    * Méthode intégrée
    * Fonction DesignScript
    * Nœud personnalisé (fonction DS)

* TraceSerializer
  * Sérialise les classes marquées `ISerializable` et `[Serializable]` en traçage.
  * Gère la sérialisation et la désérialisation des données dans le traçage.
  * TraceBinder contrôle la liaison des données désérialisées à un type d’exécution. (crée une instance de classe réelle)

#### À quoi cela ressemble-t-il ?
----

Les données de traçage sont sérialisées dans le fichier .dyn à l’intérieur d’une propriété appelée Bindings. Il s’agit d’un tableau de callsites-ids -> data. Un site d’appel représente l’emplacement/l’instance particulière où un nœud est appelé dans la machine virtuelle DesignScript. Il convient de mentionner que les nœuds d’un graphe Dynamo peuvent être appelés plusieurs fois et donc que plusieurs sites d’appel peuvent être créés pour une même instance de nœud.

```json
"Bindings": [
    {
      "NodeId": "1e83cc25-7de6-4a7c-a702-600b79aa194d",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_1e83cc25-7de6-4a7c-a702-600b79aa194d":  "Base64 Encoded Data"
      }
    },
    {
      "NodeId": "c69c7bec-d54b-4ead-aea8-a3f45bea9ab2",
      "Binding": {
        "WrapperObject_InClassDecl-1_InFunctionScope-1_Instance0_c69c7bec-d54b-4ead-aea8-a3f45bea9ab2": "Base64 Encoded Data"
      }
    }
  ],

 
```

 il n’est *PAS* conseillé de dépendre du format des données codées en base64 sérialisées.


#### Quel problème cherchons-nous à résoudre ?
----


Il existe de nombreuses raisons pour lesquelles on voudrait enregistrer des données arbitraires à la suite de l’exécution d’une fonction, mais dans ce cas, Trace a été développé pour résoudre un problème spécifique que les utilisateurs rencontrent fréquemment lorsqu’ils construisent et itèrent sur des programmes logiciels qui créent des éléments dans des applications hôtes.

Le problème est celui que nous avons appelé `Element Binding` et l’idée est la suivante :

Lorsqu’un utilisateur développe et exécute un graphe Dynamo, il est probable qu’il génère de nouveaux éléments dans le modèle de l’application hôte. Pour notre exemple, supposons que l’utilisateur dispose d’un petit programme qui génère 100 portes dans un modèle architectural. Le nombre et l’emplacement de ces portes sont contrôlés par leur programme.

La première fois que l’utilisateur exécute le programme, il génère ces 100 portes.

Plus tard, lorsque l’utilisateur modifie une entrée de son programme et la réexécute, son programme *(sans liaison d’éléments)* crée 100 nouvelles portes, les anciennes portes existent toujours dans le modèle avec les nouvelles.

----

Étant donné que Dynamo est un environnement de programmation en direct et qu’il dispose d’un mode d’exécution `"Automatic"` selon lequel les modifications apportées au graphe déclenchent une nouvelle exécution, cela peut rapidement encombrer un modèle avec les résultats de nombreuses exécutions du programme.


Nous avons constaté que ce n’est généralement pas ce que les utilisateurs attendent, mais qu’avec la liaison d’éléments activée, les résultats précédents d’une exécution de graphe sont nettoyés et supprimés ou modifiés. L’élément *supprimé ou modifié* dépend de la flexibilité de l’API de votre hôte. Lorsque la liaison d’éléments est activée, après la deuxième, la troisième ou la 50e exécution du programme Dynamo de l’utilisateur, il ne reste plus que 100 portes dans le modèle.

Pour ce faire, il ne suffit pas de sérialiser les données dans le fichier .dyn et, comme vous le verrez ci-dessous, il existe dans DynamoRevit des mécanismes basés sur le traçage pour prendre en charge ces processus de reliaison.

----

Nous en profitons pour mentionner l’autre cas d’utilisation important de la liaison d’éléments pour les hôtes comme Revit. Étant donné que les éléments créés avec la liaison d’éléments activée tentent de conserver les ID d’éléments existants (modifier les éléments existants), la logique qui a été élaborée sur ces éléments dans l’application hôte continue d’exister après l’exécution d’un programme Dynamo. Exemples :

Revenons à notre exemple de modèle architectural.

Voyons d’abord un exemple avec la liaison d’élément désactivée. Cette fois-ci, l’utilisateur dispose d’un programme qui génère des murs architecturaux.

 Il exécute son programme, qui génère des murs dans l’application hôte. Il quitte ensuite le graphe Dynamo et utilise les outils Revit normaux pour placer des fenêtres dans ces murs. Les fenêtres sont liées à ces murs spécifiques dans le cadre du modèle Revit.

L’utilisateur redémarre Dynamo et exécute à nouveau le graphe. Maintenant, comme dans notre dernier exemple, il a deux ensembles de murs. Le premier ensemble comprend les fenêtres qui ont été ajoutées, mais pas les nouveaux murs.

Avec la liaison d’éléments activée, nous aurions pu conserver le travail existant qui a été effectué manuellement dans l’application hôte sans Dynamo. Par exemple, si la liaison avait été activée lors de la deuxième exécution du programme de l’utilisateur, les murs auraient été modifiés et non supprimés, et les modifications en aval apportées à l’application hôte auraient été conservées. Le modèle contiendrait les murs avec les fenêtres, au lieu de deux ensembles de murs dans des états différents.

-----

![Création de murs](images/creates_walls.png)


#### Liaison d’éléments par rapport au traçage
----

Trace est un mécanisme de DynamoCore qui utilise une variable statique de sites d’appel aux données pour mapper vos données au site d’appel d’une fonction du graphe, comme décrit ci-dessus.

Il vous permet également de sérialiser des données arbitraires dans le fichier .dyn lors de l’écriture de nœuds Dynamo Zero-Touch. Cette opération n’est généralement pas conseillée, car elle implique que le code Zero-Touch potentiellement transférable dépend désormais de DynamoCore.

N’ayez pas recours au format sérialisé des données dans le fichier .dyn, utilisez plutôt l’attribut et l’interface [Serializable]

Par ailleurs, ElementBinding est construit sur les API Trace et implémenté dans l’intégration Dynamo *(DynamoRevit, Dynamo4Civil, etc.)*


#### API Trace
Voici certaines des API Trace de bas niveau à connaître :

``` c#
public static ISerializable GetTraceData(string key)
///Returns the data that is bound to a particular key

public static void SetTraceData(string key, ISerializable value)
///Set the data bound to a particular key
```

vous pouvez voir leur utilisation dans l’exemple ci-dessous

pour interagir avec les données de Trace que Dynamo a chargées à partir d’un fichier existant ou qu’il est en train de générer, vous pouvez consulter les éléments suivants :

```c#
 public IDictionary<Guid, List<CallSite.RawTraceData>> 
 GetTraceDataForNodes(IEnumerable<Guid> nodeGuids, Executable executable)
```
[GetTraceDataForNodes](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs#L218)

[RuntimeTrace.cs](https://github.com/DynamoDS/Dynamo/blob/master/src/Engine/ProtoCore/RuntimeData.cs)


#### Exemple de traçage simple à partir d’un nœud
----
Un exemple de nœud Dynamo qui utilise directement Trace est fourni ici dans le référentiel [DynamoSamples](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryZeroTouch/Examples/TraceExample.cs)

Le résumé de la classe qui s’y trouve explique l’essentiel de Trace :

```
  /*
     * After a graph update, Dynamo typically disposes of all
     * objects created during the graph update. But what if there are 
     * objects which are expensive to re-create, or which have other
     * associations in a host application? You wouldn't want those those objects
     * re-created on every graph update. For example, you might 
     * have an external database whose records contain data which needs
     * to be re-applied to an object when it is created in Dynamo.
     * In this example, we use a wrapper class, TraceExampleWrapper, to create 
     * TraceExampleItem objects which are stored in a static dictionary 
     * (they could be stored in a database as well). On subsequent graph updates, 
     * the objects will be retrieved from the data store using a trace id stored 
     * in the trace cache.
     */
```

Cet exemple utilise directement les API Trace dans DynamoCore pour stocker des données chaque fois qu’un nœud particulier s’exécute. Dans ce cas, un dictionnaire joue le rôle du modèle de l’application hôte, comme la base de données de modèles de Revit.

La configuration approximative est la suivante :

Une classe util statique `TraceExampleWrapper` est importée en tant que nœud dans Dynamo. Elle contient une méthode unique `ByString` qui crée `TraceExampleItem`. Ce sont des objets .net normaux qui contiennent une propriété `description`.

Chaque `TraceExampleItem` est sérialisé en traçage représenté par un `TraceableId`. Il s’agit simplement d’une classe contenant un `IntId` qui est marqué `[Serializeable]` afin de pouvoir être sérialisé avec l’outil de formatage `SOAP`. Voir [ici pour plus d’informations sur l’attribut sérialisable](https://docs.microsoft.com/en-us/dotnet/api/system.serializableattribute?view=netframework-4.8)

Vous devez également implémenter l’interface `ISerializable` définie [ici](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.serialization.iserializable?view=netframework-4.8)


``` c#
    [IsVisibleInDynamoLibrary(false)]
    [Serializable]
    public class TraceableId : ISerializable
    {
    }
```

Cette classe est créée pour chaque `TraceExampleItem` que nous souhaitons enregistrer dans Trace, sérialisé, codé en base64 et enregistré sur le disque lorsque le graphe est enregistré, afin que les liaisons puissent être réassociées, même plus tard lorsque le graphe est rouvert sur un dictionnaire d’éléments existant. Cette opération ne fonctionnera pas bien dans le cas de cet exemple, car le dictionnaire n’est pas vraiment persistant comme peut l’être un document Revit.

Enfin, la dernière partie de l’équation est `TraceableObjectManager`, qui est similaire à `ElementBinder` dans `DynamoRevit`. Cet élément gère la relation entre les objets présents dans le modèle document de l’hôte et les données que nous avons stockées dans Dynamo Trace.

Lorsqu’un utilisateur exécute pour la première fois un graphe contenant le nœud `TraceExampleWrapper.ByString`, un nouveau `TraceableId` est créé avec un nouvel identifiant, l’élément `TraceExampleItem` est stocké dans le dictionnaire mappé à ce nouvel identifiant et nous stockons `TraceableID` dans Trace.

À l’exécution suivante du graphe, nous recherchons l’ID stocké dans Trace, localisons l’objet mappé à cet ID et renvoyons cet objet ! Au lieu de créer un tout nouvel objet, nous modifions celui qui existe déjà.

Le flux de deux exécutions consécutives de graphe qui crée un seul `TraceExampleItem` ressemble au suivant :

![Premier appel](images/Trace-first-call.png)

![Deuxième appel](images/Trace-second-call.png)

La même idée est illustrée dans l’exemple suivant avec un cas d’utilisation plus réaliste d’un nœud DynamoRevit.

#### Diagramme de Trace

![Étapes de Trace](images/trace_diagram.png) ![Flux de Trace](images/trace_alt_diagram.png)

#### REMARQUE :
Dans les versions récentes de Dynamo, l’utilisation de TLS (stockage local des threads) a été remplacée par l’utilisation statique des membres.

#### Exemple d’implémentation de liaison d’éléments

-----
Voyons rapidement à quoi ressemble un nœud qui utilise la liaison d’éléments lorsqu’il est implémenté pour DynamoRevit. Il s’agit d’un nœud analogue au type de nœud utilisé ci-dessus dans les exemples de création de murs donnés.


---


``` c#
    private void InitWall(Curve curve, Autodesk.Revit.DB.WallType wallType, Autodesk.Revit.DB.Level baseLevel, double height, double offset, bool flip, bool isStructural)
        {
            // This creates a new wall and deletes the old one
            TransactionManager.Instance.EnsureInTransaction(Document);

            //Phase 1 - Check to see if the object exists and should be rebound
            var wallElem =
                ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>(Document);

            bool successfullyUsedExistingWall = false;
            //There was a modelcurve, try and set sketch plane
            // if you can't, rebuild 
            if (wallElem != null && wallElem.Location is Autodesk.Revit.DB.LocationCurve)
            {
                var wallLocation = wallElem.Location as Autodesk.Revit.DB.LocationCurve;
                <SNIP>

                    if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;

                  <SNIP>
                
            }

            var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
            InternalSetWall(wall);

            TransactionManager.Instance.TransactionTaskDone();

            // delete the element stored in trace and add this new one
            ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
        }
```

Le code ci-dessus illustre un exemple de constructeur pour un élément de mur. Ce constructeur est appelé à partir d’un nœud dans Dynamo, comme suit : `Wall.byParams`

Les phases importantes de l’exécution du constructeur en ce qui concerne la liaison des éléments sont les suivantes :

1. Utilisez `elementBinder` pour vérifier s’il existe des objets créés précédemment qui ont été liés à ce site d’appel dans une ancienne exécution. `ElementBinder.GetElementFromTrace<Autodesk.Revit.DB.Wall>`
2. Si tel est le cas, essayez de modifier ce mur au lieu d’en créer un nouveau.

```c#
 if(!CurveUtils.CurvesAreSimilar(wallLocation.Curve, curve))
                        wallLocation.Curve = curve;
```

3. Sinon, créez un mur.
```c#
  var wall = successfullyUsedExistingWall ? wallElem :
                     Autodesk.Revit.DB.Wall.Create(Document, curve, wallType.Id, baseLevel.Id, height, offset, flip, isStructural);
                     
```

4. Supprimez l’ancien élément que nous venons de récupérer de Trace et ajoutez le nouveau afin de pouvoir rechercher cet élément à l’avenir :
```c#
 ElementBinder.CleanupAndSetElementForTrace(Document, InternalWall);
```

### Discussion

#### Rendement
* Actuellement, chaque objet de Trace sérialisé est sérialisé à l’aide du formatage XML SOAP. L’explication est assez longue et reprend beaucoup d’informations identiques. Ensuite, les données sont codées deux fois en base64\. Ce codage n’est pas efficace en termes de sérialisation ou de désérialisation. L’efficacité peut être améliorée à l’avenir si le format interne n’est pas construit dessus. Encore une fois, nous le répétons, n’ayez pas recours au format des données sérialisées au repos.

#### ElementBinding doit-il être activé par défaut ?
* Il existe des cas d’utilisation où la liaison d’éléments n’est pas souhaitée. Que se passe-t-il si l’on est un utilisateur avancé de Dynamo développant un programme qui doit être exécuté plusieurs fois pour générer des éléments de regroupement aléatoires. L’intention du programme est de créer des éléments supplémentaires chaque fois que le programme est exécuté. Ce cas d’utilisation n’est pas facilement réalisable sans solutions de contournement pour empêcher la liaison d’éléments de fonctionner. Il est possible de désactiver elementBinding au niveau de l’intégration, mais il s’agit probablement d’une fonctionnalité de base de Dynamo. Le degré de granularité de cette fonctionnalité n’est pas clair : au niveau du nœud ? au niveau du site d’appel ? toute la session Dynamo ? L’espace de travail ? etc.

## Nœuds de sélection Dynamo Revit (quels sont-ils ?) 

En général, ces nœuds permettent à l’utilisateur de décrire d’une manière ou d’une autre un sous-ensemble du document Revit actif qu’il souhaite référencer. L’utilisateur peut référencer un élément Revit de différentes façons (décrites ci-dessous), et la sortie résultante du nœud peut être un wrapper d’élément Revit (wrapper DynamoRevit) ou une géométrie Dynamo (convertie à partir d’une géométrie Revit). Il sera utile de prendre en compte la différence entre ces types de sortie dans le contexte d’autres intégrations d’hôtes.

À un niveau élevé, **pour conceptualiser ces nœuds, il est judicieux d’utiliser une fonction qui accepte un identifiant d’élément et renvoie un pointeur vers cet élément ou une géométrie qui représente cet élément.** 

Il existe plusieurs nœuds `�Selection�` dans DynamoRevit. Nous pouvons les diviser en au moins deux groupes : 

![Nœuds de sélection Revit](images/revitSelectionNodes.png)


 

1. Choix de l’interface utilisateur : 

    Des nœuds `DynamoRevit` de cette catégorie sont, par exemple, `SelectModelElement`, `SelectElementFace`

    Ces nœuds permettent à l’utilisateur de basculer dans le contexte de l’interface utilisateur Revit et de sélectionner un élément ou un ensemble d’éléments, les ID de ces éléments sont capturés et une fonction de conversion est exécutée, soit un wrapper est créé, soit la géométrie est extraite et convertie à partir de l’élément. La conversion qui s’exécute dépend du type de nœud choisi par l’utilisateur. 

2. Requête de document : 

    Des nœuds de cette catégorie sont, par exemple, `AllElementsOfClass`, `AllElementsOfCategory`

    Ces nœuds permettent à l’utilisateur d’interroger l’ensemble du document à la recherche d’un sous-ensemble d’éléments. Ils renvoient généralement des wrappers qui pointent vers les éléments Revit sous-jacents. Ces wrappers font partie intégrante de l’expérience DynamoRevit, car ils donnent accès à des fonctionnalités plus avancées, telles que la liaison d’éléments. Ils permettent également aux intégrateurs Dynamo de choisir les API hôtes qui sont exposées en tant que nœuds aux utilisateurs.


### Workflows utilisateur Dynamo Revit : 

#### Exemples 

1. 
    * L’utilisateur sélectionne un mur Revit avec `SelectModelElement`. Un wrapper Dynamo Wall est renvoyé dans le graphe (visible dans la bulle d’aperçu du nœud) 

    * L’utilisateur place le nœud Element.Geometry et joint la sortie `SelectModelElement` à ce nouveau nœud. La géométrie du mur ayant fait l’objet d’un retournement est extraite et convertie en géométrie Dynamo à l’aide de l’API LibG. 

    * L’utilisateur bascule le graphe en mode d’exécution automatique. 

    * L’utilisateur modifie le mur d’origine dans Revit. 

    * Le graphe est réexécuté automatiquement lorsque le Revit document a déclenché un événement signalant que certains éléments ont été mis à jour. Le nœud de sélection observe cet événement et voit que l’identifiant de l’élément qu’il a sélectionné a été modifié. 

### Workflows utilisateur DynamoCivil : 

Les workflows D4C sont très similaires à la description ci-dessus pour Revit. Voici deux ensembles typiques de nœuds de sélection dans D4C :  

![Nœuds de sélection Civil 3D](images/civilSelectionNodes.png)



### Problèmes : 

* En raison de l’outil de mise à jour des modifications de document, dont les nœuds de sélection dans `DynamoRevit` sont implémentés, des boucles infinies peuvent facilement se créer : imaginez un nœud qui surveille le document au niveau de tous les éléments, puis crée de nouveaux éléments quelque part en aval de ce nœud. Lorsqu’il est exécuté, ce programme déclenche une boucle. `DynamoRevit` essaie d’intercepter ces cas de différentes manières à l’aide des identifiants de transaction et évite de modifier le document lorsque les entrées des constructeurs d’éléments n’ont pas changé.

    Cela doit être pris en compte si l’exécution automatique du graphe est lancée lorsqu’un élément sélectionné est modifié dans l’application hôte ! 

* Les nœuds de sélection dans `DynamoRevit` sont implémentés dans le projet `RevitUINodes.dll` qui fait référence à WPF. Ce n’est peut-être pas un problème, mais il vaut la peine d’en être conscient en fonction de votre plate-forme cible. 
 

### Diagrammes de flux de données

![Flux de sélection](images/selectModelElement.png)

![Flux de sélection 2](images/selectElementFace.png)



### Implémentation technique (voir les diagrammes ci-dessus) : 

Les nœuds de sélection sont implémentés en héritant des types `SelectionBase` génériques : `SelectionBase<TSelection, TResult> ` et d’un ensemble minimal de membres : 

* Implémentation d’une méthode `BuildOutputAST` : cette méthode doit renvoyer un AST, qui sera exécuté à un moment donné dans le futur, lorsque le nœud devra être exécuté. Dans le cas des nœuds de sélection, il doit renvoyer des éléments ou une géométrie à partir des identifiants d’éléments. https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs#L280 

* L’implémentation de `BuildOutputAST` est l’une des parties les plus difficiles de l’implémentation des nœuds `NodeModel` / UI. Il est préférable de mettre autant de logique que possible dans une fonction c# et d’incorporer simplement un nœud d’appel de fonction AST dans l’AST. Notez qu’ici, `node` est un nœud AST dans l’arborescence syntaxique abstraite et non un nœud du graphe Dynamo. 

![Flux de sélection 2](images/selectionAST.png)


* Sérialisation -  

  * Étant donné qu’il s’agit de types dérivés `NodeModel` explicites (et non ZeroTouch), ils nécessitent également l’implémentation d’un [JsonConstructor] qui sera utilisé lors de la désérialisation du nœud à partir d’un fichier .dyn. 

    Les références d’éléments de l’hôte doivent être enregistrées dans le fichier .dyn de sorte que lorsqu’un utilisateur ouvre un graphe contenant ce nœud, sa sélection soit toujours définie. Dans Dynamo, les nœuds NodeModel utilisent json.net pour la sérialisation, toutes les propriétés publiques seront sérialisées automatiquement à l’aide de Json.net. Utilisez l’attribut [JsonIgnore] pour sérialiser uniquement ce qui est nécessaire. 

* Les nœuds Document Query sont un peu plus simples, car ils n’ont pas besoin de stocker une référence à des ID d’élément. Voir ci-dessous pour les implémentations de classe `ElementQueryBase` et dérivées. Lorsqu’ils sont exécutés, ces nœuds appellent l’API Revit et interrogent le document sous-jacent à la recherche d’éléments, puis effectuent la conversion mentionnée précédemment en géométrie ou en wrappers d’éléments Revit. 

### Références :

#### Classes de base de DynamoCore : 

* [https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs](https://github.com/DynamoDS/Dynamo/blob/ec10f936824152e7dd7d6d019efdcda0d78a5264/src/Libraries/CoreNodeModels/Selection.cs )

* [Étude de cas de modèle de nœud : interface utilisateur personnalisée](11\_developer\_primer/3\_developing\_for\_dynamo/5-nodemodel-case-study-custom-ui.md)
* [Mise à jour des packages et des bibliothèques Dynamo pour Dynamo 2.x](11\_developer\_primer/3\_developing\_for\_dynamo/6-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Mise à jour des packages et des bibliothèques Dynamo pour Dynamo 3.x](11\_developer\_primer/3\_developing\_for\_dynamo/updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)



#### DynamoRevit : 

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Selection.cs )

* [https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs](https://github.com/DynamoDS/DynamoRevit/blob/master/src/Libraries/RevitNodesUI/Elements.cs)


## Présentation des packages intégrés Dynamo

Le mécanisme des packages intégrés vise à regrouper davantage de contenu de nœuds avec Dynamo Core, sans étendre le noyau lui-même en tirant parti de la fonctionnalité de chargement de packages Dynamo implémentée par l’extension `PackageLoader` et `PackageManager`.

Dans ce document, nous utiliserons indistinctement les termes Packages intégrés, Packages intégrés Dynamo, packages intégrés, pour signifier la même chose.

### Dois-je expédier un package en tant que package intégré ?
* Le package doit avoir des points d’entrée binaires signés, sinon il ne sera pas chargé.
* Il faut veiller à éviter d’interrompre les modifications apportées à ces packages. Cela signifie que le contenu du package doit comporter des tests automatisés.
* Gestion des versions sémantiques : il peut convenir de versionner votre package à l’aide d’un schéma de gestion de versions sémantiques et de le communiquer aux utilisateurs dans la description du package ou d’autres documents.
* Tests automatisés : veuillez voir ci-dessus, si un package est inclus à l’aide du mécanisme de package intégré, pour un utilisateur, il semble faire partie du produit et doit être testé comme un produit.
* Haut niveau d’affinage : icônes, documents de nœud, contenu localisé.
* N’expédiez pas de packages que vous ou votre équipe ne pouvez pas gérer.
* N’expédiez pas de packages tiers de cette façon (voir ci-dessus).

Essentiellement, vous devez avoir un contrôle total sur le package, la possibilité de le corriger, de le maintenir à jour et de le tester par rapport aux dernières modifications apportées à Dynamo et à votre produit. Vous devez également être en mesure de le signer.


### Packages intégrés et packages spécifiques à l’intégration de l’hôte

Nous voulons que les `Built-In Packages` soient une fonctionnalité de base, un ensemble de packages auxquels tous les utilisateurs ont accès, même s’ils n’ont pas accès au gestionnaire de packages. À l’heure actuelle, le mécanisme sous-jacent permettant de prendre en charge cette fonctionnalité est un emplacement de chargement par défaut supplémentaire pour les packages directement dans le répertoire de Dynamo Core, par rapport à DynamoCore.dll.

Avec quelques contraintes, cet emplacement sera utilisable par les clients et intégrateurs ADSK Dynamo pour distribuer leurs packages spécifiques à l’intégration. *(Par exemple, l’intégration de Dynamo Formit nécessite un package Dynamo Formit personnalisé).*

Étant donné que le mécanisme de chargement sous-jacent est le même pour les packages spécifiques au noyau et à l’hôte, il sera nécessaire de s’assurer que les packages distribués de cette manière n’entraînent pas de confusion entre les packages `Built-In Packages` principaux et les packages spécifiques à l’intégration qui ne sont disponibles que dans un seul produit hôte. Afin d’éviter toute confusion chez les utilisateurs, nous recommandons d’introduire des packages spécifiques à l’hôte lors de discussions avec les équipes Dynamo.


### Localisation des packages

Étant donné que les packages inclus dans les `Built-In Packages` seront mis à la disposition d’un plus grand nombre de clients et que les garanties que nous donnons les concernant seront plus strictes (voir ci-dessus), ils doivent être localisés.

Pour les packages ADSK internes destinés à l’inclusion de `Built-In Packages`, les limites actuelles de l’impossibilité de publier du contenu localisé dans le gestionnaire de packages ne sont pas des obstacles, car les packages n’ont pas nécessairement besoin d’être publiés dans le gestionnaire de packages.

À l’aide d’une solution de contournement, il est possible de créer manuellement (et même de publier) des packages avec des sous-répertoires de culture dans le dossier /bin d’un package.

Tout d’abord, créez manuellement les sous-répertoires spécifiques à la culture dont vous avez besoin dans le dossier `/bin` des packages. 

Si, pour une raison quelconque, le package doit également être publié dans le gestionnaire de packages, vous devez d’abord publier une version du package qui ne contient pas ces sous-répertoires de culture, puis publier une nouvelle version du package à l’aide du `publish package version` DynamoUI. Le téléversement de la nouvelle version dans Dynamo ne doit pas supprimer vos dossiers et fichiers sous `/bin`, que vous avez ajoutés manuellement à l’aide de l’explorateur de fichiers Windows. Le processus de chargement des packages dans Dynamo sera mis à jour pour répondre aux exigences relatives aux fichiers localisés à l’avenir.

Ces sous-répertoires de culture sont chargés sans problème par l’exécution .net s’ils se trouvent dans le même répertoire que les fichiers binaires nœud/extension.

Pour plus d’informations sur les assemblages de ressources et les fichiers .resx, consultez : [https://docs.microsoft.com/fr-fr/dotnet/framework/resources/creating-resource-files-for-desktop-apps](https://docs.microsoft.com/en-us/dotnet/framework/resources/creating-resource-files-for-desktop-apps).

Vous allez probablement créer les fichiers `.resx` et les compiler avec Visual Studio. Pour un assemblage `xyz.dll` donné, les ressources résultantes seront compilées dans un nouvel assemblage `xyz.resources.dll`, comme décrit ci-dessus, l’emplacement et le nom de cet assemblage sont importants.

Le `xyz.resources.dll` généré doit être disposé comme suit : `package\bin\culture\xyz.resources.dll`.

Pour accéder aux chaînes localisées dans votre package, vous pouvez utiliser ResourceManager, mais encore plus simplement, vous devriez être en mesure de faire référence à `Properties.Resources.YourLocalizedResourceName` à partir de l’assemblage pour lequel vous avez ajouté un fichier `.resx`. Par exemple, reportez-vous à : 

[https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodes/List.cs#L457) pour obtenir un exemple de message d’erreur localisé

ou [https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19](https://github.com/DynamoDS/Dynamo/blob/master/src/Libraries/CoreNodeModels/ColorRange.cs#L19) pour obtenir un exemple de chaîne d’attribut NodeDescription Dynamo localisée spécifique.

ou [https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs](https://github.com/DynamoDS/DynamoSamples/blob/master/src/SampleLibraryUI/Examples/LocalizedCustomNodeModel.cs) pour consulter un autre exemple.

### Disposition de la bibliothèque de nœuds

Normalement, lorsque Dynamo charge des nœuds à partir d’un package, il les place dans la section `Addons` de la bibliothèque de nœuds. Pour mieux intégrer les nœuds de packages intégrés à d’autres contenus intégrés, nous avons ajouté la possibilité pour les créateurs de packages intégrés de fournir un fichier `layout specification` partiel pour aider à placer les nouveaux nœuds dans la catégorie de niveau supérieur appropriée de la section de bibliothèque `default`.

Par exemple, le fichier json de spécification de présentation suivant, s’il se trouve au niveau du chemin d’accès `package/extra/layoutspecs.json` placera les nœuds spécifiés par `path` dans la catégorie `Revit` de la section `default` qui est la section principale des nœuds intégrés.

Notez que les nœuds importés à partir d’un package intégré porteront le préfixe `bltinpkg://` lorsqu’ils seront pris en compte pour la mise en correspondance avec un chemin inclus dans la spécification de présentation.

```json
{
  "sections": [
    {
      "text": "default",
      "iconUrl": "",
      "elementType": "section",
      "showHeader": false,
      "include": [ ],
      "childElements": [
        {
          "text": "Revit",
          "iconUrl": "",
          "elementType": "category",
          "include": [],
          "childElements": [
            {
              "text": "some sub group name",
              "iconUrl": "",
              "elementType": "group",
              "include": [
                {
                  "path": "bltinpkg://namespace.namespace",
                  "inclusive": false
                }
              ],
              "childElements": []
            }
          ]
        }
      ]
    }
  ]
}
```

Les modifications complexes de la présentation ne sont pas bien testées ni prises en charge, l’intention pour ce chargement particulier de spécification de présentation est de déplacer un espace de noms de package entier sous une catégorie d’hôte particulière comme `Revit` ou `Formit`. 
