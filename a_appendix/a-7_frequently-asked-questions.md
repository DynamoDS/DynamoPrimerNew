# Questions fréquemment posées

## Comment utiliser les builds Dynamo

### Builds quotidiens et builds stables

Par tradition, l’équipe Dynamo d’Autodesk maintient un rythme d’itération rapide en publiant à la fois des builds quotidiens par validation et des builds stables après le cycle de test et de publication du système. Notre équipe aimerait redémarrer les builds quotidiens et stables afin que les utilisateurs puissent contrôler localement l’endroit d’extraction de DynamoCore sur leur disque et l’utiliser en toute confiance, sans affecter Dynamo pour les autres produits ADSK. Il existe quelques candidats naturels à cette fin, notamment .nupkg, un fichier .zip ou un programme d’installation dédié permettant aux utilisateurs de choisir le chemin d’installation ou d’autres options.

Compte tenu de notre objectif de mettre à la disposition des utilisateurs notre dernier code de la manière la plus simple possible, nous avons décidé de fournir un fichier .zip contenant les fichiers binaires DynamoCore et Dynamo Sandbox qui peut être utilisé sans Revit (selon quelques contraintes).

### Builds Dynamo en ZIP

#### Définition et source

Le build en ZIP de DynamoCoreRuntime est un instantané des fichiers binaires DynamoCore, qui est créé au cours de nos builds automatisés.

Vous devriez être en mesure de lancer DynamoSandbox.exe dans le dossier extrait pour utiliser Dynamo avec une configuration minimale.

#### Composants requis

| Version de Dynamo | Microsoft Visual C++ | DirectX                         |   |   |   |   |
| --------------    | -------------------- | ------------------------------- | - | - | - | - |  
| 2.0 à 2.6         | 2015 Redistribuable  | 10                              |   |   |   |   |   
| 2.7               | 2019 Redistribuable  | 11/12 (inclus avec Windows 10   |   |   |   |   |  
| >=2.8             | 2019 Redistribuable  | 11/12 (inclus avec Windows 10   |   |   |   |   | 

**Microsoft DirectX, qui est également disponible publiquement dans notre référentiel GitHub Dynamo** [**ici**](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX)

**7-zip utilisé pour décompresser le package** [**ici**](https://www.7-zip.fr/download.html)

**Microsoft Visual C++ 2015-2024 Redistributable (x64)** [**ici**](https://aka.ms/vs/17/release/vc_redist.x64.exe)

**Composants facultatifs**

Bibliothèque de géométries (elle ne sera disponible qu’avec certains outils de modélisation Autodesk, tels que Revit, Civil 3D, Advanced Steel, etc.)

### Dépannage

Si vous avez décompressé le build et que vous n’avez pas pu lancer DynamoSandbox.exe du tout, assurez-vous d’utiliser [7-zip](https://www.7-zip.fr/download.html) pour décompresser le build. Vous pouvez également débloquer manuellement l’archive .zip _avant_ de l’extraire, si vous disposez des autorisations nécessaires sur votre ordinateur.

![](images/a-7/dynamo-builds-1.png)

S’il vous manque l’un des composants requis, vous risquez de rencontrer des problèmes lors de l’utilisation de Dynamo et certaines parties de l’interface utilisateur peuvent ne pas se charger.

D’après la capture d’écran suivante donnée à titre d’exemple, en décompressant notre build sur une machine virtuelle Windows 10 propre sans processeur graphique, il manque les deux composants requis sur cette machine. Cette information est indiquée sur la console Dynamo.

![](images/a-7/dynamo-builds-2.png)

**Installation de DirectX**

Suivez les instructions de Microsoft ici pour vérifier si DirectX est déjà installé. Si ce n’est pas le cas, vous pouvez ouvrir DXSETUP.exe dans notre référentiel GitHub Dynamo [ici](https://github.com/DynamoDS/Dynamo/tree/master/tools/install/Extra/DirectX). Une fois que la boîte de dialogue ci-dessous s’affiche, n’hésitez pas à cliquer sur Suivant pour installer DirectX à l’emplacement par défaut.

![](images/a-7/dynamo-builds-3.png)

**Installation de Microsoft Visual C++ 2015-2024 Redistributable (x64)**

Veuillez télécharger la dernière version [ici](https://aka.ms/vs/17/release/vc_redist.x64.exe). Vous devriez alors être en mesure d’exécuter le programme d’installation nommé vc_redist.x64.exe dans l’emplacement de téléchargement de votre navigateur. Une fois que la boîte de dialogue ci-dessous s’affiche, n’hésitez pas à cliquer sur Installer pour placer ce composant à l’emplacement par défaut.

![](images/a-7/dynamo-builds-4.png)

Après avoir installé les deux composants requis à partir du lien ci-dessus, relancez DynamoSandbox.exe. Vous devriez obtenir le résultat suivant :

![](images/a-7/dynamo-builds-5.png)

**Graphiques 3D manquants**

Vous pouvez également rencontrer des problèmes graphiques en exécutant Sandbox pour la première fois. Vous pouvez suivre la section de questions fréquemment posées sur les problèmes graphiques standard ici :

[https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-FAQ)

En général, vous devrez probablement forcer le mode de processeur graphique haute performance pour votre carte graphique lors de l’utilisation du DynamoSandbox.exe

_Exemple de panneau de configuration NVIDIA :_

![](images/a-7/dynamo-builds-6.png)

**Installation de WebView2 Runtime**

À l’heure actuelle, les modules Dynamo suivants utilisent le composant WebView2 : navigateur de documentation, visites guidées et bibliothèque. Donc, pour nous assurer que ces parties de Dynamo affichent correctement le contenu Web, nous devons installer le programme d’installation WebView2 Evergreen Runtime (vous devrez vérifier s’il est déjà installé sur l’ordinateur ou s’il doit être installé).

Voici le lien pour installer le WebView2 Runtime : [https://developer.microsoft.com/fr-fr/microsoft-edge/webview2/#download-section](https://developer.microsoft.com/fr-fr/microsoft-edge/webview2/?form=MA13LH#download-section)

![](images/a-7/dynamo-builds-7.png)

Ceux qui doivent être installés (un seul d’entre eux) sont Evergreen Bootstrapper ou Evergreen Standalone Installer, le premier télécharge un programme d’installation de 1,50 Mo et le second télécharge un programme d’installation de 130 Mo.

Une fois le Runtime installé, les composants Dynamo suivants devraient fonctionner correctement :

![](images/a-7/dynamo-builds-8.png)

**Problèmes liés aux nœuds Dynamo Excel**

Vous pouvez vous référer à cet [article](https://www.autodesk.com/fr/support/technical/article/caas/sfdcarticles/sfdcarticles/FRA/Warning-Data-ImportExcel-operation-failed-Could-not-load-file-or-assembly-Microsoft-Office-Interop-Excel-when-running-the-Dynamo-script-in-Revit.html) pour les diagnostics.

### Emplacement des builds Dynamo

Versions stables

[https://dynamobim.org/download/](https://dynamobim.org/download/)

[https://github.com/DynamoDS/Dynamo/releases](https://github.com/DynamoDS/Dynamo/releases)

Builds quotidiens et versions stables

[https://dynamobuilds.com/](https://dynamobuilds.com/)
