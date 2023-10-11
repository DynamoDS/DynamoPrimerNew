# Knihovna uzlů

Již dříve jsme uvedli, že **uzly** jsou základními stavebními bloky grafu aplikace Dynamo a jsou uspořádány do logických skupin v **knihovně**. V aplikaci Dynamo for Civil 3D jsou v knihovně dvě kategorie (neboli **police**), které obsahují vyhrazené uzly pro práci s objekty aplikací AutoCAD a Civil 3D, například trasy, profily, koridory, reference bloků atd. Zbytek knihovny obsahuje uzly, které jsou obecnější povahy, a jsou konzistentní mezi všemi verzemi aplikace Dynamo (například Dynamo for Revit, Dynamo Sandbox atd.).

{% hint style="info" %}
 Další informace o uspořádání uzlů v základní knihovně aplikace Dynamo naleznete v části [2-library.md](../3\_user\_interface/2-library.md "mention") . 
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-node-library.png" alt="" width="563"><figcaption><p>Knihovna uzlů v aplikaci Dynamo for Civil 3D</p></figcaption></figure>

> 1. Specifické uzly pro práci s objekty aplikací AutoCAD a Civil 3D
> 2. Obecné uzly
> 3. Uzly z **balíčků** třetích stran, které lze instalovat samostatně.

{% hint style="warning" %}
 Použijete-li uzly, které se nacházejí v policích pro aplikace AutoCAD a Civil 3D, bude graf aplikace Dynamo fungovat pouze v aplikaci Dynamo for Civil 3D. Pokud bude graf pro aplikaci Dynamo for Civil 3D otevřen jinde (například v aplikaci Dynamo for Revit), tyto uzly budou označeny upozorněním a nebudou spuštěny. 
{% endhint %}

{% hint style="info" %}
 **Proč jsou pro aplikace AutoCAD a Civil 3D k dispozici dvě samostatné police?**

Toto uspořádání odlišuje uzly pro nativní objekty aplikace AutoCAD (úsečky, křivky, reference bloků atd.) od uzlů pro objekty Civil 3D (trasy, koridory, povrchy atd.). Z technického hlediska jsou aplikace AutoCAD a Civil 3D dvě samostatné aplikace – AutoCAD je základní aplikace a aplikace Civil 3D je na ní postavena. 
{% endhint %}

## Hierarchie uzlů

Aby bylo možné pracovat s uzly aplikací AutoCAD a Civil 3D, je důležité plně porozumět hierarchii objektů v jednotlivých policích. Pamatujete si taxonomii z biologie? Říše, kmen, třída, řád, čeleď, rod, druh? Objekty aplikací AutoCAD a Civil 3D jsou uspořádány do kategorií podobným způsobem. Vysvětleme si to na několika příkladech.

### Objekty aplikace Civil

Jako příklad použijeme trasu.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment.png" alt=""><figcaption></figcaption></figure>

Řekněme, že vaším cílem je změnit název trasy. Dalším uzlem, který byste zde měli přidal, je uzel **CivilObject.SetName**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-name (1).png" alt=""><figcaption></figcaption></figure>

Zpočátku se to nemusí zdát příliš intuitivní. Co je **CivilObject** a proč knihovna nemá uzel **Alignment.SetName**? Odpověď souvisí s _opakovatelnou použitelností_ a _jednoduchostí_. Pokud se nad tím zamyslíte, proces změny názvu objektu aplikace Civil 3D je stejný, ať už se jedná o trasu, koridor, profil nebo něco jiného. Takže místo opakujících se uzlů, které v podstatě provádějí totéž (například **Alignment.SetName, Corridor.SetName, Profile.SetName** atd.), by bylo vhodné tuto funkci zabalit do jediného uzlu. Přesně to dělá uzel **CivilObject.SetName**!

Jiný způsob, jak o tom přemýšlet, je z hlediska _vztahů_. Trasa a koridor jsou oba typy **objektů aplikace Civil**, stejně jako jablko a hruška jsou oba typy ovoce. Uzly CivilObject se používají u všech typů objektů aplikace Civil, stejně jako můžete chtít použít jednu škrabku k loupání jablek i hrušek. Ve vaší kuchyni by byl pořádný chaos, kdybyste měli pro každý typ ovoce samostatnou škrabku! V tomto smyslu je knihovna uzlů aplikace Dynamo stejná jako vaše kuchyně.

### Objekty

Nyní se posuneme o krok dál. Řekněme, že chcete změnit hladinu trasy. Uzel, který byste použili, je uzel **Object.SetLayer**.

<figure><img src="../.gitbook/assets/c3d-node-library-alignment-set-layer.png" alt=""><figcaption></figcaption></figure>

Proč neexistuje uzel s názvem **CivilObject.SetLayer**? Platí zde stejné zásady opakované použitelnosti a jednoduchosti, o kterých jsme hovořili dříve. Vlastnost _hladina_ je společná pro všechny objekty v aplikaci AutoCAD, které lze nakreslit nebo vložit, jako je úsečka, křivka, text, reference bloku atd. Objekty aplikace Civil 3D, například trasy a koridory, spadají do stejné kategorie, takže jakýkoliv uzel, který se vztahuje k **objektu**, lze použít také s libovolným **objektem aplikace Civil**.

