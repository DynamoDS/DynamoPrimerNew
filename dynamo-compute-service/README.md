# Služba Dynamo Cloud Compute



Služba Dynamo Cloud Compute přináší výkon vizuálního programovacího prostředí Dynamo do cloudového prostředí. Místo spouštění grafů v místním počítači je jejich zpracování prováděno v zabezpečeném cloudovém prostředí a výsledky jsou vráceny zpět.

## Co je aplikace Dynamo?

Dynamo je vizuální programovací jazyk a tvůrčí prostředí, které umožňuje vytvářet programy propojováním uzlů v grafu. Tyto grafy spouští modul runtime aplikace Dynamo, což umožňuje automatizovat složité úlohy, generovat geometrii a integrovat je s jiným softwarem.

## Jak funguje výpočetní služba

Když používáte Dynamo prostřednictvím cloudového klienta (například Přehrávač skriptů Dynamo v aplikaci Forma), soubory grafu `.dyn` se odesílají do výpočetní služby ke spuštění. Služba:

1. Přijme graf a veškeré vstupní parametry.
2. Spustí graf v izolovaném cloudovém prostředí.
3. Vrátí výsledky zpět do klientské aplikace.

Tento cloudový přístup znamená, že můžete spouštět grafy Dynamo bez místní instalace aplikace Dynamo a současně využívat výpočetní výkon cloudu pro složité úlohy.

## Proč používat službu Dynamo Cloud Compute?

Služba Dynamo Cloud Compute podporuje následující scénáře:

**Spouštění grafů bez instalace desktopové aplikace**: Grafy Dynamo lze spouštět přímo z webových aplikací, aniž by uživatelé museli instalovat počítačovou aplikaci Dynamo.

**Spolupráce a sdílení**: Sdílejte grafy s členy týmu, kteří je mohou spouštět prostřednictvím webových rozhraní, jako je Forma, což usnadňuje distribuci automatizovaných pracovních postupů v rámci organizace.

**Využívání cloud computingu**: Využívejte cloudovou infrastrukturu pro výpočetně náročné operace, které by v místních počítačích mohly trvat déle.

**Standardizace spouštěcího prostředí**: Zajistěte konzistentní chování na různých zařízeních a u různých uživatelů spouštěním grafů v řízeném cloudovém prostředí.

**Připojení k aplikaci Forma**: Využijte Dynamo k interakci s rozhraním API aplikace Forma. [Další podrobnosti najdete v tomto příspěvku na blogu.](https://dynamobim.org/design-to-configuration-your-rules-in-forma-and-revit-via-dynamo-part-1/)

## Klíčové vlastnosti

**Spuštění v cloudu**: Grafy jsou spouštěny v cloudu, nikoli v místním počítači. To znamená:
- Ke spouštění grafů není nutné instalovat počítačovou aplikaci Dynamo.
- Přístup ke zdrojům cloud computingu.
- Konzistentní spouštění prostředí pro různé uživatele.

**Zabezpečení**: Grafy jednotlivých uživatelů jsou službou spouštěny v izolovaných prostředích, aby byla zajištěna bezpečnost dat a soukromí. Vaše grafy a data jsou odděleny od ostatních uživatelů.

**Asynchronní zpracování**: Spuštění grafu probíhá asynchronně – klienti odešlou úlohu a mohou kontrolovat její stav, dokud není dokončena. To umožňuje provádět dlouhotrvající výpočty, aniž by docházelo k blokování vašeho pracovního postupu.

## Aktuální dostupnost

Služba Dynamo Cloud Compute je aktuálně dostupná prostřednictvím:
- **Přehrávače skriptů Dynamo v otevřené beta verzi aplikace Forma**: Odesílejte, sdílejte a spouštějte grafy Dynamo přímo ve webovém rozhraní Autodesk Forma.

## Další informace

- [Rozdíly mezi službou Dynamo Cloud Compute a počítačovou aplikací Dynamo](../dynamo-in-forma-beta/dynamo-compute-service-differences-with-desktop-dynamo.md) – Důležité rozdíly, které je třeba zohlednit při tvorbě grafů určených ke spouštění v cloudovém prostředí.
- [Životní cyklus modulu](engine-lifecycle.md) – Informace o podporovaných verzích modulů a jejich životním cyklu.

-----


> **Poznámka: Služba ve verzi Beta**  
Služba Dynamo Cloud Compute je aktuálně ve verzi beta. Harmonogram podpory a zásady aktualizací popsané v tomto dokumentu představují naše aktuální záměry při experimentování se službou a jejím zdokonalování. Nejsou garantované a mohou se měnit na základě zpětné vazby od uživatelů a provozních zkušeností.