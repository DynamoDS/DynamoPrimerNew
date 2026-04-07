# .NET Compatibility

The table below shows which .NET version each major Dynamo release series targets. This is relevant when building packages or extensions, as your project must target a compatible .NET version.

| Dynamo Version | .NET Version       |
| -------------- | ------------------ |
| 0.9 – 2.0      | .NET Framework 4.5 |
| 2.1 – 2.6      | .NET Framework 4.7 |
| 2.7 – 2.19     | .NET Framework 4.8 |
| 3.0 – 3.6      | .NET 8             |
| 3.3.2          | .NET 10            |
| 3.7            | .NET 10            |
| 4.0+           | .NET 10            |

{% hint style="info" %}
3.3.2 and 3.7 are special releases with .NET 10 backported from the 4.0 release candidate.
{% endhint %}

For guidance on updating your packages to a new .NET version, see the migration guides in the Developer Primer:

* [Updating Packages for Dynamo 2.x](../11\_developer\_primer/3\_developing\_for\_dynamo/6-0-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Updating Packages for Dynamo 3.x / .NET 8](../11\_developer\_primer/3\_developing\_for\_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
* [Updating Packages for Dynamo 4.x / .NET 10](../11\_developer\_primer/3\_developing\_for\_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)
