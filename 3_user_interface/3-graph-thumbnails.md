# Miniatures de graphiques pour la page d’accueil de Dynamo

La page d’accueil de Dynamo affiche les miniatures de graphiques sur une carte large. La carte est au format 16:9. La page d’accueil centre l’image et rogne le haut et le bas.

Une miniature carrée peut sembler complète dans Generative Design, qui affiche le carré entier. La même image peut perdre ses titres ou ses logos dans la page d’accueil.

Cela s’applique à tout graphique (`.dyn`), et pas seulement aux graphiques contenus dans un package. Définissez l’image dans les propriétés du graphique. L’image est stockée dans le fichier du graphique.

{% hint style="info" %}
Placez les titres, les logos et l’image principale dans la **zone de sécurité** centrale. N’utilisez le haut et le bas d’une miniature carrée que pour l’arrière-plan.
{% endhint %}

## Modèles de miniatures

Téléchargez un modèle et créez votre visuel par-dessus. La bande centrale, plus sombre, correspond à ce qu’affiche la page d’accueil. Les bandes plus claires, en haut et en bas, sont rognées dans la page d’accueil. Generative Design affiche le carré entier.

> Téléchargez le modèle SVG pour le modifier dans un logiciel de dessin vectoriel, ou le modèle PNG pour placer votre visuel dans un éditeur d’images.

{% file src="../.gitbook/assets/Template_Thumbnail.svg" %}

{% file src="../.gitbook/assets/DynamoThumbnailTemplate.png" %}

<figure><img src="../.gitbook/assets/DynamoThumbnailTemplate.png" alt="Modèle de miniature carrée. La bande centrale, plus sombre, est visible dans la page d’accueil de Dynamo. Les bandes supérieure et inférieure, plus claires, sont rognées dans la page d’accueil et restent visibles dans Generative Design."><figcaption><p>La bande centrale, plus sombre, est visible dans la page d’accueil. Les bandes supérieure et inférieure, plus claires, sont rognées.</p></figcaption></figure>

## Composer la miniature

1. Ouvrez le modèle SVG ou PNG dans un éditeur d’images.
2. Placez votre visuel sur le carré rose du modèle.
3. Placez le titre, le logo et l’image principale dans la bande centrale, plus sombre.
4. N’utilisez les bandes supérieure et inférieure, plus claires, que pour la couleur d’arrière-plan ou des éléments graphiques supplémentaires.
5. Masquez ou supprimez le repère rose, puis exportez un PNG **carré**.

Utilisez une image **carrée** (1:1). La page d’accueil affiche toujours la bande centrale, plus sombre, recadrée au format 16:9. La taille en pixels ne modifie pas le recadrage. Si vous partez d’un fichier vierge plutôt que du modèle, donnez-lui un format carré. Le modèle de miniature lui-même mesure 840 × 840 pixels.

## Ajouter la miniature à un graphique

La page d’accueil lit la miniature dans le fichier du graphique (`.dyn`), et non dans le formulaire de publication du gestionnaire de package.

1. Ouvrez le graphique dans Dynamo.
2. Accédez à **Fichier > Afficher les propriétés du graphique > Général**.
3. Ajoutez le PNG exporté dans le champ d’image.
4. Enregistrez le graphique.

Si le graphique est distribué dans un package, incluez ce fichier `.dyn` lors de la publication. Reportez-vous à la rubrique [Publication d’un package](../6_custom_nodes_and_packages/6-2_packages/4-publishing.md).

## Vérifier le résultat dans la page d’accueil

1. Ouvrez la page d’accueil de Dynamo.
2. Sous Récent, recherchez le graphique que vous venez d’enregistrer.
3. Vérifiez que le titre et le logo sont entièrement visibles sur la carte.

Si du texte est coupé, rapprochez-le du centre, exportez de nouveau l’image, puis mettez à jour les propriétés du graphique. Ne vous fiez pas uniquement à Generative Design. Generative Design affiche le carré entier.
