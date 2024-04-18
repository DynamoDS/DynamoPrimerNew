# Aktualizowanie pakietów i bibliotek dodatku Dynamo dla dodatku Dynamo 3.x i platformy NET8

### Wprowadzenie <a href="#introduction" id="introduction"></a>

Ta sekcja zawiera informacje na temat problemów, które mogą wystąpić podczas migrowania wykresów, pakietów i bibliotek do dodatku Dynamo 3.x.

Dodatek Dynamo 3.0 jest wersją główną i niektóre interfejsy API zostały w nim zmienione lub usunięte. Największą zmianą, która może mieć konsekwencje dla programisty lub użytkownika dodatku Dynamo 3.x, jest przejście na platformę .NET8.

Dotnet/.NET to środowisko wykonawcze obsługujące język C#, w którym jest napisany dodatek Dynamo. Zaktualizowaliśmy środowisko wykonawcze do najnowszej wersji wraz z resztą ekosystemu firmy Autodesk.

Więcej informacji można znaleźć w [tym wpisie w blogu](https://dynamobim.org/dynamo-on-net-8/).
***

### Zgodność pakietów <a href="#package-compatibility" id="package-compatibility"></a>

#### Korzystanie z pakietów dodatku Dynamo 2.x w dodatku Dynamo 3.x 
Ponieważ dodatek Dynamo 3.x działa teraz w środowisku wykonawczym .NET8, nie ma gwarancji, że pakiety utworzone dla dodatku Dynamo 2.x (*przy użyciu platformy .NET48*) będą działać w dodatku Dynamo 3.x. W przypadku próby pobrania w dodatku Dynamo 3.x pakietu opublikowanego w wersji Dynamo starszej niż 3.0 zostanie wyświetlone ostrzeżenie, że pakiet pochodzi ze starszej wersji dodatku Dynamo. 

**To nie oznacza, że pakiet nie będzie działał**. Jest to po prostu ostrzeżenie, że mogą wystąpić problemy ze zgodnością, i że ogólnie warto sprawdzić, czy nie istnieje nowsza wersja, która została opracowana specjalnie dla dodatku Dynamo 3.x.

Ten typ ostrzeżenia może też pojawiać się w plikach dziennika dodatku Dynamo podczas wczytywania pakietu. Jeśli wszystko działa poprawnie, można zignorować to ostrzeżenie.

#### Korzystanie z pakietów dodatku Dynamo 3.x w dodatku Dynamo 2.x 

Jest bardzo mało prawdopodobne, że pakiet utworzony dla dodatku Dynamo 3.x (*przy użyciu platformy .Net8*) będzie działał w dodatku Dynamo 2.x. Ponadto w przypadku pobierania w starszej wersji dodatku Dynamo pakietów przeznaczonych dla nowszych wersji również są wyświetlane ostrzeżenia.


