# Czego Autodesk Assistant jeszcze nie potrafi

### Aktualizowanie pakietów

Autodesk Assistant nie potrafi jeszcze pobierać i instalować pakietów w dodatku Dynamo

![](../../.gitbook/assets/Aa_Update.jpg)

Jak widać na ilustracji powyżej, Autodesk Assistant nie potrafi modyfikować, instalować ani odinstalowywać zewnętrznych pakietów powiązanych z dodatkiem Dynamo. Jest w stanie natomiast podać instrukcje, dzięki którym możesz je samodzielnie zainstalować.


### Bezpośrednie odczytywanie i zapisywanie plików

Autodesk Assistant obsługuje pliki grafów Dynamo: może otworzyć graf, wstawić graf do obszaru rysunku, uruchomić graf i zapisać pracę. Nie może samodzielnie odczytywać ani zapisywać innych plików na dysku twardym.

W poniższym przykładzie na obszarze rysunku utworzono geometrię. Autodesk Assistant otrzymuje polecenie pobrania punktów tworzących tę geometrię i wyeksportowania ich jako kodu JSON. Mimo że Autodesk Assistant nie może bezpośrednio wygenerować tego kodu JSON, proponuje utworzenie skryptu Dynamo, który to robi.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

Ta propozycja jest schematem działania, o którym warto pamiętać. Wszystko, co znajduje się poza grafem, jest uzyskiwane za pośrednictwem węzłów, a nie bezpośrednio przez narzędzie Assistant.

### Zmienianie modelu programu Revit bez grafu

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Narzędzie Autodesk Assistant for Dynamo nie ma bezpośredniego połączenia z modelem programu Revit. Poproś je o edytowanie modelu w taki sam sposób, jak robisz to w przypadku narzędzia Assistant w programie Revit, a otrzymasz odpowiedź, że nie jest to możliwe.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

Nadal może zmienić model programu Revit. Robi to tak, jak Ty to robisz: umieszcza węzły programu Revit i łączy je ze sobą, a model zmienia się po uruchomieniu grafu.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Z takiego sposobu działania wynikają dwie rzeczy:

- **Nic się nie zmienia, dopóki nie zostanie uruchomiony graf.** Najpierw możesz odczytać graf i dostosować wartości. Model jest modyfikowany wyłącznie po uruchomieniu grafu — przez Ciebie albo przez narzędzie Assistant w ramach realizacji Twojego polecenia.
- **Zmianę można powtórzyć.** Graf pozostaje na obszarze rysunku. Zapisz go. Teraz możesz ponownie wykonać to samo zadanie na innym modelu albo udostępnić je swojemu zespołowi.

Jeśli chcesz wprowadzić jednorazową, bezpośrednią zmianę bez użycia grafu, użyj narzędzia Autodesk Assistant w programie Revit.
