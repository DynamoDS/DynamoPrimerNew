# Интерфейс командной строки Dynamo

***
      -o, -O, --OpenFilePath        Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox
***
      -c, -C, --CommandFilePath     Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox                      
***
      -v, -V, --Verbose             Instruct Dynamo to output all evaluations it performs to an XML file at this path
***                                        
      -g, -G, --Geometry            Instruct Dynamo to output geometry from all evaluations to a JSON file at this path
***
      -h, -H, --help                Get some help
***
      -i, -I, --Import              Instruct Dynamo to import an assembly as a node library. This argument should be a 
                                    file path to a single.dll - if you wish to import multiple dlls - list the dlls 
                                    separated by a space: -i 'assembly1.dll' 'assembly2.dll'
***
      --GeometryPath                Relative or absolute path to a directory containing ASM. When supplied, instead of 
                                    searching the hard disk for ASM, it will be loaded directly from this path
***
      -k, -K, --KeepAlive           Keepalive mode, leave the Dynamo process running until a loaded extension shuts it 
                                    down
***
      --HostName                    Identify Dynamo variation associated with the host
***
      -s, -S, --SessionId           Identify Dynamo host analytics session id
***
      -p, -P, --ParentId            Identify Dynamo host analytics parent id
***
      -x, -X, --ConvertFile         When used in combination with the 'O' flag, opens a .dyn file from the specified 
                                    path and converts it to .json. The file will have the .json extension and be 
                                    located in the same directory as the original file
***
      -n, -N, --NoConsole           Don't rely on the console window to interact with CLI in Keepalive mode
***
      -u, -U  --UserData            Specify user data folder to be used by PathResolver with CLI
***
      --CommonData                  Specify common data folder to be used by PathResolver with CLI
***
      --DisableAnalytics            Disables analytics in Dynamo for the process lifetime
***
      --CERLocation                 Specify the crash error report tool located on the disk
***
      --ServiceMode                 Specify the service mode startup


#### Для чего? 
 Управлять Dynamo из командной строки можно по разным причинам, включая следующие: 
 
 * автоматизация нескольких запусков Dynamo;
 * тестирование графов Dynamo (также обратите внимание на -c при использовании DynamoSandbox);
 * запуск последовательности графов Dynamo в определенном порядке;
 * создание пакетных файлов, которые используются для запуска нескольких экземпляров командной строки;
 * написание другой программы для автоматизации выполнения графов Dynamo и управления ими, а также для выполнения интересных операций с результатами этих вычислений.

#### Что?
Интерфейс командной строки (DynamoCLI) — это дополнение к DynamoSandbox. Данная утилита командной строки DOS/терминала предназначена для обеспечения удобства использования аргументов командной строки для запуска Dynamo. В первой реализации интерфейс не запускается автономно, его необходимо запускать из папки, в которой находятся двоичные файлы Dynamo, поскольку он зависит от тех же основных библиотек DLL, что и Sandbox. Интерфейс не взаимодействует с другими сборками Dynamo.

Существует четыре способа запуска интерфейса командной строки (CLI): из командной строки DOS, из пакетных файлов DOS и с помощью ярлыка на рабочем столе Windows, в путь которого включены соответствующие флаги командной строки. Спецификация файла DOS может быть полностью квалифицированной или относительной, поддерживаются подключенные диски и синтаксис URL-адресов. Его также можно создать с помощью Mono и запускать на Linux или Mac в окне терминала.

Утилита поддерживает пакеты Dynamo, однако загрузить пользовательские узлы (DYF) не удастся; можно загрузить только автономные графы (DYN).

На этапе предварительного тестирования утилита интерфейса командной строки поддерживает локализованные версии Windows, и вы можете указывать аргументы спецификации файла с помощью символов ASCII в верхнем регистре.

Доступ к CLI можно получить через приложение DynamoCLI.exe. Это приложение позволяет пользователю или другому приложению взаимодействовать с моделью оценки Dynamo путем вызова DynamoCLI.exe в командной строке. Результат может выглядеть следующим образом:
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
Эта команда предписывает Dynamo открыть указанный файл в папке *C:\\someReallyCoolDynamoFile.Dyn* без отображения пользовательского интерфейса, а затем запустить его. После завершения выполнения графа приложение Dynamo закрывается. 

