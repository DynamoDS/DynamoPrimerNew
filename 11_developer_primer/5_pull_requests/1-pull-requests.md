# Žádosti o přijetí změn

Aplikace Dynamo závisí na kreativitě a angažovanosti své komunity a tým aplikace Dynamo podporuje přispěvatele, aby zkoumali možnosti, testovali nápady a zapojovali komunitu do procesu zpětné vazby. Ačkoli jsou inovace velmi podporovány, změny budou začleněny pouze tehdy, pokud usnadní používání aplikace Dynamo a budou splňovat pokyny definované v tomto dokumentu. Změny s úzce definovanými přínosy nebudou začleněny.

#### Očekávání kladené na žádosti o přijetí změn<a href="#pull-request-expectations" id="pull-request-expectations"></a>

Tým aplikace Dynamo očekává, že žádosti o přijetí změn budou dodržovat následující pravidla:

* Dodržujte naše [standardy kódování](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) a [standardy pojmenování uzlů](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards).
* Při přidávání nových funkci zahrňte testování jednotek.
* Při opravě chyby přidejte test jednotek, který upozorní na to, jak je porušováno aktuální chování.
* Udržujte diskusi zaměřenou na jeden problém. Pokud se objeví nové nebo související téma, vytvořte nový problém.

A několik pokynů, co nedělat:

* Neodesílejte velké žádosti o přijetí změn. Než investujete velké množství času, nahlaste problém a zahajte diskusi, abychom se mohli dohodnout na směru.
* Neodevzdávejte kód, který jste nenapsali. Pokud najdete kód, o kterém si myslíte, že by bylo vhodné jej přidat do aplikace Dynamo, nahlaste problém a než budete pokračovat, zahajte diskusi.
* Neodesílejte žádosti o přijetí změn, které mění soubory nebo záhlaví související s licencemi. Pokud se domníváte, že je s nimi nějaký problém, nahlaste jej a my jej s vámi rádi projednáme.
* Nepřidávejte rozhraní API, aniž abyste nejprve nahlásili problém a prodiskutovali jej s námi.

#### Vyplnění šablony žádosti o přijetí změn <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

Při odesílání žádosti o přijetí změn použijte [výchozí šablonu žádosti o přijetí změn](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Před odesláním své žádosti o přijetí změn se ujistěte, že je jasně popsán účel a jsou splněna všechna následující prohlášení:

* Základ kódu bude po této žádosti o přijetí změn v lepším stavu.
* Základ kódu je dokumentován podle [standardů](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards).
* Úroveň testování, kterou tato žádost o přijetí změn zahrnuje, je přiměřená.
* Případné řetězce zaměřené na uživatele jsou extrahovány do souborů `*.resx`.
* Všechny testy projdou pomocí samoobslužného CI.
* Je pořízek snímek případných změn uživatelského rozhraní.
* Změny rozhraní API se řídí pravidly pro [sémantické verze](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions) a jsou zdokumentovány v dokumentu popisujícím [změny rozhraní API](https://github.com/DynamoDS/Dynamo/wiki/API-Changes).

Tým aplikace Dynamo přiřadí vaší žádosti o přijetí změn příslušného posuzovatele.

#### Proces kontroly žádosti o přijetí změn <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Po odeslání žádosti o přijetí změn bude možná nutné, abyste zůstali zapojeni do procesu přezkoumání. Mějte na paměti následující kritéria kontroly:

* Tým aplikace Dynamo se schází jednou měsíčně, aby zkontroloval žádosti o přijetí změn, od nejstarších po nejnovější.
* Pokud kontrolovaná žádost o přijetí změn vyžaduje změny ze strany vlastníka, má vlastník žádosti o přijetí změn 30 dní na odpověď. Pokud do příštího zasedání nebude ohledně žádosti o přijetí změn zaznamená žádná aktivita, bude žádost buď týmem uzavřena, nebo ji v závislosti na její užitečnosti převezme někdo z týmu.
* Žádosti o přijetí změn by měly používat výchozí šablonu žádosti o přijetí změn aplikace Dynamo.
* Žádosti o přijetí změn, které nemají kompletně vyplněné šablony se všemi prohlášeními, nebudou přezkoumány.

#### Výběr příspěvků pro doplněk DynamoRevit <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Vzhledem k tomu, že na je trhu k dispozici více verzí aplikace Revit, může být nutné provést výběr změn do konkrétních větví doplňku DynamoRevit, aby nové funkce mohly využívat různé verze aplikace Revit. Během procesu kontroly budou přispěvatelé zodpovědní za výběr svých kontrolovaných příspěvků do ostatních větví doplňku DynamoRevit, jak určí tým aplikace Dynamo.
