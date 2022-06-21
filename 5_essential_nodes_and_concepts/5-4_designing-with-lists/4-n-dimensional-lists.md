# N-rozměrné seznamy

Postoupíme dále ve složitosti a přidáme do hierarchie ještě více vrstev. Datovou strukturu je možné rozšířit daleko za hranice dvourozměrného seznamu seznamů. Vzhledem k tomu, že seznamy jsou v aplikaci Dynamo položky v nich samotných a jich samotných, je možné vytvořit data s tolika rozměry kolik je jen možné.

Analogie, se kterou budeme pracovat, je založena na ruských matrjoškách. Každý seznam je možné chápat jako kontejner obsahující více položek. Každý seznam má své vlastnosti a také je chápán jako svůj vlastní objekt.

![Matrjošky](../images/5-4/4/145493363\_fc9ff5164f\_o.jpg)

> Sada ruských matrjošek (autor fotografie: [Zeta](https://www.flickr.com/photos/beppezizzi/145493363)) je analogií pro n-rozměrné seznamy. Každá vrstva představuje seznam a každý seznam obsahuje položky. V případě aplikace Dynamo může mít každý kontejner uvnitř více kontejnerů (představujících položky každého seznamu).

N-rozměrné seznamy se těžko vysvětlují vizuálně, v této kapitole jsme však připravili několik cvičení, která se zaměřují na práci se seznamy přesahujícími dva rozměry.

### Mapování a kombinace

Mapování je pravděpodobně nejsložitější součástí správy dat v aplikaci Dynamo a je obzvláště důležité při práci se složitými hierarchiemi seznamů. Pomocí řady cvičení níže si ukážeme, kdy je třeba použít mapování a kombinace, když se data stanou vícerozměrnými.

Předběžná představení uzlů **List.Map** a **List.Combine** najdete v předchozí části. V posledním cvičení níže použijeme tyto uzly na složitou datovou strukturu.

## Cvičení – 2D seznamy – základní

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/5-4/4/n-Dimensional-Lists.zip" %}

Toto cvičení je první v řadě tří, které se zaměřuje na členění importované geometrie. Každá část této řady cvičení zvýší složitost datové struktury.

![Exercise](<../images/5-4/4/n-dimensional lists - 2d lists basic 01.jpg>)

> 1. Začněte souborem .sat ve složce souborů cvičení. Tento soubor můžete načíst pomocí uzlu **File Path**.
> 2. Pomocí uzlu **Geometry.ImportFromSAT** se geometrie importuje do náhledu aplikace Dynamo jako dva povrchy.

V tomto cvičení se pracuje z důvodu zjednodušení pouze s jedním povrchem.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 02.jpg>)

> 1. Výběrem indexu 1 uchopte povrch. Index vyberete pomocí uzlu **List.GetItemAtIndex**.
> 2. Vypněte náhled geometrie v náhledu uzlu **Geometry.ImportFromSAT**.

Dalším krokem je rozdělení povrchu na osnovu bodů.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 03.jpg>)

> 1\. Pomocí **bloku kódu** vložte tyto dva řádky kódu: `0..1..#10;` `0..1..#5;`.
>
> 2\. Připojte dvě hodnoty bloku kódu ke vstupům u a _v_ uzlu **Surface.PointAtParameter**. Změňte _vázání_ tohoto uzlu na _„Vektorový součin“_.
>
> 3\. Výstup odhalí datovou strukturu, která je viditelná také v náhledu aplikace Dynamo.

Dále pomocí bodů z posledního kroku vygenerujte deset křivek podél povrchu.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 04.jpg>)

> 1. Chcete-li se podívat, jak je datová struktura uspořádána, připojte objekt **NurbsCurve.ByPoints** k výstupu uzlu **Surface.PointAtParameter**.
> 2. Nyní můžete vypnout náhled z uzlu **List.GetItemAtIndex**, abyste získali jasnější výsledek.

![](<../images/5-4/4/n-dimensional lists - 2d lists basic 05.jpg>)

> 1. Základní uzel **List.Transpose** převrátí sloupce a řádky seznamu seznamů.
> 2. Pokud se výstup uzlu **List.Transpose** připojí k uzlu **NurbsCurve.ByPoints**, přes povrch bude nyní horizontálně umístěno pět křivek.
> 3. Chcete-li dosáhnout stejného výsledku jako na obrázku, můžete v předchozím kroku vypnout náhled z uzlu **NurbsCurve.ByPoints**.

## Cvičení – 2D seznamy – rozšířené

