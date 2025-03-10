# Interfejs wiersza polecenia dodatku Dynamo

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


#### Dlaczego? 
 Sterowanie dodatkiem Dynamo z poziomu wiersza polecenia może być konieczne z różnych powodów: 
 
 * Automatyzacja wielu przebiegów dodatku Dynamo
 * Testowanie wykresów Dynamo (w przypadku korzystania z aplikacji DynamoSandbox należy również zapoznać się z flagą -c)
 * Uruchamianie sekwencji wykresów Dynamo w określonej kolejności
 * Zapisywanie plików wsadowych, które uruchamiają wiele wykonań wiersza polecenia
 * Pisanie innego programu w celu sterowania działaniem wykresów Dynamo i automatyzowania go oraz wykonywania interesujących zadań dotyczących wyników tych obliczeń

#### Co?
Interfejs wiersza polecenia (DynamoCLI) jest uzupełnieniem aplikacji DynamoSandbox. Jest to narzędzie wiersza polecenia typu DOS/Terminal zaprojektowane w celu zapewnienia wygody argumentów wiersza polecenia na potrzeby uruchamiania dodatku Dynamo. W pierwszej implementacji ten komponent nie działa samodzielnie — należy go uruchomić z poziomu folderu, w którym znajdują się pliki binarne dodatku Dynamo, ponieważ jest zależny od tych samych bibliotek podstawowych DLL co środowisko Sandbox. Komponent ten nie może współdziałać z innymi kompilacjami dodatku Dynamo.

Istnieją cztery sposoby uruchamiania interfejsu wiersza polecenia: z wiersza polecenia systemu Dos, z plików wsadowych systemu Dos oraz za pomocą skrótu na pulpicie systemu Windows, którego ścieżka jest zmodyfikowana tak, aby zawierała określone flagi wiersza polecenia. Specyfikacja pliku systemu Dos może być w pełni kwalifikowana lub względna. Obsługiwane są też dyski zamapowane i składnia adresów URL. Plik można również utworzyć za pomocą platformy Mono i uruchamiać w systemie Linux lub Mac z poziomu terminala.

Pakiety Dynamo są obsługiwane przez to narzędzie, jednak nie umożliwia ono wczytywania węzłów niestandardowych (DYF), a jedynie wykresy autonomiczne (DYN).

We wstępnych testach narzędzie interfejsu wiersza polecenia obsługuje zlokalizowane wersje systemu Windows i można określać argumenty specyfikacji plików za pomocą wyższych znaków ASCII.

Dostęp do interfejsu wiersza polecenia można uzyskać za pośrednictwem aplikacji DynamoCLI.exe. Ta aplikacja umożliwia użytkownikowi lub innej aplikacji interakcję z modelem ewaluacyjnym dodatku Dynamo przez wywołanie aplikacji DynamoCLI.exe za pomocą ciągu polecenia. Wyniki mogą wyglądać następująco:
 
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
 
To polecenie poinstruuje dodatek Dynamo, aby otworzył określony plik w ścieżce *„C:\\someReallyCoolDynamoFile.Dyn”* bez wyświetlania interfejsu użytkownika, a następnie uruchomi go. Dodatek Dynamo zostanie zamknięty po zakończeniu działania wykresu. 

**Nowości w wersji 2.1**: aplikacja DynamoWPFCLI.exe. Ta aplikacja obsługuje wszystko to, co obsługuje aplikacja DynamoCLI.exe, z dodatkiem opcji Geometry (-g). Aplikacja DynamoWPFCLI.exe jest przeznaczona tylko dla systemu Windows.

#### Ważne uwagi

* Preferowaną metodą interakcji z interfejsem DynamoCLI jest interfejs wiersza polecenia.
* Obecnie należy uruchamiać interfejs DynamoCLI z jego lokalizacji instalacji w folderze [Wersja dodatku Dynamo]. Interfejs wiersza polecenia wymaga dostępu do tych samych plików .dll co dodatek Dynamo, więc nie należy go przenosić.
* Powinno być możliwe wykonywanie wykresów, które są aktualnie otwarte w dodatku Dynamo, ale może to spowodować niezamierzone skutki uboczne.
* Wszystkie ścieżki plików są w pełni zgodne z systemem DOS, więc ścieżki względne i w pełni kwalifikowane powinny działać, ale pamiętaj, aby ujmować ścieżki w cudzysłów *"C:ścieżka\\do\\pliku.dyn"* 
* Interfejs DynamoCLI to nowa funkcja, która obecnie podlega ciągłym zmianom: **ten interfejs wiersza polecenia wczytuje obecnie tylko podzestaw** standardowych bibliotek dodatku Dynamo. Należy o tym pamiętać, jeśli wykres nie jest wykonywany poprawnie. Biblioteki te są określone [tutaj](https://github.com/DynamoDS/Dynamo/blob/master/src/DynamoApplications/PathResolvers.cs#L28) 
* Obecnie nie są dostarczane żadne standardowe dane wyjściowe, jeśli nie wystąpią żadne błędy. Interfejs wiersza polecenia zostanie po prostu zamknięty po zakończeniu przebiegu.
 
#### W jaki sposób?

Flaga `-o` umożliwia otwarcie dodatku Dynamo przez wskazanie pliku .dyn w trybie bezobsługowym, który spowoduje uruchomienie tego wykresu.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"
```
Flagi `-v` można używać, gdy dodatek Dynamo działa w trybie bezobsługowym (gdy użyto flagi `-o` do otwarcia pliku). Ta flaga spowoduje iterowanie wszystkich węzłów na wykresie i zrzucenie ich wartości wyjściowych do prostego pliku XML. Ponieważ flaga `--ServiceMode` może wymusić na dodatku Dynamo uruchomienie wielu obliczeń wykresu, plik wyjściowy będzie zawierać wartości dla każdego wykonania obliczeń.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"
```
        
Wyjściowy plik XML będzie miał postać:
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
Flagi `-g` można używać, gdy dodatek Dynamo działa w trybie bezobsługowym (gdy użyto flagi `-o` do otwarcia pliku). Ta flaga spowoduje wygenerowanie wykresu, a następnie zrzucenie geometrii wynikowej do pliku JSON. 
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"
```  
Plik geometrii JSON będzie miał postać:
```
 TBD - Work in progress
```
Użycie flagi `-h` pozwala uzyskać listę możliwych opcji
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h
```
Flagi -i można używać wielokrotnie w celu zaimportowania wielu zespołów, których wymaga uruchomienie otwieranego wykresu.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"
```

Flagi -i można używać do uruchamiania dodatku Dynamo w ramach innych ustawień regionalnych. Zazwyczaj jednak ustawienia regionalne nie mają wpływu na wyniki wykresu
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"
```

Za pomocą flagi --GeometryPath można wskazać aplikacji DynamoSandbox lub interfejsowi wiersza polecenia konkretny zestaw plików binarnych ASM — używaj jej w następujący sposób:
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```

lub
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"
```
Flaga -k umożliwia pozostawienie uruchomionego procesu dodatku Dynamo do momentu zamknięcia go przez wczytane rozszerzenie.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k
```
Flaga --HostName umożliwia zidentyfikowanie odmiany dodatku Dynamo skojarzonej z programem nadrzędnym.
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"
```
lub
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"
```
Flaga -s służy do identyfikowania identyfikatora sesji analizy programu nadrzędnego dodatku Dynamo
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]
```
Flaga -p służy do identyfikowania identyfikatora nadrzędnego analizy programu nadrzędnego dodatku Dynamo
```
    C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"