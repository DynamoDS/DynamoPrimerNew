# Ce qu’Autodesk Assistant ne peut pas encore faire

### Mise à jour des packages

Autodesk Assistant ne peut pas encore télécharger ni installer de packages dans Dynamo

![](../../.gitbook/assets/Aa_Update.jpg)

Comme le montre l’image ci-dessus, Autodesk Assistant ne peut pas modifier, installer ni désinstaller les packages externes associés à Dynamo. Il peut en revanche fournir des instructions pour que vous les installiez vous-même.


### Lecture et écriture directes de fichiers

Autodesk Assistant fonctionne avec les fichiers de graphique Dynamo : il peut ouvrir un graphique, en insérer un dans votre zone de dessin, en exécuter un et enregistrer votre travail. Il ne peut pas, de lui-même, lire ni écrire d’autres fichiers sur votre disque dur.

Dans l’exemple ci-dessous, une géométrie a été créée dans la zone de dessin. Il est demandé à Autodesk Assistant de récupérer les points qui composent la géométrie et de les exporter au format JSON. Bien qu’Autodesk Assistant ne puisse pas générer ce fichier JSON directement, il suggère de créer un script Dynamo qui s’en charge.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

C’est ce type de suggestion qu’il faut retenir. Tout ce qui se trouve en dehors du graphique est accessible au moyen de nœuds, et non directement par Assistant.

### Modification du modèle Revit sans graphique

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant pour Dynamo n’a pas de connexion directe avec le modèle Revit. Demandez-lui de modifier le modèle comme vous le demanderiez à Assistant dans Revit : il vous répondra qu’il ne peut pas le faire.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

Il peut néanmoins modifier votre modèle Revit. Il s’y prend comme vous le feriez : il place des nœuds Revit, les relie entre eux, et le modèle est modifié lorsque le graphique s’exécute.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Cette façon de procéder a deux conséquences :

- **Rien ne change tant que le graphique ne s’exécute pas.** Vous pouvez d’abord lire le graphique et ajuster les valeurs. Le modèle n’est modifié que lorsque le graphique est exécuté, par vous ou par Assistant dans le cadre de votre demande.
- **La modification peut être reproduite.** Le graphique reste dans la zone de dessin. Enregistrez-le pour pouvoir exécuter de nouveau la même tâche sur un autre modèle, ou pour le partager avec votre équipe.

Si vous souhaitez effectuer une modification ponctuelle et directe, sans graphique, utilisez Autodesk Assistant dans Revit.
