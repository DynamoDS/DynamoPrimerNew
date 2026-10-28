# Co je server MCP aplikace Dynamo

**MCP** je zkratka pro Model Context Protocol. Jedná se o otevřený standard, který umožňuje připojit modely AI k softwaru prostřednictvím společného protokolu. Server MCP zpřístupňuje nástroje a data programu, aby AI klient mohl číst živý kontext, provádět akce a vracet strukturované výsledky.

**DynamoMCP** je integrovaný server MCP aplikace Dynamo. Propojuje nástroj Autodesk Assistant s aktivní relací aplikace Dynamo – grafem, uzly, dráty a stavem pracovního prostoru, takže pomoc AI vychází z vašeho skutečného pracovního kontextu.

Pro uživatele aplikace Dynamo to znamená, že Assistant dokáže více než jen odpovídat na obecné otázky. Může zkontrolovat otevřený graf, pomáhat vyřešit problémy s uzly a upravovat kreslicí plochu na základě reálného kontextu relace namísto ručního kopírování a vkládání.

Server DynamoMCP je k dispozici v aplikaci Dynamo 4.2.0 a novějších verzích.

### Jak používat DynamoMCP

Server DynamoMCP používáte prostřednictvím nástroje Autodesk Assistant – neexistuje žádné samostatné nastavení ani samostatné rozhraní. Assistant slouží jako vstupní bod pro kladení otázek, získávání nápovědy ke grafu a umožňuje umělé inteligenci pracovat s aktuálním kontextem aplikace Dynamo.

Místo, kde Assistant otevřete, závisí na tom, kde právě pracujete:

- **Dynamo Sandbox** – Nástroje Dynamo MCP jsou k dispozici v nástroji Autodesk Assistant v aplikaci Dynamo.
- **Revit** – Nástroje Dynamo MCP jsou k dispozici v nástroji Autodesk Assistant v aplikaci Revit.

V obou případech zůstanete v aktuální relaci a k DynamoMCP přistupujete prostřednictvím stejného rozhraní nástroje Assistant.
### Možnost serveru DynamoMCP

DynamoMCP poskytuje nástroji Assistant sadu **nástrojů**. Každý nástroj provádí v aplikaci Dynamo jednu úlohu, například čte obsah kreslicí plochy, vkládá uzel nebo spouští graf Tyto nástroje nikdy nevoláte sami. Stačí běžným jazykem popsat, co chcete, a Assistant vybere nástroje, které potřebuje.

Znalost dostupných nástrojů je přesto užitečná. Díky tomu víte, co Autodesk Assistant dokáže, což vám pomáhá formulovat požadavky, které může provést.

{% hint style="info" %}
Názvy nástrojů začínající na `get_` pouze čtou informace z relace. Všechny ostatní nástroje mohou graf změnit.
{% endhint %}

#### Čtení aktuálního grafu

Tyto nástroje umožňují nástroji Assistant zjistit, na čem pracujete, ještě předtím, než odpoví nebo provede nějakou změnu.

| Nástroj | Co dělá |
| --- | --- |
| `get_workspace_info` | Vypíše seznam položek na kreslicí ploše: uzly, dráty, skupiny, poznámky a podrobnosti o pracovním prostoru. |
| `get_selection` | Informuje o vybraných uzlech, drátech a skupinách. |
| `get_node_connections` | Zobrazí propojení uzlu s ostatními uzly, port po portu. |
| `get_node_property` | Zobrazí nastavení uzlů, například náhled, zmrazení, připnutý náhled a popisky. |
| `get_node_lacing` | Zobrazí nastavení vázání uzlu. |
| `get_installation_info` | Zobrazí vaši verzi aplikace Dynamo, hostitele, na kterém je spuštěna, a odkaz na poznámky k verzi. |

*Příklad pokynu: „Co je právě teď na kreslicí ploše?“*

#### Vyhledání správného uzlu

Aplikace Dynamo obsahuje tisíce uzlů, včetně uzlů z nainstalovaných balíčků. Následující nástroje pomáhají nástroji Assistant vybrat správný uzel místo hádání.

| Nástroj | Co dělá |
| --- | --- |
| `get_nodes_info` | Prohledává knihovnu uzlů včetně uzlů z balíčků a vrací přesný uzel, který se má vložit. |
| `get_node_help` | Vrací dostupnou nápovědu k uzlu a cestu k ukázkovému grafu, pokud existuje. |
| `get_type_schema` | Vrátí očekávaný formát dat pro typ aplikace Dynamo, například bod, číslo nebo barvu. |

*Příklad pokynu: „Který uzel mám použít k získání plochy povrchu?“*

#### Vytvoření a úprava grafu

Tyto nástroje slouží k provádění operací na kreslicí ploše.

