# Выполнение сценариев Python в узлах Zero-Touch (C#)

### Выполнение сценариев Python в узлах Zero-Touch (C#) <a href="#executing-python-scripts-in-zero-touch-nodes-c" id="executing-python-scripts-in-zero-touch-nodes-c"></a>

Если вы умеете писать сценарии на языке Python и хотите получить расширенные возможности при использовании стандартных узлов Dynamo Python, создайте собственный узел с помощью технологии Zero-Touch. Начнем с простого примера, в котором мы передадим сценарий Python в виде строки в узел Zero-Touch, где сценарий выполнится и вернет результат. В этом практическом примере мы опираемся на пошаговые инструкции и примеры из раздела «Начало работы». Если вы еще не работали с узлами Zero-Touch, начните с этого раздела.

![Узел Zero-Touch, выполняющий строку сценария Python](../../.gitbook/assets/python-case-study.png)

> Узел Zero-Touch, выполняющий строку сценария Python

#### Обработчик Python <a href="#python-engine" id="python-engine"></a>

PythonNet3 теперь является обработчиком по умолчанию и по сравнению с CPython работает более плавно. Все новые узлы Python, создаваемые в Dynamo 4.0 или более поздней версии, по умолчанию используют PythonNet3.

Этот узел основан на экземпляре обработчика сценариев IronPython. Нам нужно создать ссылки на несколько дополнительных сборок. Чтобы настроить базовый шаблон в Visual Studio, выполните следующие действия:

* Создайте новый проект класса Visual Studio.
* Добавьте ссылку на файл `IronPython.dll`, расположенный в папке `C:\Program Files (x86)\IronPython 2.7\IronPython.dll`.
* Добавьте ссылку на файл `Microsoft.Scripting.dll`, расположенный в папке `C:\Program Files (x86)\IronPython 2.7\Platforms\Net40\Microsoft.Scripting.dll`.
* Включите в класс операторы `IronPython.Hosting` и `Microsoft.Scripting.Hosting` `using`.
* Добавьте частный пустой конструктор, чтобы предотвратить добавление дополнительного узла в библиотеку Dynamo с пакетом.
* Создайте новый метод, при котором в качестве входного параметра используется одна строка.
* В рамках этого метода создается экземпляр нового обработчика Python и пустая область сценария. Вы можете представить эту область как глобальные переменные в экземпляре интерпретатора Python.
* Затем вызовите `Execute` в обработчике, передав входную строку и область в качестве параметров.
* Наконец, извлеките и верните результаты сценария. Для этого вызовите `GetVariable` в области и передайте имя переменной из сценария Python, содержащего возвращаемое значение. (См. пример ниже.)

Следующий код представляет пример для описанного выше шага. При сборке решения будет создан новый файл `.dll`, расположенный в папке bin нашего проекта. Теперь `.dll` можно импортировать в Dynamo в составе пакета или выбрав `File < Import Library...` (Файл > Импортировать библиотеку).

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

namespace PythonLibrary
{
    public class PythonZT
    {
        // Unless a constructor is provided, Dynamo will automatically create one and add it to the library
        // To avoid this, create a private constructor
        private PythonZT() { }

        // The method that executes the Python string
        public static string executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of the 'output' variable from the Python script below
            var output = scope.GetVariable("output");
            return (output);
        }
    }
}
```

Сценарий Python возвращает переменную `output`, значит в сценарии Python требуется переменная `output` . Используйте этот пример сценария для тестирования узла в Dynamo. Если вы уже использовали узел Python в Dynamo, то вам будет знаком следующий фрагмент кода. Дополнительную информацию смотрите в разделе руководства, посвященном [Python](https://primer2.dynamobim.org/8_coding_in_dynamo/8-3_python/1-python).

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
volume = cube.Volume
output = str(volume)
```

#### Несколько выходных данных <a href="#multiple-outputs" id="multiple-outputs"></a>

Одно из ограничений стандартных узлов Python заключается в том, что они имеют только один порт вывода, поэтому, если мы хотим вернуть несколько объектов, мы должны создать список и извлечь каждый объект. Если мы изменим приведенный выше пример для возврата словаря, мы можем добавить любое количество портов вывода. Более подробные сведения о словарях см. в разделе «Возврат нескольких значений» документа «Дальнейшая работа с Zero-Touch».

![Этот узел позволяет возвращать как объем кубоида, так и его центр тяжести.](../../.gitbook/assets/python-multi-case-study.png)

> Этот узел позволяет возвращать как объем кубоида, так и его центр тяжести.

Изменим предыдущий пример, выполнив следующие действия:

* Добавьте ссылку на `DynamoServices.dll` из диспетчера пакетов NuGet.
* В дополнение к предыдущим сборкам включите `System.Collections.Generic` и `Autodesk.DesignScript.Runtime`.
* Измените тип возвращаемого значения метода, чтобы он возвращал словарь с выходными данными.
* Все выходные данные должны извлекаться из области по отдельности (рекомендуется создать простой цикл для больших наборов выходных данных)

