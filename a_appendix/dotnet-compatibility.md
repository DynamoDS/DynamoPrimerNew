# Совместимость с .NET

В приведенной ниже таблице показано, на какие версии .NET ориентированы основные серии выпусков Dynamo. Это нужно учитывать при сборке пакетов или расширений, так как проект должен поддерживать совместимую версию .NET.

| Версия Dynamo | Версия .NET       |
| -------------- | ------------------ |
| 0.9–2.0      | .NET Framework 4.5 |
| 2.1–2.6      | .NET Framework 4.7 |
| 2.7–2.19     | .NET Framework 4.8 |
| 3.0–3.6      | .NET 8             |
| 3.3.2          | .NET 10            |
| 3.7            | .NET 10            |
| 4.0 и более поздние версии           | .NET 10            |

{% hint style="info" %}Версии 3.3.2 и 3.7 являются специальными версиями, в которых поддержка .NET 10 была перенесена из версии-кандидата 4.0.{% endhint %}

Инструкции по обновлению .NET в пакетах до новой версии см. в руководстве по переносу для разработчиков:

* [Обновление пакетов для Dynamo 2.x](../11\_developer\_primer/3\_developing\_for\_dynamo/6-0-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Обновление пакетов для Dynamo 3.x/.NET 8](../11\_developer\_primer/3\_developing\_for\_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
* [Обновление пакетов для Dynamo 4.x/.NET 10](../11\_developer\_primer/3\_developing\_for\_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)
