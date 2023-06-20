# Дальнейшая работа с Zero-Touch 

Понимая, как создать проект Zero-Touch, мы можем подробно рассмотреть особенности создания узла, изучив пример ZeroTouchEssentials на Dynamo Github.

![Узлы Zero-Touch](images/ootbzerotouch.png)

> Многие стандартные узлы Dynamo, по сути, являются узлами Zero-Touch, как и большинство узлов Math, Color и DateTime.

Для начала скачайте проект ZeroTouchEssentials на странице [https://github.com/DynamoDS/ZeroTouchEssentials](https://github.com/DynamoDS/ZeroTouchEssentials)

В Visual Studio откройте файл решения `ZeroTouchEssentials.sln` и выполните сборку.

![ZeroTouchEssentials в Visual Studio](images/vs-build-zte.jpg)

> Файл `ZeroTouchEssentials.cs` содержит все методы, которые будут импортированы в Dynamo.

Откройте Dynamo и импортируйте `ZeroTouchEssentials.dll`, чтобы получить узлы, на которые мы будем ссылаться в следующих примерах.

Примеры кода извлекаются из [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/master/ZeroTouchEssentials/ZeroTouchEssentials.cs). XML-документация удалена для краткости. В каждом примере с помощью кода будет создан узел, приведенный на изображении над ним.

#### Входные значения по умолчанию <a href="#default-input-values" id="default-input-values"></a>

Dynamo поддерживает определение значений по умолчанию для входных портов в узле. Значения по умолчанию передаются в узел, если у портов нет соединений. Значения по умолчанию выражаются с помощью механизма указания дополнительных аргументов на C# в соответствии с материалами [Руководства по программированию на C#](https://msdn.microsoft.com/en-us/library/dd264739.aspx). По умолчанию задаются следующим образом.

* Задайте для параметров метода значение по умолчанию: `inputNumber = 2.0`

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // Set the method parameter to a default value
        public static double MultiplyByTwo(double inputNumber = 2.0) 
        {
            return inputNumber * 2.0;
        }
    }
}
```

![Значение по умолчанию](images/defaultval.jpg)

> 1. Значение по умолчанию отображается при наведении курсора на входной порт узла.

#### Возврат нескольких значений <a href="#returning-multiple-values" id="returning-multiple-values"></a>

Возврат нескольких значений — несколько более сложная задача, чем создание нескольких наборов входных данных. В этом случае данные возвращаются с использованием словаря. Записи словаря становятся портами на выходной стороне узла. Порты с возвратом нескольких значений создаются следующим образом.

* Добавьте `using System.Collections.Generic;` для использования `Dictionary<>`.
* Добавьте `using Autodesk.DesignScript.Runtime;` для использования атрибута `MultiReturn`. В данном случае ссылка на DynamoServices.dll содержится в пакете NuGet DynamoServices.
* Добавьте к методу атрибут `[MultiReturn(new[] { "string1", "string2", ... more strings here })]`. Строки ссылаются на ключи в словаре и становятся именами выходных портов.
* Выполните возврат `Dictionary<>` из функции с ключами, совпадающими с именами параметров в атрибуте: `return new Dictionary<string, object>`.

```
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        [MultiReturn(new[] { "add", "mult" })]
        public static Dictionary<string, object> ReturnMultiExample(double a, double b)
        {
            return new Dictionary<string, object>

                { "add", (a + b) },
                { "mult", (a * b) }
            };
        }
    }
}
```

> См. этот пример кода в [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L70)

Узел, возвращающий несколько выходных данных.

![Несколько выходных данных](images/multipleoutputs.png)

> 1. Обратите внимание, что теперь существует два выходных порта, названных в соответствии с указанными ключами словаря.

#### Документация, подсказки и поиск <a href="#documentation-tooltips-and-search" id="documentation-tooltips-and-search"></a>

Рекомендуется прикреплять к узлам Dynamo документацию, описывающую функции узла, входные и выходные данные, теги поиска и т. д. Для этого используются теги XML-документации. XML-документация создается следующим образом.

* Документацией считается любой текст комментария после значка «///» (три наклонные черты), 
  * Например: `/// Documentation text and XML goes here` (Текст документации и XML размещается здесь)
* После трех наклонных черт создайте теги XML над методами, которые Dynamo будет считывать при импорте DLL-файла.
  * Например: `/// <summary>...</summary>`
