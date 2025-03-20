# Dynamo Command Line Interface

```plaintext
      -o, -O, --OpenFilePath        Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox
```

<div style="overflow-x: auto; white-space: nowrap;">

```plaintext
      -c, -C, --CommandFilePath     Instruct Dynamo to open a command file and run the commands it contains at 
                                    this path, this option is only supported when run from DynamoSandbox                      
```

</div> 

<div style="overflow-x: auto; white-space: nowrap;">
  <pre>
    <code>
      -v, -V, --Verbose             Instruct Dynamo to output all evaluations it performs to an XML file at this path
    </code>
  </pre>
</div>

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


#### Why 
 You might want to control Dynamo from the command line for various reasons, these might include: 
 
 * Automating many Dynamo runs
 * Testing of Dynamo Graphs (also look at -c when using DynamoSandbox)
 * Running a sequence of Dynamo graphs in a specific order
 * Writing batch files that run multiple command line executions
 * Writing another program to control and automate the running of Dynamo graphs and do cool things with the results of those computations

#### What
The Command Line Interface (DynamoCLI) is a supplement to DynamoSandbox. it is a dos/terminal command line utility designed to provide the convenience of command line arguments to run Dynamo. In its first implementation, it does not run standalone, it must be run from the folder where the Dynamo binaries reside, as it depends on the same core DLLs as the Sandbox. It can not interoperate with other builds of Dynamo.

There are four ways to run CLI: from a Dos prompt, from Dos batch files, and as a Windows desktop shortcut whose path is modified to include the specified command line flags. The Dos file spec can be fully qualified or relative, and mapped drives and URL syntax is supported as well. It can also be built with Mono and run on Linux or Mac from the terminal.

Dynamo packages are supported by the utility, however, you can not load custom nodes (dyf) only standalone graphs (dyn).

In preliminary testing, the CLI utility supports localized versions of Windows and you can specify filespec arguments with upper Ascii characters.

The CLI can be accessed through the DynamoCLI.exe application. This application lets a user or another application interact with the Dynamo evaluation model by invoking DynamoCLI.exe with a command string. This might look something like this:
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
This command will tell Dynamo to open the specified file at *"C:\someReallyCoolDynamoFile.Dyn"*, without drawing any UI, and then run it. Dynamo will then exit when the graph has completed running. 

**New in version 2.1**: The DynamoWPFCLI.exe application. This application supports everything that the DynamoCLI.exe application supports with the addition of the Geometry (-g) option. The DynamoWPFCLI.exe application is Windows only.

#### Important Notes

* The preferred method of interacting with DynamoCLI is through a command prompt interface.
* At this time you will need to run DynamoCLI from its install location inside the [Dynamo Version] folder. The CLI needs access to the same .dlls as Dynamo so it should not be moved.
* You should be able to execute graphs that are currently open in Dynamo, but this may cause unintended side effects.
* All file paths are fully dos compliant so relative and fully qualified paths should work, but be sure to enclose your paths in quotes *"C:path\to\file.dyn"* 
* DynamoCLI is new functionality and currently in flux: the **CLI loads only a subset** of standard Dynamo libraries at this time, note this if a graph does not execute correctly. These libraries are specified [here](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) 
* Currently no std output is provided if no errors are encountered, the CLI will simply exit after the run completes.
 
#### How

`-o` you can open dynamo pointing to a .dyn,  in a headless mode that will run the graph.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v` can be used when Dynamo is running in a headless mode (when we have used `-o` to open a file), this flag will iterate all nodes in the graph and dump their output values to a simple XML file. Because the `--ServiceMode` flag can force Dynamo to run multiple graph evaluations, the output file will hold values for each evaluation that occurs.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
The XML output file would have the form:
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
`-g` can be used when Dynamo is running in a headless mode (when we have used `-o` to open a file), this flag will generate the graph and then dump the resulting geometry into a JSON file. 
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
The JSON geometry file would have the form:
```
 TBD - Work in progress
```
`-h` use this to get a list of the possible options
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
the -i flag can be used multiple times to import multiple assemblies which the graph you are trying to open requires to run.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

the -l flag can be used to run Dynamo under a different locale setting. But usually, locale setting does not affect graph results
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

the --GeometryPath flag can be used to point DynamoSandbox or CLI to a specific set of ASM binaries - use it like
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

or
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
the -k flag can be used to leave the Dynamo process running until a loaded extension shuts it down.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
the --HostName flag can be used to identify Dynamo variation associated with the host.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
or
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
the -s flag can be used to identify Dynamo host analytics session id
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
the -p flag can be used to identify Dynamo host analytics parent id
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"