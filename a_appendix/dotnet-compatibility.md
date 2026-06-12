# Compatibilité .NET

Le tableau ci-dessous indique la version .NET ciblée par chaque série de versions majeures de Dynamo. C’est important lors de la création de packages ou d’extensions, car votre projet doit cibler une version compatible de .NET.

| Version de Dynamo | Version .NET       |
| -------------- | ------------------ |
| 0.9 à 2.0      | .NET Framework 4.5 |
| 2.1 à 2.6      | .NET Framework 4.7 |
| 2.7 à 2.19     | .NET Framework 4.8 |
| 3.0 à 3.6      | .NET 8             |
| 3.3.2          | .NET 10            |
| 3.7            | .NET 10            |
| 4.0 et ultérieures           | .NET 10            |

{% hint style="info" %} Les versions 3.3.2 et 3.7 sont des versions spéciales avec .NET 10 rétroporté à partir de la version candidate 4.0\. {% endhint %}

Pour obtenir des conseils sur la mise à jour de vos packages vers une nouvelle version .NET, consultez les guides de migration dans le guide du développeur :

* [Mise à jour des packages pour Dynamo 2.x](../11\_developer\_primer/3\_developing\_for\_dynamo/6-0-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Mise à jour des packages pour Dynamo 3.x/.NET 8](../11\_developer\_primer/3\_developing\_for\_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
* [Mise à jour des packages pour Dynamo 4.x/.NET 10](../11\_developer\_primer/3\_developing\_for\_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)
