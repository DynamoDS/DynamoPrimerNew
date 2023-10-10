# Python и Civil 3D

Dynamo — это чрезвычайно мощный инструмент [визуального программирования](../../a\_appendix/a-1\_visual-programming-and-dynamo.md), однако его можно использовать не только для работы с узлами и проводами, но и для написания кода в текстовом формате. Сделать это можно двумя способами.

1. Написать код **DesignScript** с помощью Code Block.
2. Написать код **Python** с помощью узла Python.

В этом разделе рассматривается использование Python в среде Civil 3D для работы с .NET API программ AutoCAD и Civil 3D.

{% hint style="info" %}\r\n Общие сведения об использовании Python в Dynamo см. в разделе [8-3_python](../../8\_coding\_in\_dynamo/8-3\_python/ "mention"). \r\n{% endhint %}

## Документация по API

В AutoCAD и Civil 3D есть несколько API-интерфейсов, которые позволяют разработчикам расширять базовые возможности этих программ за счет пользовательских функций. В отношении Dynamo для этого применяются **управляемые .NET API**. По ссылкам ниже можно ознакомиться со сведениями, необходимыми для понимания структуры API-интерфейсов и принципа их работы.

[Руководство разработчика .NET API AutoCAD](https://help.autodesk.com/view/OARX/2024/RUS/?guid=GUID-C3F3C736-40CF-44A0-9210-55F6A939B6F2) (англ.)

[Справочное руководство по .NET API AutoCAD](https://help.autodesk.com/view/OARX/2024/RUS/?guid=OARX-ManagedRefGuide-What_s_New) (англ.)

[Руководство разработчика .NET API Civil 3D](https://help.autodesk.com/view/CIV3D/2024/RUS/?guid=GUID-DA303320-B66D-4F4F-A4F4-9FBBEC0754E0) (англ.)

[Справочное руководство по .NET API Civil 3D](https://help.autodesk.com/view/CIV3D/2024/RUS/?guid=73fd1950-ee31-00b8-4872-c3f328ea1331) (англ.)

{% hint style="info" %}\r\n В этом разделе могут встречаться незнакомые вам понятия, такие как базы данных, транзакции, методы, свойства и т. д. Многие из этих понятий необходимо знать для работы с .NET API, и они не относятся исключительно к Dynamo или Python. Мы не будем подробно рассматривать эти понятия в данном руководстве и потому рекомендуем обратиться к приведенным выше ссылкам для получения дополнительной информации. \r\n{% endhint %}

## Шаблон кода

Когда вы впервые откроете новый узел Python для его редактирования, он будет по умолчанию заполнен шаблонным кодом. Ниже приводится описание данного шаблона с пояснениями по каждому блоку.

<figure><img src="../../.gitbook/assets/Python_Template (1).png" alt=""><figcaption><p>Шаблон Python по умолчанию в Civil 3D</p></figcaption></figure>

> 1. Импорт модулей `sys` и `clr`, необходимых для правильной работы интерпретатора Python. В частности, модуль `clr` позволяет обрабатывать пространства имен .NET как пакеты Python.
> 2. Загрузка стандартных сборок (например, файлов DLL) для работы с управляемыми .NET API для AutoCAD и Civil 3D.
> 3. Добавление ссылок на стандартные пространства имен AutoCAD и Civil 3D. Эквивалентно директивам `using` и `Imports` в C# и VB.NET, соответственно.
> 4. Доступ к входным портам узла осуществляется с помощью предварительно определенного списка `IN`. Для доступа к данным в определенном порте можно использовать его порядковый номер, например: `dataInFirstPort = IN[0]`.
> 5. Получение активных документа и редактора.
> 6. Блокировка документа и запуск транзакции базы данных.
> 7. Здесь размещается основная часть логики сценария.
> 8. Раскомментируйте эту строку, чтобы зафиксировать транзакцию после выполнения основного объема работы.
> 9. Если требуется вывести какие-либо данные из узла, назначьте их переменной `OUT` в конце сценария.

{% hint style="info" %}\r\n **Хотите адаптировать шаблон?**\
 Шаблон Python по умолчанию можно изменить, отредактировав файл `PythonTemplate.py`, расположенный в папке `C:\ProgramData\Autodesk\C3D <version>\Dynamo`. \r\n{% endhint %}

## Пример

Чтобы ознакомиться с некоторыми из основных понятий, относящихся к написанию сценариев Python в Dynamo for Civil 3D, рассмотрим пример.

### Цель

> :dart: Получение геометрии границ всех водосборов на чертеже.

### Набор данных

Ниже приведены файлы примеров, которые можно использовать в этом упражнении.

{% file src="../../.gitbook/assets/Python_Catchments.dyn" %}

{% file src="../../.gitbook/assets/Python_Catchments.dwg" %}

### Обзор решения

Ниже представлен обзор логики, используемой в этом графике.

> 1. Изучить документацию по API Civil 3D.
> 2. Выбрать все водосборы в документе по имени слоя.
> 3. «Развернуть» объекты Dynamo для доступа ко внутренним элементам API Civil 3D.
> 4. Создать точки Dynamo на основе точек AutoCAD.
> 5. Создать сложные кривые по точкам.

Приступим!

### Изучение документации по API

Прежде чем приступить к созданию графика и написанию кода, ознакомьтесь с документацией по API Civil 3D, чтобы получить представление о доступных возможностях. В данном случае нас интересует [свойство в классе Catchment](https://help.autodesk.com/view/CIV3D/2024/RUS/?guid=d3140831-672f-d9bb-3be7-9886a0e19f5c), которое возвращает точки контура водосбора. Обратите внимание, что это свойство возвращает объект `Point3dCollection`, но программа Dynamo не умеет обрабатывать такие объекты. Другими словами, мы не сможем создать сложную кривую на основе `Point3dCollection`, поэтому нам придется преобразовать все полученные данные в точки Dynamo. Позже мы рассмотрим этот процесс подробнее.

### Получение всех водосборов

Теперь можно приступить к выстраиванию логики графика. Сначала необходимо получить список всех водосборов в документе. Для решения этой задачи можно использовать узлы, поэтому мы не будем включать ее в сценарий Python. Использование узлов упрощает визуальное восприятие графика (не нужно пробираться через большой объем кода Python), а также позволяет посвятить сценарий Python выполнению одной задачи: получение точек контура водосборов.

<figure><img src="../../.gitbook/assets/Python_Get_Catchments.png" alt=""><figcaption><p>Получение всех водосборов в документе по слоям</p></figcaption></figure>

Обратите внимание, что выходные данные узла **All Objects on Layer** представляют собой список элементов CivilObjects. Это связано с тем, что в Dynamo for Civil 3D в данный момент нет узлов для работы с водосборами, и именно поэтому нам требуется получить доступ к API через Python.

### Развертка объектов

Прежде чем продолжить, кратко рассмотрим одну важную концепцию. В разделе [node-library.md](../node-library.md "mention") была рассмотрена взаимосвязь элементов Object и CivilObject. Если рассмотреть эту взаимосвязь чуть глубже, можно увидеть, что каждый элемент **Object** Dynamo представляет собой оболочку для элемента **Entity** AutoCAD. Аналогичным образом, элемент **CivilObject** Dynamo представляет собой оболочку для элемента **Entity** Civil 3D. Эту оболочку можно снять, «развернув» элемент Object путем доступа к его свойствам `InternalDBObject` или `InternalObjectId`.

<table data-full-width="false"><thead><tr><th width="377.3333333333333">Тип Dynamo</th><th width="373">Оболочка для</th></tr></thead><tbody><tr><td><strong>Object</strong><br>Autodesk.AutoCAD.DynamoNodes.Object</td><td><strong>Entity</strong><br>Autodesk.AutoCAD.DatabaseServices.Entity</td></tr><tr><td><strong>CivilObject</strong><br>Autodesk.Civil.DynamoNodes.CivilObject</td><td><strong>Entity</strong><br>Autodesk.Civil.DatabaseServices.Entity</td></tr></tbody></table>

{% hint style="warning" %}\r\n На практике в большинстве случаев надежнее сначала получить идентификатор объекта с помощью свойства `InternalObjectId`, а затем получить доступ к объекту-оболочке в транзакции. Причина в том, что свойство `InternalDBObject` возвращает элемент DBObject AutoCAD, недоступный для записи. \r\n{% endhint %}

### Сценарий Python

Вот полный сценарий Python, который обращается к внутренним объектам водосбора и получает их точки контура. В сценарии выделены строки, содержащие измененный шаблонный код или новый пользовательский код.

{% hint style="info" %}\r\n Для просмотра пояснений щелкните подчеркнутые строки сценария. \r\n{% endhint %}

<pre class="language-python" data-line-numbers><code class="lang-python"># Загрузка стандартной библиотеки Python и библиотеки DesignScript
import sys
import clr

# Добавление сборок для AutoCAD и Civil 3D
clr.AddReference('AcMgd')
clr.AddReference('AcCoreMgd')
clr.AddReference('AcDbMgd')
clr.AddReference('AecBaseMgd')
clr.AddReference('AecPropDataMgd')
clr.AddReference('AeccDbMgd')

<strong><a data-footnote-ref href="#user-content-fn-1">clr.AddReference('ProtoGeometry')</a>
</strong>
# Импорт ссылок из AutoCAD
from Autodesk.AutoCAD.Runtime import *
from Autodesk.AutoCAD.ApplicationServices import *
from Autodesk.AutoCAD.EditorInput import *
from Autodesk.AutoCAD.DatabaseServices import *
from Autodesk.AutoCAD.Geometry import *

# Импорт ссылок из Civil 3D
from Autodesk.Civil.ApplicationServices import *
from Autodesk.Civil.DatabaseServices import *

<strong><a data-footnote-ref href="#user-content-fn-2">from Autodesk.DesignScript.Geometry import Point as DynPoint</a>
</strong>
# Входные данные для этого узла будут храниться в виде списка в переменных IN.
<strong><a data-footnote-ref href="#user-content-fn-3">objs</a> = <a data-footnote-ref href="#user-content-fn-4">IN[0]</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-5">output = []</a> 
</strong>
<strong><a data-footnote-ref href="#user-content-fn-6">if objs is None:</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-7">sys.exit("The input is null or empty.")</a>
</strong>
<strong><a data-footnote-ref href="#user-content-fn-8">if not isinstance(objs, list):</a>
</strong><strong>    <a data-footnote-ref href="#user-content-fn-9">objs = [objs]</a>
</strong>    
adoc = Application.DocumentManager.MdiActiveDocument
editor = adoc.Editor

with adoc.LockDocument():
    with adoc.Database as db:
        
        with db.TransactionManager.StartTransaction() as t:
<strong>            <a data-footnote-ref href="#user-content-fn-10">for obj in objs:</a>              
</strong><strong>                <a data-footnote-ref href="#user-content-fn-11">id = obj.InternalObjectId</a>
</strong><strong>                <a data-footnote-ref href="#user-content-fn-12">aeccObj = t.GetObject(id, OpenMode.ForRead)</a>                
</strong><strong>                <a data-footnote-ref href="#user-content-fn-13">if isinstance(aeccObj, Catchment):</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-14">catchment = aeccObj</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-15">acPnts = catchment.BoundaryPolyline3d</a>                    
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-16">dynPnts = []</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-17">for acPnt in acPnts:</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-18">pnt = DynPoint.ByCoordinates(acPnt.X, acPnt.Y, acPnt.Z)</a>
</strong><strong>                        <a data-footnote-ref href="#user-content-fn-19">dynPnts.append(pnt)</a>
</strong><strong>                    <a data-footnote-ref href="#user-content-fn-20">output.append(dynPnts)</a>
</strong>            
            # Фиксация перед завершением транзакции
<strong>            <a data-footnote-ref href="#user-content-fn-21">t.Commit()</a>
</strong>            pass
            
# Назначение выходных данных переменной OUT
<strong><a data-footnote-ref href="#user-content-fn-22">OUT = output</a>
</strong></code></pre>

{% hint style="warning" %}\r\n На практике основной объем логики сценария рекомендуется размещать внутри транзакции. Это обеспечивает безопасный доступ к объектам, которые сценарий считывает или записывает. Если пропустить транзакцию, во многих случаях это может привести к неустранимой ошибке. \r\n{% endhint %}

### Создание сложных кривых

На этом этапе сценарий Python должен вывести список точек Dynamo, который можно увидеть в фоновом просмотре. Нам осталось просто создать сложных кривые на основе этих точек. Обратите внимание, что сделать это можно напрямую в сценарии Python, однако мы намеренно размещаем эту операцию за пределами сценария, в узле, чтобы сделать ее более заметной. Вот как выглядит итоговый график.

<figure><img src="../../.gitbook/assets/Python_Final_Script (1).png" alt=""><figcaption><p>Итоговый график</p></figcaption></figure>

### Результат

А вот итоговая геометрия Dynamo.

<figure><img src="../../.gitbook/assets/Python_Dynamo_Curves.png" alt=""><figcaption><p>Полученные сложные кривые Dynamo для границ водосборов</p></figcaption></figure>

> :tada: Миссия выполнена!

## IronPython и CPython

Небольшой комментарий перед завершением данного раздела. Настройка узла Python может выполняться по-разному в зависимости от используемой версии Civil 3D. В **Civil 3D 2020 и 2021** в Dynamo использовался инструмент **IronPython**, который позволял переносить данные между объектами .NET и сценариями Python. Однако в **Civil 3D 2022** в Dynamo применяется стандартный встроенный интерпретатор Python (**CPython**), в котором используется Python 3. Переход на этот вариант обеспечивает ряд преимуществ, в том числе доступ к популярным современным библиотекам и новым возможностям платформы, а также установку обновлений и исправлений для системы безопасности.

{% hint style="info" %}\r\n Подробные сведения об этом переходе и обновлении сценариев предыдущих версий можно найти в [блоге Dynamo](https://dynamobim.org/why-has-dynamo-switched-to-python-3-should-i-update-too/). Если вы хотите и дальше работать с IronPython, установите пакет **DynamoIronPython2.7** с помощью Dynamo Package Manager. \r\n{% endhint %}

[^1]. По умолчанию библиотека геометрии Dynamo не добавляется в среду Python. Мы создаем этот сценарий, чтобы с его помощью вывести список точек Dynamo для границ водосбора, поэтому необходимо добавить эту строку, чтобы создать эти точки позднее.

[^2]. Эта строка позволяет нам получить определенный класс из библиотеки геометрии Dynamo. Обратите внимание, что здесь мы указываем `import Point as DynPoint`, а не `import *`. Это нужно, чтобы избежать конфликтов имен.

[^3]. Здесь мы переименовали переменную по умолчанию `dataEnteringNode` в `objs`, чтобы ее назначение было более очевидным.

[^4]. Здесь мы указали, какой именно порт ввода содержит нужные данные, вместо того чтобы использовать значение по умолчанию `IN`, которое ссылается на список всех портов ввода.

[^5]. Эта строка создает пустой список, в который мы позже добавим выходные данные.

[^6]. Эта строка добавлена на случай, если выходные данные окажутся пустыми или нулевыми.

[^7]. Вместо того чтобы просто прервать сценарий, мы получим информативное сообщение с объяснением причин, по которым продолжить выполнение сценария невозможно.

[^8]. Начиная с этого места в сценарии предполагается, что у нас есть список из нескольких объектов, а не один объект. Однако мы хотим, чтобы сценарий выполнялся даже при наличии только одного объекта (например, если в чертеже есть только один водосбор).

[^9]. Если в качестве входных данных используется не список, а одиночный объект, будет создан новый список, включающий только этот объект.

[^10]. Запускаем цикл для каждого объекта в списке.

[^11]. «Разворачиваем» объект Dynamo путем получения соответствующего Object ID.

[^12]. Извлекаем «завернутый» объект из базы данных AutoCAD. Обратите внимание, что для OpenMode установлено значение `ForRead`, так как мы не планируем вносить изменения в объекты. Мы просто «запрашиваем» данные.

[^13]. Входной список объектов может содержать не только водосборы, но и другие элементы. Необходимо проверить, так ли это, и принять соответствующие меры (итерация цикла продолжается только в том случае, если элемент действительно является водосбором).

[^14]. Если мы добрались до этой строки, значит, объект является водосбором. Добавим новую переменную, чтобы обеспечить понятное именование.

[^15]. Здесь мы получаем точки контура водосбора с помощью соответствующего свойства, которое мы нашли ранее в документации по API. Как уже упоминалось, в результате мы получим объект `Point3dCollection`, который по сути является списком точек AutoCAD. Чтобы использовать эти точки, нам потребуется преобразовать их в точки Dynamo.

[^16]. Создаем пустой список для хранения точек контура данного водосбора.

[^17]. Запускаем цикл для каждого объекта `Point3d` в `Point3dCollection`.

[^18]. Создаем точку Dynamo на основе координат точки AutoCAD.

[^19]. Добавляем точку в список.

[^20]. После преобразования всех точек AutoCAD в точки Dynamo добавляем полученный список к выходным данным. После этого внешний цикл начнет обработку следующего объекта в списке входных данных.

[^21]. Раскомментируем эту строку, чтобы зафиксировать транзакцию.

[^22]. Наконец, выводим список точек Dynamo.
