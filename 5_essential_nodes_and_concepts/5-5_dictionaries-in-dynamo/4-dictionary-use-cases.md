# Případy použití aplikace Revit

Chtěli jste někdy v aplikaci Revit vyhledávat položky podle jejich dat?

Pokud ano, je pravděpodobné, že jste udělali něco podobného jako v následujícím příkladu.

Na obrázku níže získáme všechny místnosti v modelu aplikace Revit, zjistíme index požadované místnosti (podle čísla místnosti) a nakonec získáme místnost s tímto indexem.

\![](<../../.gitbook/assets/dictionary - collect room in revit model.jpg>)

> 1. Získejte všechny místnosti v modelu.
> 2. Číslo místnosti, kterou chcete najít.
> 3. Získejte číslo místnosti a její index.
> 4. Získejte místnost s daným indexem.

## Cvičení: Slovník místností

### Část I: Vytvoření slovníku místností

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../../.gitbook/assets/roomDictionary (1).dyn" %}

Pojďme udělat totéž pomocí slovníků. Nejdříve je nutné získat všechny místnosti v modelu aplikace Revit.

\![](<../../.gitbook/assets/dictionary - exercise I - 01.jpg>)

> 1. Vybereme kategorii aplikace Revit, se kterou chceme pracovat (v tomto případě místnosti).
> 2. Přikážeme aplikaci Dynamo, aby získala všechny prvky

Dále je nutné rozhodnout, podle jakých klíčů se budou data vyhledávat. (Informace o klíčích naleznete v části [Co je slovník?](1-what-is-a-dictionary.md))

\![](<../../.gitbook/assets/dictionary - exercise I - 02.jpg>)

> 1. Data, která použijeme, jsou čísla místností.

Nyní vytvoříme slovník s danými klíči a prvky.

\![](<../../.gitbook/assets/dictionary - exercise I - 03.jpg>)

> 1. Uzel **Dictionary.ByKeysValues** vytvoří slovník podle odpovídajících vstupů.
> 2. `Keys` musí být řetězce, zatímco `values` mohou být různé typy objektů.

Nyní můžeme načíst místnost ze slovníku pomocí čísla místnosti.

\![](<../../.gitbook/assets/dictionary - exercise I - 04.jpg>)

> 1. Uzel `String` bude klíč, který použijeme k vyhledání objektu ze slovníku.
> 2. Uzel **Dictionary.ValueAtKey** načte objekt ze slovníku.

### Část II: Vyhledání hodnot

Stejným způsobem lze vytvářet slovníky seskupených objektů. Pokud bychom chtěli vyhledat všechny místnosti na daném podlaží, můžeme upravit graf následovně.

\![](<../../.gitbook/assets/dictionary - exercise II - 01.jpg>)

> 1. Místo čísla místnosti použijeme jako klíč hodnotu parametru (v tomto případě podlaží).

\![](<../../.gitbook/assets/dictionary - exercise II - 02.jpg>)

> 2. Nyní můžete místnosti seskupit podle podlaží, na kterém se nacházejí.

\![](<../../.gitbook/assets/dictionary - exercise II - 03.jpg>)

> 3. Po seskupení místností podle podlaží, můžeme použít sdílené (jedinečné) klíče jako klíče pro slovník a seznamy místností jako prvky.

\![](<../../.gitbook/assets/dictionary - exercise II - 04.jpg>)

> 4. Nakonec můžeme pomocí podlaží v modelu aplikace Revit a pomocí slovníku vyhledat, které místnosti se na daném podlaží nacházejí. Uzel `Dictionary.ValueAtKey` načte jako vstup název podlaží a vrátí objekty místností na daném podlaží.

Možnosti slovníků jsou opravdu nekonečné. Možnost propojit BIM data v aplikaci Revit s prvkem samotným nabízí mnohá využití.
