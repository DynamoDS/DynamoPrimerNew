# Dynamo 指令行介面

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


#### 原因 
 出於各種原因，您會透過指令行控制 Dynamo，這些原因可能包括： 
 
 * 自動執行多個 Dynamo
 * 測試 Dynamo 圖表 (使用 DynamoSandbox 時也要查看 -c)
 * 以特定順序執行一系列 Dynamo 圖表
 * 撰寫執行多個指令行執行的批次檔
 * 撰寫另一個程式來控制和自動執行 Dynamo 圖表，並且運用這些計算結果做一些特別的事情

#### 內容
指令行介面 (DynamoCLI) 是對 DynamoSandbox 的補充內容。它是一個 DOS/終端機指令行公用程式，設計來提供指令行引數方便執行 Dynamo。在一開始的實作中，它無法獨立執行，必須從 Dynamo 二進位檔所在的資料夾執行，因為它依賴與沙箱相同的核心 DLL。無法與其他 Dynamo 建置版本互通。

有四種方式可以執行 CLI：從 DOS 命令提示執行、從 DOS 批次檔執行，以及以 Windows 桌面捷徑執行 (其路徑修改為包括指定的指令行旗標)。DOS 檔規格可以是完整的或相對的，也支援對映的磁碟機和 URL 語法。它也可以使用 Mono 建置，並在 Linux 或 Mac 上從終端機執行。

公用程式支援 Dynamo 套件，但是您無法載入自訂節點 (dyf)，只能載入獨立圖表 (dyn)。

在初步測試中，CLI 公用程式支援本土化的 Windows 版本，而且可以使用大寫的 Ascii 字元指定 filespec 引數。

可透過 DynamoCLI.exe 應用程式存取 CLI。此應用程式允許使用者或其他應用程式使用指令字串叫用 DynamoCLI.exe 與 Dynamo 演算模型互動。看起來可能會像下面這樣：
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
此指令會告訴 Dynamo 開啟 *C:\\someReallyCoolDynamoFile.Dyn* 的指定檔案，而不繪製任何使用者介面，然後執行它。圖表執行完成後，Dynamo 就結束。 

**2.1 版中的新功能**：DynamoWPFCLI.exe應用程式。此應用程式透過加入「幾何圖形 (-g)」選項支援 DynamoCLI.exe 應用程式支援的所有功能。DynamoWPFCLI.exe 應用程式僅限 Windows。

#### 重要注意事項 

* 與 DynamoCLI 互動的首選方式是透過指令提示介面。
* 目前，您需要從安裝位置的 [Dynamo 版本] 資料夾內執行 DynamoCLI。CLI 需要存取與 Dynamo 相同的 .dll，因此不應移動。
* 您應該能夠執行目前在 Dynamo 中開啟的圖表，但這可能會導致意外的副作用。
* 所有檔案路徑都完全遵守 DOS 規範，因此相對路徑和完整路徑應該都能運作，但請務必將路徑放在引號 *"C:path\\to\\file.dyn"* 中 
* DynamoCLI 是新功能，目前不斷在變化：目前 **CLI 只會載入一小組**標準 Dynamo 資源庫，如果圖表未正確執行，請注意這一點。[此處](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28)指定這些資源庫 
* 目前不提供 std 輸出，如果未遇到錯誤，CLI 將在執行完成後直接結束。
 
#### 方法

`-o`：您可以用無頭模式開啟 Dynamo 指向將執行圖表的 .dyn。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v`：可在 Dynamo 以無頭模式執行 (當我們使用 `-o` 打開檔案) 時使用，此旗標會反覆運算圖表中的所有節點，並將輸出值傾印到簡單的 XML 檔中。由於 `--ServiceMode` 旗標可能會強制 Dynamo 執行多個圖表演算，因此輸出檔案會保留發生的每個演算值。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
XML 輸出檔的格式如下：
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
`-g`：可在 Dynamo 以無頭模式執行 (當我們使用 `-o` 開啟檔案) 時使用，此旗標會產生圖表，然後將產生的幾何圖形傾印到 JSON 檔中。 
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
JSON 幾何圖形檔案的格式如下：
```
 TBD - Work in progress
```
`-h`：使用此旗標可取得可能選項的清單
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
-i 旗標可使用多次來匯入多個您嘗試開啟的圖表需要執行的組合。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

-l 旗標可用來在不同地區設定下執行 Dynamo。但通常而言，地區設定不會影響圖表結果
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

--GeometryPath 旗標可用來將 DynamoSandbox 或 CLI 指向一組特定的 ASM 二進位檔案 - 使用方式如下
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

或者
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
-k 旗標可用來維持 Dynamo 程序執行，直到載入的延伸將其關閉。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
--HostName 旗標可用來識別與主體關聯的 Dynamo 變體。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
或者
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
-s 旗標可用來識別 Dynamo 主體分析階段作業 ID
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
-p 旗標可用來識別 Dynamo 主體分析父系 ID
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"