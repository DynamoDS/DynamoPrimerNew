# Funkce

Funkce lze vytvořit v bloku kódu a lze je znovu načíst jinde v definici aplikace Dynamo. Tím se vytvoří další hladina ovládacího prvku v parametrickém souboru a lze ji zobrazit jako textovou verzi vlastního uzlu. V tomto případě je „nadřazený“ blok kódu snadno dostupný a může být umístěn kdekoli na grafu. Nepotřebuje žádné dráty!

### Parent (Nadřazená jednotka)

První řádek obsahuje klíčové slovo „def“, pak název funkce a názvy vstupů v závorkách. Závorky definují tělo funkce. Vrátí hodnotu s „return =“. Uzly bloku kódu, které definují funkci, nemají vstupní nebo výstupní porty, protože se volají z jiných uzlů bloku kódu.

![](<../images/8-1/4/functions parent def.jpg>)

```
/*This is a multi-line comment,
which continues for
multiple lines*/
def FunctionName(in1,in2)
{
//This is a comment
sum = in1+in2;
return sum;
};
```

### Podřazené položky

Volejte funkci s jiným uzlem bloku kódu ve stejném souboru, a to poskytnutím stejného názvu a stejného počtu argumentů. Funguje stejně uzly v knihovně.

![](<../images/8-1/4/functions children call def.jpg>)

```
FunctionName(in1,in2);
```

## Cvičení: Koule podle Z

> Kliknutím na odkaz níže si stáhněte vzorový soubor.
>
> Úplný seznam vzorových souborů najdete v dodatku.

{% file src="../datasets/8-1/4/Functions_SphereByZ.dyn" %}

V tomto cvičení vytvoříme obecnou definici, která vytvoří koule ze vstupního seznamu bodů. Poloměr těchto koulí je řízen vlastností Z každého bodu.

Začneme řadou deseti hodnot v rozsahu od 0 do 100. Tyto položky můžete vložit do uzlů **Point.ByCoordinates** za účelem vytvoření diagonální úsečky.

![](<../images/8-1/4/functions - exercise - 01.jpg>)

Vytvořte **blok kódu** a vložte naši definici.

![](<../images/8-1/4/functions - exercise - 02.jpg>)

> 1. Použijte tyto řádky kódu:
>
>    ```
>    def sphereByZ(inputPt)
>    {
>    
>    };
>    ```
>
> _InputPt_ je název, který jsme zadali k reprezentaci bodů, které budou řídit funkci. Zatím funkce nic nedělá, ale v následujících krocích ji rozšíříme.

![](<../images/8-1/4/functions - exercise - 03.jpg>)

> 1. Přidáme-li funkci **bloku kódu**, umístíme komentář a proměnnou _sphereRadius_, která dotazuje pozici _Z_ každého bodu. Nezapomeňte, že metoda _inputPt.Z_ nevyžaduje jako metoda závorky. Toto je _dotaz_ vlastností existujícího prvku, takže nejsou nutné žádné vstupy:
>
> ```
> def sphereByZ(inputPt,radiusRatio)
> {
> //get Z Value, ise ot to drive radius of sphere
> sphereRadius=inputPt.Z;
> };
> ```

![](<../images/8-1/4/functions - exercise - 04.jpg>)

> 1. Nyní si připomeňme funkci, kterou jsme vytvořili v jiném **bloku kódu**. Pokud dvakrát klikneme na kreslicí plochu a vytvoříme nový _blok kódu_ a zadáme jej do položky _sphereB_, všimneme si, že aplikace Dynamo navrhne funkci _sphereByZ_, kterou jsme definovali. Vaše funkce byla přidána do knihovny intellisense. Působivé.

![](<../images/8-1/4/functions - exercise - 05.jpg>)

> 1. Nyní zavoláme funkci a vytvoříme proměnnou s názvem _Pt_, která bude zahrnovat body vytvořené v dřívějších krocích:
>
>    ```
>    sphereByZ(Pt)
>    ```
> 2. Ve výstupu si všimneme, že máme všechny hodnoty null. Jak je to možné? Když jsme definovali funkci, vypočítali jsme proměnnou _sphereRadius_, ale nedefinovali jsme, co by měla funkce _vrátit_ jako _výstup_. To můžeme opravit v dalším kroku.

![](<../images/8-1/4/functions - exercise - 06.jpg>)

> 1. Důležitý krok je, abychom definovali výstup funkce přidáním řádku `return = sphereRadius;` do funkce _sphereByZ_.
> 2. Nyní vidíme, že výstupem bloku kódu jsou souřadnice Z každého bodu.

Nyní vytvoříme skutečné koule úpravou _nadřazené_ funkce.

![](<../images/8-1/4/functions - exercise - 07.jpg>)

> 1. Nejprve definujeme kouli pomocí řádku kódu: `sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);`
> 2. Dále změníme návratovou hodnotu na _sphere_ místo _sphereRadius_: `return = sphere;`. Díky tomu uvidíme v náhledu aplikace Dynamo obří koule!

![](<../images/8-1/4/functions - exercise - 08.jpg>)

> 1\. Chcete-li zmírnit velikost těchto koulí, aktualizujte hodnotu sphereRadius přidáním oddělovače: `sphereRadius = inputPt.Z/20;`. Nyní můžeme vidět jednotlivé koule a začít chápat vztah mezi poloměrem a hodnotou Z.

![](<../images/8-1/4/functions - exercise - 09.jpg>)

> 1. V uzlu **Point.ByCoordinates** změnou vázání z možnosti Nejkratší seznam na Kartézský součin vytvoříme osnovu bodů. Funkce _sphereByZ_ je stále plně funkční, takže všechny body vytvářejí koule s poloměry na základě hodnot Z.

![](<../images/8-1/4/functions - exercise - 10.jpg>)

> 1. A jen tak na zkoušku připojíme původní seznam čísel do vstupu X uzlu **Point.ByCoordinates**. Teď máme krychli koulí.
> 2. Poznámka: Pokud výpočet trvá na vašem počítači dlouhou dobu, zkuste změnit číslo _#10_ na hodnotu _#5_.

Pamatujte, že funkce _sphereByZ_, kterou jsme vytvořili, je obecná funkce, takže můžeme vyvolat šroubovici z předchozí lekce a použít na ni tuto funkci.

![](<../images/8-1/4/functions - exercise - 11.jpg>)

Jeden poslední krok: Pojďme řídit poměr poloměru s uživatelem definovaným parametrem. Chcete-li to udělat, je nutné vytvořit nový vstup pro funkci a také nahradit rozdělovač _20_ parametrem.

![](<../images/8-1/4/functions - exercise - 12.jpg>)

> 1. Aktualizujte definici _sphereByZ_ na:
>
>    ```
>    def sphereByZ(inputPt,radiusRatio)
>    {
>    //get Z Value, use it to drive radius of sphere
>    sphereRadius=inputPt.Z/radiusRatio;
>    //Define Sphere Geometry
>    sphere=Sphere.ByCenterPointRadius(inputPt,sphereRadius);
>    //Define output for function
>    return sphere;
>    };
>    ```
> 2. Aktualizujte podřazené **bloky kódu** přidáním proměnné ratio ke vstupu: `sphereByZ(Pt,ratio);`. Připojte posuvník k nově vytvořenému vstupu **bloku kódu** a změňte velikost poloměrů podle poměru poloměrů.
