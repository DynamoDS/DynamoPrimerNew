# Compatibilidade com o .NET

A tabela abaixo mostra qual versão do .NET é usada por cada série principal de versões do Dynamo. Isso é relevante ao criar pacotes ou extensões, pois o projeto deve ser direcionado para uma versão compatível do .NET.

| Versão do Dynamo | Versão do .NET       |
| -------------- | ------------------ |
| 0.9 – 2.0      | .NET Framework 4.5 |
| 2.1 – 2.6      | .NET Framework 4.7 |
| 2.7 – 2.19     | .NET Framework 4.8 |
| 3.0 – 3.6      | .NET 8             |
| 3.3.2          | .NET 10            |
| 3.7            | .NET 10            |
| 4.0 e superior           | .NET 10            |

{% hint style="info" %}
3.3.2 e 3.7 são versões especiais com o .NET 10 integrado retroativamente da versão candidata 4.0.
{% endhint %}

Para obter orientações sobre como atualizar os pacotes para uma nova versão do .NET, consulte os guias de migração no Manual do Desenvolvedor:

 * [Atualizar pacotes para o Dynamo 2.x](../11_developer_primer/8_updating_packages/1-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
 * [Atualizar pacotes para o Dynamo 3.x/.NET 8](../11_developer_primer/8_updating_packages/2-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
 * [Atualizar pacotes para o Dynamo 4.x/.NET 10](../11_developer_primer/8_updating_packages/3-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)