# Aktualizace nebo přidání nové závislosti v aplikaci Dynamo

### Kdy se tato wiki použije
Při práci na nové funkci nebo při aktualizaci stávající závislosti byste měli před vložením nové závislosti do úložiště aplikace Dynamo vyhodnotit následující skutečnosti.

### Skutečnosti ke zvážení:
1. Jaká je licence nové nebo aktualizované závislosti. Bez konzultace s právním oddělením společnosti Autodesk jsou schváleny pouze některé licence open source.
    * Po vyřešení otázky licence se ujistěte, že jsou v interní wiki zaznamenány informace o závislostech a verzi.
    * Pokud je licence `LGPL`, `GPL` nebo `Apache`, musí být licenční soubor zkopírován do podsložky _Open Source Licenses_ sestavení aplikace Dynamo.
    * Jestliže je licence `LGPL`, musí být na stránku [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution) nahrán úplný zdrojový kód všech komponent třetích stran spolu s textovými kopiemi jejich příslušných licencí open source.
2. Změnil se při aktualizaci typ licence oproti předchozí verzi?
3. Je závislost multiplatformní? 
    * Má nativní komponenty (jako `CEFSharp` nebo `ImageMagick`)? Pokud ano, ztíží to nasazení napříč platformami.
    * Obsahuje reference pouze na systém Windows? Pokud ano, neměla by to být závislost na komponentě Dynamo Core nebo jiných multiplatformních částech Dynama (hladina modelu).
4. Je závislost při sestavení správně přibalena do složky bin se všemi požadovanými závislostmi?
    * Jsou při aktualizaci v důsledku aktualizace odebrány některé soubory? Je tato verze aplikace Dynamo určena pro minoritní verze hostitelských produktů? Pokud ano, budete muset ponechat staré binární soubory až do vydání globální (hlavní) verze, abyste mohli podporovat instalační programy oprav. Viz podrobnosti [zde](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me).
5. Je závislost nebo její strom závislostí v konfliktu s jinými existujícími závislostmi v aplikaci Dynamo?
6. ?? Je závislost nebo její strom závislostí v konfliktu s existujícími závislostmi v produktech, které integrují aplikaci Dynamo do procesu (Revit, Civil atd.)? **To je důležité, protože tyto problémy lze odhalit až v době integrace, pokud není předem provedena kontrola.**