* Включите XML-документацию в Visual Studio, выбрав `Project > Project Properties > Build` (Проект > Свойства проекта > Сборка) и поставив флажок `XML documentation file` (Файл XML-документации).

![Создание XML-файла](images/vs-xml.jpg)

> 1. Visual Studio создаст XML-файл в указанной папке.

Типы тегов:

* `/// <summary>...</summary>` обозначает основную документацию для узла, которая отображается в виде подсказки над узлом в строке поиска слева.
* `/// <param name="inputName">...</param>` создает документацию для конкретных входных параметров.
* `/// <returns>...</returns>` создает документацию для выходного параметра.
* `/// <returns name = "outputName">...</returns>` создает документацию для нескольких выходных параметров.
* `/// <search>...</search>` сопоставляет узел с результатами поиска на основе списка, разделенного запятыми. Например, если создать узел, который разделяет сетку, можно добавить такие теги, как mesh, subdivision и catmull-clark.

Ниже приведен пример узла с описаниями входных и выходных данных, а также сводкой, которая будет отображаться в библиотеке.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        /// <summary>
        /// This method demonstrates how to use a native geometry object from Dynamo
        /// in a custom method
        /// </summary>
        /// <param name="curve">Input Curve. This can be of any type deriving from Curve, such as NurbsCurve, Arc, Circle, etc</param>
        /// <returns>The twice the length of the Curve </returns>
        /// <search>example,curve</search>
        public static double DoubleLength(Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> См. пример кода в [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L80)

Обратите внимание на содержимое этого примера узла.

> 1. Сводка узла.
> 2. Описание входных данных.
> 3. Описание выходных данных.

#### Объекты <a href="#objects" id="objects"></a>

В Dynamo отсутствует ключевое слово `new`, поэтому объекты создаются с использованием статических методов конструирования. Объекты создаются следующим образом.

* Сделайте конструктор внутренним `internal ZeroTouchEssentials()`, если не требуется иное.
* Постройте объект с помощью статического метода, например `public static ZeroTouchEssentials ByTwoDoubles(a, b)`.

> Примечание. В Dynamo используется префикс «By», указывающий на то, что статический метод является конструктором. Использовать этот префикс необязательно, но с ним ваша библиотека будет соответствовать общепринятому стилю Dynamo.

```
namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        private double _a;
        private double _b;

        // Make the constructor internal
        internal ZeroTouchEssentials(double a, double b)
        {
            _a = a;
            _b = b;
        }

        // The static method that Dynamo will convert into a Create node
        public static ZeroTouchEssentials ByTwoDoubles(double a, double b)
        {
            return new ZeroTouchEssentials(a, b);
        }
    }
}
```

> См. пример кода в [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L26)

После импорта DLL-файла ZeroTouchEssentials в библиотеке появится узел ZeroTouchEssentials. Этот объект можно создать с помощью узла `ByTwoDoubles`.

![Узел ByTwoDoubles](images/dyn-constructor.jpg)

#### Использование типов геометрии Dynamo <a href="#using-dynamo-geometry-types" id="using-dynamo-geometry-types"></a>

В библиотеках Dynamo можно использовать собственные типы геометрии Dynamo в качестве входных данных и создавать новую геометрию в качестве выходных данных. Типы геометрии создаются следующим образом.

* Вставьте ссылку на файл ProtoGeometry.dll в проект, добавив `using Autodesk.DesignScript.Geometry;` в верхней части файла C# и добавив пакет NuGet библиотеки ZeroTouchLibrary в проект.
* **Важно.** Дополнительную информацию об управлении ресурсами геометрии, возврат которых не выполняется из функций, см. в приведенном ниже разделе **Удаление/использование операторов**.

> Примечание. Геометрические объекты Dynamo используются так же, как и любые другие объекты, передаваемые функциям.

```
using Autodesk.DesignScript.Geometry;

namespace ZeroTouchEssentials
{
    public class ZeroTouchEssentials
    {
        // "Autodesk.DesignScript.Geometry.Curve" is specifying the type of geometry input, 
        // just as you would specify a double, string, or integer 
        public static double DoubleLength(Autodesk.DesignScript.Geometry.Curve curve)
        {
            return curve.Length * 2.0;
        }
    }
}
```

> См. пример кода в [ZeroTouchEssentials.cs](https://github.com/DynamoDS/ZeroTouchEssentials/blob/9917fd8159afc9e7bdb2944c960155a496e0b2dc/ZeroTouchEssentials/ZeroTouchEssentials.cs#L86)

Узел, который получает длину кривой и удваивает ее.

![Входные данные кривой](images/doublelength.png)

> 1. В качестве входных данных для этого узла можно использовать геометрию кривой.

#### Удаление/использование операторов <a href="#disposeusing-statements" id="disposeusing-statements"></a>

Если вы не используете Dynamo версии 2.5 или более поздней, управлять геометрическими ресурсами, которые не возвращены из функций, необходимо вручную. В Dynamo 2.5 и более поздних версий геометрические ресурсы обрабатываются системой, однако в сложных сценариях и для сокращения объема используемой памяти вы можете удалять геометрию вручную. Движок Dynamo обрабатывает все геометрические ресурсы, которые возвращаются из функций. Невозвращенные геометрические ресурсы можно обработать вручную следующими способами.

*   С помощью оператора using:

    ```
    using (Point p1 = Point.ByCoordinates(0, 0, 0))
    {
      using (Point p2 = Point.ByCoordinates(10, 10, 0))
      {
          return Line.ByStartPointEndPoint(p1, p2);
      }
    }
    ```

    > [Документация](https://msdn.microsoft.com/en-us/library/yh598w02.aspx) об использовании оператора using
    >
    > Дополнительные сведения о новых функциях обеспечения стабильности, появившихся в Dynamo 2.5, см. в разделе [Повышение стабильности геометрии в Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297)
*   При выполнении вызовов Dispose вручную:

    ```
    Point p1 = Point.ByCoordinates(0, 0, 0);
    Point p2 = Point.ByCoordinates(10, 10, 0);
    Line l = Line.ByStartPointEndPoint(p1, p2);
    p1.Dispose();
    p2.Dispose();
    return l;
    ```

#### Миграция <a href="#migrations" id="migrations"></a>

При публикации более поздней версии библиотеки имена узлов могут измениться. Изменения имени можно указать в файле миграции, чтобы графики, построенные на основе предыдущих версий библиотеки, продолжали работать правильно при обновлении. Миграция выполняется следующим образом.

* Создайте файл `.xml` в той же папке, что и файл `.dll`, в следующем формате: ИмяФайлаDLL.Migrations.xml.
* В диалоговом окне `.xml` создайте один элемент `<migrations>...</migrations>`.
* Внутри элемента миграции создайте элементы `<priorNameHint>...</priorNameHint>` для каждого изменения имени.
* При каждом изменении имени укажите `<oldName>...</oldName>` и `<newName>...</newName>`

![Файл миграции](images/vs-migrations-file.jpg)

> 1. Щелкните правой кнопкой мыши и выберите `Add > New Item` (Добавить > Новый элемент).
> 2. Выберите `XML File` (XML-файл).
> 3. В нашем проекте мы присвоим файлу миграции имя `ZeroTouchEssentials.Migrations.xml`.

Этот код указывает Dynamo, что узлы с именем `GetClosestPoint` теперь называются `ClosestPointTo`.

```
<?xml version="1.0"?>
<migrations>
  <priorNameHint>
    <oldName>Autodesk.DesignScript.Geometry.Geometry.GetClosestPoint</oldName>
    <newName>Autodesk.DesignScript.Geometry.Geometry.ClosestPointTo</newName>
  </priorNameHint>
</migrations>
```

> См. пример кода в файле [ProtoGeometry.Migrations.xml](https://github.com/DynamoDS/Dynamo/blob/master/extern/ProtoGeometry/ProtoGeometry.Migrations.xml)

#### Универсальные типы <a href="#generics" id="generics"></a>

Zero-Touch в настоящее время не поддерживает использование универсальных типов. Их можно использовать, но не в коде, который импортируется напрямую, если тип не задан. Универсальные методы, свойства и классы, у которых нет типа, нельзя предоставить.

В примере ниже узел Zero-Touch типа `T` не будет импортирован. Если остальные компоненты библиотеки импортируются в Dynamo, возникнут исключения отсутствующих типов.

```
public class SomeGenericClass<T>
{
    public SomeGenericClass()
    {
        Console.WriteLine(typeof(T).ToString());
    }  
}
```

При использовании универсального типа с типом, заданным в этом примере, импорт в Dynamo будет выполнен.

```
public class SomeWrapper
{
    public object wrapped;
    public SomeWrapper(SomeGenericClass<double> someConstrainedType)
    {
        Console.WriteLine(this.wrapped.GetType().ToString());
    }
}
```
