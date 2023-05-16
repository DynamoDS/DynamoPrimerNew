# Extensions en tant que packages

### Extensions en tant que packages <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### Vue d’ensemble <a href="#overview" id="overview"></a>

Les extensions Dynamo peuvent être déployées dans le gestionnaire de package tout comme les bibliothèques de nœuds Dynamo. Lorsqu’un package installé contient une extension de vue, l’extension est chargée à l’exécution lors du chargement de Dynamo. Vous pouvez vérifier dans la console Dynamo que l’extension a été correctement chargée.

### Structure du package <a href="#package-structure" id="package-structure"></a>

La structure d’un package d’extension est la même que celle d’un package normal contenant...

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

En supposant que vous ayez déjà créé votre extension, vous disposerez (au minimum) d’un assemblage .NET et d’un fichier manifest. L’assemblage doit contenir une classe qui implémente `IViewExtension` ou `IExtension`. Le fichier manifest .XML indique à Dynamo la classe à instancier pour lancer votre extension. Pour que le gestionnaire de package puisse localiser correctement l’extension, le fichier manifest doit correspondre exactement à l’emplacement et au nom de l’assemblage.

Placez les fichiers d’assemblage dans le dossier `bin` et le fichier manifest dans le dossier `extra`. Toutes les ressources supplémentaires peuvent également être placées dans ce dossier.

Exemple de fichier manifest .XML :

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### Téléchargement <a href="#uploading" id="uploading"></a>

Une fois que vous avez un dossier contenant les sous-répertoires décrits ci-dessus, vous êtes prêt à envoyer (télécharger) vers le gestionnaire de package. Notez que vous ne pouvez pas publier de packages à partir de Dynamo Sandbox. Cela signifie que vous devez utiliser Dynamo Revit. Une fois dans Dynamo Revit, accédez à Packages => Publier le nouveau package. Cette opération invite l’utilisateur à se connecter à son compte Autodesk auquel il souhaite associer le package.

À ce stade, vous devriez vous trouver dans la fenêtre normale de publication de package où vous saisirez tous les champs requis concernant votre package/extension. Il existe une étape supplémentaire **très importante** qui nécessite de s’assurer qu’aucun de vos fichiers d’assemblage n’est marqué comme bibliothèque de nœuds. Pour ce faire, cliquez avec le bouton droit de la souris sur les fichiers que vous avez importés (dossier de package créé ci-dessus). Un menu contextuel apparaît et vous permet de cocher (ou décocher) cette option. Tous les assemblages d’extension doivent être décochés.

![Publier un package](images/ViewExtension_Search.png)

Avant de publier publiquement, vous devez toujours publier localement pour vous assurer que tout fonctionne comme prévu. Une fois la vérification terminée, vous pouvez démarrer en ligne en sélectionnant Publier.

### Tirer <a href="#pulling" id="pulling"></a>

Pour vérifier que votre package a été correctement téléchargé, vous devez être en mesure de le rechercher en fonction du nom et des mots clés spécifiés lors de l’étape de publication. Enfin, il est important de noter que ces extensions nécessitent un redémarrage de Dynamo avant de fonctionner. En général, ces extensions requièrent des paramètres spécifiés lors du démarrage de Dynamo.

![Rechercher des packages](images/ViewExtension_Search.jpg)
