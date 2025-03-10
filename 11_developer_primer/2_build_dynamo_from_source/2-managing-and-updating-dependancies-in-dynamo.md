# Aktualizowanie zależności lub dodawanie nowej zależności do dodatku Dynamo

### Kiedy ta strona wiki ma zastosowanie
Podczas pracy nad nową funkcją lub po prostu aktualizowania istniejącej zależności przed wprowadzeniem nowej zależności w repozytorium dodatku Dynamo należy ocenić następujące czynniki.

### Zagadnienia do rozważenia
1. Jaka jest licencja nowej lub zaktualizowanej zależności — tylko niektóre licencje open source są zatwierdzane bez uprzedniej rozmowy z działem prawnym ADSK.
    * Po rozwiązaniu problemu z licencją upewnij się, że zależność i wersja są zapisane na wewnętrznej stronie wiki.
    * Jeśli licencja to `LGPL`, `GPL` lub `Apache`, plik licencji musi zostać skopiowany do podfolderu _„Open Source Licenses”_ kompilacji dodatku Dynamo.
    * Jeśli licencja to `LGPL`, pełny kod źródłowy wszystkich komponentów producentów zewnętrznych wraz z treścią tekstową ich odpowiednich licencji open source musi zostać przekazany do [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. W przypadku aktualizacji: czy typ licencji zmienił się w stosunku do poprzedniej wersji?
3. Czy zależność jest międzyplatformowa? 
    * Czy ma natywne komponenty (takie jak `CEFSharp` lub `ImageMagick`)? Utrudni to wdrażanie na wielu platformach
    * Czy ma odniesienia tylko do systemu Windows? W takim przypadku nie powinna funkcjonować jako zależność od DynamoCore lub innych wieloplatformowych części dodatku Dynamo (warstwy modelu).
4. Czy zależność jest poprawnie pakowana w folderze bin podczas kompilacji ze wszystkimi wymaganymi zależnościami?
    * W przypadku aktualizacji: czy jakieś pliki są usuwane w wyniku aktualizacji? Czy ta wersja dodatku Dynamo jest przeznaczona dla wersji przyrostowych produktów nadrzędnych? Jeśli tak, musisz zachować stare pliki binarne do roku premiery globalnej, aby obsługiwać instalatory poprawek. Zobacz [tutaj](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. Czy zależność lub jej drzewo zależności nie koliduje z innymi istniejącymi zależnościami w dodatku Dynamo?
6. ?? Czy zależność lub jej drzewo zależności nie jest w konflikcie z istniejącymi zależnościami w produktach, które integrują dodatek Dynamo w procesie (Revit, Civil itp.) — **jest to ważne, ponieważ te problemy można wykryć tylko w czasie integracji, chyba że praca jest wykonywana na początku.**