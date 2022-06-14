# Případy použití aplikace Revit

Chtěli jste někdy v aplikaci Revit vyhledávat položky podle jejich dat?

Pokud ano, je pravděpodobné, že jste udělali něco podobného jako v následujícím příkladu.

Na obrázku níže získáme všechny místnosti v modelu aplikace Revit, zjistíme index požadované místnosti (podle čísla místnosti) a nakonec získáme místnost s tímto indexem.

![](<../images/5-5/4/dictionary - collect room in revit model.jpg>)

> 1. Získejte všechny místnosti v modelu.
> 2. Číslo místnosti, kterou chcete najít.
> 3. Získejte číslo místnosti a její index.
> 4. Získejte místnost s daným indexem.

## Cvičení: Slovník místností

### Část I: Vytvoření slovníku místností

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/5-5/4/roomDictionary.dyn" %}

Pojďme udělat totéž pomocí slovníků. Nejdříve je nutné získat všechny místnosti v modelu aplikace Revit.

![](<../images/5-5/4/dictionary - exercise I - 01.jpg>)

> 1. Vybereme kategorii aplikace Revit, se kterou chceme pracovat (v tomto případě místnosti).
> 2. Přikážeme aplikaci Dynamo, aby získala všechny prvky

Dále je nutné rozhodnout, podle jakých klíčů se budou data vyhledávat. (Informace o klíčích naleznete v části [9-1 Co je slovník?)](9-1\_what-is-a-dictionary.md)).

![](<../images/5-5/4/dictionary - exercise I - 02.jpg>)

> 1. Data, která použijeme, jsou čísla místností.

Nyní vytvoříme slovník s danými klíči a prvky.

![](<../images/5-5/4/dictionary - exercise I - 03.jpg>)

> 1. Uzel **Dictionary.ByKeysValues** vytvoří slovník podle odpovídajících vstupů.
> 2. `Keys` musí být řetězce, zatímco `values` mohou být různé typy objektů.

Nyní můžeme načíst místnost ze slovníku pomocí čísla místnosti.

![](<../images/5-5/4/dictionary - exercise I - 04.jpg>)

> 1. Uzel `String` bude klíč, který použijeme k vyhledání objektu ze slovníku.
> 2. Uzel **Dictionary.ValueAtKey** načte objekt ze slovníku.

### Část II: Vyhledání hodnot

Stejným způsobem lze vytvářet slovníky seskupených objektů. Pokud bychom chtěli vyhledat všechny místnosti na daném podlaží, můžeme upravit graf následovně.

![](<../images/5-5/4/dictionary - exercise II - 01.jpg>)

> 1. Místo čísla místnosti použijeme jako klíč hodnotu parametru (v tomto případě podlaží).

![](<../images/5-5/4/dictionary - exercise II - 02.jpg>)

> 1. Nyní můžete místnosti seskupit podle podlaží, na kterém se nacházejí.

![](<../images/5-5/4/dictionary - exercise II - 03.jpg>)

> 1. Po seskupení místností podle podlaží, můžeme použít sdílené (jedinečné) klíče jako klíče pro slovník a seznamy místností jako prvky.

![](<../images/5-5/4/dictionary - exercise II - 04.jpg>)

> 1. Nakonec můžeme pomocí podlaží v modelu aplikace Revit a pomocí slovníku vyhledat, které místnosti se na daném podlaží nacházejí. Uzel `Dictionary.ValueAtKey` načte jako vstup název podlaží a vrátí objekty místností na daném podlaží.

Možnosti slovníků jsou opravdu nekonečné. Možnost propojit BIM data v aplikaci Revit s prvkem samotným nabízí mnohá využití.