```
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;
using System.Collections.Generic;
using Autodesk.DesignScript.Runtime;

namespace PythonLibrary
{
    public class PythonZT
    {
        private PythonZT() { }

        [MultiReturn(new[] { "output1", "output2" })]
        public static Dictionary<string, object> executePyString(string pyString)
        {
            ScriptEngine engine = Python.CreateEngine();
            ScriptScope scope = engine.CreateScope();
            engine.Execute(pyString, scope);
            // Return the value of 'output1' from script
            var output1 = scope.GetVariable("output1");
            // Return the value of 'output2' from script
            var output2 = scope.GetVariable("output2");
            // Define the names of outputs and the objects to return
            return new Dictionary<string, object> {
                { "output1", (output1) },
                { "output2", (output2) }
            };
        }
    }
}
```

В пример сценария Python мы добавили дополнительную выходную переменную (`output2`). Следует иметь в виду, что для таких переменных можно использовать любые соглашения об именовании Python. В данном примере название «output» используется только для наглядности.

```
import clr
clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import *

cube = Cuboid.ByLengths(10,10,10);
centroid = Cuboid.Centroid(cube);
volume = cube.Volume
output1 = str(volume)
output2 = str(centroid)
```

#### Известные ограничения PythonNet3 и временные решения<a href="#pythonnet3-known-Issues-Workarounds" id="pythonnet3-known-Issues-Workarounds"></a>

Ниже приведены некоторые известные ограничения PythonNet3 и временные решения связанных с этим проблем.

* Коллекции .NET не преобразуются автоматически в списки Python
    * Необходимо явно преобразовать массивы или коллекции ```.NET``` с помощью ```list(...)``` перед использованием ```len()```, индексации или итерации.
* Для универсальных методов .NET могут требоваться явные параметры типа
    * Некоторые методы (например, ```GroupBy```) не будут работать, если не указать универсальные типы вручную, автоматическое определение не подходит.
* Методы расширения невозможно обнаружить с помощью ```dir()``` или автодополнения
    * Методы расширения могут по-прежнему работать при явном вызове, но они не появляются при интроспекции или дополнении кода.
* Методы расширения DataTable не поддерживаются
    * Происходит сбой импорта ```System.Data.DataTableExtensions```. Эти вспомогательные методы нельзя использовать напрямую.
* Некоторые основные методы Dynamo ведут себя по-другому в PythonNet3
    * Некоторые функции (например, преобразование списка в плоскую структуру) могут работать неправильно из-за более строгих правил обработки коллекции.
* Классы Python не могут передаваться между узлами, если они наследуют типы .NET
    * Классы, производные от интерфейсов или типов .NET, невозможно безопасно перенести между узлами Python.
* Python ```set()``` не поддерживает некоторые объекты .NET
    * Такие объекты, как ```InvalidElementId```, необходимо отфильтровывать или обрабатывать с помощью коллекций .NET.
* Частые вызовы ```print()``` могут привести к росту потребления памяти
    * Избегайте чрезмерного использования ```print()``` в циклах или долгоработающих сценариях.
* Возможности взаимозаменяемости словарей Dynamo и Python ограничены
    * Словари Dynamo и Python не являются полностью взаимозаменяемыми, поэтому может потребоваться преобразование вручную.
* Стал недоступен метод ```Marshal.GetActiveObject()``` для получения запущенного экземпляра COM указанного объекта
    * Используйте ```BindToMoniker```, если известен путь к используемому файлу.
    * Написание библиотеки на C# с использованием структуры классов ```Marshal.GetActiveObject()```

#### Перенос с CPython3 на PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Dynamo автоматически перенесет узлы с CPython на PythonNet3. Вот что произойдет.

> 1. Будет автоматически создана резервная копия исходного файла.
> 2. Все узлы CPython (включая пользовательские узлы, использующие CPython) будут преобразованы в PythonNet3. 
> 3. Откроется всплывающее уведомление с указанием количества перенесенных узлов.
> 4. При сохранении появится напоминание о том, что в узлах Python теперь будет использоваться PythonNet3. Не беспокойтесь об обратной совместимости. Тем, кто работает на предприятиях с несколькими версиями (например, Revit или Civil 3D 2025/2026), следует установить пакет PythonNet3 Engine в Dynamo 3.3–3.6 для обеспечения совместимости. 

#### Перенос с IronPython2 на PythonNet3<a href="#migrating-from-cpython-pythonnet3" id="migrating-from-cpython-pythonnet3"></a>

Если в графе используется обработчик IronPython, автоматический перенос не предусмотрен. 

Если соответствующий пакет IronPython установлен, граф выполняется нормально. Если он отсутствует, в расширении «Ссылки рабочего пространства» появится предупреждение о зависимости с просьбой скачать пакет. Можно продолжить работу с IronPython, переустановив пакет. Но поскольку пакет IronPython не обновлялся в течение многих лет и эти обработчики не поддерживались активно в Dynamo в течение долгого времени, мы настоятельно рекомендуем перейти на PythonNet3, чтобы обеспечить надежную работу графов в будущем. Хотя DynamoIronPython 2.7 и DynamoIronPython 3 по-прежнему будут доступны в виде пакетов в Dynamo Package Manager, команда Dynamo больше не будет их обслуживать. 

В этом случае можно перенести по одному узлу за раз с помощью Migration Assistant в редакторе Python.  

Дополнительную информацию о переносе см. в этой [публикации блога](https://dynamobim.org/dynamo-pythonnet3-upgrade-a-practical-guide-to-migrating-your-dynamo-graphs/).