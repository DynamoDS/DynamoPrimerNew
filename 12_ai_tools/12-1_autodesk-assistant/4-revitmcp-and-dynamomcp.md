# RevitMCP a DynamoMCP

Autodesk Assistant využívá funkce poskytované servery MCP. Každý server mu poskytuje sadu nástrojů a každý nástroj provádí jednu úlohu v produktu, ke kterému patří.

Při práci v aplikaci Revit jsou důležité tyto dva servery: **RevitMCP** a **DynamoMCP**. Nejde o konkurenční možnosti a nemusíte si mezi nimi vybírat. Každý funguje na jiné úrovni a Assistant používá ten, který nejlépe odpovídá vašemu požadavku.

### RevitMCP: přímé operace s modelem

RevitMCP, poskytovaný jako [veřejný server MCP aplikace Revit](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm), zpřístupňuje vybranou sadu operací rozhraní Revit API prostřednictvím nástrojů MCP. Každý nástroj odpovídá určité akci v modelu, jako je hledání prvků, kontrola parametrů a počtů, hromadná úprava parametrů nebo pořizování snímků pohledů.

Jedná se o nejrychlejší způsob provedení úlohy, kterou již dostupná sada nástrojů podporuje. Stačí jeden požadavek a jedno volání a model se okamžitě změní. Není nutné vytvářet graf ani ho následně spouštět.

### DynamoMCP: nástroje pro sestavení operace

DynamoMCP pracuje na vyšší úrovni. Jeho nástroje neprovádějí akce modelování přímo. Místo toho vytvoří graf aplikace Dynamo: umístí uzly, propojí je, nastaví hodnoty a spouštějí výsledný graf. Úplný seznam nástrojů naleznete v části [Co je server MCP aplikace Dynamo](../12-2_dynamo-mcp/1-what-is-dynamo-mcp.md).

To je důležité vzhledem k tomu, co uzel představuje. Každý uzel v knihovně Dynamo aplikace představuje jedno nebo více volání rozhraní API, která jsou již napsaná, otestovaná a zdokumentovaná. Totéž platí pro uzly z balíčků a uzly jazyka Python. I malá sada nástrojů pro vytváření grafů tak zpřístupňuje celou knihovnu aplikace Dynamo.

Výsledkem je, že nástroje DynamoMCP umožňují vytvářet nové funkce, nikoli pouze využívat ty existující. Místo pevného seznamu akcí sestaví Assistant požadovanou akci z uzlů. V praxi se nástroje MCP, které vytvářejí grafy, chovají jako nástroje, které vytvářejí další nástroje.

### Rozdíl v praxi

Řekněme, že požádáte o popisky dveří na jednom podlaží.

- Pokud úlohu zpracuje server RevitMCP, provede se přímé volání a zobrazí se popisky.
- Jestliže úlohu zpracuje server DynamoMCP, Assistant umístí uzly, které vyhledají dveře, vyfiltrují je podle zadaného podlaží, vytvoří jejich popisky a následně spustí graf.

Oba postupy opatří dveře popisky. Druhý navíc zanechá na kreslicí ploše graf.

| | RevitMCP | DynamoMCP |
| --- | --- | --- |
| Co nástroj dělá | Provede definovanou operaci aplikace Revit. | Vytvoří a spustí graf aplikace Dynamo. |
| Rozsah | Operace obsažené v sadě nástrojů | Libovolný uzel v knihovně, včetně uzlů z balíčků a uzlů jazyka Python |
| Co získáte | Změnu v modelu | Změnu v modelu plus graf, který ji provedl |
| Vhodné pro | Jedinou jasně definovanou úlohu | Logiku, kterou chcete zkontrolovat, upravit, znovu použít nebo sdílet |

### Proč se vyplatí mít graf

Graf zaznamenává celý postup práce, nejen její výsledek. Jakmile se objeví na kreslicí ploše, můžete provádět následující akce:

- Prohlédnout si ho a zjistit, co přesně se provedlo.
- Změnit hodnotu a spustit graf znovu, aniž byste museli znovu vytvářet požadavek na provedení úlohy.
- Uložit ho a použít v dalším projektu nebo jej sdílet s kolegou.
- Publikovat ho v Přehrávači skriptů Dynamo, aby graf mohli spustit i uživatelé, kteří aplikaci Dynamo nepoužívají.

Nejvíce času zabere první zadání požadavku. Každé další spuštění už je téměř bezpracné.

{% hint style="info" %}
V prostředí Dynamo Sandbox má Assistant přístup pouze k serveru DynamoMCP, protože zde není žádný model aplikace Revit, se kterým by bylo možné pracovat. V aplikaci Revit jsou k dispozici obě možnosti.
{% endhint %}

### Správná formulace požadavku

V pokynu nemusíte uvádět název serveru. Stačí popsat požadovaný výsledek a Assistant vybere nástroje.

Chcete-li konkrétně vytvořit graf, výslovně to uveďte. Například pokyn *„Vytvoř graf, který opatří dveře na podlaží 2 popisky“* jasně říká, že požadujete opakovaně použitelné řešení, nikoli pouze jednorázové označení dveří.

### Další informace

- [Autodesk Assistant (Technický náhled)](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-68D8FE6D-C5B0-4503-AE27-02C715BAC25B.htm) – Informace o funkcích Assistant v aplikaci Revit, nápověda k aplikaci Revit 2027.
- [Veřejný server MCP aplikace Revit (technický náhled)](https://help.autodesk.com/cloudhelp/2027/ENU/Revit-WhatsNew/files/GUID-97697CBF-0E11-484E-96E5-4277E3E8D61F.htm) – Server MCP pro aplikaci Revit, nápověda k aplikaci Revit 2027.
- [Autodesk Assistant: přehled a nejčastější dotazy](https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Autodesk-Assistant-Supported-Products-and-How-to-Use-It.html) – Které produkty obsahují nástroj Assistant a jak jej používat.