| Nástroj | Co dělá |
| --- | --- |
| `create_nodes` | Umístí na kreslicí plochu jeden nebo více uzlů, případně i s počátečními hodnotami. |
| `set_node_value` | Nastavuje hodnotu vstupních uzlů, jako jsou Number, String, Code Block a rozevírací seznamy. |
| `connect_nodes` | Připojí výstupní port ke vstupnímu portu. |
| `disconnect_nodes` | Odstraní dráty mezi dvěma uzly. |
| `delete_node` | Odebere uzel z kreslicí plochy. |
| `rename_node` | Přejmenuje vstupní uzly a uzly Watch tak, aby byl jasný jejich účel. Názvy funkčních uzlů zůstávají beze změny. |
| `select_nodes` | Vybere uzly na kreslicí ploše, abyste viděli, které z nich Assistant zmiňuje. |
| `set_node_property` | Zapne nebo vypne nastavení uzlů: náhled, zmrazení, připnutý náhled, popisky a vstup nebo výstup. |
| `set_node_lacing` | Nastaví vázání na některou z dostupných možností: Auto, Nejkratší, Nejdelší nebo Kartézský součin. |
| `set_node_port_list_at_level` | Nastaví úroveň seznamu na vstupním portu. |

*Příklad pokynu: „Přidej uzel Number Slider a připoj ho ke vstupu poloměru kružnice.“*

#### Uspořádání a dokumentace grafu

Graf, který funguje, nemusí být vždy přehledný. Tyto nástroje umožňují upravovat rozvržení, seskupovat uzly, měnit barvy a přidávat poznámky.

| Nástroj | Co dělá |
| --- | --- |
| `create_group` | Vytvoří skupinu kolem sady uzlů. |
| `create_groups` | Vytvoří několik skupin v jediném kroku. |
| `ungroup_by_guid` | Odebere skupinu. Uzly uvnitř zůstanou na kreslicí ploše. |
| `ungroup_by_title` | Odebere skupiny podle jejich názvu. Uzly uvnitř nich zůstanou na kreslicí ploše. |
| `get_available_group_styles` | Vypíše seznam stylů skupin, které můžete použít, včetně vlastních stylů z vašich předvoleb. |
| `assign_group_styles` | Použije styly skupin, jako jsou vstupy, akce, výstupy a kontrola. |
| `get_notes` | Vypíše poznámky v grafu. |
| `create_notes` | Přidá poznámky, které vysvětlují jednotlivé části grafu. |
| `update_notes` | Upraví text poznámky nebo jej přesune. |
| `delete_notes` | Odstraní poznámky. |
| `set_note_state` | Připne poznámku k uzlu, aby se přesouvaly společně, nebo připnutí zruší. |
| `auto_layout_workspace` | Změní uspořádání uzlů pomocí vestavěného automatického rozvržení aplikace Dynamo. |
| `set_workspace_description` | Zapíše popis grafu do vlastností souboru. |

*Příklad pokynu: „Seskup tento graf podle vstupů, akcí a výstupů, odliš skupiny barvami a přidej k nim stručné poznámky.“*

#### Spuštění grafu a kontrola výsledků

| Nástroj | Co dělá |
| --- | --- |
| `run_workspace` | Spustí aktuální graf. V automatickém režimu se graf spouští znovu při každé změně, takže je tento krok vynecháván. |
| `get_workspace_warnings` | Vypíše seznam uzlů s upozorněními nebo chybami, text zprávy a doporučené další kroky. Graf je nutné nejprve spustit. |
| `get_node_output_values_shapes` | Vypíše souhrn výstupních hodnot jednoho nebo všech uzlů v grafu. |
| `get_node_output_value` | Načte úplný výstup jednoho uzlu postupně po jednotlivých stránkách, pokud jsou výsledky příliš rozsáhlé pro shrnutí. |

*Příklad pokynu: „Spusť graf a řekni mi, proč se u uzlu List.GetItemAtIndex zobrazuje upozornění.“*

#### Práce se soubory grafů

Assistant umí pracovat i s grafy na disku, nejen s aktuálně otevřeným grafem.

| Nástroj | Co dělá |
| --- | --- |
| `new_workspace` | Otevře nový, prázdný graf. Pokud aktuální graf obsahuje neuložené změny, budete vyzváni k jejich uložení nebo zrušení. |
| `save_workspace` | Uloží aktuální pohled. Pokud není zadáno umístění, uloží jej do složky Dynamo v Dokumentech. |
| `edit_graph_from_path` | Otevře uložený graf pro úpravy, ale nespustí jej. |
| `insert_graph_from_path` | Sloučí uložený graf s aktuální kreslicí plochou. Tímto způsobem se přidávají například ukázkové grafy k uzlům. |
| `run_graph_from_path` | Spustí uložený graf s volitelnými vstupními hodnotami, které zadáte. |
| `run_graph_from_json` | Spustí graf dodaný jako formát JSON aplikace Dynamo bez souboru na disku. |
| `get_graph_info_from_path` | Načte vstupy, výstupy a metadata uloženého grafu bez jeho otevření. |
| `get_graph_info_from_json` | Načte stejné informace z grafu předaného ve formátu Dynamo JSON. |
| `get_graphs_info_from_path` | Prohledá složku s grafy a oznámí, co každý z nich obsahuje. |
| `get_available_graphs_folders` | Vypíše složky, které aplikace Dynamo zná a které mohou obsahovat grafy. |

*Příklad pokynu: „Prohledej složku s grafy Dynamo a vypiš grafy, které umísťují stěny.“*

{% hint style="warning" %}
Assistant může provádět změny v grafu. Než požádáte o rozsáhlejší úpravy, uložte svou práci a před přepsáním existujícího souboru vždy zkontrolujte výsledek.
{% endhint %}
