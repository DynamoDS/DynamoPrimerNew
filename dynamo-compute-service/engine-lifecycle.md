# Životní cyklus výpočetní služby aplikace Dynamo a frekvence aktualizací



Tento dokument popisuje frekvenci aktualizací a zásady podpory pro službu Dynamo Cloud Compute. V tomto dokumentu může být tato služba také zaměnitelně označována jako „služba“.

Dokument vysvětluje, jak jsou spravovány verze modulů, kdy dochází k aktualizacím a co mohou uživatelé očekávat při spouštění grafů aplikace Dynamo v cloudu.

---

## Frekvence aktualizací

Aby bylo možné vyhovět různým potřebám uživatelů, nabízí služba Dynamo Cloud Compute **dvě samostatné řady modulů**. Každá řada slouží konkrétnímu účelu a řídí se vlastním harmonogramem aktualizací:

### Stabilní modul (produkční verze)

Stabilní modul je navržen s důrazem na spolehlivost a konzistenci v produkčním prostředí. Je založen na nejnovější stabilní verzi modulu runtime DynamoCore a aktualizuje se, když jsou oficiální verze aplikace Dynamo přístupné uživatelům aplikace Dynamo pro stolní počítače. Zpočátku se budeme řídit frekvencí aktualizací doplňku DynamoRevit.

Tato řada je určená pro produkční úlohy, u nichž jsou rozhodující spolehlivost a předvídatelnost. Pokud používáte stabilní modul, můžete očekávat, že aktualizace budou odpovídat veřejnému harmonogramu vydávání aplikace Dynamo, což vám poskytne čas připravit se na změny a otestovat grafy dříve, než ovlivní vaše pracovní postupy.

### Modul ve verzi náhledu (náhled / každodenní sandbox)

Modul ve verzi náhledu umožňuje získat předběžný přístup k nejnovějším novinkám v aplikaci Dynamo. Je založen na nejnovějším vývojovém sestavení modulu runtime DynamoCore a je průběžně aktualizován při přidávání nových funkcí a oprav chyb.

Tato řada je určena uživatelům, kteří si chtějí vyzkoušet nadcházející změny, experimentovat s novými funkcemi před jejich oficiálním vydáním nebo ověřit, že jejich grafy budou fungovat i v budoucích verzích aplikace Dynamo. Modul ve verzi náhledu umožňuje uživatelům průběžně sledovat připravované změny a sdílet připomínky s týmem aplikace Dynamo.

---


## Časová osa podpory

Znalost doby podpory jednotlivých verzí modulu vám pomůže plánovat časová období údržby a aktualizace grafů.

### Stabilní modul

Stabilní modul je aktualizován při vydání nové stabilní verze modulu DynamoCore v aplikaci Revit. Každá stabilní verze zůstane dostupná a podporovaná až do nasazení následující stabilní verze do služby.

Například pokud služba aktuálně používá aplikaci Dynamo 3.6 (stabilní), bude tuto verzi používat až do okamžiku, kdy bude aplikace Dynamo 4.0 obecně dostupná uživatelům (obvykle jako součást vydání aplikace Revit). V tomto okamžiku bude služba aktualizována na verzi Dynamo 4.0 (stabilní).

Tento přístup zajišťuje, že cloudová služba odpovídá prostředí, které většina uživatelů používá v desktopových prostředích.

### Modul ve verzi náhledu

Modul ve verzi náhledu je průběžně aktualizován z nejnovější vývojové větve aplikace Dynamo. S postupujícím vývojem jednotlivých verzí modul ve verzi náhledu tyto změny průběžně přebírá.

Pokud je například aplikace Dynamo 4.1 ve fázi aktivního vývoje, modul ve verzi náhledu může být označen jako „Dynamo Cloud Compute Service 4.1“. Jakmile se vývoj přesune na verzi 4.2, modul začne tyto změny sledovat a může být přejmenován na „Dynamo Cloud Compute Service 4.2“.

Vzhledem k tomu, že se modul ve verzi náhledu často aktualizuje, měli byste počítat s občasnými nekompatibilními změnami nebo experimentálními funkcemi. Tento modul je určen především pro testování a ověřování, nikoli pro produkční pracovní postupy.

---

## Výběr správného modulu

Při rozhodování, kterou řadu modulů použít:

- **Zvolte stabilní**, pokud potřebujete předvídatelné a ověřené chování pro produkční pracovní postupy nebo pokud nasazujete grafy pro koncové uživatele, kteří očekávají konzistentní výsledky.

- **Zvolte verzi náhledu**, jestliže chcete testovat nové funkce v rané fázi, ověřovat, že grafy budou fungovat v nadcházejících verzích, nebo poskytovat zpětnou vazbu k vývoji aplikace Dynamo.

Oba moduly využívají stejný základní modul runtime aplikace Dynamo, rozdíl je v tom, kdy a jak často jsou aktualizovány. 

---

> **Poznámka: Služba ve verzi Beta**  
Služba Dynamo Cloud Compute je aktuálně ve verzi beta. Harmonogram podpory a zásady aktualizací popsané v tomto dokumentu představují naše aktuální záměry při experimentování se službou a jejím zdokonalování. Nejsou garantované a mohou se měnit na základě zpětné vazby od uživatelů a provozních zkušeností.



