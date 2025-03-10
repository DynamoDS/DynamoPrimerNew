# Python et Civil 3D

Bien que Dynamo soit un outil de [programmation visuelle](../../a\_appendix/a-1\_visual-programming-and-dynamo.md) extrêmement puissant, il est également possible d’aller au-delà des nœuds et des fils et d’écrire du code sous forme de texte. Vous pouvez procéder de deux manières :

1. écrire en **DesignScript** à l’aide d’un bloc de code ;
2. écrire en **Python** à l’aide d’un nœud Python.

Cette section se concentre sur la manière d’exploiter le langage Python dans l’environnement Civil 3D afin de tirer parti des API .NET d’AutoCAD et de Civil 3D.

{% hint style="info" %} Consultez la section [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention") pour plus d’informations sur l’utilisation du Python dans Dynamo. {% endhint %}

## Documentation de l’API

AutoCAD et Civil 3D disposent de plusieurs API qui permettent aux développeurs comme vous d’étendre le produit principal avec des fonctionnalités personnalisées. Dans le contexte de Dynamo, ce sont les **API .NET gérées** qui sont pertinentes. Les liens suivants sont essentiels pour comprendre la structure des API et leur fonctionnement.

[Guide du développeur de l’API .NET d’AutoCAD](https://help.autodesk.com/view/OARX/2024/FRA/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2)

[Guide de référence de l’API .NET d’AutoCAD](https://help.autodesk.com/view/OARX/2024/FRA/?guid=OARX-ManagedRefGuide-What_s_New)

[Guide du développeur de l’API .NET de Civil 3D](https://help.autodesk.com/view/CIV3D/2024/FRA/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0)

[Guide de référence de l’API .NET de Civil 3D](https://help.autodesk.com/view/CIV3D/2024/FRA/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331)

{% hint style="info" %} Au fur et à mesure que vous avancez dans cette section, il est possible que vous rencontriez des concepts que vous ne connaissez pas, tels que les bases de données, les transactions, les méthodes, les propriétés, etc. La plupart de ces concepts sont essentiels pour travailler avec les API .NET et ne sont pas spécifiques à Dynamo ou au Python. Cette section du guide n’a pas pour objet d’examiner ces éléments en détail. Nous vous recommandons donc de vous reporter fréquemment aux liens ci-dessus pour plus d’informations. {% endhint %}

## Modèle de code

Lorsque vous modifiez un nouveau nœud Python pour la première fois, il est prérempli avec un modèle de code pour vous aider à démarrer. Voici une analyse du modèle avec des explications sur chaque bloc.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Modèle Python par défaut dans Civil 3D</p></figcaption></figure>

> 1. Importe les modules `sys` et `clr`, qui sont tous deux nécessaires au bon fonctionnement de l’interpréteur Python. En particulier, le module `clr` permet de traiter les espaces nom .NET essentiellement comme des packages Python.
> 2. Charge les assemblages standard (par exemple, les DLL) pour utiliser les API .NET gérées pour AutoCAD et Civil 3D.
> 3. Ajoute des références aux espaces nom AutoCAD et Civil 3D standard. Celles-ci sont équivalentes aux directives `using` ou `Imports` en C# ou VB.NET (respectivement).
> 4. Les ports d’entrée du nœud sont accessibles à l’aide d’une liste prédéfinie appelée `IN`. Vous pouvez accéder aux données d’un port spécifique à l’aide de son numéro d’index, par exemple `dataInFirstPort = IN[0]`.
> 5. Extrait le document et l’éditeur actifs.
> 6. Verrouille le document et lance une transaction de base de données.
> 7. Vous devez insérer l’essentiel de la logique de votre script ici.
> 8. Supprimez le commentaire de cette ligne pour valider la transaction une fois votre travail principal terminé.
> 9. Si vous souhaitez générer des données à partir du nœud, affectez-les à la variable `OUT` à la fin de votre script.

{% hint style="info" %} **Envie de personnaliser ?**\
 Vous pouvez modifier le modèle Python par défaut en modifiant le fichier `PythonTemplate.py` situé dans `C:\ProgramData\Autodesk\C3D <version>\Dynamo`. {% endhint %}

## Exemple

Examinons un exemple pour démontrer certains des concepts essentiels de l’écriture de scripts Python dans Dynamo for Civil 3D.

### Objectif

> :dart: permet d’obtenir la géométrie des limites de contour de tous les bassins versants d’un dessin.

### Ensemble de données

Voici des exemples de fichiers qui peuvent vous servir de référence pour cet exercice.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Présentation de la solution

Voici une présentation de la logique de ce graphique.

> 1. Consulter la documentation relative à l’API Civil 3D
> 2. Sélectionner tous les bassins versants du document par nom de calque
> 3. « Déballer » les objets Dynamo pour accéder aux membres internes de l’API Civil 3D
> 4. Créer des points Dynamo à partir de points AutoCAD
> 5. Créer des polycourbes à partir des points

Allons-y !

### Consulter la documentation relative à l’API

Avant de commencer à créer le graphique et à écrire le code, il est conseillé de consulter la documentation relative à l’API Civil 3D et de se familiariser avec ses fonctionnalités. Dans ce cas, il existe une [propriété dans la classe Bassin versant](https://help.autodesk.com/view/CIV3D/2024/FRA/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c) qui renvoie les points de limite du contour du bassin versant. Notez que cette propriété renvoie un objet `Point3dCollection`, que Dynamo ne saura pas traiter En d’autres termes, vous ne pourrez pas créer de polycourbe à partir d’un `Point3dCollection`. Par conséquent, vous devrez éventuellement tout convertir en points Dynamo. Nous y reviendrons plus tard.

### Obtenir tous les bassins versants

Vous pouvez maintenant commencer à construire la logique graphique. La première chose à faire est d’obtenir une liste de tous les bassins versants du document. Des nœuds sont disponibles pour cela, il n’est donc pas nécessaire de l’inclure dans le script en Python. L’utilisation de nœuds permet une meilleure visibilité pour quiconque lirait le graphique (au lieu d’ajouter beaucoup de code dans un script Python), et cela permet également au script Python de se concentrer sur une seule chose : renvoyer les points de limite des bassins versants.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Obtention de tous les bassins versants du document par calque</p></figcaption></figure>

Notez ici que la sortie du nœud **Tous les objets du calque** est une liste de CivilObjects. En effet, Dynamo for Civil 3D ne dispose pas actuellement de nœuds pour travailler avec des bassins versants, ce qui explique pourquoi nous devons accéder à l’API en Python.

### Déballer des objets

Avant d’aller plus loin, il convient d’aborder brièvement un concept important. Dans la section [node-library.md](../node-library.md "mention "), nous avons vu comment les objets CivilObjects sont liés. Pour apporter un peu plus de détails, un **objet Dynamo** est un wrapper autour d’une **entité AutoCAD**. De même, un **CivilObject Dynamo** est un wrapper autour d’une **entité Civil 3D**. Vous pouvez « déballer » un objet en accédant à ses propriétés `InternalDBObject` ou `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Type Dynamo</th><th width="373">Enveloppes</th></tr></thead><tbody><tr><td><strong>Objet</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entité</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entité</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %} En règle générale, il est plus sûr d’obtenir l’ID de l’objet à l’aide de la propriété `InternalObjectId`, puis d’accéder à l’objet enveloppé dans une transaction. En effet, la propriété `InternalDBObject` renvoie un objet DBObject AutoCAD qui n’est pas accessible en écriture. {% endhint %}

### Script Python

Voici le script Python complet qui permet d’accéder aux objets internes du bassin versant et d’obtenir leurs point de limite. Les lignes en surbrillance représentent celles qui sont modifiées/ajoutées par rapport au modèle de code par défaut.

{% hint style="info" %} Cliquez sur le texte souligné dans le script pour obtenir une explication sur chaque ligne. {% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Charger les bibliothèques standard de Python et DesignScript
import sys
import clr

# Ajouter des assemblages pour AutoCAD et Civil 3D
clr.AddReference(’AcMgd’)
clr.AddReference(’AcCoreMgd’)
clr.AddReference(’AcDbMgd’)
clr.AddReference(’AecBaseMgd’)
clr.AddReference(’AecPropDataMgd’)
clr.AddReference(’AeccDbMgd’)

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference(’ProtoGeometry’)</a>
</strong>
# Importer des références à partir d’AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Importer des références à partir de Civil 3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# Les entrées de ce nœud seront stockées sous forme de liste dans les variables IN.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("L’entrée est nulle ou vide.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Valider avant de terminer la transaction
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Attribuer la sortie à la variable OUT.
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %} En règle générale, il est recommandé d’inclure l’essentiel de la logique de votre script dans une transaction. Cela garantit un accès sécurisé aux objets que votre script lit/écrit. Dans de nombreux cas, l’omission d’une transaction peut provoquer une erreur irrécupérable. {% endhint %}

### Créer des polycourbes

À ce stade, le script Python doit produire une liste de points Dynamo que vous pouvez voir dans l’aperçu en arrière-plan. La dernière étape consiste simplement à créer des polycourbes à partir des points. Notez que cette opération pourrait également être réalisée directement dans le script Python, mais vous l’avez intentionnellement placée en dehors du script dans un nœud afin qu’elle soit plus visible. Voici à quoi ressemble le graphique final.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Graphique final</p></figcaption></figure>

### Résultat

Et voici la géométrie Dynamo finale.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Polycourbes Dynamo obtenues pour les limites du bassin versant</p></figcaption></figure>

> :tada: Mission accomplie !

## Comparaison entre IronPython et CPython

Une petite remarque avant de conclure. Selon la version de Civil 3D que vous utilisez, le nœud Python peut être configuré différemment. Dans **Civil 3D 2020 et 2021**, Dynamo utilisait un outil appelé **IronPython** pour déplacer des données entre des objets .NET et des scripts Python. Cependant, dans **Civil 3D 2022**, Dynamo est passé à l’utilisation de l’interpréteur Python natif standard (également appelé **CPython**) qui utilisent Python 3. Les avantages de cette transition comprennent l’accès aux bibliothèques modernes les plus populaires et aux nouvelles fonctionnalités de la plateforme, la maintenance essentielle et les correctifs de sécurité.

{% hint style="info" %} Pour en savoir plus sur cette transition et sur la manière de mettre à jour les scripts existants, consultez le [blog Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Si vous souhaitez continuer à utiliser IronPython, il vous suffit d’installer le package **DynamoIronPython2.7** à l’aide du gestionnaire de package Dynamo. {% endhint %}

[^1]: Par défaut, la bibliothèque de géométries Dynamo n’est pas ajoutée à l’environnement Python. L’objectif de ce script est de générer une liste de points Dynamo pour les limites du bassin versant. Vous devez donc ajouter cette ligne afin de créer les points ultérieurement.

[^2]: Cette ligne récupère la classe spécifique dont nous avons besoin à partir de la bibliothèque de géométries Dynamo. Notez que nous spécifions `import Point as DynPoint` ici au lieu de `import *`, car cette instruction introduit des conflits d’attribution de noms.

[^3]: Ici, nous allons renommer la variable par défaut `dataEnteringNode` en `objs` afin qu’elle soit plus descriptive.

[^4]: Ici, nous spécifions exactement quel port d’entrée contient les données que nous voulons au lieu du `IN` par défaut, qui fait référence à la liste complète de toutes les entrées.

[^5]: Cela crée une liste vide à laquelle vous ajouterez vos données de sortie ultérieurement.

[^6]: Il s’agit d’une protection contre une éventuelle entrée vide ou nulle dans notre script.

[^7]: Au lieu de casser le script, un message utile sera affiché expliquant pourquoi le script ne peut pas continuer.

[^8]: Le reste du script suppose que nous disposons d’une liste d’objets (et non d’un seul objet). Mais au cas où il n’y aurait qu’un seul objet (c’est-à-dire un dessin avec un seul bassin versant), il faut quand même que le script puisse s’exécuter.

[^9]: Si l’entrée n’est pas une liste (c’est-à-dire un seul objet), il suffit de créer une liste avec l’objet comme seul élément.

[^10]: Lance une boucle pour chaque objet de la liste.

[^11]: « Déballe » l’objet Dynamo en obtenant son ID d’objet.

[^12]: Récupère l’objet « enveloppé » de la base de données AutoCAD. Remarquez qu’OpenMode est défini sur `ForRead`, car vous n’avez pas prévu de modifier les objets. Il faut simplement « interroger » les données.

[^13]: Il est possible que la liste d’entrée des objets contienne un mélange de bassins versants et d’autres éléments qui ne sont pas des bassins versants. Il faut vérifier cette situation et la traiter de manière appropriée (c’est-à-dire ne poursuivre l’itération de la boucle que si l’élément est effectivement un bassin versant).

[^14]: Si le script va aussi loin, cela signifie que l’objet est bien un bassin versant. Il convient d’ajouter une nouvelle variable pour que le nom de l’objet reste clair et compréhensible.

[^15]: C’est ici que les points de limite du bassin versant sont extraits à l’aide de la propriété appropriée que nous avons recherchée plus tôt dans la documentation de l’API. Comme indiqué précédemment, cela donnera un objet `Point3dCollection`, qui est essentiellement une liste de points AutoCAD. Vous devrez les « convertir » en points Dynamo afin qu’ils soient utiles.

[^16]: Crée une liste vide pour stocker les points de limite de ce bassin versant.

[^17]: Initie une boucle pour chaque objet `Point3d` dans le `Point3dCollection`.

[^18]: Crée un point Dynamo à l’aide des coordonnées du point AutoCAD.

[^19]: Ajoute le point à la liste.

[^20]: Une fois que vous avez « converti » tous les points AutoCAD en points Dynamo, vous ajoutez la liste obtenue à votre sortie. Ensuite, la boucle externe passe à l’objet suivant de la liste d’entrée.

[^21]: Supprimez le commentaire de cette ligne pour valider la transaction.

[^22]: Enfin, vous allez générer la liste des points Dynamo.
