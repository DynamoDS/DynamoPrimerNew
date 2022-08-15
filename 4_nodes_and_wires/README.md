# Nœuds et fils

## Nœuds

Dans Dynamo, les **nœuds** sont les objets que vous connectez pour former un programme visuel. Chaque **nœud** effectue une opération, parfois aussi simple que le stockage d'un nombre, ou plus complexe, comme la création ou l'envoi de requêtes à une géométrie.

### Anatomie d'un nœud

La plupart des nœuds de Dynamo sont composés de cinq éléments. Bien qu'il existe des exceptions, telles que les nœuds Input, l'anatomie de chaque nœud peut être décrite comme suit :

\![](<images/nodes and wires - nodes anatomy.jpg>)

> 1. Nom : nom du nœud conforme à la convention d'appellation `Category.Name`.
> 2. Corps principal : corps principal du nœud. Cliquez ici avec le bouton droit de la souris pour afficher les options au niveau du nœud entier
> 3. Ports (entrants et sortants) : récepteurs des fils qui fournissent les données d'entrée au nœud, ainsi que les résultats de l'action du nœud
> 4. Valeur par défaut : cliquez avec le bouton droit de la souris sur un port d'entrée. Certains nœuds ont des valeurs par défaut qui peuvent être utilisées ou non.
> 5. Icône de combinaison : indique l'[option de combinaison](../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md#lacing) spécifiée pour les entrées de liste correspondantes (plus d'informations sur cette option ultérieurement).

### Ports d'entrée/de sortie des nœuds

Les entrées et les sorties des nœuds sont appelées "ports" et servent de récepteurs pour les fils. Les données sont intégrées au nœud via des ports sur la gauche et sortent du nœud après l'exécution de son opération sur la droite.

Les ports doivent recevoir des données d'un certain type. Par exemple, la connexion d'un nombre tel que _2.75_ aux ports d'un nœud Point By Coordinates permet de créer un point. Toutefois, si vous indiquez _"Red"_ sur le même port, une erreur se produira.

{% hint style="info" %} Conseil : placez le curseur sur un port pour afficher une info-bulle contenant le type de données attendu. {% endhint %}

\![](<images/nodes and wires - nodes input and tooltip.jpg>)

> 1. Libellé de port
> 2. Info-bulle
> 3. Type de données
> 4. Valeur par défaut

### États des nœuds

Dynamo donne une indication de l'état d'exécution de votre programme visuel en effectuant le rendu des nœuds avec différents schémas de couleurs en fonction de l'état de chaque nœud. La hiérarchie des états suit la séquence suivante : Erreur > Avertissement > Infos > Aperçu.

Lorsque vous placez le curseur ou cliquez avec le bouton droit de la souris sur le nom ou les ports, vous affichez des informations et des options supplémentaires.

\![](<../.gitbook/assets/nodes and wires - node states.png>)

> 1. Entrées satisfaites : un nœud avec des barres verticales bleues sur ses ports d'entrée est bien connecté et toutes ses entrées sont connectées.
> 2. Entrées non satisfaites : ces entrées doivent être connectées à un nœud avec une barre verticale rouge sur un ou plusieurs ports d'entrée.
> 3. Fonction : un nœud qui génère une fonction et comporte une barre verticale grise sur un port de sortie est un nœud de fonction.
> 4. Sélectionné : les nœuds actuellement sélectionnés ont une bordure bleue.
> 5. Gelé : un nœud bleu translucide est gelé, ce qui interrompt son exécution.
> 6. Aperçu désactivé : une barre d'état grise sous le nœud et une icône en forme d'œil <img src="images/nodes and wires - preview off.jpg" alt="" data-size="line"> indiquent que la prévisualisation de la géométrie du nœud est désactivée.
> 7. Avertissement : une barre d'état jaune située sous le nœud indique l'état d'avertissement, ce qui signifie qu'il manque au nœud des données d'entrée ou que les types de données peuvent être incorrects.
> 8. Erreur : une barre d'état rouge située sous le nœud indique que le nœud est en état d'erreur.
> 9. Informations : la barre d'état bleue située sous le nœud indique l'état Informations, qui indique des informations utiles sur les nœuds. Cet état peut être déclenché lorsque vous approchez une valeur maximale prise en charge par le nœud, lorsque vous utilisez un nœud d'une manière susceptible d'avoir un impact sur les performances, etc.

#### Gestion des nœuds d'erreur ou d'avertissement

