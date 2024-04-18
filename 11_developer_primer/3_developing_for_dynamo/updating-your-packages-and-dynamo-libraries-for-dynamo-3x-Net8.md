# Aktualizace balíčků a knihoven aplikace Dynamo pro aplikaci Dynamo 3.x a rozhraní .NET 8

### Úvod <a href="#introduction" id="introduction"></a>

Tato část obsahuje informace o problémech, se kterými se můžete setkat při migraci grafů, balíčků a knihoven do aplikace Dynamo 3.x.

Aplikace Dynamo 3.0 je hlavní verzí, ve které byla změněna nebo odstraněna některá rozhraní API. Největší změnou, která se vás jako vývojáře nebo uživatele aplikace Dynamo 3.x pravděpodobně dotkne, je přechod na rozhraní .NET 8.

Rozhraní .NET je modul runtime, který používá jazyk C#, v němž je aplikace Dynamo napsána. Spolu s ostatními částmi ekosystému Autodesk jsme aktualizovali na moderní verzi tohoto modulu runtime.

Další informace naleznete v [tomto příspěvku na blogu](https://dynamobim.org/dynamo-on-net-8/).
***

### Kompatibilita balíčku <a href="#package-compatibility" id="package-compatibility"></a>

#### Používání balíčků aplikace Dynamo 2.x v aplikaci Dynamo 3.x 
Protože aplikace Dynamo 3.x nyní využívá rozhraní .NET 8, není u balíčků, které byly vytvořeny pro aplikaci Dynamo 2.x (*pomocí rozhraní .NET 48*), zaručeno, že budou fungovat v aplikaci Dynamo 3.x. Při pokusu o stažení balíčku v aplikaci Dynamo 3.x, který byl publikován z verze aplikace Dynamo nižší než 3.0, se zobrazí upozornění, že balíček pochází ze starší verze aplikace. 

**To neznamená, že balíček nebude fungovat.** Jedná se pouze o upozornění, že může dojít k problémům s kompatibilitou, a obecně je vhodné zkontrolovat, zda existuje novější verze, která byla vytvořena speciálně pro aplikaci Dynamo 3.x.

Tento typ upozornění můžete také zaznamenat v souborech protokolu aplikace Dynamo při načítání balíčku. Pokud vše funguje správně, můžete ho ignorovat.

#### Používání balíčků aplikace Dynamo 3.x v aplikaci Dynamo 2.x 

Je velmi nepravděpodobné, že balíček vytvořený pro aplikaci Dynamo 3.x (*pomocí rozhraní .Net 8*) bude fungovat v aplikaci Dynamo 2.x. Při stahování balíčků vytvořených pro novější verze systému Dynamo se při používání starší verze zobrazí upozornění.


