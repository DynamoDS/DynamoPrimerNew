# Interfejs wiersza polecenia dodatku Dynamo

`-o, -O, --OpenFilePath` Poinstruuj dodatek Dynamo, aby otworzył plik poleceń i uruchomił zawarte w nim polecenia w tej ścieżce. Ta opcja jest obsługiwana tylko w przypadku uruchamiania z aplikacji DynamoSandbox.  

`-c, -C, --CommandFilePath` Poinstruuj dodatek Dynamo, aby otworzył plik poleceń i uruchomił zawarte w nim polecenia w tej ścieżce. Ta opcja jest obsługiwana tylko w przypadku uruchamiania z aplikacji DynamoSandbox.  

`-v, -V, --Verbose` Poinstruuj dodatek Dynamo, aby wszystkie wykonywane obliczenia zapisywał w pliku XML w określonej ścieżce.  

`-g, -G, --Geometry` Poinstruuj dodatek Dynamo, aby zapisywał geometrię wynikową ze wszystkich obliczeń w pliku JSON w tej ścieżce.  

`-h, -H, --help` Skorzystaj z pomocy.  

`-i, -I, --Import` Poinstruuj dodatek Dynamo, aby zaimportował zespół jako bibliotekę węzłów. Tym argumentem powinna być ścieżka pliku pojedynczego pliku `.dll`. Jeśli chcesz zaimportować wiele plików `.dlls`, wymień je, rozdzielając je spacjami: `-i 'assembly1.dll' 'assembly2.dll'`.  

`--GeometryPath` Ścieżka względna lub bezwzględna katalogu zawierającego ASM. Po podaniu tej ścieżki zamiast przeszukiwania dysku twardego w poszukiwaniu ASM zostanie on wczytany bezpośrednio z tej ścieżki.  

`-k, -K, --KeepAlive` Tryb Keepalive umożliwia pozostawienie uruchomionego procesu dodatku Dynamo do momentu zamknięcia go przez wczytane rozszerzenie.  

`--HostName` Zidentyfikuj odmianę dodatku Dynamo skojarzoną z programem nadrzędnym.  

`-s, -S, --SessionId` Zidentyfikuj identyfikator sesji analizy programu nadrzędnego dodatku Dynamo.  

`-p, -P, --ParentId` Zidentyfikuj identyfikator programu nadrzędnego analizy dodatku Dynamo.  

`-x, -X, --ConvertFile` W połączeniu z flagą `-O` otwiera plik `.dyn` z określonej ścieżki i konwertuje go na plik `.json`. Plik będzie miał rozszerzenie `.json` i będzie się znajdował w tym samym katalogu co oryginalny plik.  

`-n, -N, --NoConsole` Nie polegaj na oknie konsoli w zakresie interakcji z interfejsem wiersza polecenia w trybie Keepalive.  

`-u, -U, --UserData` Określ folder danych użytkownika, który ma być używany przez narzędzie PathResolver z interfejsem wiersza polecenia.  

`--CommonData` Określ folder danych wspólnych, który ma być używany przez narzędzie PathResolver z interfejsem wiersza polecenia.  

`--DisableAnalytics` Wyłącza analizę w dodatku Dynamo dla okresu istnienia procesu.  

`--CERLocation` Określ narzędzie do zgłaszania błędów powodujących awarię znajdujące się na dysku.  

`--ServiceMode` Określ tryb uruchamiania usługi.  



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
 
 `C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`
 
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

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn"`

Flagi `-v` można używać, gdy dodatek Dynamo działa w trybie bezobsługowym (gdy użyto flagi `-o` do otwarcia pliku). Ta flaga spowoduje iterowanie wszystkich węzłów na wykresie i zrzucenie ich wartości wyjściowych do prostego pliku XML. Ponieważ flaga `--ServiceMode` może wymusić na dodatku Dynamo uruchomienie wielu obliczeń wykresu, plik wyjściowy będzie zawierać wartości dla każdego wykonania obliczeń.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -p "C:\aFileWithPresetsInIt.dyn" --ServiceMode "all" -v "C:\output.xml"`

        
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

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoWPFCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -g "C:\geometry.json"`
  
Plik geometrii JSON będzie miał postać:

 Do ustalenia — praca w toku

Użycie flagi `-h` pozwala uzyskać listę możliwych opcji

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -h`

Flagi -i można używać wielokrotnie w celu zaimportowania wielu zespołów, których wymaga uruchomienie otwieranego wykresu.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -i"a.dll" -i"aSecond.dll"`

Flagi -i można używać do uruchamiania dodatku Dynamo w ramach innych ustawień regionalnych. Zazwyczaj jednak ustawienia regionalne nie mają wpływu na wyniki wykresu

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -o "C:\someReallyCoolDynamoFile.Dyn" -l "de-DE"`

Za pomocą flagi --GeometryPath można wskazać aplikacji DynamoSandbox lub interfejsowi wiersza polecenia konkretny zestaw plików binarnych ASM — używaj jej w następujący sposób:

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

lub

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --GeometryPath "\pathToGeometryBinaries\"`

Flaga -k umożliwia pozostawienie uruchomionego procesu dodatku Dynamo do momentu zamknięcia go przez wczytane rozszerzenie.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -k`

Flaga --HostName umożliwia zidentyfikowanie odmiany dodatku Dynamo skojarzonej z programem nadrzędnym.

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe --HostName "DynamoFormIt"`

lub

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoSandbox.exe --HostName "DynamoFormIt"`

Flaga -s służy do identyfikowania identyfikatora sesji analizy programu nadrzędnego dodatku Dynamo

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -s [HostSessionId]`

Flaga -p służy do identyfikowania identyfikatora nadrzędnego analizy programu nadrzędnego dodatku Dynamo

`C:\Program Files\Dynamo\Dynamo Core\[Dynamo Version]\DynamoCLI.exe -p "RVT&2022&MUI64&22.0.2.392"`
