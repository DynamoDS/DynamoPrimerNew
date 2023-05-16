# Rozšíření jako balíčky

### Rozšíření jako balíčky <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### Přehled <a href="#overview" id="overview"></a>

Rozšíření aplikace Dynamo lze rozmístit do nástroje Package Manager stejně jako běžné knihovny uzlů aplikace Dynamo. Pokud nainstalovaný balíček obsahuje rozšíření pohledu, načte se rozšíření za běhu při načítání aplikace Dynamo. Správné načtení rozšíření můžete ověřit v konzoli aplikace Dynamo.

### Struktura balíčku <a href="#package-structure" id="package-structure"></a>

Struktura balíčku rozšíření je stejná jako u běžného balíčku...

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

Za předpokladu, že jste rozšíření již vytvořili, budete mít (minimálně) sestavu rozhraní .NET a soubor manifestu. Sestava by měla obsahovat třídu, která implementuje `IViewExtension` nebo `IExtension`. Soubor manifestu XML informuje aplikaci Dynamo, pro kterou třídu má vytvořit instanci, aby bylo možné spustit vaše rozšíření. Aby nástroj Package Manager správně vyhledal rozšíření, měl by soubor manifestu přesně odpovídat umístění a názvu sestavy.

Umístěte soubory sestav do složky `bin` a soubor manifestu do složky `extra`. Do této složky lze také umístit libovolné další komponenty.

Příklad souboru manifestu XML:

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### Odesílání <a href="#uploading" id="uploading"></a>

Jakmile máte složku obsahující výše uvedené podadresáře, můžete balíček odeslat do nástroje Package Manager. Je třeba si uvědomit, je, že aktuálně nelze publikovat balíčky z aplikace Dynamo Sandbox. To znamená, že je nutné používat aplikaci Dynamo Revit. Po přechodu do aplikace Dynamo Revit přejděte na možnost Balíčky => Publikovat nový balíček. Uživatel bude vyzván k přihlášení ke svému účtu Autodesk, se kterým chce balíček asociovat.

V tomto okamžiku byste měli být v běžném okně pro publikování balíčku, kde zadáte všechna požadovaná pole týkající se balíčku/rozšíření. Existuje jeden **velmi důležitý** další krok, který vyžaduje, abyste se ujistili, že žádný ze souborů sestavy není označen jako knihovna uzlů. To provedete kliknutím pravým tlačítkem na importované soubory (složka balíčku vytvořená výše). Zobrazí se místní nabídka, která vám umožní tuto možnost zaškrtnout (nebo zaškrtnutí zrušit). U všech sestav rozšíření by mělo být zaškrtnutí zrušeno.

![Publikování balíčku](images/ViewExtension_Search.png)

Před veřejným publikováním byste měli vždy publikovat místně, abyste se ujistili, že vše funguje podle očekávání. Po ověření funkčnosti jste připraveni spustit živé publikování výběrem možnosti Publikovat.

### Získání balíčku <a href="#pulling" id="pulling"></a>

Chcete-li ověřit, zda byl balíček úspěšně odeslán, měli byste jej vyhledat podle názvů a klíčových slov zadaných v kroku publikování. Je důležité poznamenat, že rozšíření budou před spuštěním vyžadovat restartování aplikace Dynamo. Obvykle tato rozšíření vyžadují parametry zadané při spuštění aplikace Dynamo.

![Vyhledání balíčků](images/ViewExtension_Search.jpg)