Zvýšíme složitost. Řekněme, že chcete provést operaci s křivkami vytvořenými v předchozím cvičení. Možná bude užitečné tyto křivky spojit s jiným povrchem a šablonovat mezi nimi. Toto vyžaduje věnování větší pozornosti datové struktuře, ale příslušná logika je stejná.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 01.jpg>)

> 1. Začněte krokem z předchozího cvičení, který izoluje horní povrch importované geometrie pomocí uzlu **List.GetItemAtIndex**.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 02.jpg>)

> 1. Pomocí uzlu **Surface.Offset** odsaďte povrch o hodnotu _10_.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 03.jpg>)

> 1. Stejným způsobem jako v předchozím cvičení definujte _blok kódu_ s těmito dvěma řádky kódu: `0..1..#10;` `0..1..#5;`.
> 2. Tyto výstupy připojte ke dvěma uzlům **Surface.PointAtParameter**, z nichž každý má _vázání_ nastaveno na _„Vektorový součin“_. Jeden z těchto uzlů je připojen k původnímu povrchu, zatímco druhý je připojen k odsazenému povrchu.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 04.jpg>)

> 1. Vypněte náhled těchto povrchů.
> 2. Stejně jako v předchozím cvičení připojte výstupy ke dvěma uzlům **NurbsCurve.ByPoints**. Ve výsledku se zobrazí křivky odpovídající dvěma povrchům.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 05.jpg>)

> 1. Pomocí uzlu **List.Create** je možné kombinovat dvě sady křivek do jednoho seznamu seznamů.
> 2. Všimněte si, že z výstupu máme dva seznamy každý po deseti položkách, přičemž každý z nich představuje připojenou sadu křivek nurbs.
> 3. Provedením operace uzlu **Surface.ByLoft** je možné vizuálně pochopit tuto datovou strukturu. Uzel šablonuje všechny křivky v každém dílčím seznamu.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 06.jpg>)

> 1. Vypněte náhled z uzlu **Surface.ByLoft** v předchozím kroku.
> 2. Pokud použijete uzel **List.Transpose**, nezapomeňte, že se překlopí všechny sloupce a řádky. Tento uzel převede dva seznamy deseti křivek do deseti seznamů dvou křivek. Nyní je každá křivka nurbs vztažena k sousedící křivce na druhém povrchu.
> 3. Pomocí uzlu **Surface.ByLoft** jste vytvořili žebrovanou konstrukci.

Dále si ukážeme alternativní postup k dosažení tohoto výsledku.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 07.jpg>)

> 1. Než začneme, vypněte náhled uzlu **Surface.ByLoft** v předchozím kroku, abyste se vyhnuli nejasnostem.
> 2. Alternativou uzlu **List.Transpose** je **List.Combine**. Tento uzel ovládá _„kombinátor“_ v každém dílčím seznamu.
> 3. V tomto případě uzel **List.Create** použije jako _„kombinátor“_, který vytvoří seznam jednotlivých položek v dílčích seznamech.
> 4. Pomocí uzlu **Surface.ByLoft** získáte stejné povrchy jako v předchozím kroku. Transpozice je v tomto případě jednodušší, pokud však bude datová struktura ještě složitější, uzel **List.Combine** je spolehlivější.

![](<../images/5-4/4/n-dimensional lists - 2d lists advance 08.jpg>)

> 1. Pokud bychom v jednom z předchozích kroků chtěli přepnout orientaci křivek žebrované konstrukce, před připojením k uzlu **NurbsCurve.ByPoints** bychom použili uzel **List.Transpose**. Ten převrátí sloupce a řádky a vytvoří 5 vodorovných žeber.

## Cvičení – 3D seznamy

Je čas postoupit o krok dál. V tomto cvičení budete pracovat s oběma importovanými povrchy a vytvoříte složitou datovou hierarchii. Stále je však naším cílem dokončení této operace pomocí stejné příslušné logiky.

Začněte u importovaného souboru z předchozího cvičení.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 01.jpg>)

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 02.jpg>)

> 1. Stejně jako v předchozím cvičení přidejte pomocí uzlu **Surface.Offset** odsazení o hodnotu _10_.
> 2. Všimněte si, že na výstupu se vytvořily dva povrchy s odsazeným uzlem.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 03.jpg>)

