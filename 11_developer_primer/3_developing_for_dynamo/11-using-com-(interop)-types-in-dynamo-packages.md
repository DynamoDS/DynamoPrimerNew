# Использование типов COM (взаимодействие) в пакетах Dynamo

## Общие сведения о типах взаимодействия
Какие типы моделей COM существуют? См. страницу [https://learn.microsoft.com/ru-ru/windows/win32/com/the-component-object-model](https://learn.microsoft.com/ru-ru/windows/win32/com/the-component-object-model ).

Стандартный способ использования типов COM в C# заключается в ссылке на первичные сборки взаимодействия (по сути, коллекция c большим количеством API-интерфейсов), поставляемые вместе с пакетом. 

В качестве альтернативы можно внедрить основные сборки взаимодействия (PIA) в управляемую сборку. В основном сюда входят только те типы и элементы, которые фактически используются управляемой сборкой. Однако у этого подхода есть недостатки, например проблемы с эквивалентностью типов.

Эта проблема подробно описана в следующей статье: 
* [https://learn.microsoft.com/ru-ru/dotnet/framework/interop/type-equivalence-and-embedded-interop-types](https://learn.microsoft.com/ru-ru/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)

## Управление эквивалентностью типов в Dynamo
Dynamo делегирует эквивалентность типов среде выполнения .NET. Например, два типа с одинаковым именем и пространством имен, но в разных сборках, не будут считаться эквивалентными, и при загрузке конфликтующих сборок в Dynamo выдается ошибка. Что касается типов взаимодействия, в Dynamo выполняется проверка их эквивалентности с помощью [API IsEquivalentTo](https://learn.microsoft.com/ru-ru/dotnet/api/system.type.isequivalentto)

## Недопущение конфликтов между встроенными типами взаимодействий
Некоторые пакеты уже были созданы со встроенными типами взаимодействия (например, CivilConnection). Загрузка двух пакетов со встроенными сборками взаимодействия, которые не считаются эквивалентными (разные версии, как определено [здесь](https://learn.microsoft.com/ru-ru/dotnet/framework/interop/type-equivalence-and-embedded-interop-types)), приведет к ошибке загрузки пакета `package load error`. В связи с этим мы предлагаем разработчикам пакетов использовать динамическое связывание (также называемое отложенным связыванием) для типов взаимодействия (решение также описано [здесь](https://blogs.iis.net/samng/the-pain-of-deploying-primary-interop-assemblies)).

Можно последовать этому примеру:
```
public class Application
    {
        string m_sProdID = "SomeNamespace.Application";
        public dynamic m_oApp = null; // Use dynamic so you do not need the PIA types

        public Application()
        {
            this.GetApplication();
        }

        internal void GetApplication()
        {
            try

            {
                m_oApp = System.Runtime.InteropServices.Marshal.GetActiveObject(m_sProdID);
            }
            catch (Exception /*ex*/)
            {}
        }
    }
}
```