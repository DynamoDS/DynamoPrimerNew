# Co ještě nástroj Autodesk Assistant neumí

### Aktualizace balíčků

Autodesk Assistant zatím nedokáže stahovat balíčky do aplikace Dynamo a instalovat je.

![](../../.gitbook/assets/Aa_Update.jpg)

Jak je vidět na obrázku výše, Autodesk Assistant nedokáže upravovat, instalovat ani odinstalovat externí balíčky přidružené k aplikaci Dynamo. Může vám však poskytnout pokyny, abyste si je mohli nainstalovat sami.


### Přímé čtení a zápis souborů

Autodesk Assistant pracuje se soubory grafů aplikace Dynamo: dokáže otevřít graf, vložit jej na kreslicí plochu, spustit ho a uložit vaši práci. Nedokáže samostatně číst ani zapisovat jiné soubory na pevném disku.

V níže uvedeném příkladu je na kreslicí ploše vytvořena geometrie. Autodesk Assistant bude vyzván, aby získal body, které tvoří geometrii, a exportoval je ve formátu JSON. Autodesk Assistant sice nedokáže tento soubor JSON vygenerovat přímo, ale navrhne vytvoření skriptu Dynamo, který tento úkol provede.

![](../../.gitbook/assets/Aa_WriteFileGeom.jpg)

![](../../.gitbook/assets/Aa_WriteFile.jpg)

Toto je pravidlo, které si zapamatujte. Vše mimo graf je dostupné prostřednictvím uzlů, nikoli přímo prostřednictvím nástroje Assistant.

### Změna modelu aplikace Revit bez grafu

![](../../.gitbook/assets/Aa_Data_sampleProject.jpg)

Autodesk Assistant pro aplikaci Dynamo nemá žádné přímé připojení k modelu aplikace Revit. Pokud ho požádáte, aby model upravil stejným způsobem, jakým byste žádali nástroj Assistant v aplikaci Revit, sdělí vám, že to nejde.

![](../../.gitbook/assets/Aa_Revit_Limits.jpg)

Model aplikace Revit však upravovat dokáže. Postupuje přitom stejně jako vy: umístí uzly aplikace Revit, propojí je dohromady a model se při spuštění grafu změní.

![](../../.gitbook/assets/Aa_viewScript.jpg)

Z tohoto způsobu práce vyplývají dvě věci:

- **Dokud graf nespustíte, nic se nezmění.** Nejprve si můžete graf prohlédnout a upravit hodnoty. Model se změní teprve při spuštění grafu, ať už ho spustíte vy, nebo Assistant v rámci vašeho požadavku.
- **Změny lze opakovat.** Graf zůstává na kreslicí ploše. Stačí ho uložit a poté můžete stejnou úlohu spustit na jiném modelu nebo graf sdílet s týmem.

Pokud chcete provést jedinou přímou úpravu bez grafu, použijte nástroj Autodesk Assistant v aplikaci Revit.
