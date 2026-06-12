# .NET-Kompatibilität

Die folgende Tabelle zeigt, auf welche .NET-Version die einzelnen Hauptversionsserien von Dynamo abzielen. Dies ist beim Erstellen von Paketen oder Erweiterungen relevant, da Ihr Projekt auf eine kompatible .NET-Version ausgerichtet sein muss.

| Dynamo-Version | .NET-Version       |
| -------------- | ------------------ |
| 0.9 – 2.0      | .NET Framework 4.5 |
| 2.1 – 2.6      | .NET Framework 4.7 |
| 2.7 – 2.19     | .NET Framework 4.8 |
| 3.0 – 3.6      | .NET 8             |
| 3.3.2          | .NET 10            |
| 3.7            | .NET 10            |
| 4.0 und höher           | .NET 10            |

{% hint style="info" %} Bei 3.3.2 und 3.7 handelt es sich um spezielle Versionen, wobei .NET 10 aus dem Release-Kandidaten 4.0 rückportiert wurde. {% endhint %}

Eine Anleitung zum Aktualisieren Ihrer Pakete auf eine neue .NET-Version finden Sie in den Migrationshandbüchern im Developer Primer:

* [Aktualisieren von Paketen für Dynamo 2.x](../11\_developer\_primer/3\_developing\_for\_dynamo/6-0-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Aktualisieren von Paketen für Dynamo 3.x/.NET 8](../11\_developer\_primer/3\_developing\_for\_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
* [Aktualisieren von Paketen für Dynamo 4.x/.NET 10](../11\_developer\_primer/3\_developing\_for\_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)