Si votre programme visuel contient des avertissements ou des erreurs, Dynamo fournit des informations supplémentaires sur le problème. Tout nœud jaune comporte également une info-bulle au-dessus de son nom. Placez le curseur de la souris sur l'icône d'info-bulle d'avertissement \![](<images/nodes and wires - node warning icon.png>) ou d'erreur \![](<images/nodes and wires - node error icon.png>) pour la développer.

{% hint style="info" %} Conseil : examinez les nœuds en amont à la lumière de ces informations d'info-bulle pour voir si le type ou la structure de données requis est erroné. {% endhint %}

\![](<images/nodes and wires - nodes with warning tooltip.jpg>)

> 1. Info-bulle d'avertissement : une valeur "Null" ou l'absence de donnée ne peut être comprise comme un double, c'est-à-dire un nombre
> 2. Utilisez le nœud Watch pour examiner les données d'entrée
> 3. En amont, le nœud Number contient "Red" et non un nombre

## Fils

Les fils connectent les nœuds entre eux pour créer des relations et établir le flux de votre programme visuel. Vous pouvez les considérer comme des fils électriques qui transportent des données d'un objet à l'autre.

### Flux de programme <a href="#program-flow" id="program-flow"></a>

Les fils connectent le port de sortie d'un nœud au port d'entrée d'un autre nœud. Cette direction établit le **flux de données** dans le programme visuel.

Les ports d'entrée se trouvent sur le côté gauche et les ports de sortie sur le côté droit des nœuds. Par conséquent, vous pouvez généralement dire que le flux du programme se déplace de gauche à droite.

\![](<images/nodes and wires - flow of data.jpg>)

### Création de fils <a href="#creating-wires" id="creating-wires"></a>

Cliquez avec le bouton gauche de la souris sur un port pour créer un fil, puis cliquez avec le bouton gauche de la souris sur le port d'un autre nœud pour créer une connexion. Pendant que vous réalisez une connexion, le fil s'affiche en pointillés et devient continu lorsque la connexion est établie.

Les données passent toujours par ce fil d'une sortie à une entrée. Toutefois, vous pouvez créer le fil dans les deux directions en termes d'ordre de clic sur les ports connectés.

\![](<images/nodes and wires - creating a wire.gif>)

### Modification des fils <a href="#editing-wires" id="editing-wires"></a>

Souvent, vous souhaitez ajuster le flux du programme dans votre programme visuel en modifiant les connexions représentées par les fils. Pour modifier un fil, cliquez avec le bouton gauche de la souris sur le port d'entrée du nœud déjà connecté. Vous pouvez ensuite procéder de l'une des manières suivantes :

* Pour définir la connexion sur un port d'entrée, cliquez avec le bouton gauche de la souris sur un autre port d'entrée.

\![](<images/nodes and wires - edit wire change port (2).gif>)

* Pour supprimer le fil, retirez-le et cliquez avec le bouton gauche de la souris sur l'espace de travail.

\![](<images/nodes and wires - edit wires remove.gif>)

* Reconnectez plusieurs fils à l'aide de la combinaison Maj+clic gauche.

\![](<images/nodes and wires - edit multi ports.gif>)

* Dupliquez un fil à l'aide de la combinaison Ctrl+clic gauche.

\![](<images/nodes and wires - duplicate wire.gif>)

#### Fils par défaut et fils en surbrillance <a href="#wire-previews" id="wire-previews"></a>

Par défaut, l'aperçu des fils s'affiche avec un trait gris. Lorsqu'un nœud est sélectionné, il effectue le rendu de tous les fils connectés avec la même bordure bleue que le nœud.

\![](<images/nodes and wires - default vs highlighted wires.jpg>)

> 1. Fil en surbrillance
> 2. Fil par défaut

**Masquage des fils par défaut**

Si vous préférez masquer les fils dans le graphique, vous pouvez accéder à cette option en sélectionnant Affichage > Connecteurs > Afficher les connecteurs.

Avec ce paramètre, seuls les nœuds sélectionnés et les fils qui les rejoignent présentent une bordure bleu clair.

\![](<images/nodes and wires - hide wires setting (1).gif>)

#### Masquage des fils individuels uniquement

Vous pouvez également masquer le fil sélectionné uniquement. Pour ce faire, cliquez avec le bouton droit de la souris sur la sortie Nœuds > Masquer les fils.

\![](<images/nodes and wires - hide selected wire.gif>)
