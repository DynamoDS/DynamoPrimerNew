# Efektivní práce s velkými sadami dat v aplikaci Dynamo

Na této stránce se seznámíte s některými pravidly pro efektivní práci s velkými sadami dat v aplikaci Dynamo. Tyto tipy můžete použít k identifikaci kritických bodů v grafech a zajistit, aby se váš graf spustil během několika minut, nikoli hodin.

Obsah:
* Generování geometrie vs. mozaikování
* Využití paměti
* Rozhraní API aplikace Revit

### Generování geometrie vs. mozaikování

Vytvoření prvku geometrie a jeho vykreslení jsou v aplikaci Dynamo dvě zcela odlišné události. Obecně platí, že vytvoření geometrie je mnohem rychlejší a spotřebuje méně paměti než vykreslení objektu. Geometrii si můžete představit jako seznam rozměrů potřebných k vytvoření obleku, zatímco mozaikování je samotný oblek. Z rozměrů obleku se toho dá zjistit poměrně hodně: jak dlouhé jsou paže, kolik stojí atd., ale téměř vždy je třeba hotový oblek vidět a vyzkoušet si ho, abyste zjistili, zda je v pořádku nebo ne. Podobně u nemozaikované geometrie můžete určit její ohraničující kvádr, plochu, objem; protínat ji s jinou geometrií a exportovat do formátu SAT nebo Revit. Téměř vždy však musíte geometrii mozaikovat, abyste ověřili, zda je správná nebo ne. 

Jestliže graf aplikace Dynamo obsahuje mnoho objektů a během provádění se zpomaluje, můžete z grafu odstranit kroky mozaikování, a postup tak urychlit.  

Uzly geometrie v aplikaci Dynamo jsou vždy mozaikovány*. To vám ponechává dvě možnosti práce s nemozaikovanou geometrií: uzly Python a uzly ZeroTouch. Dokud nevrátíte objekt geometrie z uzlu Python nebo ZeroTouch, geometrie nebude mozaikována. Pokud má váš graf například několik uzlů bodů, které jsou spojeny s několika uzly čar, které jsou spojeny s několika uzly šablonování, které jsou spojeny s několika uzly zesílení, geometrie bude v každém kroku mozaikována. Místo toho můžete tuto logiku spojit do uzlu Python nebo ZeroTouch a vrátit z uzlu pouze konečný objekt.

Více informací o používání uzlů ZeroTouch naleznete v části [Vývoj pro aplikaci Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) této příručky.

### Využití paměti

Pokud již geometrii nemozaikujete, může dojít k omezení paměti kvůli nadměrné akumulaci geometrie. Objekty geometrie v aplikaci Dynamo spotřebovávají při vytváření malé, nikoli však nezanedbatelné množství paměti. Pokud pracujete se stovkami tisíc nebo miliony objektů, může to způsobit zhroucení aplikace Dynamo nebo Revit. V aplikaci Dynamo verze 2.5 a novější se tento problém implicitně řeší odstraněním nepoužívaných objektů, pokud však používáte verzi starší než 2.5, pak jedním ze způsobů, jak se vyhnout vytváření velkého množství geometrie, je odstranit objekty, když s nimi skončíte. Řekněme například, že vytváříte stovky tisíc objektů NurbsCurve, z nichž každý vyžaduje desítky bodů. Jedním ze způsobů, jak je vytvořit, je použít dvourozměrný seznam v aplikaci Dynamo, který předáte do uzlu NurbsCurve.ByPoints. Toto řešení však vyžaduje vytvoření milionů bodů. Dalším způsobem je použití uzlu Python nebo ZeroTouch. V tomto uzlu můžete vytvořit desítky bodů, předat je do uzlu NurbsCurve.ByPoints a potom je odstranit voláním metody Dispose(). Více informací o používání uzlů ZeroTouch naleznete v části [Vývoj pro aplikaci Dynamo](11\_developer\_primer/3\_developing\_for\_dynamo/README.md) této příručky. Odstranění objektů geometrie po jejich vytvoření může za určitých okolností výrazně snížit množství použité paměti, a přestože to aplikace Dynamo verze 2.5 a novější zvládne bez zásahu uživatele, doporučujeme, aby uživatelé geometrii i nadále explicitně odstraňovali, pokud případ použití vyžaduje omezení paměti v deterministickém čase. Další informace o nových funkcích stability v aplikaci Dynamo 2.5 naleznete v článku popisujícím [vylepšení stability geometrie v aplikaci Dynamo](https://forum.dynamobim.com/t/dynamo-geometry-stability-improvements-request-for-feedback/39297).

### Rozhraní API aplikace Revit

Pokud agresivně odstraňujete objekty v uzlu ZeroTouch nebo Python a stále dochází k problémům s pamětí nebo výkonem, může být nutné aplikaci Dynamo zcela obejít a vytvářet objekty aplikace Revit přímo prostřednictvím rozhraní API. Můžete například analýzou souboru aplikace Excel získat informace o bodech a použít tyto informace k vytvoření prvků XYZ a dalších prvků aplikace Revit pomocí jejich rozhraní API. V tomto okamžiku se aplikace Revit stane konečným kritickým bodem, kterému se nelze vyhnout.