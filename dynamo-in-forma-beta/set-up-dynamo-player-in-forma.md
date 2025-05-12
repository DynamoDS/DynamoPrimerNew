# Konfigurowanie programu Dynamo Player w programie Forma

Dostępne są dwie opcje używania dodatku Dynamo z programem Forma: oparty na chmurze dodatek Dynamo jako usługa (DaaS) lub dodatek Dynamo na komputerze. Każda z tych opcji ma konkretne zalety w zależności od tego, co chcesz zrobić, więc zanim zaczniesz, zastanów się, która z nich najlepiej odpowiada Twoim potrzebom. Pamiętaj jednak, że w każdej chwili możesz przełączać się między tymi opcjami.

**Porównanie dodatku Dynamo jako usługi i dodatku Dynamo na komputerze**

<table><thead><tr><th>Dynamo jako usługa</th><th>Pulpit</th><th data-hidden></th></tr></thead><tbody><tr><td>Łatwa konfiguracja</td><td>Wieloetapowy proces instalacji</td><td></td></tr><tr><td>Nie ma potrzeby instalowania dodatku Dynamo ani otwierania go</td><td>Musi być otwarty dodatek Dynamo</td><td></td></tr><tr><td>Nie rozpoznaje wykresu, który jest już otwarty w dodatku Dynamo</td><td>Otwieranie wykresu w programie Player w dodatku Dynamo za pomocą jednego kliknięcia przycisku</td><td></td></tr><tr><td>Nie można używać języka Python</td><td>Można używać języka Python</td><td></td></tr><tr><td>Można używać tylko zatwierdzonych pakietów</td><td>Można używać dowolnego pakietu</td><td></td></tr></tbody></table>

Na tej stronie wyjaśnimy, jak skonfigurować obie opcje.

### Konfigurowanie dodatku Dynamo jako usługi

Dodatek Dynamo w programie Forma w wersji beta jest obecnie w otwartej wersji beta w fazie wczesnego dostępu, co oznacza, że funkcje i interfejs użytkownika mogą się często zmieniać.

Najpierw zainstalujmy program Dynamo Player w programie Forma.

1. W witrynie Forma przejdź do obszaru **Extensions** na lewym pasku bocznym i kliknij przycisk **Add extension**. Spowoduje to otwarcie sklepu Autodesk App Store.
2. Wyszukaj pozycję Dynamo i dodaj program Dynamo Player w wersji beta (Dynamo Player Beta). Przeczytaj zastrzeżenia i kliknij przycisk **Agree** (Zgadzam się).

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Teraz program Dynamo Player jest dostępny w obszarze Rozszerzenia. Kliknij go, aby go otworzyć.
4. Możesz teraz korzystać z programu Dynamo Player.

### Konfigurowanie dodatku Dynamo na komputerze

Aby można było używać dodatku Dynamo na komputerze, potrzebny jest dodatek Dynamo w postaci autonomicznej aplikacji Sandbox albo w postaci połączonej z programem Revit lub programem Civil 3D. Potrzebny będzie również pakiet DynamoFormaBeta.

#### Revit

Wykonaj poniższe czynności, aby skonfigurować dodatek Dynamo w programie Revit i pakiet DynamoFormaBeta.

1. Upewnij się, że jest zainstalowany program Revit 2024.1 lub nowszy.
2. Otwórz dodatek Dynamo z programu Revit, przechodząc do opcji Zarządzaj > Dynamo.
3. W dodatku Dynamo zainstaluj pakiet DynamoFormaBeta. Przejdź do pozycji Pakiety > Menedżer pakietów, a następnie wyszukaj pozycję DynamoFormaBeta.
   1. Jeśli masz program Revit 2024, zainstaluj pakiet DynamoFormaBeta dla wersji 2.x.
   2. Jeśli masz program Revit 2025, zainstaluj pakiet DynamoFormaBeta.

#### Civil 3D

Wykonaj poniższe czynności, aby skonfigurować dodatek Dynamo w programie Civil 3D i pakiet DynamoFormaBeta.

1. Upewnij się, że jest zainstalowany program Civil 3D 2024.1 lub nowszy.
2. Otwórz dodatek Dynamo z programu Civil 3D, przechodząc do opcji Zarządzaj > Dynamo.
3. W dodatku Dynamo zainstaluj pakiet DynamoFormaBeta. Przejdź do pozycji Pakiety > Menedżer pakietów, a następnie wyszukaj pozycję DynamoFormaBeta.
   1. Jeśli masz program Civil 3D 2024, zainstaluj pakiet DynamoFormaBeta dla wersji 2.x.
   2. Jeśli masz program Civil 3D 2025, zainstaluj pakiet DynamoFormaBeta.

#### Dynamo Sandbox

Wykonaj poniższe czynności, aby zainstalować aplikację Dynamo Sandbox i pakiet DynamoFormaBeta.

1. Pobierz wersję Dynamo 2.18.0 lub nowszą z [kompilacji Dynamo](https://dynamobuilds.com/). Aby uzyskać najlepsze środowisko, wybierz najnowszą z najbardziej stabilnych wersji, wymienionych u góry.
   1. Wersje dzienne są wersjami rozwojowymi i mogą zawierać funkcje niekompletne lub w trakcie opracowywania.
2. Rozpakuj dodatek Dynamo za pomocą programu [7zip](https://7-zip.org/) do wybranego folderu.
3. Uruchom aplikację DynamoSandbox.exe z folderu instalacyjnego dodatku Dynamo.
4. W dodatku Dynamo zainstaluj pakiet DynamoFormaBeta. Przejdź do pozycji Pakiety > Menedżer pakietów, a następnie wyszukaj pozycję DynamoFormaBeta.
   1. Jeśli masz dodatek Dynamo 2.x, zainstaluj pakiet DynamoFormaBeta dla wersji 2.x.
   2. Jeśli masz dodatek Dynamo 3.x, zainstaluj pakiet DynamoFormaBeta.

Po zainstalowaniu dodatku Dynamo można go używać z programem Forma. W przypadku uruchamianiu opcji komputerowej dodatku Dynamo w programie Forma dodatek Dynamo musi być otwarty, aby można było używać rozszerzenia Dynamo Player.

#### Uzyskiwanie dostępu do dodatku Dynamo na komputerze w programie Forma

Najpierw zainstalujmy program Dynamo Player w programie Forma.

1. W witrynie Forma przejdź do obszaru **Extensions** na lewym pasku bocznym i kliknij przycisk **Add extension**. Spowoduje to otwarcie sklepu Autodesk App Store.
2. Wyszukaj pozycję Dynamo i dodaj program Dynamo Player w wersji beta (Dynamo Player Beta). Przeczytaj zastrzeżenia i kliknij przycisk **Agree** (Zgadzam się).

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. Teraz program Dynamo Player jest dostępny w obszarze Rozszerzenia. Kliknij go, aby go otworzyć.
4. W górnej części kliknij przycisk Desktop, aby uzyskać dostęp do dodatku Dynamo na komputerze.

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. Możesz teraz korzystać z programu Dynamo Player. Jeśli wykres jest już otwarty w dodatku Dynamo, wystarczy kliknąć przycisk Open w obszarze **Connected graph**, aby wyświetlić go w programie Player.
