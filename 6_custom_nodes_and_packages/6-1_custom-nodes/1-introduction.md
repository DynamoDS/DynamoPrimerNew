# Úvod do práce s vlastními uzly

Vlastní uzly vznikají tak, že se do nich zahrnou ostatní uzly (i další vlastní uzly), proto o nich lze přemýšlet jako o kontejnerech. Pokud tento kontejnerový uzel v grafu spustíte, spustí se všechny uzly v něm – pomocí vlastních uzlů tak můžete opakovaně používat a sdílet užitečné posloupnosti uzlů.

### Přizpůsobování změnám

Pokud v grafu existuje více kopií vlastního uzlu, po úpravě základního vlastního uzlu se všechny tyto kopie aktualizují. Díky tomu můžete graf snadno aktualizovat a přizpůsobit jej změnám v pracovním postupu nebo návrhu.

### Sdílení práce

Velmi užitečnou funkcí vlastních uzlů je snadné sdílení práce. Pokud zručný uživatel vytvoří složitý graf aplikace Dynamo a předá jej konstruktérovi, který je v aplikaci Dynamo nový, může tento graf zjednodušit do základní podoby. Vlastní uzel je možné otevřít a upravit jeho vnitřní graf, může však vypadat i jako jednoduchý „kontejner“. Pomocí vlastních uzlů mohou uživatelé aplikace Dynamo uchovat svůj graf v přehledné a intuitivní podobě.

![](../images/6-1/1/customnodeintro-worksharing01.jpg)

### Různé způsoby tvorby uzlu

Vlastní uzly je možné v aplikaci Dynamo vytvářet mnoha způsoby. V příkladech této kapitoly vytvoříme vlastní uzly přímo v uživatelském rozhraní aplikace Dynamo. Pokud jste programátor a zajímá vás formátování C# nebo Zero-Touch, naleznete podrobnější informace na [této stránce](https://github.com/DynamoDS/Dynamo/wiki/How-To-Create-Your-Own-Nodes) na Wiki aplikace Dynamo.

### Prostředí vlastních uzlů a vytvoření prvního vlastního uzlu

V této části přejdeme do prostředí vlastního uzlu a vytvoříme jednoduchý uzel pro výpočet procent. Prostředí vlastního uzlu se od prostředí grafu aplikace Dynamo liší, ale práce v něm je prakticky stejná. Pojďme tedy vytvořit první vlastní uzel.

Chcete-li vytvořit nový vlastní uzel, spusťte aplikaci Dynamo a vyberte možnost Vlastní uzel, případně na pracovní ploše stiskněte kombinaci kláves Ctrl+Shift+N.

![](../images/6-1/1/customnodeintro-customnodeenvironment01.jpg)

V dialogu Vlastnosti vlastního uzlu zadejte název, popis a kategorii.

![](../images/6-1/1/customnodeintro-customnodeenvironment02.jpg)

> 1. **Název:** Procento
> 2. **Popis:** Vypočítá procentuální podíl jedné hodnoty vůči druhé hodnotě.
> 3. **Kategorie:** Math.Functions

Tím se otevře pracovní plocha se žlutým pozadím, která naznačuje, že pracujete ve vlastním uzlu. Na této pracovní ploše máte přístup ke všem základním uzlům aplikace Dynamo a také k uzlům v kategoriích Input a Output, které slouží k nastavení vstupu a výstupu dat u vlastních uzlů. Lze je najít v nabídce Input > Basic.

![](../images/6-1/1/customnodeintro-customnodeenvironment03.jpg)

![](../images/6-1/1/customnodeintro-customnodeenvironment04.jpg)

> 1. **Vstupy:** Vstupní uzly na vlastních uzlech vytvářejí vstupní porty. Syntaxe pro vstupní port je _název_vstupu : datovýtyp = výchozí_hodnota(volitelná)._
> 2. **Výstupy:** Podobně jako u vstupních uzlů, tyto vstupy na vlastních uzlech vytvářejí výstupní porty. Ke vstupním a výstupním portům je možné přidat **vlastní komentář** s informacemi o vstupech a výstupech. Vlastní komentáře jsou podrobněji popsány podrobněji v části [Tvorba vlastních uzlů](2-creating.md).

Tento vlastní uzel je možné uložit jako soubor .dyf (a nikoliv standardní .dyn). To způsobí, že bude automaticky přidán do relace a všech budoucích relací. Vlastní uzel najdete ve své knihovně v části Doplňky.

![](../images/6-1/1/customnodeintro-customnodeenvironment05.jpg)

### Pokračování

Nyní, když jsme vytvořili první vlastní uzel, se v dalších částech podrobněji podíváme na funkce vlastních uzlů a na to, jak publikovat obecné pracovní postupy. V další části vyvineme vlastní uzel, který přenese geometrii z jedné plochy na druhou.