**Новые возможности версии 2.1**: приложение DynamoWPFCLI.exe. Это приложение поддерживает все, что поддерживается приложением DynamoCLI.exe с добавлением параметра «Геометрия» (-g). Приложение DynamoWPFCLI.exe доступно только для Windows.

#### Важные примечания

* Предпочтительным способом взаимодействия с DynamoCLI является интерфейс командной строки.
* На этот раз необходимо запустить DynamoCLI из папки установки [Версия Dynamo]. Интерфейсу CLI требуется доступ к тем же файлам DLL, что и программе Dynamo, поэтому его не следует перемещать.
* Вы сможете запускать графы, которые в данный момент открыты в Dynamo, но это может вызвать непредвиденное поведение.
* Все пути к файлам полностью совместимы с DOS, поэтому относительные и полные пути должны работать, но обязательно заключайте пути в прямые кавычки *"C:path\\to\\file.dyn"*. 
* DynamoCLI — это новая функция, которая в настоящее время находится в процессе доработки. В настоящее время **CLI загружает только подмножество** стандартных библиотек Dynamo. Обратите на это внимание, если граф выполняется неправильно. Эти библиотеки указаны [здесь](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28). 
* В настоящее время вывод STD не предусмотрен. Если ошибок не обнаружено, интерфейс CLI просто закрывается по завершении выполнения.
 
#### Как?

`-o` — можно указать на файл DYN, чтобы открыть Dynamo в режиме без графического пользовательского интерфейса, в котором будет запущен граф.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v` — этот флаг можно использовать, когда Dynamo работает в режиме без графического пользовательского интерфейса (когда мы использовали флаг `-o` для открытия файла); данный флаг будет перебирать все узлы в графе и выводить их выходные значения в простой файл XML. Поскольку флаг `--ServiceMode` можно использовать для принудительного выполнения программой Dynamo нескольких оценок графа, выходной файл будет содержать значения для каждого выполненного вычисления.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
Выходной файл XML будет иметь следующий вид:
``` XML
    <evaluations>
        <evaluation0>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state1.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation0>
        <evaluation1>
            <Node guid="e2a6a828-19cb-40ab-b36c-cde2ebab1ed3">
                <output0 value="str" />
            </Node>
            <Node guid="67139026-e3a5-445c-8ba5-8a28be5d1be0">
                <output0 value="C:\Users\Dale\state2.txt" />
            </Node>
            <Node guid="579ebcb8-dc60-4faa-8fd0-cb361443ed94">
                <output0 value="null" />
            </Node>
        </evaluation1>
    </evaluations>
```
`-g` — этот флаг можно использовать, когда Dynamo работает в режиме без графического пользовательского интерфейса (когда мы использовали флаг `-o` для открытия файла); данный флаг будет генерировать граф и выводить результирующую геометрию в файл JSON. 
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
Файл геометрии JSON будет иметь следующий вид:
```
 TBD - Work in progress
```
`-h` — используйте этот флаг для получения списка возможных параметров.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
Флаг -i можно использовать несколько раз для импорта нескольких сборок, которые требуются для запуска открываемого графа.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

Флаг -l можно использовать для запуска Dynamo с другими региональными настройками. Однако обычно региональные настройки не влияют на результаты графа.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

Флаг --GeometryPath можно использовать, чтобы указать среде DynamoSandbox или интерфейсу CLI на определенный набор двоичных файлов ASM. Используйте его следующим образом.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

или
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
Флаг -k можно использовать, чтобы не завершать запущенный процесс Dynamo до тех пор, пока загруженное расширение не завершит его.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
Флаг --HostName можно использовать для идентификации варианта Dynamo, связанного с главным узлом.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
или
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
Флаг -s можно использовать для идентификации сеанса аналитики узла Dynamo.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
Флаг -p можно использовать для идентификации родительского идентификатора аналитики узла Dynamo.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"