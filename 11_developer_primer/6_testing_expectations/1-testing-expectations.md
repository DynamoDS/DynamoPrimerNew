# Očekávání při testování

Tato stránka popisuje, co hledáme při testování nového kódu přidávaného do aplikace Dynamo.

Máte nový uzel, který chcete přidat. Výborně. Je čas přidat nějaké testy. Existují dva důvody, proč to udělat.

1. Testování pomáhá zjistit, kde to případně nefunguje.
2. Když někdo jiný změní něco, co způsobí nefunkčnost vašeho uzlu, mělo by to způsobit selhání testování. Osoba, která způsobila neúspěšné testování, pak musí problém opravit. Pokud chybu neodhalí testování, budete se muset sami vypořádat s uživateli, jejichž modely přestanou fungovat.

Testování v aplikaci Dynamo se dělí na dva základní typy: jednotkové testy a systémové testy.

## Jednotkové testy

Jednotkové testy by měly testovat v co nejmenším rozsahu. Pokud jste vytvořili uzel, který počítá odmocniny prostřednictvím uzlu zero touch jazyka C#, měl by test otestovat pouze tuto knihovnu DLL a volat jazyk C# přímo do tohoto kódu. Jednotkové testy by neměly zahrnovat ani aplikaci Dynamo.

Měly by zahrnovat:

* Pozitivní testy (vše funguje správně).
* Negativní testy (při zadání chybného vstupu není hlášena chyba).
* Regresní testy (když někdo najde chybu ve vašem kódu, napište test, který zajistí, že se chyba nebude opakovat).

Testy by měly být malé, rychlé a spolehlivé. Většinu testů by měly tvořit jednotkové testy.

## Systémové testy

Systémové testy pracují s více komponentami a testují jejich vzájemnou funkčnost. Měly by být vytvářeny a používány s opatrností. 

Proč? Jejich provedení je nákladné. Potřebujeme, aby testovací sada běžela s co nejmenší latencí.

Při psaní systémového testu se nejprve podívejte na [tento](https://github.com/DynamoDS/Dynamo/blob/master/doc/system/Layer%20Diagram.pdf) diagram a zjistěte, které podsystémy pokrýváte.

V ideálním případě by měla existovat postupná řada testů pokrývající rostoucí sady vzájemně se ovlivňujících bloků, takže když testy začnou selhávat, lze velmi rychle zjistit, kde je problém.

Příklady, kdy jsou vyžadovány systémové testy:

* Nový typ uzlu aplikace Revit, který ukládá do trasování několik prvků místo jednoho prvku.
* Nový uzel Watch, který zobrazuje data odlišným způsobem.

Příklady, kdy systémové testy nejsou potřeba:

* Nový matematický uzel.
* Knihovna pro zpracování řetězců.

Systémové testy by měly:

* Zajistit správné chování.
* Zajistit nepřítomnost patologického chování, např. žádné výjimky.