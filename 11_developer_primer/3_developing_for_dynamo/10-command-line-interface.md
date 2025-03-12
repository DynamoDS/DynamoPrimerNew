# Dynamo コマンド ライン インタフェース

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


#### 使用する理由 
 次のようなさまざまな理由により、コマンド ラインから Dynamo をコントロールする場合があります。 
 
 * 多数の Dynamo の実行を自動化する
 * Dynamo グラフをテストする(DynamoSandbox を使用する場合は -c も参照する)
 * 一連の Dynamo グラフを特定の順序で実行する
 * 複数のコマンド ラインを実行するバッチ ファイルを作成する
 * Dynamo グラフの実行をコントロールおよび自動化する別のプログラムを記述し、その計算結果を使用してさまざまな処理を行う

#### 何を行うか
コマンド ライン インタフェース(DynamoCLI)は、DynamoSandbox を補完するものです。これは、Dynamo を実行するコマンド ライン引数の利便性を提供するように設計された、DOS/ターミナル コマンド ライン ユーティリティです。最初の実装では単独で実行できません。Sandbox と同じコア DLL に依存するため、Dynamo バイナリが存在するフォルダから実行する必要があります。Dynamo の他のビルドと相互運用することはできません。

CLI の実行方法は 4 つあります。まず DOS プロンプトから実行する、DOS バッチ ファイルから実行する、指定したコマンド ライン フラグを含むようにパスを変更した Windows デスクトップ ショートカットとして実行するという方法です。DOS ファイルの仕様は完全修飾か相対で記述できます。また、マッピングされたドライブと URL 構文もサポートされます。Mono でビルドし、ターミナルから Linux または Mac 上で実行するという方法もあります。

このユーティリティは Dynamo パッケージをサポートしていますが、カスタム ノード(dyf)をロードすることはできません。ロードできるのはスタンドアロンのグラフ(dyn)のみです。

予備テストでは、CLI ユーティリティはローカライズされたバージョンの Windows をサポートしており、filespec 引数を大文字の ASCII 文字で指定できます。

CLI は、DynamoCLI.exe アプリケーションからアクセスできます。このアプリケーションを使用すると、コマンド文字列を指定して DynamoCLI.exe を呼び出すことで、ユーザまたは別のアプリケーションが Dynamo 評価モデルとやりとりを行うことができます。結果は次のようになります。
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
このコマンドは、Dynamo に対して、*"C:\\someReallyCoolDynamoFile.Dyn"* にある指定ファイルを、UI を描画せずに開き、実行するように指示します。グラフの実行が完了すると、Dynamo は終了します。 

**バージョン 2.1 で追加**: DynamoWPFCLI.exe アプリケーションを追加しました。このアプリケーションは、ジオメトリ(-g)オプションを追加して、DynamoCLI.exe アプリケーションがサポートするすべての対象をサポートします。DynamoWPFCLI.exe アプリケーションは Windows 専用です。

#### 重要な注意事項

* DynamoCLI の操作には、コマンド プロンプト インタフェースを使用する方法が推奨されます。
* この場合、[Dynamo Version]フォルダ内のインストール場所から DynamoCLI を実行する必要があります。CLI は Dynamo と同じ .dll にアクセスする必要があるため、場所を移動しないでください。
* Dynamo で現在開いているグラフを実行することは可能ですが、これにより意図しない副作用が発生する可能性があります。
* すべてのファイル パスは完全に DOS に準拠しているため、相対パスと完全修飾パスは機能しますが、パスは引用符で囲んでください。例: *"C:path\\to\\file.dyn"* 
* DynamoCLI は新しい機能であり、仕様は現在流動的です。現時点では、CLI は標準 Dynamo ライブラリの**サブセットのみをロード**します。グラフが正しく実行されない場合は、この点にご注意ください。これらのライブラリは、[こちら](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28)で指定されています 
* 現在、エラーが発生しなかった場合は、標準出力が提供されず、CLI は実行完了後に終了します。
 
#### 使用方法

`-o` グラフを実行するヘッドレス モードで、.dyn を参照する Dynamo を開くことができます。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v` このフラグは、Dynamo がヘッドレス モードで実行されている場合(`-o` を使用してファイルを開いた場合)に使用できます。このフラグは、グラフ内のすべてのノードを反復し、その出力値を単純な XML ファイルにダンプします。`--ServiceMode` フラグは Dynamo に複数のグラフ評価を実行するように強制できるため、出力ファイルには各評価の値が含まれます。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
XML 出力ファイルには、次の形式があります。
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
`-g` Dynamo がヘッドレス モードで実行されている場合(`-o` を使用してファイルを開いた場合)に使用できます。このフラグはグラフを生成し、結果のジオメトリを JSON ファイルにダンプします。 
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
JSON ジオメトリ ファイルには、次のような形式があります。
```
 TBD - Work in progress
```
`-h` これを使用して、使用可能なオプションのリストを取得します
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
-i フラグを複数回使用して、開こうとしているグラフの実行に必要な複数のアセンブリを読み込むことができます。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

-l フラグは、異なるロケール設定で Dynamo を実行するために使用できます。ただし通常は、ロケールの設定はグラフの結果に影響しません
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

--GeometryPath フラグを使用すると、DynamoSandbox または CLI に ASM バイナリの特定のセットを指すことができます。次に使用例を示します。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

または
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
-k フラグを使用すると、ロードされた拡張機能によってシャットダウンされるまで、Dynamo プロセスを実行したままにすることができます。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
--HostName フラグを使用すると、ホストに関連付けられている Dynamo バリエーションを識別できます。
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
または
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
-s フラグを使用すると、Dynamo ホスト解析セッション ID を識別できます
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
-p フラグを使用すると、Dynamo ホスト解析の親 ID を識別できます
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"