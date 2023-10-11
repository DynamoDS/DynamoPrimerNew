# Vazby objektů

Aplikace Dynamo for Civil 3D obsahuje velmi výkonný mechanismus pro „zapamatování“ objektů vytvořených jednotlivými uzly. Tento mechanismus se nazývá **vazby objektů** a umožňuje grafu aplikace Dynamo vytvářet konzistentní výsledky při každém spuštění ve stejném dokumentu. I když je to v mnoha situacích velmi žádoucí, existují jiné situace, kdy budete chtít mít nad chováním aplikace Dynamo větší kontrolu. Tato část vám pomůže pochopit, jak vazby objektů fungují a jak je můžete využít.

## Příklad

Podívejme se na tento graf, který vytváří kružnici v modelovém prostoru v aktuální hladině.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>Jednoduchý graf k vytvoření kružnice</p></figcaption></figure>

Všimněte si, co se stane, když se poloměr změní.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Změna vstupu poloměru v aplikaci Dynamo</p></figcaption></figure>

Toto je vazba objektů v akci. Aplikace Dynamo se ve výchozím nastavení chová tak, že _upraví_ poloměr kružnice, místo aby při každé změně vstupu poloměru vytvořila novou kružnici. Je tomu tak proto, že uzel **Object.ByGeometry** si „pamatuje“, že při každém spuštění grafu vytvořil tuto _konkrétní_ kružnici. Aplikace Dynamo si navíc tuto informaci uloží, takže při příštím otevření dokumentu aplikace Civil 3D a spuštění grafu se bude chovat úplně stejně.

## Jiný příklad

Podívejme se na příklad, ve kterém můžete chtít změnit výchozí chování vazby objektů aplikace Dynamo. Řekněme, že chcete vytvořit graf, který umístí text do středu kružnice. Vaším záměrem je však, aby graf mohl být spouštěn stále dokola a přitom se pokaždé umístil nový text do jakékoli vybrané kružnice. Níže vidíte, jak by mohl tento graf vypadat.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>Jednoduchý graf, který umístí text do středu vybrané kružnice</p></figcaption></figure>

Toto se však ve skutečnosti stane, když je vybrána jiná kružnice.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>Výchozí chování aplikace Dynamo při výběru nové kružnice</p></figcaption></figure>

Vypadá to, že text je při každém spuštění grafu odstraněn a znovu vytvořen. Ve skutečnosti se pozice textu _upravuje_ podle toho, která kružnice je vybrána. Jedná se tedy o stejný text, jen na jiném místě. Chcete-li pokaždé vytvořit nový text, je nutné upravit nastavení vazby objektů aplikace Dynamo tak, aby nebyla zachována žádná data vazby (viz část [\#binding-settings](object-binding.md#binding-settings "mention") níže).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Nastavení vazby objektů</p></figcaption></figure>

Po provedení této změny získáme požadované chování.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>Chování s vypnutou vazbou objektů</p></figcaption></figure>

## Nastavení vazby

Aplikace Dynamo for Civil 3D umožňuje upravit výchozí chování vazby objektů pomocí nastavení **Úložiště dat vazeb** v nabídce aplikace **Dynamo**.

{% hint style="info" %}
 Možnosti nastavení Úložiště dat vazeb jsou k dispozici v aplikaci **Civil 3D 2022.1** a vyšších verzích. 
{% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

Ve výchozím nastavení jsou povoleny všechny možnosti. Zde je souhrn toho, co jednotlivé možnosti dělají.

### Možnost 1: Nebyla zachována žádná data vazeb

Pokud je tato povolena možnost, aplikace Dynamo „zapomene“ na objekty, které vytvořila při posledním spuštění grafu. Graf lze tedy spustit v libovolném výkresu v libovolné situaci a pokaždé vytvoří nové objekty.

{% hint style="info" %}
 **Vhodné použití**

Tuto možnost použijte, pokud chcete, aby aplikace Dynamo „zapomněla“ na vše, co provedla v předchozích spuštěních, a pokaždé vytvořila nové objekty. 
{% endhint %}

### Možnost 2: Uložit v grafu pro aplikaci Dynamo

Tato možnost znamená, že metadata vazby objektů budou při ukládání serializována do grafu (soubor .dyn). Pokud graf zavřete nebo znovu otevřete a spustíte jej ve **stejném výkresu**, pak by mělo vše fungovat stejně, jako když jste jej opustili. Jestliže graf spustíte v **jiném výkresu**, budou data vazby z grafu odstraněna a vytvoří se nové objekty. To znamená, že pokud otevřete původní výkres a spustíte graf znovu, vytvoří se kromě starých objektů i nové.

{% hint style="info" %}
 **Vhodné použití**

Tuto možnost použijte, pokud chcete, aby si aplikace Dynamo „zapamatovala“ objekty, které vytvořila při posledním spuštění v **určitém výkresu**. 
{% endhint %}

{% hint style="warning" %}
 Tato možnost je vhodná pro situace, kdy je možné zachovat vztah 1:1 mezi **konkrétním výkresem** a grafem aplikace Dynamo. Možnosti 1 a 3 jsou vhodnější pro grafy, které jsou navrženy tak, aby je bylo možné spouštět ve více výkresech. 
{% endhint %}

### Možnost 3: Uložit ve výkresu pro aplikaci Dynamo

Tato možnost je podobná možnosti 2, s tím rozdílem, že data vazby objektu jsou jsou serializována ve výkresu namísto v souboru .dyn. Pokud graf zavřete nebo znovu otevřete a spustíte jej ve **stejném výkresu**, pak by mělo vše fungovat stejně, jako když jste jej opustili. Jestliže graf spustíte v **jiném výkresu**, data vazby zůstanou zachována v původním výkresu, protože jsou uložena ve výkresu, nikoli v grafu.

{% hint style="info" %}
 **Vhodné použití**

Tuto možnost použijte, pokud chcete použít stejný graf ve **více výkresech** a aplikace Dynamo si má „pamatovat“, co provedla v každém z nich. 
{% endhint %}

### Možnost 4: Uložit ve výkresu pro aplikaci Přehrávač skriptů Dynamo

V první řadě je třeba poznamenat, že tato možnost nemá žádný vliv na interakci grafu s výkresem při spuštění grafu prostřednictvím hlavního rozhraní aplikace Dynamo. Tato možnost se použije _pouze_ tehdy, když je graf spuštěn pomocí Přehrávače skriptů Dynamo.

{% hint style="info" %}
 Pokud je pro vás Přehrávač skriptů Dynamo novinkou, přečtěte si část [dynamo-player.md](../dynamo-player.md "mention"). 
{% endhint %}

Jestliže graf spustíte pomocí hlavního rozhraní aplikace Dynamo a pak jej zavřete a spustíte stejný graf pomocí Přehrávače skriptů Dynamo, vytvoří se nové objekty nad těmi, které byly vytvořeny dříve. Jakmile však Přehrávač skriptů Dynamo graf jednou spustí, serializuje data vazeb objektů ve výkresu. Pokud tedy graf spustíte vícekrát prostřednictvím Přehrávače skriptů Dynamo, bude objekty aktualizovat, místo aby vytvářel nové. Jestliže graf spustíte v Přehrávači skriptů Dynamo v **jiném výkresu**, data vazby zůstanou zachována v původním výkresu, protože jsou uložena ve výkresu, nikoli v grafu.

{% hint style="info" %}
 **Vhodné použití**

Tuto možnost použijte, pokud chcete spustit graf pomocí Přehrávače skriptů Dynamo ve více výkresech a nechat jej, aby si „pamatoval“, co provedl v každém z nich. 
{% endhint %}
