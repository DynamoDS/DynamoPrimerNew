# Rozhraní příkazového řádku aplikace Dynamo

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


#### K čemu to slouží? 
 Dynamo můžete chtít ovládat z příkazového řádku z různých důvodů. Mezi ně patří například: 
 
 * Automatizace mnoha spuštění aplikace Dynamo.
 * Testování grafů aplikace Dynamo (při použití aplikace DynamoSandbox se podívejte také na možnost -c).
 * Spuštění posloupnosti grafů aplikace Dynamo v určitém pořadí.
 * Tvorba dávkových souborů, které spouštějí více příkazových řádků.
 * Tvorba dalšího programu, který bude řídit a automatizovat spuštění grafů aplikace Dynamo a provádět s výsledky těchto výpočtů další skvělé akce.

#### Co to je?
Rozhraní příkazového řádku (DynamoCLI) je doplněk k aplikaci DynamoSandbox. Jedná se o nástroj příkazového řádku typu DOS / terminál určený k pohodlnému spouštění aplikace Dynamo pomocí argumentů příkazového řádku. Ve své první implementaci se nespouští samostatně, ale musí být spuštěn ze složky, kde se nacházejí binární soubory aplikace Dynamo, protože závisí na stejných základních knihovnách DLL jako Sandbox. Nemůže spolupracovat s jinými sestaveními aplikace Dynamo.

Rozhraní příkazového řádku (CLI) lze spustit několika různými způsoby: z příkazového řádku DOS, z dávkových souborů DOS a jako zástupce na ploše systému Windows, jehož cesta je upravena tak, aby obsahovala zadané příznaky příkazového řádku. Specifikace souboru DOS může být plně kvalifikovaná nebo relativní a podporovány jsou také mapované jednotky a syntaxe adres URL. Lze jej také sestavit pomocí platformy Mono a spustit v systému Linux nebo Mac z terminálu.

Balíčky Dynamo jsou nástrojem podporovány, nelze však načíst vlastní uzly (DYF), pouze samostatné grafy (DYN).

Při předběžném testování nástroj CLI podporuje lokalizované verze systému Windows a argumenty filespec lze zadávat velkými znaky ASCII.

K CLI lze přistupovat prostřednictvím aplikace DynamoCLI.exe. Tato aplikace umožňuje uživateli nebo jiné aplikaci komunikovat s vyhodnocovacím modelem aplikace Dynamo vyvoláním souboru DynamoCLI.exe s řetězcem příkazu. Ten může vypadat například takto:
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
Tento příkaz aplikaci Dynamo sdělí, aby otevřela zadaný soubor v umístění *"C:\\someReallyCoolDynamoFile.Dyn"* bez vykreslení uživatelského rozhraní a poté jej spustila. Po dokončení běhu grafu se aplikace Dynamo ukončí. 

**Novinka ve verzi 2.1**: Aplikace DynamoWPFCLI.exe. Tato aplikace podporuje vše, co aplikace DynamoCLI.exe, navíc s možností Geometry (-g). Aplikace DynamoWPFCLI.exe je určena pouze pro systém Windows.

#### Důležité poznámky

* Upřednostňovanou metodou interakce s DynamoCLI je rozhraní příkazového řádku.
* V tuto chvíli je rozhraní DynamoCLI třeba spouštět z jeho instalačního umístění ve složce [Verze aplikace Dynamo]. Rozhraní CLI potřebuje přístup ke stejným knihovnám DLL jako aplikace Dynamo, proto by se nemělo přesouvat.
* Měli byste být schopni spouštět grafy, které jsou aktuálně otevřené v aplikaci Dynamo, ale může to způsobit nežádoucí vedlejší účinky.
* Všechny cesty k souborům jsou plně kompatibilní se systémem DOS, takže by relativní a plně kvalifikované cesty měly fungovat, ale nezapomeňte cesty uzavřít do uvozovek: *"C:cesta\\k\\soubor.dyn"*. 
* DynamoCLI je nová funkce, která se v současné době vyvíjí: aktuálně **CLI načítá pouze podmnožinu ** standardních knihoven Dynamo. Mějte to na paměti, pokud se graf nespustí správně. Tyto knihovny jsou uvedeny [zde](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) 
* V současné době není k dispozici žádný standardní výstup. Pokud nedojde k žádným chybám, CLI se po dokončení běhu jednoduše ukončí.
 
#### Jak se to používá?

`-o`: Aplikaci Dynamo můžete otevřít tak, že bude odkazovat na soubor .dyn, a to v bezobslužném režimu, který spustí graf.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
`-v`: Lze použít, když je aplikace Dynamo spuštěna v bezobslužném režimu (pokud jste k otevření souboru použil příznak `-o`). Tento příznak iteruje všechny uzly v grafu a vypíše jejich výstupní hodnoty do jednoduchého souboru XML. Vzhledem k tomu, že příznak `--ServiceMode` může vynutit, aby aplikace Dynamo spustila více vyhodnocení grafu, bude výstupní soubor obsahovat hodnoty pro každé vyhodnocení, ke kterému dojde.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
Výstupní soubor XML by měl následující podobu:
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
`-g`: Lze použít, když je aplikace Dynamo spuštěna v bezobslužném režimu (pokud jste k otevření souboru použil příznak `-o`). Tento příznak vygeneruje graf a poté vypíše výslednou geometrii do souboru JSON. 
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
Soubor geometrie JSON by měl následující podobu:
```
 TBD - Work in progress
```
`-h`: Slouží k získání seznamu možných možností.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
Příznak -i je možné použít vícekrát k importu více sestavení, která graf, jenž se pokoušíte otevřít, vyžaduje ke spuštění.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

Příznak -l je možné použít ke spuštění aplikace Dynamo v jiném nastavení národního prostředí. Nastavení národního prostředí však obvykle neovlivňuje výsledky grafu
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

Příznak --GeometryPath lze použít k nasměrování aplikace DynamoSandbox nebo CLI na konkrétní sadu binárních souborů ASM. Použijte jej jako v následujícím příkladu:
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

nebo
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
Příznak -k lze použít k tomu, aby proces aplikace Dynamo běžel, dokud jej načtené rozšíření nevypne.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
Příznak --HostName lze použít k identifikaci variace aplikace Dynamo přidružené k hostiteli.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
nebo
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
Příznak -s lze použít k identifikaci ID relace analytického hostitele aplikace Dynamo.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
Příznak -p lze použít k identifikaci ID nadřazeného analytického hostitele Dynamo.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"