> 1. Stejným způsobem jako v předchozím cvičení definujte **blok kódu** s těmito dvěma řádky kódu: `0..1..#20;` `0..1..#20;`.
> 2. Tyto výstupy připojte ke dvěma uzlům **Surface.PointAtParameter**, z nichž každý má vázání nastaveno na _„Vektorový součin“_. Jeden z těchto uzlů je připojen k původním povrchům, zatímco druhý je připojen k odsazeným povrchům.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 04.jpg>)

> 1. Stejně jako v předchozím cvičení připojte výstupy ke dvěma uzlům **NurbsCurve.ByPoints**.
> 2. Při pohledu na výstup uzlu **NurbsCurve.ByPoints** si všimněte, že se jedná o seznam dvou seznamů, což je složitější struktura než v předchozím cvičení. Data jsou kategorizována podle základních povrchů, čili do strukturovaných dat byla přidána další vrstva.
> 3. Všimněte si, že v uzlu **Surface.PointAtParameter** se již situace stane složitější. V tomto případě máme seznam seznamů seznamů.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 05.jpg>)

> 1. Než budeme pokračovat, vypněte náhled existujících povrchů.
> 2. Pomocí uzlu **List.Create** sloučíme křivky nurbs do jedné datové struktury, čímž vytvoříte seznam seznamů seznamů.
> 3. Připojením uzlu **Surface.ByLoft** získáte verzi původních povrchů, protože každý zůstane ve svém vlastním seznamu vytvořeném z původní datové struktury.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 06.jpg>)

> 1. V předchozím cvičení bylo možné vytvořit žebrovanou konstrukci pomocí uzlu **List.Transpose**. Toto zde nebude fungovat. Transpozice by měla být použita na dvourozměrný seznam a vzhledem k tomu, že máme trojrozměrný seznam, operace „převrácení sloupců a řádků“ nebude fungovat tak snadno. Nezapomeňte, že seznamy jsou objekty, čili uzel **List.Transpose** převrátí naše seznamy bez dílčích seznamů, ale nepřevrátí křivky nurbs o jeden seznam níže v hierarchii.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 07.jpg>)

> 1. Zde bude lépe fungovat uzel **List.Combine**. Když se dojde na složitější datové struktury, chceme použít uzly **List.Map** a **List.Combine**.
> 2. Pokud použijeme uzel **List.Create** jako _„kombinátor“_, vytvoříme datovou strukturu, která nám bude lépe fungovat.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 08.jpg>)

> 1. Datovou strukturu o jeden krok níže v hierarchii je stále třeba transponovat. Toto provedete pomocí uzlu **List.Map**. Funguje jako uzel **List.Combine**, jen má jeden vstupní seznam místo dvou a více.
> 2. Funkce použitá na uzel **List.Map** je **List.Transpose**, která převrátí sloupce a řádky dílčích seznamů v hlavním seznamu.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 09.jpg>)

> 1. Nakonec můžete šablonovat křivky nurbs společně s vhodnou hierarchií dat. Tím vznikne žebrovaná konstrukce.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 10.jpg>)

> 1. Nyní přidáme do geometrie hloubku pomocí uzlu **Surface.Thicken** se vstupním nastavením, jak je znázorněno na obrázku.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 11.jpg>)

> 1. Bylo by vhodné přidat do této struktury podkladový povrch. Přidejte tedy další uzel **Surface.ByLoft** a jako vstup použijte první výstup uzlu **NurbsCurve.ByPoints** ze staršího kroku.
> 2. Protože se náhled stává nepřehledným, vypněte náhled pro tyto uzly kliknutím pravým tlačítkem na každý z nich a zrušením zaškrtnutí políčka Náhled, abyste lépe viděli výsledek.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 12.jpg>)

> 1. A zesílením těchto vybraných povrchů je dokončeno i členění.

Nejedná se zrovna o nejpohodlnější houpací křeslo, ale pracuje se u něj s mnoha daty.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 13.jpg>)

V posledním kroku obrátíme směr žlábkovaných členů. V předchozím kroku jsme použili transpozici, tady uděláme něco podobného.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 14.jpg>)

> 1. Vzhledem k tomu, že hierarchie má ještě další vrstvu, je třeba pomocí uzlu **List.Map** a funkce **List.Tranpose** změnit směr křivek nurbs.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 15.jpg>)

> 1. Mohli bychom chtít zvýšit počet vzorků běhounu, což provedeme změnou **bloku kódu** na `0..1..#20;` `0..1..#30;`.

První verze houpacího křesla byla štíhlá, čili druhý model nabízí robustní off-roadovou verzi usazení.

![](<../images/5-4/4/n-Dimensional-Lists - 3d list 16.jpg>)
